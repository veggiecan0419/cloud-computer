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
    # Installing cinnamon. Installing xfce4 solves grey screen issue
    apt-get install -y cinnamon-desktop-environment dbus-x11 xfce4 xfce4-goodies \
    python3-pip pipx cabextract htop software-properties-common wget curl ssl-cert && \
    # Remove dummy Firefox
    apt-get remove -y firefox && \ 
    # Install Firefox without Snap
    echo 'Package: firefox*\nPin: release o=LP-PPA-mozillateam\nPin-Priority: 1001' | tee /etc/apt/preferences.d/Mozilla && \
    add-apt-repository -y ppa:mozillateam/ppa && \
    apt-get update && \
    apt-get install -y firefox

# Download and Install KasmVNC for Ubuntu 24.04 (Noble)
RUN KASMVNC_TAG=$(curl -s https://api.github.com/repos/kasmtech/KasmVNC/releases/latest | grep -oP '"tag_name": "\K[^"]*') && \
    KASMVNC_VERSION=${KASMVNC_TAG#v} && \
    wget https://github.com/kasmtech/KasmVNC/releases/download/${KASMVNC_TAG}/kasmvncserver_noble_${KASMVNC_VERSION}_amd64.deb -O /tmp/kasmvnc.deb && \
    apt-get update && \
    apt-get install -y /tmp/kasmvnc.deb && \
    rm /tmp/kasmvnc.deb

# Add the default devcontainer user to the ssl-cert group so KasmVNC can run
RUN adduser vscode ssl-cert

# Pre-create KasmVNC configuration directory to avoid permission issues later
RUN mkdir -p /home/vscode/.kasmenv /home/vscode/.vnc && \
    chown -R vscode:vscode /home/vscode/.kasmenv /home/vscode/.vnc
