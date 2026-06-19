# Day-01
> [!IMPORTANT]
>GitHub:- It is an online source control management. used to store Mangae & collabrate on code. Simply it is centralized datebase for our code.
>   Where you can keep the code and take update and push the changed again. so that every one have the update code.
>
>        https://github.com ==> Public
>
>             https://chandu.dneg.com ===> is the enterprise edition one.
>
>   IN the Market similar SCm are avaible:-
>
>                                      GitHub
>                                      GitLAb
>                                       BitBucket
>                                       SVN
>                                      CVS
>                                       TFS
>
>  To practice the Git we have to create an account on GitHUb it is free.
>
> Once you create account -
>                         create a repo
>                        create a team
>                        add your teammates or learning partners in the team
>                        Provide repo access to the team.
>
>       To do above first you need to create a organization:-
>                                                            Form the top + botton => Create Org => Pick a Plan => free
>                                                            org-name: give orgname
>                                                            contact: give email
>                                                            then select for my personal account

>> Then create a repo start practicing..

## Git :- Git is the tool which will support gitLab, GitHub, Bitbucket ------ we can communicate and manage through commands

### Git commands:- 


```
 git init:- 
 When You ran the command inside your git folder then it will create a empty local repository.
            The normal folder now became Git-Managed project folder. It will start tracking the files/folders which are inside.
            Also it will create a .git folder and the .git contain 
                                                                 complete version history
                                                                 commit objects
                                                                 Branch info
                                                                 configuration
                                                                 Head Pointer [Current Branch]


    "branches  COMMIT_EDITMSG  COMMIT_EDITMSGo  config  description  FETCH_HEAD  HEAD  hooks  index  info  logs  objects  ORIG_HEAD  packed-refs  refs"


    To check whether your folder under git control/Git init happend or not , You can run git status and see..
```
    >> git status :- It will tell is there any files are in working area (Untracked) or Staging area (tracked)

```
 ` Logical areas In Git:- `

  >> Working Area (Untracked Files)                                      Staging area (Tracked Files)                                 Local repository
   =============================                                      ============================                                 ==================
  Apple.txt 
  Chandu.css
                       ------------------------->
                            Git add                                        Apple.txt
                                                                    Chandu.css                git commit   
                                                                                    ---------------------------->               Commit ID (Apple.txt and Chandu.css)

   When create a file or modified existing files those are avaible in your Working area (check with git status )
   In order to move staging area You can use git add command
                                                         git add .
                                                         git add *
                                                         git add specific filename
                                                         git add f1 f2 f3 f4 --

   From staging area to your local repo .. git commit -m "Message" 

   If existing file -- you can directly move from working area to Local repository by using -a flag => git commit -a -m "Your message"
   Then Push the chages to remote repository.

   Working Area --> (git add) --> Staging Area --> (git commit) --> Local Repo --> (git push) --> Remote Repo


   For the first time you are commiting it will ask who you are..
   you can tell with below command
   
                              git config --global user.name "Your name"
                              git config --global user.email "Your email"
                 To check - git config --global --list
```                 


  ### How to mapp Your local repository to remote repository ?

>   git remote add  chandu1 https:/url
    here chadnu1 - is the alias it will register url in chandu1 
     so next time you no need to give full url just git push chandu1 master (to for pushing)

```
 git log ---- Git log command will show what are the commits having in  your local repositry and which are pushed into remote repo.

  if commit HEAD pointing to master only that means it is pointing you local repository not pushed the changes
  if commit HEAD pointing to orgin/master origin/HEAD that means those commits are pushed.

  By default git log command will give details output, It see the output usable you can use below command
  git log --oneline
```
> git show commit id - will give what changes done at what file.

>  git show --pretty="" --name-only commit id ==> will show the only filename

``` 
For suppose you given commit with wrong message but didn't push , you can change the message by using below
  git commit --amend -m "your new message" 
```