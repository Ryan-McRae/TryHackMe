# TryHackMe Documentation

## Introduction

This repository contains my documentation and walkthroughs for various Capture The Flag (CTF) challenges on TryHackMe, and potentially other cybersecurity challenges in the future. The primary purpose of this repository is to showcase my learning progress as well as the technical skills I acquire while completing these challenges.

My academic and professional background is in Electrical and Electronic Engineering, so much of this material is new to me. As such, there will inevitably be mistakes. I intend to revisit and correct them as my understanding improves. Constructive feedback and advice are always welcome.

## Setup

For these challenges, I use a virtualized Kali Linux environment running on an Ubuntu host machine.

### Virtualization Environment

Initially, I planned to use **VirtualBox** with the latest available Kali Linux VirtualBox image:

- `kali-linux-2025.3-virtualbox-amd64`

However, during setup I encountered a conflict caused by another virtualization system already present on my machine: **KVM**. After further investigation, I decided to switch to KVM-based virtualization. Many sources report superior performance on Linux hosts compared to VirtualBox, although this claim is not objectively guaranteed. Regardless, it was an approach I had not explored before and decided to adopt.

### Installing KVM and Required Tools

To get started with KVM, I installed the necessary packages using the following command:

```bash
sudo apt install virt-manager qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
```

These packages provide:

- `virt-manager`: A GUI application for managing virtual machines
- `qemu-kvm`: The core virtualization engine
- `libvirt-daemon-system`: Background services for managing virtualization
- `libvirt-clients`: Command-line management tools
- `bridge-utils`: Networking utilities for VM network interfaces

### User Permissions

By default, only the root user can access virtualization features. To allow my user account to create and manage virtual machines, I added it to the appropriate groups:

```bash
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

After this change, a logout or system restart is required for the new group memberships to take effect.

### Enabling the Virtualization Service

I then enabled and started the libvirt service so that it runs automatically on boot:

```bash
sudo systemctl enable --now libvirtd
```

### Kali Linux VM Configuration

The Kali Linux virtual machine was configured with the following resources:

- **Memory**: 4 GB RAM
- **CPU**: 3 vCPUs
- **Networking**: NAT

Using NAT networking ensures that the VM shares the host’s network connection. As a result, any VPN connection established on the host machine is also accessible from within the VM.

### TryHackMe VPN Connection

Once the VM was created, I downloaded the OpenVPN configuration file from the TryHackMe _Access_ page on my account.

After installing OpenVPN on the host machine, I established the VPN connection using:

```bash
sudo openvpn filename.ovpn
```

To verify the connection, I first checked the TryHackMe access page and then confirmed that the tunnel interface was active:

```bash
ip addr show tun0
```

At this point, the host machine (and by extension the Kali VM) is successfully connected to the TryHackMe network, allowing interaction with challenge machines.
