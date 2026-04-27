# Linux Commands Reference

## Core Concepts

- **Operators** are used to make commands more powerful.
- **Flags** are used to modify how a command works.
	- Short flags → -l, -a, -r
	- Long flags → --help, --version

---

## File & Folder Permissions

### For Files

| Permission    | Meaning                       |
|---------------|-------------------------------|
| `r` (read)    | Can open & view file content  |
| `w` (write)   | Can modify file               |
| `x` (execute) | Can run file as program       |

### For Folders

| Permission    | Meaning                          |
|---------------|----------------------------------|
| `r` (read)    | Listing directory content        |
| `w` (write)   | Create/delete directory content  |
| `x` (execute) | Opening directory (`cd`)         |

---

## Running Multiple Commands Together

| Symbol | Meaning                            |
|--------|------------------------------------|
| `&&`   | Run second only if first succeeds  |
| `;`    | Run second anyway                  |
| `\|`   | Pass output to next command        |

```bash
# Using &&  (recommended)
mkdir -p five/six/seven && touch five/six/{c.txt,seven/error.log}

# Using ;
mkdir -p five/six/seven ; touch five/six/{c.txt,seven/error.log}
```

---

## Port Numbers

| Port range  | Who can use       |
|-------------|-------------------|
| 1 – 1023    | Only root (`sudo`)|
| 1024+       | Normal users      |

---

## Managing Software

### Install & Uninstall

| Action                        | Command                             | Explanation                         |
|-------------------------------|-------------------------------------|-------------------------------------|
| Install single package        | `sudo apt install htop`             | Installs htop                       |
| Install multiple packages     | `sudo apt install htop vim nginx`   | Installs htop, vim, nginx together  |
| Uninstall single package      | `sudo apt remove nginx`             | Removes nginx (keeps config files)  |
| Uninstall multiple packages   | `sudo apt remove nginx vim htop`    | Removes many apps at once           |
| Completely uninstall (clean)  | `sudo apt purge nginx`              | Removes nginx + config files        |

### Installation Methods in Ubuntu

| Method      | Example Command           | Purpose                                        |
|-------------|---------------------------|------------------------------------------------|
| `apt`       | `sudo apt install nginx`  | Default Ubuntu package manager (most common)   |
| `snap`      | `sudo snap install code`  | Universal packages, mainly desktop apps        |
| `flatpak`   | `flatpak install app`     | Cross-Linux distribution apps                  |
| `.deb` file | `sudo dpkg -i file.deb`   | Manual package installation                    |

---
<br>

## Core Commands

### 1. `man` — Manual

Shows manual (help) pages for Linux commands.

```bash
man ls
```

---

### 2. `cd` — Change Directory

Used for changing directory (folder).

```bash
cd Documents
```

| Variant  | Behaviour                              |
|----------|----------------------------------------|
| `cd .`   | Stays in current directory             |
| `cd ..`  | Moves one level up (parent folder)     |
| `cd ~`   | Goes to home directory (`~` = home)    |
| `cd -`   | Goes to previous directory             |
| `cd /`   | Goes to root directory                 |

**Understanding `..` vs `-`**

*Case 1 — Step by step:*
```
cd home → cd rahul → cd Downloads   # Now at: /home/rahul/Downloads

cd ..  →  goes to /home/rahul  (parent)
cd -   →  goes to /home/rahul  (previous)
# Both look the same here
```

*Case 2 — Direct jump:*
```
cd /home/rahul/Downloads   # Came directly from /home

cd ..  →  goes to /home/rahul
cd -   →  goes back to /home
# Now they are DIFFERENT
```

---

### 3. `mkdir` — Make Directory

Creates a new directory (folder).

```bash
mkdir projects
mkdir -p dev/java/project1   # -p creates parent folders automatically
```

---

### 4. `mv` — Move / Rename

Moves or renames files and folders.

```bash
# Move
mv file1.txt Documents/

# Rename
mv oldname.txt newname.txt
```

> **Note:** Renaming to an *existing* filename will silently overwrite and delete the old file.
>
> Example: `mv file1.txt file2.txt` — the original `file2.txt` is deleted, and `file1.txt` becomes `file2.txt`.

---

### 5. `cp` — Copy

Copies files and folders.

```bash
# File
cp file1.txt file2.txt

# Folder (requires -r flag)
cp -r project backup_project
```

