
# Venus

How to set up local Venus instance and add it to path to be able to run it from anywhere in the WSL terminal and not having to write java -jar /path/to/venus.jar file_name every time to run it. (or keeping local copies of venus.jar in every folder to decrease the path length, even with which the command 'java -jar venus.jar file_name' is still a bit long).

After this setup, one should just be able to write 'venus file_name' to the same output.

The provided 295 setup already has java installed and following these instructions does NOT remove the requirement of having java installed on your system. 
# Content
1. [Venus](#venus "#venus")
2. [Content](#content "#content")
3. [Download Links](#download-links "#download-links")
4. [Set up Instructions.](#set-up-instructions "#set-up-instructions")  
  4.1. [Prereq](#1-prereq "#1-prereq")  
  4.2. [Make a permanent install location](#2-make-a-permanent-install-location "#2-make-a-permanent-install-location")  
  4.3. [Create a small wrapper script](#3-create-a-small-wrapper-script "#3-create-a-small-wrapper-script")  
  4.4. [Verify everything works](#4-verify-everything-works "#4-verify-everything-works")

# Download Links
Click to download [Venus.jar](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar "https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar") for local execution of code. (Can also download the venus.jar file that is provided in files folder [Venus.jar](files/Venus.jar "files/Venus.jar")).
Download/move the file to your WSL downloads folder.

<details>
<summary>Download how?</summary>
  

#### option 1:  
- Open your WSL terminal.
- Navigate to your downloads folder using `cd ~/Downloads`.
- Use the wget command to download the Venus.jar file:

  ```bash
  wget https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar
  ```
#### option 2:
- Download the Venus.jar file using your web browser.
- Move the downloaded file to your WSL downloads folder. You can access your Windows files from WSL at `/mnt/c/Users/YourUsername/Downloads`.
(can even open file explorer normally and then copy and paste the file from your windows downloads to the wsl downloads folder, use the left sidebar to go to "Linux" followed by "CMPT295" and then navigate to the downloads(home/vagrant/downloads))
  
  <br>
</details>

# Set up Instructions.

### 1. Prereq  
 
  1.1 Ensure you have the WSL up and running as directed in the [PortVMDK](../PortVMDK/PortVMDK.md).  

  1.2 Ensure you have the [Venus.jar](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar "https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar") file in your WSL downloads folder.

### 2. Make a permanent install location
    
        cd ~
        sudo mkdir -p /venus
        sudo cp ~/Downloads/venus.jar venus

### 3. Create a small wrapper script

        sudo sh -c 'echo "java -jar /home/vagrant/venus/venus.jar \"\$@\"" > /usr/local/bin/venus'
        sudo chmod +x /usr/local/bin/venus

### 4. Verify everything works

  Run
     
    which venus
        
  which should give

    /usr/local/bin/venus
    
  Then run :

    venus --help

  which should give you the default output similar to what you get when you run:

    java -jar /home/vagrant/venus/venus.jar --help