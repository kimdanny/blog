---
title: "How to (recursively) delete and ignore files/folders on github - Remove cache from tree"
categories:
  - git-github
tags:
  - gitignore
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---
## Background
You have mistakenly committed and even pushed unnecessary files or folders, such as .idea, .DS_Store, target folder. 
You now want to un-push / delete those from Github and stop tracking the changes from those files. 
You make a plan to delete those manually from Github, and then pull the remote changes from local repository. 
However, you will never see those files again from you local repo, once you pull.  
You might want to keep those file in your local repo, but only want to remove them from your github.  

Let's delete those files only from Github and let those stay intact in you local repo.


## 1. Delete unnecessary files or folders

### 1.1 Files
Remove single file:
```bash
rm file.txt
```

Remove multiple files:
```bash
rm file1.txt file2.csv .DS_Store
```

Be cautious with -i flag  
i tag act as a pause function and ask you to get confirm of the removal:
```bash
rm -i file.txt
rm -i file1.txt file2.csv .DS_Store
```

### 1.2 Folders / Directories
Remove **empty** directories:
```bash
rmdir target
```

Remove **non-empty** directories  
Recursively delete whole subdirectories and files:
```bash
rmdir -R target
```

Be cautious with -i flag  
i tag act as a pause function and ask you to get confirm of the removal:
```bash
rm -iR target
```

### 1.3 Recursively Delete certain files with the same extension (optional)
This is a bit different from 1.2 recursive.  
By this, you can recursively delete all files of a specific extension in the current directory.

`.xml` example:
```bash
find . -name "*.xml" -type f -delete
```
Just to be safe, check what you are deleting by doing:
```bash
find . -name "*.xml" -type f
```

## 2. Include them in .gitignore
Check [this post](https://kimdanny.github.io/git-github/gitignore/) out for more information on gitignore file!  

Example .gitignore file:
```bash
.DS_Store
target/
node_modules/
*.xml
```

## 3. Commit the changes and Checkout .gitignore
Add and commit
```bash
git add <path(s) or . for all>
git commit -m "your message"
```

Checkout .gitignore:
```bash
git checkout master -- .gitignore
```

## 4. Remove from git tree and then commit again!

E.g:
```bash
git rm --cached -r .DS_Store
git rm --cached -r target
git rm --cached -r node_modules
```