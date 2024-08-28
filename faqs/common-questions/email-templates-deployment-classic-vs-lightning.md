# Email Templates Deployment: Classic vs Lightning

This document provides information on the issues that may arise during the deployment of email templates, particularly in the context of Salesforce Classic and Lightning.

## Packaging Lightning Email Templates

One of the known issues is that Lightning email templates cannot be packaged. This issue has been documented in the Salesforce CLI GitHub repository. For more information, refer to the following link: [Salesforce CLI GitHub Issue](https://github.com/forcedotcom/cli/issues/2902).

## Identifying the Type of Email Template

When working with email templates, it's important to identify the type of email template not from the file type, but from the type tag. This is because both types of templates are referred to as 'email', while one is of type 'SFX' and the other is of type 'Aloha'.

## Distinguishing Folders

Folders can be more reliably distinguished. They use 'EmailFolder' for Classic and 'EmailTemplateFolder' for Lightning as their opening tags. This distinction is important to note before deploying the templates.

Please ensure to take these precautions prior to deployment to avoid any issues.
