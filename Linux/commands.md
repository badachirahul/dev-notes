- Operators: Operators are used to make commands more powerful.
- Flags: Flags used to modify how a command works

- head command prints starting 10 lines of a file.
- nano command is used to edit a file by entering inside.
- grep (Global Regular Expression)

- 📂 For FILES:
| Permission  | Meaning                      |
| ----------- | ---------------------------- |
| r (read)    | can open & view file content |
| w (write)   | can modify file              |
| x (execute) | can run file as program      |


- 📁 For FOLDERS (important difference!)
| Permission  | Meaning                    	   |
| ----------- | -----------------------------------|  
| r (read)    | Listing directory content  	   |
| w (write)   | Create/Delete directory content    |
| x (execute) | Opening directory ()cd             |
 



- Running Multiple Commands Together: 
| Symbol | Meaning                           |
| ------ | --------------------------------- |
| &&     | run second only if first succeeds |
| ;      | run second anyway                 |
| |      | pass output to next               |

     ✅ Correct ways:
	✔ Using && (recommended):
	mkdir -p five/six/seven && touch five/six/{c.txt,seven/error.log}

	✔ Or using ; :
	mkdir -p five/six/seven ; touch five/six/{c.txt,seven/error.log}



- Port number who can use:
	| Port range | Who can use      |
	| ---------- | ---------------- |
	| 1 – 1023   | Only root (sudo) |
	| 1024+      | Normal users     |



- Managing software installing and uninstalling:
	| Action                       | Command                           | Explanation                        |
	| ---------------------------- | --------------------------------- | ---------------------------------- |
	| Install single package       |  sudo apt install htop            | Installs htop                      |
	| Install multiple packages    |  sudo apt install htop vim nginx  | Installs htop, vim, nginx together |
	| Uninstall single package     |  sudo apt remove nginx            | Removes nginx (keeps config files) |
	| Uninstall multiple packages  |  sudo apt remove nginx vim htop   | Removes many apps at once          |
	| Completely uninstall (clean) |  sudo apt purge nginx             | Removes nginx + config files       |
	
     📌 Main ways to install software in Ubuntu	
	| Method      | Command Example          | Purpose                                      |
	| ----------- | ------------------------ | -------------------------------------------- |
	| `apt`       | `sudo apt install nginx` | Default Ubuntu package manager (most common) |
	| `snap`      | `sudo snap install code` | Universal packages, mainly desktop apps      |
	| `flatpak`   | `flatpak install app`    | Cross-Linux distribution apps                |
	| `.deb` file | `sudo dpkg -i file.deb`  | Manual package installation                  |





Bread and Butter Commands - all important:

1. man - (Manual)
   - shows manual (help) pages for Linux commands. 
   - man is used to learn any command in Linux.
   - ex: man ls

2. cd - (Change Directory) { .(dot) , ..(double Dot), ~(tilde), -(dash) }
   - Used for changing directory (folder).
   - ex: cd Documents
   
   	- cd .   - stays in current directory.  
           	   ex: cd .

	- cd ..  - moves one level up (parent folder).  
           	   ex: cd ..

	- cd ~   - goes to home directory.   (~ = home)  
           	   ex: cd ~

	- cd -   - goes to previous directory.  
           	   ex: cd -

	Understanding the difference bt .. and -
	Case 1: Step by step
		cd home
		cd rahul
		cd Downloads     Now we are in:  /home/rahul/Downloads
		
		Now:
		👉 cd .. → goes to /home/rahul (parent folder)
		👉 cd - → goes to /home/rahul (previous folder)

		👉 Here BOTH look same ✅ (by coincidence)
		
	Case 2: Direct jump
		cd /home/rahul/Downloads          Now we came directly from /home
		
		Now:
		👉 cd .. → goes to /home/rahul
		👉 cd - → goes back to /home

		👉 Now they are DIFFERENT ✅
		

3. mkdir - (Make Directory)
   - creates a new directory (folder)
   - ex: mkdir projects
   
   Note: - mkdir -p = create multiple parent folders automatically. (p means parents)
   	 - ex: mkdir -p dev/java/project1
 	 
 	 
