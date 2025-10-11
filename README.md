# portVmdk

Porting a .vmdk (virtualbox) file for use in WSL

# Content

1. [Motivation](#motivation)
2. [CMPT295](#cmpt295)
    - [Downloads](#downloads)
    <!-- [Requirements](#requirements) -->
    
# Motivation

The idea that in the end VMs are more or less 'emulators' and their 'emulation' of linux systems of course takes time and a lot of resources. The resources that are allocated to a vm(in most cases) are bound to the vm when the vm is active, thus you lose some(or more) of your computing power in the host(unless are on linux and using qemu with a setup so that you are sharing the resources as needed)(or any other variant where you can share resources).

# CMPT295

The main sample that we will be taking and modifying wil be the .vmdk for CMPT295 taught by Arrvindh Shriraman. Specifically the CMP295.vmdk. 

<!--## Requirements

(Unconfirmed, could change from system to system and OS to OS) 

[Forgot what the point of this was, shouldn't this and the downloads be one and the same]: #
 -->
## Downloads and Installations

- [CMPT295.vmdk](https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5 "https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5") Google Drive link to CMPT295 VM files (also provided in github repo)
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/install "https://learn.microsoft.com/en-us/windows/wsl/install") Installing WSL on Windows.

 ### Optional
 (might or might not affect the final outcome, preferably do install these)    
    - [HyperV](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/Install-Hyper-V "https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/Install-Hyper-V") also check- installing HyperV on non Win_11 Pro [versions](install/hyper_.md "installing hyper V on windows 11 home or other non pro versions") )



## Instructions

1. Open PowerShell and check for WSL intallation: ` wsl -l -v `  
    Sample Otput:  
    NAME      STATE           VERSION  
    Ubuntu    Stopped         2

2. Open any working WSL distro and install qemu-utils : `sudo apt update && sudo apt install -y qemu-utils qemu-img lvm2`

3. As we have a .vmdk file, we need to convert it to a .vhdx file first (still inside WSL):
` qemu-img convert -O vhdx /mnt/path/to/stored/CMPT295.vmdk /mnt/path/to/save/CMPT295.vhdx ` 
(Replace /path/to/stored/ with the actual path where the CMPT295.vmdk is stored and /path/to/save/ with the path where you want to save the .vhdx file)(Keep the path to CMPT295.vhx in mind) (This might take some time, could be 5-15 mins)(do not interrupt)

4. Connect the .vhdx using Network Block Device
` sudo modprobe nbd max_part=8 `  
` sudo qemu-nbd --connect=/dev/nbd0 /mnt/path/to/save/CMPT295.vhdx `
` sudo fdisk -l /dev/nbd0 ` 
You should see something like `/dev/nbd0p1` with type `Linux LVM`.

5. Scan and activate LVM volumes: ` sudo vgscan; `  ` sudo vgchange -ay; `  
Expected output:
` Found volume group "ubuntu-vg" using metadata type lvm2  
  2 logical volume(s) in volume group "ubuntu-vg" now active `

6. List available logical volumes: ` sudo lvdisplay `
Confirm the root LV path, 
expected output:
` --- Logical volume ---
    LV Path                /dev/vagrant-vg/root
    LV Name                root
    VG Name                vagrant-vg
    LV UUid                1ERTYo-9WsJ-bxud-oD15-ul12-GBYJ-x7d8I8
`  


7. Mount the Root Filesystem:  
` sudo mkdir /mnt/vhd  
sudo mount /dev/vagrant-vg/root /mnt/vhd `  
  
Check if successful: ` ls /mnt/vhd `  
Should see directories such as `bin`, `etc`, `home`, `usr`, etc  

8. Create a TAR of the mounted root filesystem(and save it in your desired location and remember the path):  
Inside WSL:
` cd /mnt/vhd`  
` sudo tar --numeric-owner -czf /mnt/path/where/you/want/to/save/it/rootfs.tar.gz . `  
Notes:

- Ignore messages like socket ignored.

- Size ≈ 2–5 GB, depending on your system.

- It may take a few minutes.

What it does:

- Archives the entire filesystem of your old VM

- Preserves Linux ownership & permissions

- Saves it directly onto your Windows filesystem (so you can import it from PowerShell later)

(Approx file size: 3 GBs)

9. Disconnect and clean up 

` cd;
sudo umount /mnt/vhd;
sudo vgchange -an;
sudo qemu-nbd --disconnect /dev/nbd0;
`

If you get a 'target is busy' error, use:
` sudo umount -l /mnt/vhd; `

10. Import into WSL 2 (PowerShell as Administrator)
In PowerShell, run:
` wsl --import CMPT295 "C:\WSL\CMPT295" "C:\path\to\rootfs.tar.gz" --version 2 `
Explanation:

- CMPT295 → your new distro name (you can pick anything)

- C:\WSL\CMPT295 → where its files will live (choose any folder)

- The .tar.gz → the exported filesystem

If successful, you’ll see:  
` The operation completed successfully. `


11. Set a default non-root user(PowerShell):
` wsl --setdefaultuser vagrant --distribution CMPT295 `

Launch your new distro (PowerShell): ` wsl -d CMPT295 `





