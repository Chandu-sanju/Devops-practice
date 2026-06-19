> [!IMPORTANT]
> Today We are going to discuss about below 
> git reset
> git revert
> what is the .gitignore file in Git and it's imporant (Interview question)

## Git Reset

* We know all editing files and newly created files are avaible in our working area, post moved to staging aread you realized I need miss to add something in the staging are files.
* In this case you can use git reset.

> Command: git reset filename   [ Moved back files from staging are to working area]
           git reset without mentioning filename it will take all filename by default

#### In the above git reset we used for file  but also we can do for commit see below how?

```
   git reset commitid ==> by default it takes --mixed
   git reset --soft commitid
   git reset --hard commitid
   
   when you do git reset commit history going to remove that means HEAD will point to the previous commit and deletes current commit one" & data depends on soft hard and mixed

 --mixed => it will remove current commit and HEAD points to previous commit - data/file going to working area (Untracked)
 --soft => it will remove current commit and HEAD points to previous commit - data/file going to staging area (tracked)
 -- hard =>  it will remove current commit and HEAD point s to previous commit - date/file completedly deleted from local repository , Dangrous

## after pushing the changes if you git reset --hard commitid all the commit history and file/folder will be deleted" then you have to do git push --force

 ```

## Git Revert

* For suppose commited some changes in the code and pushed into remote beacuse this bad commit website is not working or something not correct"
* In this case you are changed 100 of lines and it is not possible to revert the chages manually. simple you can use rever

> git revert :- It is used to undo changes which we done in previous it is better to use for previous commit and we can use for all the commits there will be some     challenges and conflict will face. 
> Git revert preserve the commit history and on top of that it will create another commit by adding Revert word infront of previous commit message.

```
Syntax :- git revert commitid
          git revert HEAD
          git revert HEAD~1
          git revert commit1 commit2 commit3
          git revert --no-edit

Git Reset vs Git Revert

Git Reset

Removes commit history and moves your HEAD to an earlier commit.

Behavior depends on the flag:

--soft => changes stay staged

--mixed => changes go back to working area (unstaged)

--hard => changes are deleted completely

 Rewrites history, so not safe after push.

Git Revert

Preserves commit history.

Creates a new commit that undoes the changes of a previous commit.

Safe for shared branches because it does not remove old commits.  
```

# What is the .gitignore file

* .gitignore is a special file in a Git repository.

* It tells Git: “Ignore these files or folders — don’t track them in commits.”

* Files listed here will not appear in git status and won’t be pushed to remote.

|| Importance of .gitignore
```
 Keeps repository clean

 Prevents unnecessary or temporary files from cluttering the repo.

 Avoids sensitive data leaks

  For example, API keys, passwords, or configuration files.

 Prevents OS/IDE files from being tracked

 Every developer has local system files that are not needed on GitHub.

 Reduces merge conflicts

 Ignoring machine-specific files avoids unnecessary conflicts.

 How to add files in git ignore file

 *.txt
 *.json
 !text.txt --- to allow only this file
 !text.json - to allow only this file
 temp/
 By default git ignores empty folder remember it.
```



 
          