# Linux_With_Shell_Script_Study_Plan

# Basics:- 

        Reading the man pages => 
        Whatis ==> where man pages located and what is the purpose of command
        whereis ==> Which tells where man page path and section of that man
        which $command:- to find the binary file

# Commands:-
         
         PWd:- To know where you are curretnly
         cd :- Change directory
              cd . -- currebt dir
              cd .. -- parent dir
              cd - => it will jump previous
              cd ~  => Home dire
              cd  => Home Dire
              cd ../../ ==> backward
              cd /home #cd path

         ls:- List the files/directories

              ls
              ls -l
              ls -a == hidden
              ls -ltrh
              ls -ld ==> To list the direcotry properties
              ls folder1 # ls path

         mkdir:-
             
              mkdir $dirname
              mkdir -p dirname
              mkdir -m dirname -- along with permissions
              rmdir $dirname => when dir is empty #not useful
 
# Deal with files :-

Everything in linux is a file, directory is also file
Types of Files in Linux:-
1. Regular files (-f)
2. Direcoty files (-d)
3. block special (-b)
4. character TTY (-c)
5. Link files (-l)
6. pipe file (-p)
7. Socket file (-s)

Related file commands:-
                      file - which will tell what type of file it is
                      touch - to create the empty files
                      cat - to list the file content and create the file as well
                      vi or vim
                               i           => insert
                               r           => Replace mode
                               d           ==> to check diff b/w to files
                               esc+shift+G => Go to the last line
                               esc + o     => create a line below the cursor
                               esc+shift+o  => create a line above the cursor
                               esc yy       => copy the lines ; esc 10 yy -to copy 10 lines
                               esc p        => paste the copied lines
                               esc u        => undo
                               esc dd       => delete the line ; esc 10 dd - to delete 10
                               / $word      => to search word                           
                           ###    esc+shift+:%old/new => replace old to new
                               set nu  => set line number
                               esc+shift+linenumber => to go the particular line
                               set nonu => to remove the line numbers
   
                      cp => copy the file/direcotory

                               cp source destination
                               cp -p => to preserve the permissions
                               cp -R and cp -r both are same -R is conisitent => to copy the directory
                               =============You just need to know; No need to remember
                               cp -i => it will ask before overwriting
                               cp -u => only copy only source file is update then destination
                               cp -f => forcefully overwriting
                               cp -n => no overrite
                               cp --remove-destination => first it will remove the file from destination and then copy

                        mv =>  It will usefull for both rename and move.
                               if file with in directory it will rename
                               if file outside then it will move => cut and paste

                        rm -rf => To remove the file& direcotres forcefully
                        
                        While using mv and rm -rf , We have to think twice or thrice

# File content display commands:-

                         head :- To see top lines by default 10 line
                         head -n 100 - To see top 100 lines
                         tail => To see bottom of the lines from file by default 10 lines
                         tail -n 100 - to see the bottom 100 lines
                         tail -f => more usefull when troubleshooting and give live logs
                         wc => word count 
                              wc -l => line numbers
                              wc -w => how many words
                              wc -c => How many characters
                        more => it will show the file contents page by page - space bar you can use
                        less => it will show till file contents fit the screen
                        strings => to read the binary files
        Question :- If a file contain 200 lines, To see lines from 100 to 110 how you can do ?
           addition:- how many ways you can see the how many lines in a file.

# File Permissions Commands:-

                    By default for files 666 and dir 777 permission applied. Due to security concerns it should not be , now umask will comes to the picture, User mask which will protect the default permissions.

                    Default umask value = 022
                    --------------------------
                    so Default File per - 666
                    Default Dir  per    - 777
                               (-)     - 022
                                ---------------
              default file permission  => 644
              default dir permissions  => 755

               How to check the umask value => Just type umask on terminal it will give your umask value
               How to change the umask value => umask $newmask value; umask 024 - It will only change temporarly
               How you can change permanently => You can edit - .bash_profile/.mycshrc/.zshrc/ user config file
               How you can change it globally => /etc/login.defs, /etc/profile

