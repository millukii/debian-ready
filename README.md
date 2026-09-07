# debian-ready
Debian 13 User mode

## Add user to sudoers
- sudo visudo -f /etc/sudoers.d/mr
mr ALL=(ALL:ALL) ALL

## Add Repo
- sudo nano /etc/apt/sources.list

deb https://deb.debian.org/debian/ trixie contrib main non-free non-free-firmware
deb https://deb.debian.org/debian/ trixie-updates contrib main non-free non-free-firmware
deb https://deb.debian.org/debian/ trixie-proposed-updates contrib main non-free non-free-firmware
deb https://deb.debian.org/debian/ trixie-backports contrib main non-free non-free-firmware
deb https://security.debian.org/debian-security/ trixie-security contrib main non-free non-free-firmware

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
- sudo  apt install psensor
