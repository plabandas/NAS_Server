# NAS & FTP Server With Orange Pi

This guide provides step-by-step instructions to set up OpenMediaVault (OMV6) on an Orange Pi Zero 2 running Debian Linux.

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
  - [Rufus](https://rufus.ie/) burning software
  - [PuTTY](https://www.putty.org/) SSH client

## Setup Steps

### 1. Download Required Files

- **OS Image:**
  - Download `Orangepizero2_3.0.6_debian_bullseye_server_linux` from the official [Orange Pi Zero 2 website](https://www.orangepi.org/).

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
- Set the warning threshold if desired.
- Click Save.
- Apply the pending configuration changes by clicking the checkmark icon. 
- 


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
- Username: orangepi and the password that have been set earlier.

   
   
   
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
   





