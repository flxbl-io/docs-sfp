# Setting up SFP Server on EC2

This guide provides a definitive, step-by-step process for deploying SFP Server to an AWS EC2 instance.

### 1. Prerequisites

Before you begin, ensure all the following requirements are met.

#### Local Machine Requirements

The machine you run the deployment commands from must have:

* **AWS CLI**: To interact with AWS services.
* **jq**: A command-line JSON processor.
* **sfp CLI**: The SFP command-line interface. Install with `npm install -g @flxbl-io/sfp`.
* **SSH Key Pair**: An SSH private key (`.pem` or similar) that corresponds to the public key installed on your EC2 instance.

#### AWS Infrastructure Requirements

* **EC2 Instance**:
  * **OS**: Amazon Linux 2 (Recommended) or Ubuntu 22.04.
  * **Instance Size**:
    * **Recommended**: `t3.large` (2 vCPU, 8 GB RAM) or greater for production workloads.
    * **Minimum**: `t3.medium` (2 vCPU, 4 GB RAM) for light usage or evaluation.
  * **Storage**: 50 GB of EBS storage (gp3) is recommended.
  *   **IAM Role**: The EC2 instance **must** have an IAM role attached with a policy granting read access to the secrets you will create in AWS Secrets Manager.

      ```json
      {
          "Version": "2012-10-17",
          "Statement": [
              {
                  "Effect": "Allow",
                  "Action": "secretsmanager:GetSecretValue",
                  "Resource": "arn:aws:secretsmanager:YOUR_REGION:YOUR_ACCOUNT_ID:secret:sfp-server/*"
              }
          ]
      }
      ```
* **Security Group**: Configure the EC2 instance's security group to:
  * **Allow Inbound SSH (Port 22)** from your local machine's IP address for deployment.
  * **Allow Inbound HTTPS (Port 443)** from the internet (`0.0.0.0/0`) so users can access the server.

#### Service Requirements

* **Supabase Project**: A fully provisioned, publicly accessible Supabase project (either on Supabase Cloud or self-hosted). You will need the following:
  * Supabase URL
  * Service Key
  * Anon Key
  * JWT Secret
* **GitHub App**: A configured GitHub App for repository integration. You will need:
  * App ID
  * App Private Key
* **Domain Name**: A registered domain name (e.g., `sfp.yourcompany.com`) that you can point to the EC2 instance's public IP address.

### 2. Configuration

This section covers the one-time setup for your secrets and local environment.

#### Step 1: Store Credentials in AWS Secrets Manager

The `sfp server init` command fetches secrets from your local environment. The recommended way to manage these is to store them in AWS Secrets Manager and load them locally.

Create the following secrets in AWS Secrets Manager:

1.  **`sfp-server/supabase`**: A secret containing your Supabase credentials.

    ```bash
    aws secretsmanager create-secret --name "sfp-server/supabase" --secret-string '{
        "SUPABASE_URL": "YOUR_SUPABASE_URL",
        "SUPABASE_SERVICE_KEY": "YOUR_SUPABASE_SERVICE_KEY",
        "SUPABASE_ANON_KEY": "YOUR_SUPABASE_ANON_KEY",
        "SUPABASE_JWT_SECRET": "YOUR_SUPABASE_JWT_SECRET",
        "SUPABASE_ENCRYPTION_KEY": "YOUR_BASE64_ENCODED_ENCRYPTION_KEY"
    }'
    ```

    _Note: The `SUPABASE_ENCRYPTION_KEY` must be a 32-byte, Base64-encoded string._
2.  **`sfp-server/github`**: A secret for your GitHub App credentials.

    ```bash
    # The private key must be formatted as a single-line string with \n for newlines.
    export GITHUB_KEY=$(awk 'NF {printf "%s\n", $0}' /path/to/your-github-app.pem)

    aws secretsmanager create-secret --name "sfp-server/github" --secret-string "{
        "GITHUB_APP_ID": "YOUR_APP_ID",
        "GITHUB_APP_PRIVATE_KEY": "$GITHUB_KEY"
    }"
    ```

#### Step 2: Prepare Your Local Terminal

