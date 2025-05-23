
# Custom Frigate Configuration for NVidia GTX 750 TI

## System Configuration
- **Host:** Proxmox → LXC → Docker
- **RAM:** 18 GB
- **GPU:** NVidia GTX 750 TI (2 GB RAM)
- **Storage:** SSD passed from host to LXC to Docker for media storage

---

## Overview
I have tried the Frigate LXC template from the helperscripts. It worked, but the Nvidia is always at 2% usage. I want to make use of the 2GB Nvidia card in full. So, I installed a Debian LXC on Proxmox.

If running directly on Debian, skip to **NVIDIA Installation on Debian**.

---

## If Using Proxmox

1. **Install NVIDIA on Proxmox host.** Many tutorials are available. [This one worked for me](https://www.youtube.com/watch?v=-Us8KPOhOCY). **Do not install the drivers in the lxc yet.**
2. **Install Debian LXC** from [this community script](https://community-scripts.github.io/ProxmoxVE/scripts?id=debian-vm).
3. **LXC container config** [included in this repo](proxmox.lxc.conf) for reference.

---

## NVIDIA Installation on Debian

### Method 1 (Preferred)

1. SSH into your Debian system.
2. Run the following to install NVIDIA runtime:
    ```bash
    sudo nvidia-ctk runtime configure --runtime=containerd
    ```

### Method 2 (Manual Driver Install. Only follow this if you can't get the above to work) 

1. Create a folder named `downloads`:
    ```bash
    mkdir downloads
    cd downloads
    ```
2. Download the Linux driver for your graphics card from the [NVIDIA Site](https://www.nvidia.com/en-us/drivers/).
    - For example: [Linux x64 Driver 570.124.04](https://www.nvidia.com/en-us/drivers/details/241089/)
    ```bash
    wget "https://us.download.nvidia.com/XFree86/Linux-x86_64/570.124.04/NVIDIA-Linux-x86_64-570.124.04.run"
    ```
3. Give execute permissions to the downloaded file:
    ```bash
    chmod +x NVIDIA-Linux-x86_64-570.124.04.run
    ```
4. Run the installer:
    ```bash
    sudo bash NVIDIA-Linux-x86_64-570.124.04.run
    ```

---

## Installing and Configuring Docker

1. Use Docker Compose to create the Frigate container. I tried many, but the [compose file in this repo](docker-compose.yml) worked for me.
2. Once inside the Frigate container, use the sample Frigate [config file in this repo](frigate.config.yaml).

---

Enjoy hardware-accelerated Frigate with your NVIDIA GTX 750 TI!
