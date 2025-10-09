# portVmdk

Porting a .vmdk (virtualbox) file for use in WSL

# Content

1. [Motivation](#motivation)
2. [CMPT295](#cmpt295)
    - [Requirements](requirements)
    - [Downloads](#downloads)

# Motivation

The idea that in the end VMs are more or less 'emulators' and their 'emulation' of linux systems of course takes time and a lot of resources. The resources that are allocated to a vm(in most cases) are bound to the vm when the vm is active, thus you lose some(or more) of your computing power in the host(unless are on linux and using qemu with a setup so that you are sharing the resources as needed)(or any other variant where you can share resources).

# CMPT295

The main sample that we will be taking and modifying wil be the .vmdk for CMPT295 taught by Arrvindh Shriraman. Specifically the CMP295.vmdk. 

## Requirements

     [Forgot what the point of this was, shouldn't this and the downloads be one and the same]: #
 (Unconfirmed, could change from system to system and OS to OS) 

## Downloads

- [CMPT295.vmdk](https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5 "https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5") Google Drive link to CMPT295 VM files (also provided in github repo)
- [WSL](https://learn.microsoft.com/en-us/windows/wsl/install "https://learn.microsoft.com/en-us/windows/wsl/install") Installing WSL on Windows.

<!-- markdownlint-disable MD033 -->
<details>
    <summary>Optional(might or might not affect the final outcome, preferably do install these)</summary>
    <!-- markdownlint-enable MD033 -->
    -[HyperV](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/Install-Hyper-V) (also check- installing (Hyper_V on non-Wi_n_11 Pr_o versions)[install/hyper_.md "installing hyper V on windows 11 home or other non pro versions"])
    <!-- markdownlint-disable MD033 -->
</details>
<!-- markdownlint-enable MD033 -->
