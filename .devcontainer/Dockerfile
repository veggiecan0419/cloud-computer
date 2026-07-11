# Use the base image
FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

# Enable privileged mode
USER root

# Set up environment variables to suppress interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Preconfigure the keyboard layout
RUN echo 'keyboard-configuration keyboard-configuration/layoutcode string us' | debconf-set-selections && \
    echo 'keyboard-configuration keyboard-configuration/layout select English (US)' | debconf-set-selections

# Install necessary packages
RUN apt-get update && \
    apt-get upgrade -y && \
	# Installing cinnamon and xfce4. Installing xfce4 solves grey screen issue
    apt-get install -y cinnamon-desktop-environment dbus-x11 xfce4 xfce4-goodies python3-pip pipx cabextract htop software-properties-common wget && \
	# Download and install KasmVNC
	wget https://github.com/kasmtech/KasmVNC/releases/download/v1.4.0/kasmvncserver_noble_1.4.0_amd64.deb && \
	apt-get install -y ./kasmvncserver_noble_1.4.0_amd64.deb && \
	rm ./kasmvncserver_noble_1.4.0_amd64.deb && \
	# Add vscode user to ssl-cert group
	id -u vscode &>/dev/null && adduser vscode ssl-cert || true && \
	# Remove dummy Firefox
    apt-get remove -y firefox && \ 
	# Install Firefox without Snap
	echo 'Package: firefox*\nPin: release o=LP-PPA-mozillateam\nPin-Priority: 1001' | tee /etc/apt/preferences.d/Mozilla && \
	add-apt-repository ppa:mozillateam/ppa && \
	apt-get update && \
    apt-get install -y firefox
