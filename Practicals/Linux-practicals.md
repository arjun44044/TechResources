# -----TEST QUESTION - 1------

**🟢 1. Basic Linux Commands – Practice Questions**

These are for getting started with the shell, navigation, and exploring the system.

**🔹 Tasks**

1. What command shows your current directory?
2. List all files (including hidden ones) in the current directory.
3. Display the full path of your current directory.
4. What command clears the terminal screen?
5. Use a command to print the calendar for this month.
6. Display today’s date and time.
7. Who are you logged in as? (Show both username and UID)
8. Use the `man` command to find out what `ls` does.
9. Find the absolute path of the `bash` shell.
10. Count the number of files and folders in your home directory.

**📁 2. File and Directory Management – Practice Questions**

**🔹 Tasks**

1. Create a directory named `Practice`.
2. Inside `Practice`, create files named `file1.txt`, `file2.txt`, and `file3.txt`.
3. Create a subdirectory `SubFolder` inside `Practice`.
4. Move `file1.txt` to `SubFolder`.
5. Copy `file2.txt` to `SubFolder` (keep the original).
6. Rename `file3.txt` to `file_renamed.txt`.
7. Remove the file `file_renamed.txt`.
8. Delete the `SubFolder` directory and its contents.
9. Create a file with today’s date in its name.
10. Display the directory tree of the `Practice` folder (if `tree` is installed).

**👤 3. Users and Permissions – Practice Questions**

**🔹 Tasks**

1. Check which user you're currently logged in as.
2. List all users on the system.
3. Create a new user called `testuser`. (needs sudo)
4. Switch to the new user `testuser`.
5. View the permissions of a file using `ls -l`.
6. Change the permission of a file to `rw-r--r--`.
7. Make a file executable only by the owner.
8. Change the owner of a file to `testuser`. (needs sudo)
9. Create a group `devs` and add `testuser` to it.
10. List all groups `testuser` belongs to.

**📊 4. System Monitoring – Practice Questions**

**🔹 Tasks**

1. Show current system uptime.
2. Display the number of users currently logged in.
3. Show memory usage with `free`.
4. List all currently running processes.
5. Show CPU usage using `top` or `htop`.
6. Show disk usage of all mounted filesystems.
7. Show disk usage of the current directory.
8. Check which processes are using the most memory.
9. Monitor real-time logs using `tail -f /var/log/syslog`.
10. Find the PID of a process like `sshd` or `nginx`.

📝 **5. Text Viewing, Editing, and Processing – Practice Questions**

**🔹 Tasks**

1. View a file one page at a time (`less` or `more`).
2. Display the first 10 lines of a file.
3. Show the last 5 lines of a file.
4. Search for the word “error” in a log file.
5. Count the number of lines, words, and characters in a file.
6. Sort a file alphabetically.
7. Remove duplicate lines from a file.
8. Replace all occurrences of "apple" with "orange" using `sed`.
9. Extract the second column from a comma-separated file.
10. Edit a file using `nano` or `vim` and save changes.

🔧 **6. System Utilities – Practice Questions**

**🔹 Tasks**

1. Schedule a one-time job using `at`.
2. List all scheduled cron jobs for current user.
3. Create a cron job that runs every day at midnight.
4. Find the location of the `python3` binary.
5. Measure the time taken to run a command.
6. Set an environment variable temporarily and use it.
7. Create a symbolic link to a file.
8. Display the current environment variables.
9. Find all `.log` files in `/var` recursively.
10. Check disk I/O statistics using `iostat` (if installed).

📦 **7. Package Management – Practice Questions**

> Choose your system type first:

* For Debian-based (Ubuntu): `apt`
* For Red Hat-based (CentOS, RHEL): `yum` or `dnf`

**🔹 Tasks (Assuming Ubuntu/Debian)**

1. Update the package list.
2. Upgrade all packages to the latest version.
3. Install a package like `htop`.
4. Remove a package like `htop`.
5. Search for a package related to `nginx`.
6. List all installed packages.
7. Check the version of an installed package.
8. Show information about a package.
9. Clean unused packages and cache.
10. List services started by an installed package (e.g., `systemctl status nginx`).

