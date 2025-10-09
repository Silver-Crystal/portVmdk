# portVmdk
Porting a .vmdk (virtualbox) file for use in WSL

# Content
1. [Motivation](#MotivationId)
2. [CMPT295](#CMPT295Id)
    - [Requirements](#RequrementsId)
    - [Downloads](#downloads)

# Motivation {#MotivationId}
The idea that in the end VMs are more or less 'emulators' and their 'emulation' of linux systems of course takes time and a lot of resources. The resources that are allocated to a vm(in most cases) are bound to the vm when the vm is active, thus you lose some(or more) of your computing power in the host(unless are on linux and using qemu with a setup so that you are sharing the resources as needed)(or any other variant where you can share resources).

# CMPT295 {CMPT295Id}
The main sample that we will be taking and modifying wil be the .vmdk for CMPT295 taught by Arrvindh Shriraman. Specifically the CMP295.vmdk. 

## Requirements (Unconfirmed, could change from system to system and OS to OS) {#RequirementsId}

## Downloads {#DownloadsId}

- [CMPT295.vmdk](https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5 "https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5") Google Drive link to CMPT295 VM files (also provided in github repo)
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/install "https://learn.microsoft.com/en-us/windows/wsl/install") Installing WSL on Windows.
<details>
    <summary>Optional(might or might not affect the final outcome, preferably do install these)</summary>
    -[HyperV](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/Install-Hyper-V) (also check- installing (Hyper_V on non-Wi_n_11 Pr_o versions)[])
</details>
After the file has been downloaded, we can go ahead and install [WSL]()