Before running any `sfp server` command, you must export the secrets from AWS into your local terminal session. This allows the CLI to securely forward them to the remote server.

```bash
# Run this in your terminal before deployment
# This command fetches the secrets and exports them as environment variables

export $(aws secretsmanager get-secret-value \
  --secret-id sfp-server/supabase \
  --query SecretString \
  --output text | jq -r 'to_entries|map("\\(.key)=\\(.value)")|.[]')

export $(aws secretsmanager get-secret-value \
  --secret-id sfp-server/github \
  --query SecretString \
  --output text | jq -r 'to_entries|map("\\(.key)=\\(.value)")|.[]')
```

**This step must be repeated for each new terminal session.**

### 3. Deployment

Follow these steps to provision the EC2 instance and deploy the SFP Server.

#### Step 1: Prepare the EC2 Instance

Connect to your newly created EC2 instance and run the following script. This installs Docker, Docker Compose, and other dependencies.

```bash
# SSH into your instance
ssh -i /path/to/your-key.pem ec2-user@<YOUR_EC2_IP>

# --- Run the following commands on the EC2 instance ---

# 1. Update system and install Docker
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user

# 2. Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# 3. Create the base directory for the server
sudo mkdir -p /opt/sfp-server
sudo chown ec2-user:ec2-user /opt/sfp-server

# 4. Log in to the Docker registry (if using a private image, e.g., ghcr.io)
# This is critical for the server to be able to pull the SFP Server image.
# For GHCR, use a Personal Access Token (PAT) with 'read:packages' scope.
echo "YOUR_GITHUB_PAT" | sudo docker login ghcr.io -u YOUR_GITHUB_USERNAME --password-stdin

# 5. Log out for group changes to take effect
exit
```

#### Step 2: Deploy the Server

**Run this command from your local machine.** Ensure you have exported your local environment variables as described in Configuration Step 2.

```bash
sfp server init \
  --tenant your-company-name \
  --mode prod \
  --secrets-provider custom \
  --domain sfp.yourcompany.com \
  --base-dir /opt/sfp-server \
  --ssh-host <YOUR_EC2_IP> \
  --ssh-username ec2-user \
  --ssh-key-path /path/to/your-key.pem \
  --no-interactive
```

* `--secrets-provider custom`: This is mandatory and tells the CLI to use the variables you exported locally.
* `--ssh-*`: These flags provide the connection details for your remote EC2 instance.
* `--no-interactive`: Ensures the command runs without prompts, making it suitable for automation.

#### Step 3: Configure Auto-Start on Reboot

To ensure the SFP Server restarts automatically if the EC2 instance is rebooted, connect to your instance one last time.

```bash
# SSH into your instance
ssh -i /path/to/your-key.pem ec2-user@<YOUR_EC2_IP>

# --- Run the following commands on the EC2 instance ---

# 1. Enable the Docker service to start on boot
sudo systemctl enable docker

# 2. Set the restart policy for all running SFP containers
# This ensures that if a container stops, Docker will restart it.
docker update --restart=unless-stopped $(docker ps -q)
```

Your SFP Server is now fully deployed and configured to run reliably.

### 4. Server Management

For all ongoing management tasks (starting, stopping, updating), run the following commands from your **local machine**.

**Important**: Remember to export your secrets to your local environment (Configuration Step 2) before running these commands if you are in a new terminal session.

**Start the Server:** (Only needed if you manually stop it)

```bash
sfp server start \
  --tenant your-company-name \
  --secrets-provider custom \
  --ssh-host <YOUR_EC2_IP> \
  --ssh-username ec2-user \
  --ssh-key-path /path/to/your-key.pem
```

**Stop the Server:**

```bash
sfp server stop \
  --tenant your-company-name \
  --ssh-host <YOUR_EC2_IP> \
  --ssh-username ec2-user \
  --ssh-key-path /path/to/your-key.pem
```

**Update the Server to a New Version:**

```bash
sfp server update \
  --tenant your-company-name \
  --secrets-provider custom \
  --ssh-host <YOUR_EC2_IP> \
  --ssh-username ec2-user \
  --ssh-key-path /path/to/your-key.pem
```
