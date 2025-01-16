# README

## Overview

This repository contains Ansible playbooks to automate the installation and configuration of CUDA, NVIDIA drivers, Docker, NVIDIA Docker, and user setup with SSH keys. Below are detailed instructions for each playbook.

## Prerequisites

- Ansible installed on your control node
- SSH access to the target hosts with sudo privileges

## Playbooks

1. **CUDA Installing Playbook**
2. **NVIDIA Driver and Pip3 Installing Playbook**
3. **Docker Installing Playbook**
4. **NVIDIA Docker Setting Playbook**
5. **User and SSH Setup Playbook**

## Usage Instructions

### Playbook Execution Sequence

The playbooks should be executed in the following sequence:

1. **CUDA Installing Playbook**
2. **NVIDIA Driver and Pip3 Installing Playbook**
3. **Docker Installing Playbook**
4. **NVIDIA Docker Setting Playbook**

The **User and SSH Setup Playbook** can be executed at any time and is not dependent on the sequence of the other playbooks.

### 1. CUDA Installing Playbook

This playbook installs CUDA Toolkit 12.5 on a target host.

#### Steps:
1. Navigate to the directory containing the playbook.
2. Run the playbook with the following command:
   ```sh
   ansible-playbook -i hosts.txt CUDA_installing.yml

## 2. NVIDIA Driver and Pip3 Installing Playbook

This playbook installs the specified NVIDIA driver version and Pip3 on a target host.

### Steps:

1. Navigate to the directory containing the playbook.
2. Edit the `Installing_the_selected_version_of_nvidia_drivers_and_pip3.yml` file to specify the desired NVIDIA driver version under `vars`. (IMPORTANT Before installing a new version of drivers, you need to remove their previous version)
*
If the `nvidia-smi` command gives an error:
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.

(Run all subsequent commands on the target machine)
And the command data 

```sh
ls -l /usr/bin/nvidia-smi
ls -l /usr/bin/nvidia-settings
```

Give the following result:
```sh
-rwxr-xr-x 1 root root 1087072 Jul 26 13:36 /usr/bin/nvidia-smi
-rwxr-xr-x 1 root root 330888 Jul 26 13:36 /usr/bin/nvidia-settings
```

Then do the following:
```sh
sudo apt-get install linux-headers-$(uname -r)
sudo apt-get install dkms
sudo dkms install nvidia/560.28.03
```
In the place "560.28.03" indicate your driver version.

3. Run the playbook with the following command:

   ```sh
   ansible-playbook -i hosts.txt Installing_the_selected_version_of_nvidia_drivers_and_pip3.yml

## 3. Docker Installing Playbook

This playbook installs Docker on Ubuntu/Debian systems.

###Steps:
1. Navigate to the directory containing the playbook.
2. Run the playbook with the following command:

    ```sh
    ansible-playbook -i hosts.txt docker_playbook.yml
    ```

## 4. NVIDIA Docker Setting Playbook

This playbook sets up NVIDIA Docker to enable GPU support for Docker containers.

###Steps:
1. Navigate to the directory containing the playbook.
2. Run the playbook with the following command:

    ```sh
    ansible-playbook -i hosts.txt nvidia_docker_playbook.yml
    ```

## 5. User and SSH Setup Playbook

This playbook creates users and sets up SSH keys for them.

###Steps:
1. Navigate to the directory containing the playbook.
2. Edit the `setup_user_and_ssh.yml` file to add or modify users and their respective GitHub keys URL under `vars`.
3. Run the playbook with the following command:

    ```sh
    ansible-playbook -i hosts.txt setup_user_and_ssh.yml
    ```

## Inventory File

An inventory file specifies the target hosts. Here is an example inventory file (`hosts.txt`):

```ini
client01 ansible_host=<IP_ADDRESS> ansible_user=<USERNAME> ansible_ssh_private_keyfile=<PATH_TO_PRIVATE_KEY>
client02 ansible_host=<IP_ADDRESS> ansible_user=<USERNAME> ansible_ssh_pass=<PASSWORD>
