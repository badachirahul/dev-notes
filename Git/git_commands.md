## ✅ Git Basics:

	git init – Used when creating a new project to initialize a Git repository (creates .git folder).
		 – 👉 git init = convert normal folder into Git repo 📁➡️📦

	git add . – Used to stage (prepare) changes for commit.

	git commit – Saves the staged changes as a snapshot in the local repository (in .git).

	git push – Sends local commits to the remote repository.

	git status – Shows which files are modified/staged/untracked.

	git diff – Shows what all changes made in files before commit.
	
	git show – Shows the last commit details. (by default, the latest).
		__________________________________________________
		| Command  | When          | Shows               |
		| -------- | ------------- | ------------------- |
		| git diff | before commit | current changes     |
		| git show | after commit  | last commit changes |




## ✅ Git Branching:
    
	git checkout -b 'new_branch_name'  ––––––––––– 🔵 Creates a new branch and switch to it.
	
	git checkout 'branch_name'  –––––––––––––––––– 🔵 Switch to another branch.
	
	git branch  –––––––––––––––––––––––––––––––––– 🔵 List all the branches.
	
	git branch -d 'branch-name'  ––––––––––––––––– 🔵 Deletes local branch.
	
	git branch -m 'new_name'–––––––––––––––––––––– 🔵 Rename Current Branch
	
	git branch -m 'old-name' 'new-name' –––––––––– 🔵 Rename Some Other Branch
    


## ✅ Git Remote:  
	git remote rename origin 'new_name' –––––––––– 🔵 Rename remote origin
	
	git remote -v –––––––––––––––––––––––––––––––– 🔵 checks repo connection 
	
	

## ✅ Git Merge:
	🧠 So, merge conflict occurs when pulling or merging changes from another branch that modified the same file in the same place.

	

## ✅ Undoing in Git:
	👉 Undo Staging: 
	   - This undo the staged files. 
	   - ex: git reset  🔀  git reset 'File_Name'
	
	👉 Undo Commit:
	    - ex: git reset --mixed HEAD~1  🔀  git reset HEAD~1  
	          Removes the last commit from history and keeps all file changes UNSTAGED.
	         
	    - ex: git reset --soft HEAD~1  
	          Removes the last commit from history and keeps all file changes STAGED.

	    - ex: git reset --hard HEAD~1
	  	      Removes the last commit from history and also deletes all file changes.
	  	 
	         - To go to a specific commit, use its commit ID instead of HEAD~1.

	    - ex: git reset --hard  
	          Removes all uncommitted file changes and keeps commit history.
	        
	    - 📌 git update-ref -d HEAD
		👉 Deletes the only (current) commit when no previous commit exists
		👉 Keeps all file changes safe in working directory


✅ Questions:
	
1. What is PR.
   - Request to merge code
   
2. What is Fork?
   - A Fork is a copy of someone else’s repository into our own GitHub account.

3. What is Merge?
   - Combining one branch into another. (e.g., feature branch into main).