# Changing/managing File Permissions:-

     when you do ls -l filename / ls -ld dirname => it list the current permissions
      here we can three portions => drwx r-x r-x  2 vcse users 4096 Aug 31 07:51 Desktop
                               first portion(u) -- the owner permissions
                               second portion (g) -- Owner Primary group permissions
                               third portion  (o) -- Except owner and primary group members 
                               all are the comes under others
      We can manage in two ways numeric and albhabetical way
       Numeric => Recommeded
                r=4 ; w=2 ; x =1
                usually file permission start with 000 -- no permission
                                                   001 -- -x -1
                                                   010 -- -w -2
                                                   011 -- -wx -3
                                                   100 --  -r -4
                                                   101 --  -rx -5
                                                   110 --  -rw -6
                                                   111 --  -rwx -7
                How it will be if 1 in first =4 ; 2 then 2 ; if in 3 then 1

                chmod is nothing but change the modification
                chown is nothing change the ownership
                chgrp is nothing but change the group ownership => will discusse at user & groups


               chmod 666 file name
               chmod 777 direcotry
               chmod -R 666 file/directory => reccuresively 
               It means it will changes inside the all subfolder and files along with main 
      Symbolic/Alphabetical =>
               chmod u=rwx,w=rw,o=x filename
               chmod a=rwx ; for all 
               chmod +x filename --- it will execute for all

# File Globbing :- 

               * Astric symbol:- represent of all files inthe current direcory
                                 for suppose a dir called practicedir and contains below files
                                 File01 File02 File03-----File09 , File01.txt File02.txt---File09.txt
                                 when search ls * - it will print the all the above files
                                if you did - ls *.txt => it only prints .txt files
                
                ? - Question mark - Represent of one character

                                ls -l File0?.txt - it only print File0--- then whatever have single charcter..
                    
                [] -Square Bracket - represents of mathcing character inside[A5]
                                 file1 file2 file3 File4 File55 FileA fileab Fileab FileAB
                                 ls File[5A]
         
                 ^ - Represents starting of the line
                 $ - Represents ending of the line

===============================User Management==============================================
# A user in Linux is an identity, By default when we create below 
                                                1. UID
                                                2. GID
                                                3. Home Directory
                                                4. Default shell
                                                5. Default password/permissions

# User management Imp Files:-
                          1. /etc/login.defs ==Uid Range,Home Permissions, password expiry
                          2. /etc/default/useradd; useradd -D ==> default shell , Home location, Where the skeleton location
                          3. /etc/skell ==> what files here those will create in /home/user direcotory
                          4. /etc/passwd ==> Contains user info
                          5. /etc/shadow ===> User Password Info

# How we can create a user in linux and what happening in the backend ?

   Command:- useradd chandu

   Backend Process:- When we hit above command

                  1 => Useradd will check for /etc/login.defs -> Uid Range,Home Permissions, password expiry
                  2 => /etc/default/useradd -> Having the info about default shell , Home location, Where the skeleton directory location
                  3 => Then it will look into /etc/passwd file and looks for last UID and assign next one
                  4 => it will create a group and userinto the group those config also    from /etc/login.defs if
                                       USEEGROUPS_ENAB yes put the group in /etc/group
                  5 => create home directory as per the /etc/default/useradd
                  6 => copy the files from /etc/skel/ to user home, dir cp -r  /etc/skel/. /home/username/

