# Install sfp-pro

### Step 1: Download the Installer

1. Go to [https://source.flxbl.io/flxbl/sfp-pro/releases](https://source.flxbl.io/flxbl/sfp-pro/releases)
2. Select the latest release (or a specific version you need)
3. Download the appropriate installer for your platform:

| Platform                  | Installer | Filename Example                      |
| ------------------------- | --------- | ------------------------------------- |
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
