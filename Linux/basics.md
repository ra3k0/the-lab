## Introduction to Linux Command Line

### 1. Getting Started:
   - Open a Terminal: This is your gateway to the command line.

### 2. Basic Commands:
   - `ls`: List files and directories in the current location.
   - `ll`: Alias for `ls -l`, providing a detailed list with file permissions and additional information.
   - `cd`: Change directory. For example, `cd Documents` will move you into the "Documents" directory.
   - `pwd`: Print working directory. Shows your current location.

### 3. File Operations:
   - `touch`: Create an empty file. `touch filename.txt`
   - `mkdir`: Create a new directory. `mkdir new_directory`
   - `cp`: Copy files or directories. `cp file.txt destination`
   - `mv`: Move or rename files. `mv file.txt new_location/` or `mv old_name.txt new_name.txt`

### 4. Viewing and Editing Files:
   - `cat`: Display content of a file.
   - `nano` or `vim`: Text editors. Use `nano filename` or `vim filename` to edit.

### 5. File Permissions:
   - `chmod`: Change file permissions. e.g., `chmod +x script.sh` makes a script executable.

### 6. SSH Usage:
   - `ssh`: Secure Shell, used for remote login to another machine.
     - `ssh username@hostname`: Connect to a remote server.
   - `scp`: Secure Copy, used for securely copying files between local and remote systems.
     - Copy a file from local to remote: `scp local_file username@hostname:remote_location`
       - Example: `scp myfile.txt user@remote-server:/path/to/destination`
     - `ssh-keygen`: Generate SSH keys for secure authentication.
       - `ssh-keygen -t rsa -b 2048`
     - `ssh-copy-id`: Copy your public key to a remote machine's authorized_keys file for passwordless logins.
       - `ssh-copy-id username@hostname`

### 7. System Information:
   - `uname -a`: Display system information.
   - `df -h`: Show disk space usage.
   - `free -m`: Display RAM usage.
   - `top`: Display real-time system information, including processes and resource usage.

### 8. Package Management: 
   - `sudo apt-get update` to update package lists.
   - `sudo apt-get install package_name` to install a package.

### 9. Searching for Files:
   - `find`: Search for files and directories. e.g., `find / -name filename`

### 10. Redirection and Pipes:
   - `>`: Redirect output to a file. e.g., `echo "Hello" > greeting.txt`
   - `|`: Pipe command output to another command. e.g., `ls -l | grep "file"`

### 11. Exiting the Terminal:
   - `exit`: Close the terminal.