# How to give custom details while creating the user:
            
    Command:- useradd -u 2222 -g 2221 -G 2220 -d /home/customdir -m -s /bin/zsh -c developer -e 09/10/2026 -f 3 Chandu

    -u => Custom UiD
    -g - Primary group
    -G - Secondary group
    -d - Home direcotry
    -m - Make 
    -s - Shell 
    -c - comment about the user
    -e : expiry date (month/date/year)
    -f inactive days login after pasword expiry 
    username:- The user which we are going to create

  Most Asked interview question about user Management - Fields in /etc/passwd file

    chandu:x:1001:1001:developer:/home/chandu:/bin/bash
   
   There are 7 fileds seperated by colons :
                                        1.  username/loginname - chandu
                                        2.  x - Password place holder - means password stored  in /etc/shadow
                                        3.  UID - Unique user id of the user
                                        4. Primary Group Id 
                                        5. Comment - Developer
                                        6. Home directory - /home/chandu
                                        7. Login Shell - /bin/bash

   Fileds in /etc/Shadow file:-
                                 
                      chandu:$6$hashvalue$abc123:19500:0:99999:7:::

                    1. Username
                    2. Hasshed Passwd
                    3. Password Last modified date - (1970 jan 1 , it will count)
                    4. Minimum days required that password not change
                    5. Maximum days required - Till this days you no need to change passworsd, SOme organzition it is 365 days
                    6. Warning days - it will warn before 7 days to change the password before expiry
                    7. Inactive days login after password expiry, that means if password is expired april 18 , the inactive days set to 7 days till april 25 the account won't lock at april 26 it lock the account
                    8. Account expiration date
                    9. It will be user for future use

                      userdel :- Delete the user and you should use useradd -r $username to remove /home directoy too
                      Usermod - By this you can do the modification.
                      changning the primary or secondary groups, Uid , ---

# Password Management:-
 
             To check the status of a user password :- 
                command:- passwd -S usernmae # PS means notlocked and Lk means locked
                to Lock - passwd -l usernmae
                to unlock - passwd -u username
                If delete the passwd -d username
                               Sometime times we need to setup password like once they login then they has to set the new password so that time, we can do two ways
                                    # passwd -f username ## Actully it immediatly expired and make to change the password in next login, Default last password date set to 1970
                                   # chage -d 0 username - Same thing but it will setup last passwd changed to 0 so, User needs to change the passwd after first login


                  If password is set & locked it will be looks below that means double !! and encrypted password
                        shobha:!!$6$EWkhaqTE9VPvxycS$vkHnhcCE5MyjrAo2yzPAEBKWfGzcSMuoAL6aeS.4YmVyknQ9UgzprHwitCtzVS3A41DEsY.nLEGngDfQP7FvP/:20429:0:99999:7::: 

                      If password is set and not locked then there will be no ! and you can also 
                        find the passwd -S useranme
                         hari:$6$JvAu/.v3vbF4BP9m$DIdu.b77.kIfGc/OY5XkACC8r5eAdOx0d7wGNN4QThdqJ94FQ1FJ3X2IpD1dUoZyKuxb6wh.UMQYe4Hcmdlae:20429:0:99999:7:::
                      If Password is not set then it will be like
                            ramm:!!:20429:0:99999:7:::
                  There is another if user password is set and account itself locked the it will be ## usermod -L to lock usermod -U , to unlock 
                       sanju:!$6$xQYM8H/YQRIeBbkU$Q5o053qOQeGmi18z7PLBPoHUHCiQy.cOfbiV/k1RTfks2usMQg338TR8x3cAp6q7S/M/6V38D1Ag6rP2LJZUA1:20429:0:99999:7:::
           => How to change the Password the age of the user
              chage -m 10 -M 20 -W 5 username

# Group managemet :- 

  A group is collection of users, Instead of giving permission to indiviadual , we can combined group of people into one group and create a group and give the permission, SO i will be easier as well as decrease administrative workload.

  Important group Management commands:- 

    groupadd groupname => assigned default gid and create

    groupadd -g 10021 groupname => we can specify the gid

    groupinfo -- /etc/group

    grouppasswd info - /etc/gshadow

    groupmod -g 11222 aws => we can chage the gid

    groupdell groupname =- to delete the group

    we can also edit /etc/group and user, afer if we use below

    grpck -- to find the error if any it will show
    
    gpasswd - to set the password and by using this we can add / remove users from the group


