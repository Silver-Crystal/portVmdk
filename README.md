# portVmdk
Porting a .vmdk (virtualbox) file for use in WSL 

# Content
1. [Motivation](Motivation)
2. [CMPT295](CMPT295)
    - [Requirements](Requrements)
    - [Downloads](Downloads)

# Motivation
The idea that in the end VMs are more or less 'emulators' and their 'emulation' of linux systems of course takes time and a lot of resources. The resources that are allocated to a vm(in most cases) are bound to the vm when the vm is active, thus you lose some(or more) of your computing power in the host(unless are on linux and using qemu with a setup so that you are sharing the resources as needed)(or any other variant where you can share resources).

# CMPT295
The main sample that we will be taking and modifying wil be the .vmdk for CMPT295 taught by Arrvindh Shriraman. Specifically the CMP295.vmdk. 

## Reuirements (Unconfirmed, could change from system to system and OS to OS)
It can be downloaded from [CMPT295.vmdk](https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5).
(Link - [https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5](https://drive.google.com/drive/folders/1NtEAY5WizhVu7gosZEvyQgGE33o1sgi5) )

After the file has been downloaded, we can go ahead and install [WSL]()