4. mv - (Move)
   - moves or renames files and folders.
   - move: mv file1.txt Documents/
   - rename: - syntax: mv old_name new_name
 	     -     ex: mv oldname.txt newname.txt
 	     
    Note: We have these files:  file1.txt   file2.txt   our-dog.txt
          - Rename to an EXISTING name
          - mv file1.txt file2.txt
          - Before: file1.txt   file2.txt   our-dog.txt
          - After: file2.txt   our-dog.txt
          
	👉 Old file2.txt is deleted
	👉 file1.txt becomes file2.txt
	(so still only one file2.txt remains)
		    
		    
5. cp - (Copy)
   - copies files and folders.   
   - File: syntax: cp source_file destination_file
               ex: cp file1.txt file2.txt
          
   - Folder: syntax: cp -r source_folder destination_folder
             ex: cp -r project backup_project
             
   - here, (-r = recursive) and Without -r → folder won’t copy ❌
                                With -r → full folder + inside files copy

   
   
6. ls - (List)
   - lists files and folders in a directory.
   - ex: ls
   - Important useful flag: 
	- ls -l   - lists all files in long format (details like size, permission, owner).  
		  - ex: ls -l

	- ls -a   - lists all files including hidden files.   (-a = all)  
		  - ex: ls -a

	- ls -lh  - lists all files in long format with sizes in human readable form (KB, MB, GB).  
		  - ex: ls -lh

	- ls -la  - lists all files in long format including hidden files.  
		  - ex: ls -la
		 
		 
		ls -l = detailed list
		ls -a = include hidden files
		ls -lh = detailed + readable size
		ls -la = detailed + hidden files
		
		
7. pwd - (Print Working Directory)
   - prints current directory path
   - pwd = show current location (folder path)	
   

8. rm - (Remove)
   - deletes files and folders.
   - ex: rm file1.txt
   - Important useful flag:
	- rm -r   - deletes folders and all files inside (no confirmation).   (-r = recursive)  
		  - ex: rm -r myFolder

	- rm -i   - asks confirmation before deleting.   (-i = interactive)  
		  - ex: rm -i file1.txt

	- rm -ri  - deletes folder with confirmation.  
		  - ex: rm -ri myFolder

	- rm -f   - force delete without asking (even protected files).   (-f = force)  
		  - ex: rm -f file1.txt
		  
	| Command | Full Form        | What it does                                                              |
	| ------- | ---------------- | ------------------------------------------------------------------------- |
	|  rm     | Remove           | Deletes files and folders (can delete non-empty folders with options)     |
	|  rmdir  | Remove Directory | Deletes only empty folders                                                |


9. chmod - (Change Mode)
   - changes file/folder permissions.
   - We can change permission in 3 different ways:
     i.   Numeric (Octal) method — MOST COMMON
     ii.  Symbolic (rwx) method
     iii. Mixed (multiple at once)
     
     i. Numeric (Octal) method: 
        - Format: chmod XYZ file (Where: X = owner, Y = group, Z = others)
	- Each permission has value:
	  r = 4
	  w = 2
	  x = 1
	  - = 0
	  
	- Table:
	  | Permissions | Calculation | Number |
	  |-------------|-------------|--------|
	  | ---         | 0+0+0       | 0      |
	  | r--         | 4+0+0       | 4      |
	  | rw-         | 4+2+0       | 6      |
	  | r-x         | 4+0+1       | 5      |
	  | rwx         | 4+2+1       | 7      |
	 
	- ex: chmod 755 file.txt
	      chmod 644 file.txt
	      chmod 600 secret.txt
	      

     ii. Symbolic (rwx) method:
         - Format: chmod [who][operator][permission] file
         - 👥 who:
		u = owner
		g = group
		o = others
		a = all
	 - ➕ operators:
		+ = add
		- = remove
		= = set exactly
	 - ex:  chmod u+x file.sh  (add execute permission for owner)
		chmod g-w file.txt (remove write permission for group)  
		chmod o=r file.txt (set only read permission for others (overwrites old perms) ) 
		chmod a+r file.txt (add read permission for all (owner, group, others))

      iii. Mixed (multiple at once)
           - ex: chmod u=rwx,g=rx,o=r file.txt
           

