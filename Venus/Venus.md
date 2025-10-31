
# Venus

How to set up local Venus instance and add it to path to be able to run it from anywhere in the WSL terminal and not having to write java -jar /path/to/venus.jar file_name every time to run it. (or keeping local copies of venus.jar in every folder to decrease the path length, even with which the command 'java -jar venus.jar file_name' is still a bit long).

After this setup, one should just be able to write 'venus file_name' to the same output.
# Download Links
Click to download [Venus.jar](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar "wget https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar") for local execution of code.

# Set up Instructions.
1. Ensure you have the WSL up and running as directed in the [PortVMDK](../PortVMDK/PortVMDK.md).
2. Download the [Venus.jar](https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar "wget https://www.cs.sfu.ca/~ashriram/Courses/CS295/assets/distrib/Venus/jvm/venus.jar") file to your WSL downloads folder.
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

3. Make a permanent install location:
    
        cd ~
        sudo mkdir -p /venus
        sudo cp ~/Downloads/venus.jar venus

4. Create a small wrapper script

        sudo sh -c 'echo "java -jar /home/vagrant/venus/venus.jar \"\$@\"" > /usr/local/bin/venus'
        sudo chmod +x /usr/local/bin/venus

5. Verify everything works:

        which venus
        
    which should give
    
        /usr/local/bin/venus
    
    Then :

        venus --help

    which should give you the default output similar to what you get when you type

        java -jar /home/vagrant/venus/venus.jar --help