📦 **8. Archiving and Compression – Practice Questions**

**🔹 Tasks**

1. Compress a file using `gzip`.
2. Decompress a `.gz` file.
3. Create a `.tar` archive of a directory.
4. Create a compressed `.tar.gz` archive.
5. Extract a `.tar.gz` archive to a folder.
6. View the contents of a `.tar.gz` without extracting.
7. Use `zip` to compress multiple files.
8. Unzip a `.zip` file into another folder.
9. Archive files modified in the last 24 hours.
10. Create a backup of a directory into a tarball with the date in its filename.

🌐 **9. Networking Tools – Practice Questions**

**🔹 Tasks**

1. Display your IP address (`ip a` or `ifconfig`).
2. Ping `google.com` 4 times.
3. Check open ports on your system using `netstat` or `ss`.
4. Test DNS resolution of `google.com`.
5. Display your routing table.
6. Connect to a server on port 80 using `telnet` or `nc`.
7. Download a file using `wget` or `curl`.
8. Show all network interfaces.
9. Check current network connections.
10. Find the hostname and domain name of the system.

--------------------------------------------------------------------------------------------------------------------------------------

## 🧮 **"How to count the number of files and folders in your home directory?"**

Here are a few easy ways:

##### ✅ **1. Count only files and directories (not recursively):**

```bash
ls -1 ~ | wc -l
```

**Explanation:**

* `ls -1 ~` lists all items in your home directory, one per line.
* `wc -l` counts the number of lines → hence, the number of items.

##### ✅ **2. Count all files and directories recursively (including inside subfolders):**

```bash
find ~ | wc -l
```

This includes:

* every file,
* every folder,
* and even symbolic links, recursively.

##### ✅ **3. Count only files (recursively):**

```bash
find ~ -type f | wc -l
```

##### ✅ **4. Count only directories (recursively):**

```bash
find ~ -type d | wc -l
```

............................................................................................................................................................................................................................................

## 🧮 **"Move `file1.txt` to `SubFolder`."**

🔴 Problem:

You ran:

```bash
mv 'file1.txt' /SubFolder
```

This tells Linux:

> "Move `file1.txt` into the directory `/SubFolder` at the **root** of the filesystem (`/`)".

And since your user  **doesn't have write permission to `/`** , it says:

> `Permission denied`

### ✅ Solution:

Use the **relative path** instead of the  **absolute path** .

You are already in:

```bash
~/Documents/practise
```

So just run:

```bash
mv file1.txt SubFolder/
```

or with full relative path:

```bash
mv ./file1.txt ./SubFolder/
```

### 🧠 Tip:

* `/SubFolder` → absolute path at the **root**
* `SubFolder/` → relative path in your **current folder**

............................................................................................................................................................................................................................................

## 🧮"Create a file with  **today’s date in its filename"**

✅ Command:

```bash
touch "report-$(date +%F).txt"
```

### 🔍 Explanation:

* `touch` → creates an empty file
* `$(date +%F)` → inserts today’s date in `YYYY-MM-DD` format (e.g., `2025-07-17`)
* The whole command becomes:

```bash
touch "report-2025-07-17.txt"
```

This creates a file like:

```
report-2025-07-17.txt
```

### 🧠 Bonus Formats:

* `$(date +%Y-%m-%d)` → same as `%F` → `2025-07-17`
* `$(date +%d-%m-%Y)` → `17-07-2025`
* `$(date +%Y%m%d_%H%M%S)` → for unique timestamps → `20250717_194300`

............................................................................................................................................................................................................................................

## 🧮 "Print even numbers from **1 to 20** using a `while` loop"

```bash
#!/bin/bash

# Print even numbers from 1 to 20 using a while loop
num=1
echo "Printing even numbers from 1 to 20 using a while loop"

while [[ $num -le 20 ]]; do
    if (( num % 2 == 0 )); then
        echo $num
    fi
    ((num++))
done

```
andkanslkdnlksndcknksdncskcdksdck