10. chown – (Change Owner)
    - changes file/folder owner and group.   Full form: Change Owner  
    - ex: sudo chown user file.txt

    📁 For files:
       📌 Change only owner:
	  sudo chown newUser file.txt
	  👉 owner becomes newUser
	  👉 group remains same

       📌 Change owner + group together:
	  sudo chown newUser:newGroup file.txt
	  👉 owner → newUser
	  👉 group → newGroup

       📌 Change only group:
	  sudo chown :newGroup file.txt
	  👉 owner unchanged
	  👉 group changed	
	
    📁 For folders:
	📌 Only folder (not inside):
	   ex: sudo chown newUser:newGroup folderName

	📌 Folder + all contents:
	   ex: sudo chown -R newUser:newGroup folderName
	👉 -R = recursive
     

11. sudo - (Super User Do)
    - runs a command with admin (root) privileges.
    - ex: sudo apt install tree
	
     🧠 What does sudo do?
	👉 Temporarily gives you root (administrator) power
	👉 Only for that one command
	
     📌 Why sudo is needed:
	• install software
	• change owner (chown)
	• edit system files
	• system-level changes


12. apt - (Advanced Package Tool)
    - installs, updates, and removes software packages.  
    - ex: sudo apt install tree 
    
    📌 Common useful commands:
       - sudo apt update      - refresh package list
       - sudo apt upgrade     - upgrade installed software
       - sudo apt install pkg - install software
       - sudo apt remove pkg  - remove software
    
   🧠 - apt usually needs sudo
      - Some commands don’t need sudo:
		- apt search nginx
		- apt show nginx


13. touch -
    - creates a new empty file (or If file already exists updates timestamp). 
    - ex: touch file1.txt
    
    
14. cat - (Concatenate)
    - displays file content on terminal. 	
    - cat file1.txt
    
   🧠 Useful things: 
      - cat file1.txt file2.txt   
        👉 shows both files together
      
      - cat > file.txt            
        👉 Deletes old content first and Writes new content. (Ctrl+D to save)
        
      - cat >> file.txt         
        👉 Adds new content at the end. (Ctrl+D to save)


15. less - 
    - views file content page by page.  
    - ex: less file1.txt

    📌 Useful keys inside less:
	• ↑ ↓ → scroll
	• Space → next page
	• b → previous page
	• q → quit
	
	
16. more -
    - display the contents of a file in a terminal.
    - ex: more file1.txt
    
    📝 Short notes:
	- more = basic page viewer
        - less = better viewer and many features.
	
17. tail - 
    - shows by default last 10 lines of a file.
    - ex: tail file1.txt
    
18. head -
    - shows by default first 10 lines of a file.  
    - ex: head file1.txt

  📝 Custom printing:
     - head -15 file1.txt = show first 15 lines
     - head -n 15 file1.txt = same thing
     
     
19. rsync - 
    - a fast, versatile, remote (and local) file-copying tool.
    
    👉 Every time we run rsync:
	• It compares source and destination
	• Checks which files are same
	• Finds which files changed
   👉 Then:
	✅ Same files → skipped
	✅ Changed/new files → copied
	
    📌 -a (archive)
	Means:
	✔ copy folders + files
	✔ keep permissions
	✔ keep ownership
	✔ keep timestamps
	👉 Basically: copy everything properly

    📌 -v (verbose)
	👉 Shows what rsync is doing (list of files being copied)

   - ex: rsync -av source/ destination/
         rsync -av project_fly/ backup_project_fly/

20. grep - (Global Regular Expression Print)
    - searches for a word/pattern in files.  
    - ex: grep "error" file.txt
    
   | Command                   | Option | Explanation                                                                       |
   | ------------------------- | ------ | --------------------------------------------------------------------------------- |
   |  grep -i "word" file.txt  |  -i    | Ignore Case (matches Word, WORD, word)                                            |
   |  grep -n "word" file.txt  |  -n    | Line Number (shows line number where match appears)                               |
   |  grep -r "word" folder/   |  -r    | Recursive (search inside all files in folder and subfolders)                      |
   |  grep -v "word" file.txt  |  -v    | Invert Match (shows lines that DO NOT contain the word)                           |
   |  grep -o "word" file.txt  |  -o    | Only Matching (prints only the matched word, not full line — useful for counting) |

       
    📌 Example:
      - If file has:  Java is good
		      Linux is good
		      Spring Boot is powerful

      - Run:  grep "good" file.txt

      👉 Output:  Java is good
		  Linux is good


