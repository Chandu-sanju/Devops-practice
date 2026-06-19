> [!IMPORTANT]
> Let's discuss about branches in the Git.
> Git Tag's
> Git stash
> Git cherry-pick

```
Branches are reffered to another line one work, Without distrubing the main code. Master branch is the main branch which is having final code & to it deployed into
the production.
IF you need any new features or client asked to do some changes then you can use different branch and develop the code and once it is tested then you can merge with master.
Usually we are using the devlopment and testing brnaches mostly.
```

## Git Branch Commands

* git branch - To list the all branch , You can us -a to list remote branch as well
* git branch brname - it will create a new branch
* git checkout branchname - to switch to another branch
* git checkout -b branchname -- In one command it will create branch and it will switch you
* git branch -d branchename - do delete the branch if there is no un merged commits
* git branch -D branchname - do delete forcefull even unmerged commits

## Git Merge & Merge conflict

* Once you created a seperate lineof work and developed something now you need to merge the change which you done on another branch to master
then you can use
>> git merge devlopement  ( Here you have to swtich to master branch then run)

#### Merge conflict.
 
 * if Git cannot automatically decide how to combine changes from two branches.

> [TIP]
> Common Reasons for conflict.
>
> Same file, same lines, different changes (MOST COMMON)
>
> One branch deletes a file, other branch edits it
>
>One branch renames a file, other edits it
>
>Overlapping changes in nearby lines ( If line 10 changed in master and line 11 changed in development)
>
>Rebase conflicts 


# GIT Tag's
>> Git tags are labels attached to specific commits, usually to mark important points in history, like releases.
```
 You want to take backup of current production deployed code, Then you can give tag to that and store it in the form of zip or tar file with specifc versionname
 Later whatever change you did in your code you can make backup with versions.
 ```
 ### Git tag commands

 * git tag ---> to see current tags
 * git tag tagname --> to create a tag
 * git push aliasname tag tagname --> to push tags to remote repo
 * git push aliasname --tags --> to push all

 ### Git stash
```
For suppose you have 3 branches master,dev,stage, Currently you are working on dev branch , Due to emergency you have to fix issue on master branch.
So whatever work you are doing in dev not completed, but you need to switch to master , However without commit the changes you can't switch into another branch.
In this situation you can save your workin temporarely. by below 

git stash -- to take temporary backup    
git stash list -- howmany backup are there
git stash apply -- back to normal 
git stash apply stash{3)} - to apply specfic one
git stash drop ---> do delete backyp by default recent one
git stash pop ---> It will apply and drop (delete ) at a time, if not specified take recent one.
```

### Git Cherry-pick
====================
```
This command will use - when another branch has multiple commits and you want to merge the specific commit instead of all the commits.

command :- git cherry-pick commitid
```

#### GIT CLONE , Git Pull & Git fetch

> Git clone - you can download the repostiory by using git clone ssh url or https url
    command:- git clone url

> Git fetch ==> Will get the update data form remote repo to local repo but in order to get working copy you need to again run git merge orgin/master

> Git pull ==> it will get the updated code from remote to local & working copy 
 Simply git pull => git fetch+git merge

 > What is the diff between git pull & fetch & when to use?
  diff you know, When to use - for suppose you working on a file from last todays and you got to know there is updated code in remote that time you can git fetch so that it won't overwrite your code, and then you can do git merge if any conflicts you can check.
  Other case if you want updated code simple do git pull.