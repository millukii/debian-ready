# debian-ready
Debian 13 user setup guide

This guide was reconstructed from the actual shell history used on the machine and completes the setup flow that was executed.

## Desktop environment and application launchers

The desktop environment used by this setup is **KDE Plasma**. Application shortcuts are being added as desktop-entry files. User-specific launchers are stored in:

```text
~/.local/share/applications/
```

For example, the VS Code launcher is created at `~/.local/share/applications/code.desktop` and starts VS Code with GPU acceleration disabled and the X11 ozone backend enabled:

```bash
mkdir -p ~/.local/share/applications
tee ~/.local/share/applications/code.desktop <<'EOF'
[Desktop Entry]
Name=Visual Studio Code
Comment=Code Editing. Redefined.
Exec=/usr/bin/code --disable-gpu --ozone-platform=x11 %F
Icon=code
Terminal=false
Type=Application
Categories=Development;IDE;
MimeType=text/plain;inode/directory;
StartupNotify=true
StartupWMClass=Code
EOF
```

The important launch options are:

```text
--disable-gpu --ozone-platform=x11
```

After creating or changing a launcher, KDE may need to refresh its application menu or restart the application launcher.

## 1) Add user to sudoers

```bash
sudo visudo -f /etc/sudoers.d/mr
```

Content:

```bash
mr ALL=(ALL:ALL) ALL
```

## 2) Add Debian repositories

```bash
sudo nano /etc/apt/sources.list
```

Add:

```bash
deb https://deb.debian.org/debian/ trixie contrib main non-free non-free-firmware
deb https://deb.debian.org/debian/ trixie-updates contrib main non-free non-free-firmware
deb https://deb.debian.org/debian/ trixie-proposed-updates contrib main non-free non-free-firmware
deb https://deb.debian.org/debian/ trixie-backports contrib main non-free non-free-firmware
deb https://security.debian.org/debian-security/ trixie-security contrib main non-free non-free-firmware
```

## 3) System update and base packages

```bash
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y build-essential make automake cmake autoconf git wget curl
sudo apt install -y linux-headers-$(uname -r)
```

## 4) General utilities and security

```bash
sudo apt install -y ufw gufw
sudo ufw enable

sudo apt install -y clamav clamav-daemon clamtk
sudo apt install -y ffmpeg libavcodec-extra gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-plugins-bad gstreamer1.0-pulseaudio vorbis-tools flac
sudo apt install -y vlc
sudo apt install -y fonts-freefont-ttf fonts-freefont-otf
sudo apt-get install -y ttf-mscorefonts-installer
sudo apt-get install -y unrar-free unace sharutils lhasa
sudo apt install -y hardinfo
sudo apt install -y psensor
```

## 5) Git, GitHub and SSH

```bash
git config --global user.name "MR"
git config --global user.email "melody56789@gmail.com"
ssh-keygen -t rsa -b 4096 -C "melody56789@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub
ssh -T git@github.com
```

## 6) GitHub CLI (gh)

```bash
(type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) \
  && sudo mkdir -p -m 755 /etc/apt/keyrings \
  && out=$(mktemp) \
  && wget -nv -O "$out" https://cli.github.com/packages/githubcli-archive-keyring.gpg \
  && cat "$out" | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
  && sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
  && sudo mkdir -p -m 755 /etc/apt/sources.list.d \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
  && sudo apt update \
  && sudo apt install gh -y
```

Alternative simple install:

```bash
sudo apt update
sudo apt install gh
```

## 7) VS Code

```bash
wget https://vscode.download.prss.microsoft.com/dbazure/download/stable/a44adf7f53e00964ab890f9f8758a334f1fc15bc/code_1.136.1-1788413865_amd64.deb
sudo dpkg -i code_1.136.1-1788413865_amd64.deb
```

## 8) GitHub Copilot

Install the VS Code editor extension:

```bash
code --install-extension GitHub.copilot
code --install-extension GitHub.copilot-chat
```

Sign in to GitHub from VS Code when prompted after installation.

## 9) Postman

```bash
tar -C /tmp/ -xzf <(curl -L https://dl.pstmn.io/download/latest/linux64) && sudo mv /tmp/Postman /opt/
```

Create desktop entry:

```bash
sudo tee -a /usr/share/applications/postman.desktop <<'EOF'
[Desktop Entry]
Encoding=UTF-8
Name=Postman
Exec=/opt/Postman/Postman
Icon=/opt/Postman/app/resources/app/assets/icon.png
Terminal=false
Type=Application
Categories=Development;
EOF
```

## 10) Docker

```bash
sudo apt update
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
sudo usermod -aG docker mr
```

Add Docker APT repository:

```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
```

Install Docker:

```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 11) Android Studio

```bash
wget "https://dl.google.com/android/asfp/asfp-2023.1.1.19-linux.deb?hl=es-419"
sudo dpkg -i asfp-2023.1.1.19-linux.deb
/opt/android-studio-for-platform/bin/studio.sh
```

## 12) Flutter

```bash
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install -y curl git unzip xz-utils zip libglu1-mesa
wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.47.2-stable.tar.xz
tar -xf flutter_linux_3.47.2-stable.tar.xz -C ~/Develop/
echo 'export PATH="$HOME/Develop/flutter/bin:$PATH"' >> ~/.bashrc
```

Verify:

```bash
source ~/.bashrc
flutter --version
dart --version
```

## 13) Node.js and Gemini CLI

Install Node.js and npm:

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
node -v
npm -v
```

Install Gemini CLI:

```bash
sudo npm install -g @google/gemini-cli
gemini
```

## 14) Antigravity IDE

```bash
sudo mkdir -p /opt/antigravity-ide
sudo tar -xvzf AntigravityIDE.tar.gz -C /opt/antigravity-ide/
sudo ln -sf /opt/antigravity-ide/Antigravity-IDE/antigravity-ide /usr/local/bin/antigravity-ide
```

Create a desktop entry:

```bash
sudo tee /usr/share/applications/antigravity-ide.desktop <<'EOF'
[Desktop Entry]
Name=Antigravity IDE
Exec=/ruta/a/tu/antigravity
Icon=/ruta/a/tu/icono.png
Terminal=false
Type=Application
Categories=Development;
EOF
```

## 15) Recommended setup flow

1. Configure sudo and APT repositories.
2. Update the system and install base packages.
3. Install general utilities.
4. Configure Git and SSH/GitHub.
5. Install VS Code, GitHub Copilot, Docker, Flutter, Node.js, and Gemini CLI.
6. Install the extra IDE and create desktop launchers.

This sequence matches the actual command history used on the machine and can be reused as a practical Debian 13 setup checklist.
