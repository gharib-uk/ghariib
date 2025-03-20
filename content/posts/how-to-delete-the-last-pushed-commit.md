---
title: "How to Delete the Last Pushed Commit"
date: 2025-01-02
categories: 
  - "security"
---

Git push is a regular process used by developers to send changes to a remote repository. Recently, I noticed that I committed a few unnecessary files and pushed them to the remote Git repository. These files were not required and should have been excluded. To fix this, I plan to remove them from the repository and update the `.gitignore` file to avoid pushing such files in the future.

To delete the last pushed commit, you can follow these steps. Make sure you understand the implications of rewriting commit history, especially if working in a shared repository.

## Steps to Delete the Last Pushed Commit

1. **Check Current Commit History:**
    
    Run the following command to view your recent commit history:
    
    ```
    git log --oneline
    ```
    
    This will list the commits, starting with the most recent.
    
2. **Remove the Last Commit Locally:**
    
    Use git reset to remove the last commit while keeping your changes (soft reset) or discard the changes (hard reset):
    
    - To keep the changes made in the last commit:
        
        ```
        git reset --soft HEAD~1
        ```
        
    - To discard the changes made in the last commit:
        
        ```
        git reset --hard HEAD~1
        ```
        
3. **Force Push the Updated History:**
    
    Force-push the changes to the remote repository to remove the commit from the remote:
    
    ```
    git push origin --force
    ```
    

- **Impact of Force Push**: Force pushing rewrites the history, which can disrupt collaborators if they have already fetched the commit you are removing.

- **Communicate with Team**: If working in a shared repository, inform your team before force-pushing.

## Add the Files to .gitignore

As a best practice, you should always add unnecessary files to the .gitignore file, which tells Git to stop tracking these files. To do this, open the `.gitignore` file in a text editor and add the file names or folder paths you want to exclude. For example:

```gitignore

unnecessary_file.txt  
folder_to_ignore/
```

Save the file, and Git will ignore these files in future commits.

If in case the new files are still showing in `git status`, it means they were already being tracked before you added them to `.gitignore`. To fix this, you need to untrack these files. Run the following command:

```
git rm --cached <file_name_or_folder
```

**For example:**

```
git rm --cached unnecessary_file.txt
```

This will remove the files from the Git index but keep them in your local system. After that, you may need to commit the `.gitignore` changes:

```
git commit -m "Remove unnecessary files from tracking" .gitignore
```

Once done, the files will no longer appear in `git status`.

## Wrap Up

This tutorial helped you to remove the last pushed commit from local and remote repository. Also by adding them to `.gitignore` prevents future tracking of the same files, while untracking already committed files ensures a clean and organized repository. These practices help you to maintain a clutter-free codebase and improve collaboration in team projects. Always review your commits before pushing to avoid including files that are not needed.

The post How to Delete the Last Pushed Commit appeared first on TecAdmin.