> `-r` = recursive. Without `-r`, folders won't copy.

---

### 6. `ls` — List

Lists files and folders in a directory.

```bash
ls
```

| Flag    | Description                                          |
|---------|------------------------------------------------------|
| `ls -l` | Long format (details: size, permission, owner)       |
| `ls -a` | Include hidden files                                 |
| `ls -lh`| Long format with human-readable sizes (KB, MB, GB)  |
| `ls -la`| Long format + hidden files                           |

---

### 7. `pwd` — Print Working Directory

Prints the current directory path.

```bash
pwd
```

---

### 8. `rm` — Remove

Deletes files and folders.

```bash
rm file1.txt
```

| Flag     | Description                                          |
|----------|------------------------------------------------------|
| `rm -r`  | Deletes folders and all contents (recursive)         |
| `rm -i`  | Asks confirmation before deleting (interactive)      |
| `rm -ri` | Deletes folder with confirmation                     |
| `rm -f`  | Force delete without asking (even protected files)   |

| Command | Full Form         | What it does                                                         |
|---------|-------------------|----------------------------------------------------------------------|
| `rm`    | Remove            | Deletes files and folders (non-empty folders require flags)          |
| `rmdir` | Remove Directory  | Deletes only *empty* folders                                         |

---

### 9. `chmod` — Change Mode

Changes file/folder permissions. Three methods:

#### i. Numeric (Octal) — *Most Common*

Format: `chmod XYZ file` where X = owner, Y = group, Z = others.

| Permission | Calculation | Number |
|------------|-------------|--------|
| `---`      | 0+0+0       | 0      |
| `r--`      | 4+0+0       | 4      |
| `rw-`      | 4+2+0       | 6      |
| `r-x`      | 4+0+1       | 5      |
| `rwx`      | 4+2+1       | 7      |

```bash
chmod 755 file.txt
chmod 644 file.txt
chmod 600 secret.txt
```

#### ii. Symbolic (rwx)

Format: `chmod [who][operator][permission] file`

- **Who:** `u` = owner, `g` = group, `o` = others, `a` = all
- **Operators:** `+` = add, `-` = remove, `=` = set exactly

```bash
chmod u+x file.sh          # Add execute for owner
chmod g-w file.txt         # Remove write for group
chmod o=r file.txt         # Set only read for others
chmod a+r file.txt         # Add read for all
```

#### iii. Mixed (multiple at once)

```bash
chmod u=rwx,g=rx,o=r file.txt
```

---

### 10. `chown` — Change Owner

Changes file/folder owner and group.

```bash
# Change only owner
sudo chown newUser file.txt

# Change owner + group
sudo chown newUser:newGroup file.txt

# Change only group
sudo chown :newGroup file.txt

# Folder + all contents (recursive)
sudo chown -R newUser:newGroup folderName
```

---

### 11. `sudo` — Super User Do

Runs a command with admin (root) privileges — *only for that one command*.

```bash
sudo apt install tree
```

Common uses for `sudo`: installing software, running `chown`, editing system files, making system-level changes.

---

### 12. `apt` — Advanced Package Tool

Installs, updates, and removes software packages.

```bash
sudo apt update        # Refresh package list
sudo apt upgrade       # Upgrade installed software
sudo apt install pkg   # Install software
sudo apt remove pkg    # Remove software
```

> `apt search` and `apt show` don't require `sudo`.

---

### 13. `touch`

Creates a new empty file (or updates timestamp if file already exists).

```bash
touch file1.txt
```

---

### 14. `cat` — Concatenate

Displays file content in the terminal.

```bash
cat file1.txt
cat file1.txt file2.txt   # Show both files together
cat > file.txt            # Overwrite with new content (Ctrl+D to save)
cat >> file.txt           # Append to end (Ctrl+D to save)
```

---

### 15. `less`

Views file content page by page.

```bash
less file1.txt
```

| Key     | Action         |
|---------|----------------|
| `↑ ↓`  | Scroll         |
| `Space` | Next page      |
| `b`     | Previous page  |
| `q`     | Quit           |

---

### 16. `more`

Displays file contents in the terminal (basic pager).

```bash
more file1.txt
```

> `less` is the improved version of `more` with more features.

---

### 17. `tail`

Shows the last 10 lines of a file (default).

```bash
tail file1.txt
```

---

### 18. `head`

Shows the first 10 lines of a file (default).

