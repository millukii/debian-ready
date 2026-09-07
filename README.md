# debian-ready
Debian 13 User mode

## Add user to sudoers
- sudo visudo -f /etc/sudoers.d/mr
mr ALL=(ALL:ALL) ALL

## Add Repo
- sudo nano /etc/apt/sources.list

- deb https://deb.debian.org/debian/ trixie contrib main non-free non-free-firmware
- deb https://deb.debian.org/debian/ trixie-updates contrib main non-free non-free-firmware
- deb https://deb.debian.org/debian/ trixie-proposed-updates contrib main non-free non-free-firmware
- deb https://deb.debian.org/debian/ trixie-backports contrib main non-free non-free-firmware
- deb https://security.debian.org/debian-security/ trixie-security contrib main non-free non-free-firmware

## Update & upgrade

- sudo apt update -y
- sudo apt full-upgrade -y apt install build-essential make automake cmake autoconf git wget

## Install
- sudo apt install linux-headers-$(uname -r)
- sudo apt install build-essential make automake cmake autoconf git wget
- sudo apt install ufw gufw
- sudo ufw enable
- sudo apt install clamav clamav-daemon clamtk
- sudo apt install ffmpeg libavcodec-extra gstreamer1.0-libav gstreamer1.0-plugins-ugly gstreamer1.0-plugins-bad gstreamer1.0-pulseaudio vorbis-tools flac
- sudo apt install vlc
- sudo apt install fonts-freefont-ttf fonts-freefont-otf
- sudo apt-get install ttf-mscorefonts-installer
- sudo apt-get install unrar-free unace sharutils lhasa
- sudo apt install hardinfo
- sudo apt install psensor
- sudo apt install curl

## Tools
### VSCODE
- wget https://vscode.download.prss.microsoft.com/dbazure/download/stable/a44adf7f53e00964ab890f9f8758a334f1fc15bc/code_1.136.1-1788413865_amd64.deb
- sudo dpkg -i code_1.136.1-1788413865_amd64.deb
### POSTMAN
- tar -C /tmp/ -xzf <(curl -L https://dl.pstmn.io/download/latest/linux64) && sudo mv /tmp/Postman /opt/
- sudo tee -a /usr/share/applications/postman.desktop << END
[Desktop Entry]
Encoding=UTF-8
Name=Postman
Exec=/opt/Postman/Postman
Icon=/opt/Postman/app/resources/app/assets/icon.png
Terminal=false
Type=Application
Categories=Development;
END
### DOCKER
- sudo apt update
- sudo apt install ca-certificates curl
- sudo install -m 0755 -d /etc/apt/keyrings
- sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
- sudo chmod a+r /etc/apt/keyrings/docker.asc
- sudo usermod -aG docker mr

#### Add the repository to Apt sources:
- sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF
- sudo apt update
- sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
###ANDROID STUDIO
- wget https://dl.google.com/android/asfp/asfp-2023.1.1.19-linux.deb?hl=es-419
- sudo dpkg -i asfp-2023.1.1.19-linux.deb
- /opt/android-studio-for-platform/bin/studio.sh

### FLUTTER
- sudo apt-get update -y && sudo apt-get upgrade -y
- sudo apt-get install -y curl git unzip xz-utils zip libglu1-mesa
- wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_3.47.2-stable.tar.xz
- tar -xf flutter_linux_3.47.2-stable.tar.xz -C ~/Develop/
- echo 'export PATH="$HOME/Develop/flutter/bin:$PATH"' >> ~/.bashrc

### GIT GITHUB
- git config --global user.name "Your Name"
- git config --global user.email "your_email@example.com"
- (type -p wget >/dev/null || (sudo apt update && sudo apt install wget -y)) \
	&& sudo mkdir -p -m 755 /etc/apt/keyrings \
	&& out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
	&& cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
	&& sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
	&& sudo mkdir -p -m 755 /etc/apt/sources.list.d \
	&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
	&& sudo apt update \
	&& sudo apt install gh -y
