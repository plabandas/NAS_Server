# NAS & FTP Server With Orange Pi

This guide provides step-by-step instructions to set up OpenMediaVault (OMV6) on an Orange Pi Zero 2 running Debian Linux.

<details>
<summary><strong><big>Table of Contents</big></strong></summary>

<!-- TOC -->
- [Watch Server Setup Output](#watch-server-setup-output)
- [Watch Full Server Setup Video](#watch-full-server-setup-video)
- [Prerequisites](#prerequisites)
- [Setup Steps (Quick Setup After System Crash)](#setup-steps-quick-setup-after-system-crash)
  - [1. Download Required Files](#1-download-required-files)
  - [2. Prepare the SD Card](#2-prepare-the-sd-card)
  - [3. Connect the Orange Pi to the Network](#3-connect-the-orange-pi-to-the-network)
  - [4. Assign a Static IP Address](#4-assign-a-static-ip-address)
  - [5. Access the Orange Pi via SSH](#5-access-the-orange-pi-via-ssh)
  - [6. Update the Debian OS](#6-update-the-debian-os)
  - [7. Install OpenMediaVault](#7-install-openmediavault)
  - [8. Access the OpenMediaVault Web Interface](#8-access-the-openmediavault-web-interface)
  - [9. Change the Default Password](#9-change-the-default-password)
  - [10. Prepare the Storage Disk](#10-prepare-the-storage-disk)
  - [11. Create a Shared Folder](#11-create-a-shared-folder)
  - [12. Configure SMB/CIFS for Network Sharing](#12-configure-smbcifs-for-network-sharing)
  - [13. Set Up User Access](#13-set-up-user-access)
  - [14. Access the Shared Folder from Windows/Mac](#14-access-the-shared-folder-from-windowsmac)
- [FTP Setup](#ftp-setup)
  - [First Follow/Read the NAS Setup](#first-followread-the-nas-setup)
  - [Step 1: Install the FTP Plugin](#step-1-install-the-ftp-plugin)
  - [Step 2: Enable and Configure FTP Service](#step-2-enable-and-configure-ftp-service)
  - [Last Step: Share the FTP](#last-step-share-the-ftp)
- [Quick Setup After System Crash](#quick-setup-after-system-crash)
  - [1. Prepare the SD Card (Reference)](#1-prepare-the-sd-card-reference)
  - [2. Connect the Orange Pi to the Network (Reference)](#2-connect-the-orange-pi-to-the-network-reference)
  - [3. Access the Orange Pi via SSH (Reference)](#3-access-the-orange-pi-via-ssh-reference)
  - [4. Update the Debian OS (Reference)](#4-update-the-debian-os-reference)
  - [5. Install OpenMediaVault (Reference)](#5-install-openmediavault-reference)
  - [6. Access the OpenMediaVault Web Interface (Reference)](#6-access-the-openmediavault-web-interface-reference)
    - [6.1 Set Up User Access (Reference)](#61-set-up-user-access-reference)
    - [6.2 Create a Shared Folder (Reference)](#62-create-a-shared-folder-reference)
    - [6.3 Mount the Existing File System (Reference)](#63-mount-the-existing-file-system-reference)
    - [6.4 Configure SMB/CIFS for Network Sharing (Reference)](#64-configure-smbcifs-for-network-sharing-reference)
  - [7. Access the Shared Folder from Windows/Mac (Reference)](#7-access-the-shared-folder-from-windowsmac-reference)
- [Remote Connection With Tailscale](#remote-connection-with-tailscale)
  - [Create Tailscale Account](#create-tailscale-account)
  - [Install Tailscale In Orange PI](#install-tailscale-in-orange-pi)
  - [Install Tailscale on Client Devices](#install-tailscale-on-client-devices)
    - [For Windows:](#for-windows)
    - [For Mobile Devices (iOS/Android):](#for-mobile-devices-iosandroid)
  - [Access NAS Remotely via Tailscale](#access-nas-remotely-via-tailscale)
    - [Access OpenMediaVault Web Interface](#access-openmediavault-web-interface)
    - [Access SMB/CIFS Shares Remotely](#access-smbcifs-shares-remotely)
    - [Access FTP Remotely](#access-ftp-remotely)
  - [Additional Tailscale Configuration (Not Mandatory)](#additional-tailscale-configuration-not-mandatory)
    - [Enable Tailscale to Start on Boot](#enable-tailscale-to-start-on-boot)
    - [Troubleshooting](#troubleshooting)
<!-- /TOC -->

</details>

## Watch Server Setup Output
[![](https://img.youtube.com/vi/G83z7NtUfuE/0.jpg)](https://www.youtube.com/watch?v=G83z7NtUfuE)

## Watch Full Server Setup Video
[![](https://img.youtube.com/vi/I7mPZj1S_iA/0.jpg)](https://www.youtube.com/watch?v=I7mPZj1S_iA)


## Prerequisites

- **Hardware:**
  - Orange Pi Zero 2
  - SD card (High Speed)
  - LAN cable
  - Router (High Performance)
  - 120 GB SSD
  - High Speed Enclosure
  - 5V 3Amp Adapter

- **Software:**
  - [Orangepizero2_3.0.6_debian_bullseye_server_linux](https://www.orangepi.org/)
  - [Orangepizero2_3.1.0_debian_bookworm_server_linux6.1.31](https://romhub.io/ISO/Orange%20Pi/Orange%20Pi%20Zero2/debian/Orangepizero2_3.1.0_debian_bookworm_server_linux6.1.31.7z)
  - [Rufus](https://rufus.ie/) burning software
  - [PuTTY](https://www.putty.org/) SSH client

## Setup Steps ([Quick Setup After System Crash](#quick-setup-after-system-crash))

### 1. Download Required Files

- **OS Image:**
  - ❌ Download (Debian 11) `Orangepizero2_3.0.6_debian_bullseye_server_linux` from the official [Orange Pi Zero 2 website](https://www.orangepi.org/).
  - ✅ Download (Debian 12) `Orangepizero2_3.1.0_debian_bookworm_server_linux6.1.31` from the [Third Party Website](https://romhub.io/ISO/Orange%20Pi/Orange%20Pi%20Zero2/debian/Orangepizero2_3.1.0_debian_bookworm_server_linux6.1.31.7z).

- **Rufus:**
  - Download Rufus burning software from [here](https://rufus.ie/).

### 2. Prepare the SD Card

- Extract the image file from the downloaded archive.
- Use Rufus to burn the image to the SD card:
  - Open Rufus.
  - Select the SD card.
  - Choose the extracted image file.
  - Click **Start** to begin the burning process.

### 3. Connect the Orange Pi to the Network

- Insert the SD card into the Orange Pi Zero 2.
- Connect the Orange Pi to your router using a LAN cable.
- Power on the Orange Pi.

### 4. Assign a Static IP Address

- Log in to your router's admin panel.
- Find the IP address assigned to the Orange Pi.
- Go to the **IP & MAC Binding** section.
- Bind the IP address to the Orange Pi's MAC address to ensure it remains the same.

### 5. Access the Orange Pi via SSH

- Install PuTTY from [here](https://www.putty.org/).
- Open PuTTY.
- Enter the Orange Pi's IP address.
- Click **Open**.
- Accept any security alerts.
- Log in with the following credentials:
- **Username: root**
- **Password: orangepi**


### 6. Update the Debian OS

Run the following commands:

```bash
sudo apt update
sudo apt upgrade
```

### 7. Install OpenMediaVault

- Follow the installation guide from the [OMV install script GitHub repository](https://github.com/OpenMediaVault-Plugin-Developers/installScript).


- Run the following commands:

```bash
sudo wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```
- If logged out, log back in.
- Check if the server is running:
```bash
sudo service nginx status
```

### 8. Access the OpenMediaVault Web Interface

- Navigate to http://IP_OF_PI/#/login in a web browser.
- Log in with:
```bash 
Username: admin
Password: openmediavault
```

### 9. Change the Default Password
- Click on the profile icon in the top-right corner.
- Select Change Password.
- Set a new password.


### 10. Prepare the Storage Disk
**Delete Existing Data**
- Go to Storage > Disks.
- Select the disk.
- Click on the Wipe icon.
- Confirm the action.

**Create a New File System**
- Navigate to Storage > File Systems.
- Click on the Add icon.
- Select EXT4 or a newer file system.
- Choose the disk.
- Click Save.

Note: This may take some time depending on disk size. 

**Mount the File System**
- In Storage > File Systems, select the file system.
- Click on the Mount icon.
- Select the drive
- Set the warning threshold if desired.
- Click Save.
- Apply the pending configuration changes by clicking the checkmark icon. 



### 11. Create a Shared Folder
- Go to Storage > Shared Folders.
- Click on the Create icon.
- Enter a name for the shared folder.
- Select the disk.
- Click Save.
- Set permissions:
  - Select the shared folder.
  - Click on the Permissions icon.
  - Check Read/Write for both User and Group.
  - Click Save.
  - Apply the pending configuration changes.


### 12. Configure SMB/CIFS for Network Sharing
- Navigate to Services > SMB/CIFS > Settings.
- Enable SMB/CIFS by checking Enabled.
- Ensure the WORKGROUP is set correctly.
- Click Save.
- Go to Services > SMB/CIFS > Shares.
- Click on the Create button.
- Select the shared folder.
- Click Save.
- Apply the pending configuration changes.


### 13. Set Up User Access
- Go to Users > Users.
- Select the orangepi user.
- Click on the Edit icon.
- Set a new password.
- Click Save.

### 14. Access the Shared Folder from Windows/Mac
- Open a file explorer window.
- Enter the following path:
```bash
\\IP_OF_PI

For Example:
    \\192.168.0.226

To access the shared folder directly:
    \\192.168.0.226\drive_name
```
- Log in to server:
- Username: orangepi and the password that have been set earlier at step 13.

   
   
   
# FTP Setup

## First Follow/Read the NAS Setup

### Step 1: Install the FTP Plugin

1. Log in to OpenMediaVault's Web Interface.

2. **Go to Plugins**:
   - Navigate to:
     ```
     System -> Plugins
     ```

3. **Search for the FTP Plugin**:
   - In the search box, type `openmediavault-ftp`.

4. **Install the FTP Plugin**:
   - Select the `openmediavault-ftp` plugin from the list and click the **Install** button.

### Step 2: Enable and Configure FTP Service

1. **Go to Services**:
   - After the plugin is installed, navigate to:
     ```
     Services -> FTP -> Settings
     ```

2. **Enable and Configure FTP**:
   - Enable the FTP service and configure settings like port, maximum clients, etc.
   - Click **Save**.

### Last Step: Share the FTP

1. **Go to Services**:
   - Navigate to:
     ```
     Services -> FTP -> Shares
     ```

2. **Enable FTP Sharing**:
   - Enable FTP shares and click **Save**.


---

# Quick Setup After System Crash

## 1. Prepare the SD Card ([Reference](#2-prepare-the-sd-card))
 
- Use Rufus to burn the image to the SD card:
  - Open Rufus.
  - Select the SD card.
  - Choose the image file `Orangepizero2_3.1.0_debian_bookworm_server_linux6.1.31`.
  - Click **Start** to begin the burning process.

## 2. Connect the Orange Pi to the Network ([Reference](#3-connect-the-orange-pi-to-the-network))

- Insert the SD card into the Orange Pi Zero 2.
- Connect the Orange Pi to your router using a LAN cable.
- Power on the Orange Pi.
   
## 3. Access the Orange Pi via SSH ([Reference](#5-access-the-orange-pi-via-ssh))

- Install PuTTY from [here](https://www.putty.org/).
- Open PuTTY.
- Enter the Orange Pi's IP address.
- Click **Open**.
- Accept any security alerts.
- Log in with the following credentials:
- **Username: root**
- **Password: orangepi**


## 4. Update the Debian OS ([Reference](#6-update-the-debian-os))

Run the following commands:

```bash
sudo apt update
sudo apt upgrade
```


## 5. Install OpenMediaVault ([Reference](#7-install-openmediavault))

- Follow the installation guide from the [OMV install script GitHub repository](https://github.com/OpenMediaVault-Plugin-Developers/installScript).


- Run the following commands:

```bash
sudo wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```
- If logged out, log back in.
- Check if the server is running:
- Is OpenMediaVault available at http://IP_OF_PI/#/login   ???

## 6. Access the OpenMediaVault Web Interface ([Reference](#8-access-the-openmediavault-web-interface))

- Navigate to http://IP_OF_PI/#/login in a web browser.
- Log in with:
```bash 
Username: admin
Password: openmediavault
```
 
### 6.1 *Set Up User Access* ([Reference](#13-set-up-user-access))
- Go to Users > Users.
- Select the orangepi user.
- Click on the Edit icon.
- Set a new password.
- Click Save.

### 6.2. *Create a Shared Folder*  ([Reference](#11-create-a-shared-folder))
- Go to Storage > Shared Folders.
- Click on the Create icon.
- Enter a name for the shared folder.
- Select the disk.
- Click Save.
- Set permissions:
  - Select the shared folder.
  - Click on the Permissions icon.
  - Check Read/Write for both User and Group.
  - Click Save.
  - Apply the pending configuration changes.


### 6.3 *Mount the Existing File System* ([Reference](#10-prepare-the-storage-disk))
- In Storage > File Systems, select the file system.
- Click on the Mount icon.
- Select the drive
- Set the warning threshold if desired.
- Click Save.
- Apply the pending configuration changes by clicking the checkmark icon. 


### 6.4 *Configure SMB/CIFS for Network Sharing* ([Reference](#12-configure-smbcifs-for-network-sharing))
- Navigate to Services > **SMB/CIFS > Settings**.
- Enable SMB/CIFS by checking Enabled.
- Ensure the WORKGROUP is set correctly.
- Click Save.
- Go to Services > **SMB/CIFS > Shares**.
- Click on the Create button.
- Select the shared folder.
- Click Save.
- Apply the pending configuration changes.



### 7. Access the Shared Folder from Windows/Mac ([Reference](#14-access-the-shared-folder-from-windowsmac))
- Open a file explorer window.
- Enter the following path:
```bash
\\IP_OF_PI

For Example:
    \\192.168.0.226

To access the shared folder directly:
    \\192.168.0.226\drive_name
```
- Log in to server:
- Username: orangepi and the password that have been set earlier at step 13.





---

# Remote Connection With Tailscale

## Create Tailscale Account

1. Go to [Tailscale website](https://tailscale.com/).
2. Click on **Sign Up** or **Get Started**.
3. Create an account using:
   - Your email address, or
   - Sign in with Google, Microsoft, or GitHub
4. Verify your email address if required.
5. Complete the account setup process.

## Install Tailscale In Orange PI

1. **SSH: Login your Orange PI** (as described in [Step 5: Access the Orange Pi via SSH](#5-access-the-orange-pi-via-ssh)).

2. **Install Tailscale** by running the following command:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

3. **Start Tailscale service**:

```bash
sudo tailscale up
```

   Or alternatively:

```bash
tailscale up
```

4. **Authenticate the Orange PI**:
   - The **command will provide a URL** that you need to visit in a web browser. **Note: Make sure there are no spaces in the URL.**
   - Open the URL on any device (your computer or phone).
   - Sign in with your Tailscale account.
   - Click **Connect** to authorize this device.
   - The terminal will show **Success authentication**. 

5. **Verify the connection**:

```bash
sudo tailscale status
```

   - You should see your Orange PI listed with its Tailscale IP address (usually starts with `100.x.x.x`).

6. **Get the Tailscale IP address**:

```bash
sudo tailscale ip -4
```

   - Note this IP address (e.g., `100.64.1.2`) - you'll use it to access your NAS remotely.
   - You can also view it from your account after logging in from any browser. 

## Install Tailscale on Client Devices

### For Windows:

1. **Download Tailscale**:
   - Go to [Tailscale Downloads](https://tailscale.com/download).
   - Download the Windows installer.

2. **Install Tailscale**:
   - Run the installer.
   - Follow the installation wizard.

3. **Sign in**:
   - Open Tailscale from the Start menu or system tray.
   - Sign in with your Tailscale account.
   - The device will be added to your Tailscale network.

### For Mobile Devices (iOS/Android):

1. **Download Tailscale**:
   - Install Tailscale from App Store (iOS) or Google Play Store (Android).

2. **Sign in**:
   - Open the app and sign in with your Tailscale account.
   - The device will be added to your Tailscale network.

## Access NAS Remotely via Tailscale

### Access OpenMediaVault Web Interface

1. **Get the Tailscale IP (Public IP)** of your Orange PI:
   - SSH into Orange PI and run: `sudo tailscale ip -4`
   - Or check the Tailscale admin console at [https://login.tailscale.com/admin/machines](https://login.tailscale.com/admin/machines)

2. **Access the web interface**:
   - Open a web browser on any device connected to your Tailscale network.
   - Navigate to: `http://TAILSCALE_IP/#/login`
   - Example: `http://100.64.1.2/#/login`
   - Log in with your OpenMediaVault credentials.

### Access SMB/CIFS Shares Remotely

1. **On Windows**:
   - Open File Explorer.
   - In the address bar, enter:
   ```bash
   \\TAILSCALE_IP
   
   For Example:
       \\100.64.1.2
   
   To access the shared folder directly:
       \\100.64.1.2\drive_name
   ```
   - Enter your NAS credentials when prompted:
     - Username: `orangepi` (or your configured username)
     - Password: (the password you set earlier)

2. **On Mobile Devices**:
   - Use a file manager app that supports SMB (e.g., Files app on iOS, or apps like "File Manager" on Android).
   - Connect to: `smb://TAILSCALE_IP`
   - Enter your credentials.

### Access FTP Remotely

1. **Get the Tailscale IP** of your Orange PI.

2. **Use an FTP client** (e.g., FileZilla, WinSCP, or built-in FTP in file managers):
   - **Host**: `TAILSCALE_IP` (e.g., `100.64.1.2`)
   - **Port**: `21` (default FTP port)
   - **Protocol**: FTP
   - **Username**: `orangepi` (or your configured username)
   - **Password**: (the password you set earlier)

3. **Connect** to access your FTP shares remotely.

## Additional Tailscale Configuration ***(Not Mandatory)***

### Enable Tailscale to Start on Boot

1. **SSH into Orange PI**.

2. **Enable and start Tailscale service**:

```bash
sudo systemctl enable tailscaled
sudo systemctl start tailscaled
```

3. **Verify it's running**:

```bash
sudo systemctl status tailscaled
```

### Troubleshooting

**If Tailscale connection fails:**

1. **Check Tailscale status**:
   ```bash
   sudo tailscale status
   ```

2. **Restart Tailscale service**:
   ```bash
   sudo systemctl restart tailscaled
   ```

3. **Re-authenticate if needed**:
   ```bash
   sudo tailscale up
   ```

4. **Check firewall settings**:
   - Ensure Tailscale is allowed through any firewall on the Orange PI.
   - Tailscale typically uses port 41641/UDP.

5. **Verify both devices are online**:
   - Check that both your Orange PI and client device show as "Online" in the Tailscale admin console.


<p align="center"><span style="color: #FF0000; background-color: #ADD8E6; padding: 5px; border-radius: 5px;"> **Designed By Plaban Das**</span></p>