21. find - 
    - searches for files and folders in the system.  
    - ex: find . -name file1.txt

    📌 Common options:
	- find . -name file.txt       - search by name
	- find . -iname file.txt      - search by name (ignore case)

	- find . -type f              - find only files
	- find . -type d              - find only folders

	- find . -size +10M           - files bigger than 10MB
	- find . -size -1M            - files smaller than 1MB

	- find . -mtime -1            - files modified today
	- find . -mtime +7            - files older than 7 days	

     🧠 Note:  - find .   👉 This will list everything (all files & folders).
               - find ..  👉 This will list everything from parent (all files & folders).
     
     📌 Delete all the files having the .log extension in hello directory.
        👉 find . -name "*.log" -delete 
           • . = current folder (hello)
	   • It will search in hello + all subfolders
	   • Delete all .log files


22. sort - 
    - sorts lines of a file alphabetically or numerically.  
    - ex: sort file.txt

    📌 Useful options:
	sort file.txt        - sort alphabetically
	sort -n file.txt     - sort numerically
	sort -r file.txt     - reverse order


23. date - 
    - shows current date and time.  
    - ex: date
    
    
24. tree - 
    - shows folder structure in tree format.  (Need to install)
    - ex: tree
    
25. wc - (Word Count)
    - counts lines, words, and characters in a file.  
    - ex: wc file.txt
    
    📌 Useful options:
       - wc -l file.txt   - count lines
       - wc -w file.txt   - count words
       - wc -c file.txt   - count characters
       
 
 26. sed - (Stream Editor) 
     - 👉 It reads text line by line and can:
	   - print lines
	   - replace text
	   - delete lines
	   (Mainly used for editing text in terminal)
     - sed = powerful line-based text editor in command line
     
     - Table:
        | Command                         | Option / Part | Explanation                                           |
	| ------------------------------- | ------------- | ----------------------------------------------------- |
	|  sed 's/old/new/' file.txt      |  s            | Substitute (replace first occurrence in each line)    |
	|  sed 's/old/new/g' file.txt     |  g            | Global replace (replace all occurrences in each line) |
	|  sed '5d' file.txt              |  d            | Delete line number 5                                  |
	|  sed '10,20d' file.txt          |  d            | Delete lines from 10 to 20                            |
	|  sed -n '/Harry/p' file.txt     | -n            | -n = No automatic printing 				  |
        |                                 |  p            |  p = Print only lines that match the word             | 
	|  sed -i 's/old/new/g' file.txt  | -i            | Edit file directly (save changes)                     |

     - ex:
	  sed -n '100,200p' file.txt

	  Meaning:
	  • -n → do not print everything automatically
	  • 100,200 → line range
	  • p → print
	👉 Prints only lines 100 to 200
	
27. tr - (Translate)
    - It replaces or deletes characters in text.
    - Table with ex:    
	    | Command               | Option / Part   | Explanation                                       			      |
	    | --------------------- | --------------- | ------------------------------------------------------------------------------|
	    |  tr ' ' '\n'          | space → newline | Replaces spaces with new lines (one word per line) 			      |
	    |  tr 'a-z' 'A-Z'       | range           | Converts lowercase letters to uppercase           			      |
	    |  tr 'A-Z' 'a-z'       | range           | Converts uppercase letters to lowercase        				      |
	    |  tr -d ','            |  -d             | Deletes specified character (removes commas)     			      |
	    |  tr -d ' '            |  -d             | Deletes spaces                                                                |
            |  tr -c 'A-Za-z' '\n'  |  -c             | Replaces everything except letters with new lines (used to split clean words) |
	    

28. uniq - 
    - Table with ex:
		| Command                 | Option      | Explanation                                       |
		| ----------------------- | ----------- | ------------------------------------------------- |
		|  uniq file.txt          | (no option) | Removes duplicate consecutive lines               |
		|  sort file.txt \| uniq  | pipeline    | Sorts first, then removes all duplicates properly |
		|  uniq -c                |  -c         | Counts how many times each line appears           |
		|  uniq -d                |  -d         | Shows only duplicated lines                       |
		|  uniq -u                |  -u         | Shows only lines that appear once                 |




OS/Process Related Commands:
1. ps - (Process Status) 
   - Displays currently running processes.
   - ex: ps
  	
| Command  | Flags Meaning                                                      | Shows                         | Best Used For                        |
| -------- | ------------------------------------------------------------------ | ----------------------------- | ------------------------------------ |
|  ps -ef  |  -e = every process<br>, -f = full format                          | PID, PPID, command, user      | Seeing parent-child process relation |
|  ps aux  |   a = all users<br>, u = user format<br>, x = background processes | PID, CPU %, Memory %, command | Monitoring CPU & RAM usage           |

   
   
