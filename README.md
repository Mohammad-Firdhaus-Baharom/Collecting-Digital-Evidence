# Collecting Digital Evidence

## Objective
The objective of this lab is to cover aspects of collecting digital evidence as first responders in a way that prevents contamination of evidence. The lab discusses some tools and techniques on both Windows and Linux that are used in this context.

## Skills Learned
- Locating and navigating file systems and partitions on Linux.
- Mounting and accessing USB drives on Linux.
- Creating forensic images using `dd` and `dcfldd` commands.
- Enabling write-protection for USB drives on Windows.
- Using Autopsy on Windows for forensic analysis.

## Tools Used
### Terminal commands:
- `fdisk`, `mount`, `ls`, `mkdir`, `dd`, `dcfldd`
- `regedit` on Windows

### Log Files:
- `/dev/sda`, `/dev/sdb1`

### GUI Tools:
- Autopsy on Windows

## Steps

### Copying Data on Linux:
1. **Check Available Drives:**
   - Use `sudo fdisk -l` to list partitions and system information.
2. **Manage Disks with fdisk:**
   - Enter command mode for a specific drive/partition using `fdisk /dev/sda`.
3. **Connect USB Drive:**
   - Connect your USB drive and re-run `sudo fdisk -l` to identify the new partition (e.g., `/dev/sdb1`).
4. **Mount USB Drive:**
   - Create a mount directory and mount the USB drive:
     ```bash
     sudo mkdir /mydrives
     sudo mkdir /mydrives/usbdrive
     sudo mount /dev/sdb1 /mydrives/usbdrive
     ```
5. **Create Forensic Image Directory:**
   - Create a directory to save forensic images:
     ```bash
     sudo mkdir /myimages
     sudo mkdir /myimages/usbimage
     ```
6. **Create Forensic Image with dd:**
   - Use `dd` to create a forensic image:
     ```bash
     sudo dd if=/mydrives/usbdrive/<filename> of=/myimages/usbimage/usb.img
     ```

### Using dcfldd Command on Linux:
7. **Install dcfldd:**
   - Install `dcfldd` using:
     ```bash
     sudo apt install dcfldd
     ```
8. **Create Forensic Image with Hash:**
   - Use `dcfldd` to create a forensic image with MD5 hash:
     ```bash
     sudo dcfldd if=/mydrives/usbdrive/<filename> of=/myimages/usbimage/usb2.img hash=md5
     ```
9. **Split Data into Blocks:**
   - Split data into 50MB blocks and generate MD5 hash:
     ```bash
     sudo dcfldd if=/mydrives/usbdrive/<filename> split=50M of=/myimages/usbimage/usb3.img hash=md5
     ```
10. **Verify Image:**
    - Verify the image against the input file:
      ```bash
      sudo dcfldd if=/mydrives/usbdrive/<filename> vf=/myimages/usbimage/usb.img
      ```

### Enabling Write-Protection for a USB Drive on Windows:
11. **Start Registry Editor:**
    - Open `regedit` and back up the registry.
12. **Create Write-Protection Key:**
    - Navigate to `\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet`, create `StorageDevicePolicies` key, and `WriteProtect` DWORD value set to `1`.

### Using Autopsy on Windows:
13. **Download and Install Autopsy:**
    - Download and install Autopsy from [here](https://sleuthkit.org/autopsy/download.php).
14. **Prepare USB Drive:**
    - Insert the USB drive and ensure it is recognized by Windows VM.
15. **Create New Case in Autopsy:**
    - Start Autopsy, create a new case, and add the USB drive as a data source.
16. **Configure Ingest Modules:**
    - Select relevant ingest modules and process the data source.
17. **Start Analysis:**
    - Navigate through the data source, explore the hex representation of data, and generate a report using the Generate Report feature.
