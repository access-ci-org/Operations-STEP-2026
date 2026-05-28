### Install VS Code APT Repository on Ubuntu Linux (and derivatives)

This method adds the official Microsoft repository to your system so you can install and update VS Code natively via `apt`. Run these commands sequentially in your terminal:

**1. Update your package index and install dependencies:**

```bash
sudo apt update
sudo apt install wget gpg apt-transport-https -y

```

**2. Download and install the Microsoft GPG key:**

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg

```

**3. Add the Visual Studio Code repository to your sources:**

```bash
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | sudo tee /etc/apt/sources.list.d/vscode.list > /dev/null

```

**4. Clean up the temporary GPG file, update `apt`, and install VS Code:**

```bash
rm -f packages.microsoft.gpg
sudo apt update
sudo apt install code -y

```