```bash
head file1.txt
head -15 file1.txt       # Show first 15 lines
head -n 15 file1.txt     # Same thing
```

---

### 19. `rsync`

A fast, versatile file-copying tool (local and remote).

```bash
rsync -av source/ destination/
rsync -av project_fly/ backup_project_fly/
```

How it works each run:
- Compares source and destination
- Skips files that are the same
- Copies only changed or new files

| Flag | Meaning                                                                       |
|------|-------------------------------------------------------------------------------|
| `-a` | Archive mode: copies folders, preserves permissions, ownership, timestamps    |
| `-v` | Verbose: shows what rsync is doing                                            |

---

### 20. `grep` — Global Regular Expression Print

Searches for a word/pattern in files.

```bash
grep "error" file.txt
```

| Command                      | Option | Explanation                                              |
|------------------------------|--------|----------------------------------------------------------|
| `grep -i "word" file.txt`    | `-i`   | Ignore case (matches Word, WORD, word)                   |
| `grep -n "word" file.txt`    | `-n`   | Show line number of each match                           |
| `grep -r "word" folder/`     | `-r`   | Recursive search inside folder and subfolders            |
| `grep -v "word" file.txt`    | `-v`   | Invert match — show lines that do *not* contain the word |
| `grep -o "word" file.txt`    | `-o`   | Only matching — print matched word, not full line        |

---

### 21. `find`

Searches for files and folders in the system.

```bash
find . -name file1.txt
```

| Command                    | What it does                      |
|----------------------------|-----------------------------------|
| `find . -name file.txt`    | Search by name                    |
| `find . -iname file.txt`   | Search by name (ignore case)      |
| `find . -type f`           | Find only files                   |
| `find . -type d`           | Find only folders                 |
| `find . -size +10M`        | Files bigger than 10 MB           |
| `find . -size -1M`         | Files smaller than 1 MB           |
| `find . -mtime -1`         | Files modified today              |
| `find . -mtime +7`         | Files older than 7 days           |

```bash
# Delete all .log files in current directory and subfolders
find . -name "*.log" -delete
```

---

### 22. `sort`

Sorts lines of a file alphabetically or numerically.

```bash
sort file.txt        # Sort alphabetically
sort -n file.txt     # Sort numerically
sort -r file.txt     # Reverse order
```

---

### 23. `date`

Shows the current date and time.

```bash
date
```

---

### 24. `tree`

Shows folder structure in tree format *(requires installation)*.

```bash
tree
```

---

### 25. `wc` — Word Count

Counts lines, words, and characters in a file.

```bash
wc file.txt
wc -l file.txt   # Count lines
wc -w file.txt   # Count words
wc -c file.txt   # Count characters
```

---

### 26. `sed` — Stream Editor

Reads text line by line; can print, replace, or delete lines.

| Command                          | Option | Explanation                                          |
|----------------------------------|--------|------------------------------------------------------|
| `sed 's/old/new/' file.txt`      | `s`    | Substitute first occurrence per line                 |
| `sed 's/old/new/g' file.txt`     | `g`    | Global replace (all occurrences per line)            |
| `sed '5d' file.txt`              | `d`    | Delete line 5                                        |
| `sed '10,20d' file.txt`          | `d`    | Delete lines 10 to 20                                |
| `sed -n '/Harry/p' file.txt`     | `-n p` | Print only lines matching the word                   |
| `sed -i 's/old/new/g' file.txt`  | `-i`   | Edit file directly (save changes in place)           |

```bash
# Print only lines 100 to 200
sed -n '100,200p' file.txt
```

---

### 27. `tr` — Translate

Replaces or deletes characters in text.

| Command               | Explanation                                             |
|-----------------------|---------------------------------------------------------|
| `tr ' ' '\n'`         | Replace spaces with newlines (one word per line)        |
| `tr 'a-z' 'A-Z'`      | Convert lowercase to uppercase                          |
| `tr 'A-Z' 'a-z'`      | Convert uppercase to lowercase                          |
| `tr -d ','`           | Delete commas                                           |
| `tr -d ' '`           | Delete spaces                                           |
| `tr -c 'A-Za-z' '\n'` | Replace everything except letters with newlines         |

---

### 28. `uniq`

Removes or counts duplicate consecutive lines.