# Inodes & Soft and Hard links

       Inode:- when we do ls -li it will show inode numbers...

       Indoe:- An inode is a data structure; it stores metadata about a file.

         It won't store the file name but below..
           => File Size
           => Ownership (User& Group)
           => Permissions (r-w-x)
           => Timestamps (Created, modified, when accessed)
           => Pointer/links to the actual data blocks on disk
   Layman terms:- 
     if a file is a book, Indoe is an index of that book.
       index doesn't have the book name but where is the book who owns it and when it published , the chapter but not actual content.

    If Inodes are full , you can't create new file even disk space is full.
    Millions of logfile or temp files , caches files, to many softlinks will cause for inodes full.

 Softlinks ==>
               A softlink (Symbolic link) is a shortcut that points to the filename, not the inode
            1. softlink can links across the file system directories
            2. softlinks will use different inode numbers bcz it is pointing to the file
            3. softlink cannot use external space in the disk
            4. In softlinks if main file is deleted all the links are collapsed or not useful

 Hardlinks ==>
                 A Hard link is basically another name for the same inode

             1. Hardlinks can link with in the file system
             2. Hard links can use same inode number
             3. hard links can use eternal space in the disk
             4. In hardlink main file deleted also we can have the data from until last link


 Commands: ln -s destination source => softlink
          ln destination source => hardlink

# User Profiles :-

 When we create a user account the user environment files or profile files will be copied from /etc/skel directory to every user home fdirectory
  
  The user Environment files :- 

                          .bash_logout :  We can write the logout scripts in these files ==> It will execute when user session exists,
                                          here usually we can write clear the screen, remove temp files kill the all process realted users..

                          .bash_profiles: we can write the startup programms ==> it will run at starting the session once and it load bashrc, usually we are storing environment varibles of us, if we put your env varibles in .bashrc every new terminal it will load and create the performace issues

                          .bashrc : we can write the  user's alias commands => it will be terminal config file,  you can modify how you want and function aliases  
  We Have Two types of Profile :-
                           1. Global Profiles - /etc/profile 
                             If you write anything here, It will replicate for all users, If you write " echo Hi it will be print in all the user's terminal"
                           2. Local Profiles
                              if you want to same thing for local or particular user you can right it from his .bash_profile if user is using sh shell the it will be .profile
# Below are the some widely used shell profile and user files

  | -------- | ------------------------------------------------------------------ |

  | Shell  |    Global            User              User            User         |
  | -------- | ------------------------------------------------------------------ |
 | **Bash** | `/etc/profile` – `.bash_profile` – `.bashrc` – `.bash_logout`   

 | **Zsh**  | `/etc/zprofile` – `.zprofile` – `.zshrc` – `.zlogout` 

 | **Tcsh** | `/etc/csh.login` – `.login` – `.cshrc` – `.logout`     

 | **Ksh**  | `/etc/profile` – `.profile` – `.kshrc` – (no logout) 

 | **Dash** | `/etc/profile` – `.profile` – (no rc) – (no logout)  

 | **Fish** | `/etc/fish/config.fish` – (no login) – `config.fish` – (no logout)
 

 | -------- | ------------------------------------------------------------------ |

# let's talk about some user related commands/ user monitoring commands

      commands:- 
              
              whoami :- The whoami tells you your username ; echo $USER
              who    :- it will tells us who logged in the system , Basically it will get the
                       details from /var/run/utmp file which holding active login details
              who am i : here the who command will display only the line pointing to your 
                         current session
              w         :-  it will shows you who logged on and what they are doing

              id      :- it will give user id, primary group and list of the blonging groups
                       id username
              su       : su vcse, it will allow to run as shell as another user.

              su root ; su username :- it will just switch to another user , But still the environmet is yours and you will be in your home directory

              su - root :- here complete environment change to swithed user.

                        
                 



               
       
                    
 
                                                                                                        