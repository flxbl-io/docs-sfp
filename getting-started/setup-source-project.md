# Setup Source Project

## A. Create a new flxbl Project

```bash
sf project generate -n flxbl-demo -p flxbl -t standard

target dir = /Users/user.name/Repository
   create flxbl-demo/config/project-scratch-def.json
   create flxbl-demo/README.md
   create flxbl-demo/sfdx-project.json
   create flxbl-demo/.husky/pre-commit
   create flxbl-demo/.vscode/extensions.json
   create flxbl-demo/.vscode/launch.json
   create flxbl-demo/.vscode/settings.json
   create flxbl-demo/flxbl/main/default/lwc/.eslintrc.json
   create flxbl-demo/flxbl/main/default/aura/.eslintrc.json
   create flxbl-demo/scripts/soql/account.soql
   create flxbl-demo/scripts/apex/hello.apex
   create flxbl-demo/.forceignore
   create flxbl-demo/.gitignore
   create flxbl-demo/.prettierignore
   create flxbl-demo/.prettierrc
   create flxbl-demo/jest.config.js
   create flxbl-demo/package.json
```

## B. Setup flxbl-demo project in VS Code

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption><p>VS Code - flxbl-demo</p></figcaption></figure>

## C. Set flxbl-demo Developer Org in VS Code

1. In VS Code, go to **View > Command Pallette**
2. Enter in `> SFDX: Set a Default Org`
3. Select the "flxbl-demo" org alias from your list.

<figure><img src="../.gitbook/assets/image (24).png" alt=""><figcaption><p>Set a Default Org</p></figcaption></figure>

## D. Create a Custom Object in your Org.

1. Open up the flxbl-demo org.
2.  Navigate to **Setup > Object Manager**\


    <figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption><p>Object Manager</p></figcaption></figure>
3. Click on **Create > Create Custom Object**
4.  Create a new object named "Flxbl"\


    <figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>
5.  Click **Save**\


    <figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption><p>Flxbl Object</p></figcaption></figure>
6.