| Command                  | Explanation                              |
|--------------------------|------------------------------------------|
| `uniq file.txt`          | Remove duplicate consecutive lines       |
| `sort file.txt \| uniq`  | Sort first, then remove all duplicates   |
| `uniq -c`                | Count how many times each line appears   |
| `uniq -d`                | Show only duplicated lines               |
| `uniq -u`                | Show only lines that appear once         |

---
<br>

## OS / Process Commands

### 1. `ps` — Process Status

Displays currently running processes.

```bash
ps
```

| Command   | Flags                                                  | Shows                           | Best used for                         |
|-----------|--------------------------------------------------------|---------------------------------|---------------------------------------|
| `ps -ef`  | `-e` = every process, `-f` = full format               | PID, PPID, command, user        | Seeing parent-child process relation  |
| `ps aux`  | `a` = all users, `u` = user format, `x` = background  | PID, CPU %, Memory %, command   | Monitoring CPU & RAM usage            |

---

### 2. `top` — Table of Processes

Shows running processes in real time (live view).

```bash
top
```

---

### 3. `df` — Disk Free

Shows disk space usage.

```bash
df
df -h    # Human readable (KB, MB, GB)
```

---

### 4. `uname` — Unix Name

Shows system information.

```bash
uname
uname -a    # All system info (kernel, OS, architecture, hostname)
uname -r    # Kernel version
uname -m    # Machine type (32-bit or 64-bit)
```

---

### 5. `free` — Free Memory

Shows memory (RAM) usage.

```bash
free
free -h    # Human readable (KB, MB, GB)
```

---

### 6. `lspci` — List PCI Devices

Shows hardware devices connected to the PCI bus.

```bash
lspci
```

| Keyword in output                  | Device type          | Meaning                        |
|------------------------------------|----------------------|--------------------------------|
| `Non-Volatile memory controller`   | SSD (storage)        | Intel NVMe SSD drive           |
| `Network controller`               | Network card         | WiFi / Internet device         |
| `VGA compatible controller`        | Graphics card (GPU)  | AMD graphics processor         |
| `Audio device`                     | Sound card           | Handles audio (speaker, mic)   |
| `USB controller`                   | USB ports            | Controls USB devices           |

---

### 7. `kill` — Kill Process

Terminates a running process.

```bash
kill 1234          # Stop process with PID = 1234
pkill chrome       # Kill a process by name
```

---
<br>

## Network Commands

### 1. `ping` — Packet Internet Groper

Checks network connectivity.

```bash
ping google.com
ping -c 4 google.com    # Send 4 packets then stop
```

| Output part                        | Meaning           | Explanation                                 |
|------------------------------------|-------------------|---------------------------------------------|
| `PING google.com (142.250.70.78)`  | IP address        | Google's server IP                          |
| `64 bytes`                         | Packet size       | Data sent in each ping                      |
| `time=32 ms`                       | Response time     | How fast server replied (lower = better)    |
| `ttl=118`                          | Time to live      | Network hop limit                           |
| `4 transmitted`                    | Sent packets      | Number of pings sent                        |
| `4 received`                       | Received packets  | Successful replies                          |
| `0% packet loss`                   | Network quality   | No data lost (perfect connection)           |
| `rtt min/avg/max`                  | Speed stats       | Round-trip time summary                     |

---

### 2. `ifconfig` — Interface Configuration

Shows network interface configuration.

```bash
ifconfig
```

> `ifconfig` is the older tool; `ip a` is the modern standard.

| Interface  | Meaning                  |
|------------|--------------------------|
| `lo`       | Internal system network  |
| `wlx...`   | WiFi                     |
| `inet`     | Your IP address          |

---

### 3. `ssh` — Secure Shell

Connects to another computer remotely.

```bash
ssh user@ip_address
```

---

### 4. `ss` — Socket Statistics

Views network connections and ports (modern replacement for `netstat`).

```bash
ss -tuln    # Show all active TCP/UDP listening ports
ss -tulnp   # Same + which process owns each port
ss -tulnp | grep 5432   # Filter for port 5432 only
```

| Flag | Meaning                                   |
|------|-------------------------------------------|
| `-t` | Show TCP connections                      |
| `-u` | Show UDP connections                      |
| `-l` | Show only listening ports                 |
| `-n` | Show numbers (ports/IPs, not names)       |
| `-p` | Show which process owns the port          |

---

### 5. `which` / `whereis`

- Use `which` when running commands (finds executable location).
- Use `whereis` when exploring the system (finds binary, source, and man pages).