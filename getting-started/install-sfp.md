# Install sfp

## Install sfp community edition

### A. Install sfp in your local machine

```
npm i -g @flxbl-io/sfp
```

### B. Check sfp version

```
sfp --version
@flxbl-io/sfp/37.0.0 darwin-arm64 node-v20.3.1
```



{% hint style="warning" %}
sfp requires node-gyp for its dependencies.  If you are facing issues during installation, with node-gyp,  please follow the instructions here [https://www.npmjs.com/package/node-gyp](https://www.npmjs.com/package/node-gyp)
{% endhint %}

### C.  Validate Installation

```
sfp commands

Command                         Summary
 ─────────────────────────────── ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 apextests trigger               Triggers Apex unit test in an org. Supports test level RunAllTestsInPackage, which optionally allows validation of individual class code coverage
 artifacts fetch                 Fetch sfp artifacts from a NPM compatible registry using a release definition file
 artifacts promote               Promotes artifacts predominantly for unlocked packages with code coverage greater than 75%
 artifacts query                 Fetch details about artifacts installed in a target org
 build                           Build artifact(s) of your packages in the current project
 ...
```



## Install sfp-pro

### Step 1: Download the Installer

1. Go to [https://source.flxbl.io/flxbl/sfp-pro/releases](https://source.flxbl.io/flxbl/sfp-pro/releases)
2. Select the latest release (or a specific version you need)
3. Download the appropriate installer for your platform:

| Platform                  | Installer | Filename Example                      |
|---------------------------|-----------|---------------------------------------|
| **Windows**               | MSI       | `sfp-pro-49.3.0-windows-x64.msi`      |
| **macOS**                 | DMG       | `sfp-pro-49.3.0-darwin-universal.dmg` |
| **Linux (Debian/Ubuntu)** | DEB       | `sfp-pro_49.3.0_linux_amd64.deb`      |
| **Linux (RHEL/Fedora)**   | RPM       | `sfp-pro_49.3.0_linux_amd64.rpm`      |

### Step 2: Install

#### Windows
```powershell
# Double-click the .msi file or run:
msiexec /i sfp-pro-*.msi
```

#### macOS
```bash
# 1. Open the DMG file
# 2. Drag sfp-pro.app to your Applications folder
# 3. Run the installer script from the DMG:
sudo bash /Volumes/sfp-pro-*/install-cli.sh
# Or if you've already unmounted the DMG:
sudo /Applications/sfp-pro.app/Contents/Resources/install-cli.sh
```

#### Linux (Debian/Ubuntu)
```bash
sudo dpkg -i sfp-pro_*.deb
```

#### Linux (RHEL/Fedora/CentOS)
```bash
sudo rpm -i sfp-pro_*.rpm
# or
sudo yum install sfp-pro_*.rpm
```

### Step 3: Verify Installation

```bash
sfp --version
# Example output: @flxbl-io/sfp/49.3.0 linux-x64 node-v22.0.0
```

### Updating sfp-pro

Download and run the latest installer - it will automatically upgrade your existing installation.

### Uninstalling

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
# or
sudo yum remove sfp-pro
```
