---
description: >-
  This section deals with setting up a GitHub App which is required for sfp-pro
  server  to integrate with your GitHub org
---

# Connecting GitHub as a CI/CD provider

sfp-pro server require additional permissions which allow to write into your repository, sync environments and also permission to trigger workflows etc.

These permissions are beyond what is being provided by the built in GITHUB\_TOKEN. A Github App is recommended over using a Service Account and its Personal Access Token, as the service account takes an additional license and has limitations on the api requests.

This guide is crafted to facilitate the user to create a sfops-bot GitHub App to integrate with sfp-server. It provides a step-by-step approach for creating the app, elaborating on the necessary permissions, installation, and secure storage of sensitive information. \
You can refer to this link to understand how this work behind the scenes​

[https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/making-authenticated-api-requests-with-a-github-app-in-a-github-actions-workflow#authenticating-with-a-github-app](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/making-authenticated-api-requests-with-a-github-app-in-a-github-actions-workflow#authenticating-with-a-github-app)

### Step-by-Step Creation and Configuration <a href="#step-by-step-creation-and-configuration" id="step-by-step-creation-and-configuration"></a>

#### **Step 1: Registration of sfops-bot GitHub App** <a href="#step-1-registration-of-sfops-bot-github-app" id="step-1-registration-of-sfops-bot-github-app"></a>

* Navigate to your GitHub organization's settings.
* Click on "Developer settings" and select "GitHub Apps".
* Hit "New GitHub App" and input `sfops-bot` as the name.
* Add an icon and background color in the 'Display Information' to make the app identifiable in your workflows

#### **Step 2: Permissions Configuration** <a href="#step-2-permissions-configuration" id="step-2-permissions-configuration"></a>

* Assign the app permissions based on the requirements for sfops:

**Repository Permissions**

* **Contents**: Set to read and write for the app to manage code, branches, commits, and merges. This access allows the app to automate code integration processes.
* **Issues**: Read and write permissions enable the app to automate issue tracking, commenting, and labeling.
* **Deployments**: Read and write access empowers the app to manage deployments, essential for continuous delivery workflows.
* **Environments**: Read and write access to create environments, which will be consumed by workflows
* **Pull Requests**: Read and write permissions are necessary for the app to automate the handling of pull requests, including merging and labeling.
* **Actions**: Read and write access is crucial for the app to manage GitHub Actions, which are integral to automation workflows.
* **Variables:** Read and write permissions that permit the app to read the variables in the repo, This is vital for dynamic configuration of the environment and branch related configurations
* **Workflows**: Read and write permissions permit the app to update workflow files, which is vital for maintaining automated processes.



#### **Step 3: Generate and Secure a Private Key** <a href="#step-3-generate-and-secure-a-private-key" id="step-3-generate-and-secure-a-private-key"></a>

* In the 'General' section of your app's settings, locate the 'Private keys' subsection.
* Click on "Generate a private key" and download the `.pem` file immediately to your secure location.

#### **Step 4: Installation of the App** <a href="#step-4-installation-of-the-app" id="step-4-installation-of-the-app"></a>

* Navigate to the 'Install App' tab within your app settings.
* Click "Install" to initiate the installation process.
* Select your organization and choose to install the app on all repositories or specific ones such as Salesforce repositories

#### **Step 5: Storing the Private Key and App ID as Secrets** <a href="#step-5-storing-the-private-key-and-app-id-as-secrets" id="step-5-storing-the-private-key-and-app-id-as-secrets"></a>

* Store these keys in your secrets provider or in the .env file under GITHUB\_APP\_ID and GITHUB\_APP\_PRIVATE\_KEY

{% hint style="warning" %}
You would also need to configure a GITHUB\_TOKEN env variable, which has the following scope, packages:write and repo:read \
\
This is due to the fact the GITHUB Apps  currently do not support operations on GitHub packages.
{% endhint %}
