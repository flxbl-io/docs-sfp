---
metaLinks:
  alternates:
    - https://app.gitbook.com/s/YLI5Ts7pWhWQV9UaBn3H/getting-started/install-sfp
---

# Install sfp

## Step 1: Download and Install the CLI

1. Go to [https://source.flxbl.io/flxbl/sfp-pro/releases](https://source.flxbl.io/flxbl/sfp-pro/releases)
2. Select the latest release
3. Download the appropriate installer for your platform:

| Platform                  | Installer | Filename Example                       |
| ------------------------- | --------- | -------------------------------------- |
| **Windows**               | MSI       | `sfp-pro-51.0.0-windows-x64.msi`      |
| **macOS**                 | DMG       | `sfp-pro-51.0.0-darwin-universal.dmg`  |
| **Linux (Debian/Ubuntu)** | DEB       | `sfp-pro_51.0.0_linux_amd64.deb`       |
| **Linux (RHEL/Fedora)**   | RPM       | `sfp-pro_51.0.0_linux_amd64.rpm`       |

#### Windows

```powershell
msiexec /i sfp-pro-*.msi
```

#### macOS

```bash
# Open the DMG, drag sfp-pro.app to Applications, then run:
sudo bash /Volumes/sfp-pro-*/install-cli.sh
```

#### Linux (Debian/Ubuntu)

```bash
sudo dpkg -i sfp-pro_*.deb
```

#### Linux (RHEL/Fedora/CentOS)

```bash
sudo rpm -i sfp-pro_*.rpm
```

Verify the installation:

```bash
sfp --version
```

## Step 2: Connect to your sfp Server

Your sfp server URL is provided by your administrator or your codev platform. Configure it:

```bash
sfp config set server-url https://yourcompany.flxbl.io
```

## Step 3: Authenticate

Log in to the sfp server using your email:

```bash
sfp auth login --email your@email.com
```

This opens your browser for OAuth authentication (GitHub by default). Once complete, your token is securely stored in your OS keychain.

Verify your authentication:

```bash
sfp auth display
```

{% hint style="info" %}
For CI/CD pipelines, use application tokens instead of interactive login. See [Server Authentication](../../auth-management/server-authentication.md) for details.
{% endhint %}

{% hint style="info" %}
Your project and Salesforce orgs should already be registered with the sfp server by your administrator through the codev platform. If not, see [Project](../../collaborate/project.md) and [Org Registration](../../environment-management/environments/org-registration.md).
{% endhint %}

## Updating sfp

Download and run the latest installer — it will automatically upgrade your existing installation.

## Uninstalling

#### Windows

Use "Add or Remove Programs" in Control Panel

#### macOS

```bash
sudo rm -rf /Applications/sfp-pro.app
sudo rm -f /usr/local/bin/sfp
```

#### Linux (Debian/Ubuntu)

```bash
sudo dpkg -r sfp-pro
```

#### Linux (RHEL/Fedora/CentOS)

```bash
sudo rpm -e sfp-pro
```
