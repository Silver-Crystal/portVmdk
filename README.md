# portVmdk

Porting a .vmdk (virtualbox) file for use in WSL

# Content

1. [Motivation](#motivation)
2. [CMPT295](#cmpt295)
    - [Downloads and Installations](#downloads-and-installations)
        - [Optional](#optional)
    <!-- [Requirements](#requirements) -->
    - [Instructions](#instructions)
        - [1. Check WSL installation](#1-check-wsl-installation)
        - [2. Install qemu-utils](#2-install-qemu-utils)
        - [3. Convert .vmdk to .vhdx](#3-convert-vmdk-to-vhdx)
        - [4. Connect .vhdx using Network Block Device](#4-connect-vhdx-using-network-block-device)
        - [5. Scan and activate LVM volumes](#5-scan-and-activate-lvm-volumes)
        - [6. List available logical volumes](#6-list-available-logical-volumes)
        - [7. Mount the Root Filesystem](#7-mount-the-root-filesystem)
        - [8. Create a TAR of the mounted root filesystem](#8-create-a-tar-of-the-mounted-root-filesystem)
        - [9. Disconnect and clean up](#9-disconnect-and-clean-up)
        - [10. Import into a working WSL(2)](#10-import-into-a-working-wsl2)
        - [11. Set a default non-root user](#11-set-a-default-non-root-user)
        
    - [Uninstalling](#uninstalling)
# Motivation

The idea that in the end VMs are more or less 'emulators' and their 'emulation' of linux systems of course takes time and a lot of resources. The resources that are allocated to a vm(in most cases) are bound to the vm when the vm is active, thus you lose some(or more) of your computing power in the host(unless are on linux and using qemu with a setup so that you are sharing the resources as needed)(or any other variant where you can share resources).

# CMPT295

The main sample that we will be taking and modifying will be the .vmdk for CMPT295 taught by Arrvindh Shriraman. Specifically the CMPT295.vmdk. 

<!--## Requirements

(Unconfirmed, could change from system to system and OS to OS) 

[Forgot what the point of this was, shouldn't this and the downloads be one and the same]: #
 -->
## Downloads and Installations

- [CMPT295.vmdk](https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5 "https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5") Google Drive link to CMPT295 VM files (also provided in github repo)
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/install "https://learn.microsoft.com/en-us/windows/wsl/install") Installing WSL on Windows.

### Optional

(might or might not affect the final outcome, preferably do install these)

- [HyperV](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/Install-Hyper-V "https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/Install-Hyper-V") also check- installing HyperV on non Win_11 Pro [versions](install/hyper_.md "installing hyper V on windows 11 home or other non pro versions")



## Instructions

### 1. Check WSL installation

Open PowerShell and check for WSL installation: 

    wsl -l -v 

Sample Output:  

    NAME      STATE           VERSION  
    Ubuntu    Stopped         2

### 2. Install qemu-utils  
Open any working WSL distro and install required packages:

    sudo apt update && sudo apt install -y qemu-utils qemu-img lvm2

### 3. Convert .vmdk to .vhdx

As we have a .vmdk file, we need to convert it to a .vhdx file first (still inside WSL):

    qemu-img convert -O vhdx /mnt/path/to/stored/CMPT295.vmdk /mnt/path/to/save/CMPT295.vhdx    
  

**Notes:**
- Replace `/path/to/stored/` with the actual path where the CMPT295.vmdk is stored.
- Replace `/path/to/save/` with the path where you want to save the .vhdx file.
- Keep the path to CMPT295.vhdx in mind for later steps.
- This conversion might take 5–15 minutes; do not interrupt the process.

### 4. Connect .vhdx using Network Block Device

    sudo modprobe nbd max_part=8;
    sudo qemu-nbd --connect=/dev/nbd0 /mnt/path/to/save/CMPT295.vhdx;
    sudo fdisk -l /dev/nbd0;

You should see something like `/dev/nbd0p1` with type `Linux LVM`.

### 5. Scan and activate LVM volumes

    sudo vgscan; 
    sudo vgchange -ay; 

Expected output:

    Found volume group "vagrant-vg" using metadata type lvm2  
    2 logical volume(s) in volume group "ubuntu-vg" now active  

### 6. List available logical volumes

    sudo lvdisplay

Confirm the root 'LV path' in the output.

Expected output:

           --- Logical volume --- 
    LV Path                /dev/vagrant-vg/root 
    LV Name                root 
    VG Name                vagrant-vg 
    LV UUid                1ERTYo-9WsJ-bxud-oD15-ul12-GBYJ-x7d8I8 



### 7. Mount the Root Filesystem

    sudo mkdir /mnt/vhd ;  
    sudo mount /dev/vagrant-vg/root /mnt/vhd; 

Check if successful:

    ls /mnt/vhd ;

Should see directories such as `bin`, `etc`, `home`, `usr`, etc  

### 8. Create a TAR of the mounted root filesystem

Save it in your desired location and remember the path.

Inside WSL:

    cd /mnt/vhd  
    sudo tar --numeric-owner -czf /mnt/path/where/you/want/to/save/it/rootfs.tar.gz .

Notes:

- Ignore messages like socket ignored.
- Size is typically between 2–5 GB, depending on your system.
- May take a few minutes.

What it does:

- Archives the entire filesystem of your old VM
- Preserves Linux ownership & permissions
- Saves it directly onto your Windows filesystem (so you can import it from PowerShell later)


### 9. Disconnect and clean up

    cd;   
    sudo umount /mnt/vhd;   
    sudo vgchange -an; 
    sudo qemu-nbd --disconnect /dev/nbd0;

If you get a 'target is busy' error, it means some process is still using the mount point (e.g., a shell or open file). In such cases, you can use a lazy unmount, which detaches the filesystem immediately and cleans up once it's no longer busy:

    sudo umount -l /mnt/vhd; 

### 10. Import into a working WSL(2)

Run in PowerShell as Administrator:

    wsl --import CMPT295 "C:\WSL\CMPT295" "C:\path\to\rootfs.tar.gz" --version 2 ;

Explanation:

- CMPT295 → your new distro name (you can pick anything)
- C:\WSL\CMPT295 → where its files will live (choose any folder)
- The .tar.gz → the exported filesystem

If successful, you'll see:

    The operation completed successfully.


### 11. Set a default non-root user

In PowerShell:

     wsl --manage CMPT295 --set-default-user vagrant;
     
Set CMPT295 as your default system for wsl(Optional):

    wsl --setdefault CMPT295
    
Launch your new distro (PowerShell):

    wsl -d CMPT295 ;

Or if you set it up as your default system you can also use:

    wsl


## Uninstalling
To remove the distro you created, use the following command in PowerShell:

    wsl --unregister CMPT295 ;

Followed by going and deleting the file(if you do not need it anymore) where you saved the system in [step 10](#10-import-into-a-working-wsl2) while following the [Instructions](#instructions).