2. top - (Table Of Processes)
   - shows running processes in real time (live view)
   - ex: top
   
3. df - (Disk Free) 
   - shows disk space usage
   - ex: df
   
   - ex: df -h
         👉 -h = Human readable (shows in KB, MB, GB)


4. uname - (Unix Name) 
   - shows system information
   - ex: uname
   
   📌 Common useful options:
	| Command    | Option      | Explanation                                                       |
	| ---------- | ----------- | ----------------------------------------------------------------- |
	| `uname`    | (no option) | Shows basic system name                                           |
	| `uname -a` | `-a`        | Shows all system information (kernel, OS, architecture, hostname) |
	| `uname -r` | `-r`        | Shows kernel version                                              |
	| `uname -m` | `-m`        | Shows machine type (32-bit or 64-bit)                             |

5. free - (Free Memory) 
   - shows memory (RAM) usage
   - ex: free
   
   - ex: free -h
         👉 -h = Human readable (shows in KB, MB, GB)

6. lspci - (List PCI devices) 
   - shows hardware devices connected to PCI bus
   - ex: lspci
   
   - Table:
	| Line Keyword                     | Device Type         | What it means                |
	| -------------------------------- | ------------------- | ---------------------------- |
	| `Non-Volatile memory controller` | SSD (Storage)       | Your Intel NVMe SSD drive    |
	| `Network controller`             | Network card        | Your WiFi / Internet device  |
	| `VGA compatible controller`      | Graphics card (GPU) | Your AMD graphics processor  |
	| `Audio device`                   | Sound card          | Handles audio (speaker, mic) |
	| `USB controller`                 | USB ports           | Controls USB devices         |


7. kill - (Kill Process)
   - terminates a running process
   - ex: kill PID
   
   - ex: pkill chrome (Kill a process by name)
   
   📌 Basic example:
       - ex: kill 1234
       👉 Stops process with PID = 1234



Network Related Commands:
1. ping - (Packet Internet Groper) 
   - checks network connectivity
   - ex: ping google.com
   
   - ex: ping -c 4 google.com
   👉 send 4 packets then stop (-c = Count)

   - ping output in table:
   
   	| Output Part                       | Meaning          | Explanation                         		    |
	| --------------------------------- | ---------------- | -------------------------------------------------- |
	| `PING google.com (142.250.70.78)` | IP Address       | Google’s server IP                    		    |
	| `64 bytes`                        | Packet size      | Data sent in each ping                             |
	| `time=32 ms`                      | Response time    | How fast server replied (lower = better)           |	    
	| `ttl=118`                         | Time To Live     | Network hop limit (not important now) 		    |
	| `4 transmitted`                   | Sent packets     | Number of pings sent              		    |
	| `4 received`                      | Received packets | Successful replies                    		    |
	| `0% packet loss`                  | Network quality  | No data lost (perfect connection)                  |
	| `rtt min/avg/max`                 | Speed stats      | Ping time summary (rtt - Round Trip Time (speed))  |


2. ifconfig - (Interface Configuration) 
   - shows network interface configuration
   - ex: ifconfig
 
  
    🧠 Important to remember:
       ✔️ ifconfig = old (still used sometimes)
       ✔️ ip a = new standard
       
        | Interface | Meaning                 |
	|-----------|-------------------------|
	|  lo       | internal system network |
	|  wlx...   | your WiFi               |
	|  inet     | your IP address         |

3. ssh - (Secure Shell)
   - connects to another computer remotely
   - ex: ssh user@ip_address

4. ss - (Socket Statistics)
   - ss shows information about network sockets
   - tool to view network connections and ports (Modern replacement of netstat)
   
   📌 Display all active TCP / UDP connections
      ex: ss -tuln
		• ss → socket statistics (network info tool)
		• -t → show TCP connections
		• -u → show UDP connections
		• -l → show only listening ports (active services)
		• -n → show numbers (ports/IPs as numbers, not names)
		
    🧠 How it works:
	1️⃣ ss -tulnp
	👉 shows ports + which process owns them

	2️⃣ | grep 5432
	👉 filters only port 5432
	
5. 📌 When to use:
      • Use which when running commands
      • Use whereis when exploring system
