- Operators are used to make commands more powerful.

- 📂 For FILES:
| Permission  | Meaning                       |
| ----------- | ---------------------------- |
| r (read)    | can open & view file content |
| w (write)   | can modify file              |
| x (execute) | can run file as program      |


- 📁 For FOLDERS (important difference!)
| Permission  | Meaning                    |
| ----------- | -------------------------- |
| r (read)    | can list files (ls)        |
| w (write)   | can create/delete files    |
| x (execute) | can enter/open folder (cd) |


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
	     -      ex: mv oldname.txt newname.txt
	     
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
    
   🧠 apt usually needs sudo


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
    - shows last 10 lines of a file.
    - ex: tail file1.txt
    
18. head -
    - shows first 10 lines of a file.  
    - ex: head file1.txt

  📝 Custom printing:
     - head -15 file1.txt = show last 15 lines
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
    
    📌 Useful options:
        - grep -i "word" file.txt   - ignore case (Word, WORD, word)
        - grep -n "word" file.txt   - show line number
        - grep -r "word" folder/    - search inside folders (recursive)
       
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
	- find . -name file.txt        - search by name
	- find . -iname file.txt       - search by name (ignore case)

	- find . -type f               - find only files
	- find . -type d               - find only folders

	- find . -size +10M            - files bigger than 10MB
	- find . -size -1M             - files smaller than 1MB

	- find . -mtime -1             - files modified today
	- find . -mtime +7             - files older than 7 days
	
	👉 We can use .. as well. Which search from parent.	
	

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
	sort file.txt         - sort alphabetically
	sort -n file.txt      - sort numerically
	sort -r file.txt      - reverse order


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
