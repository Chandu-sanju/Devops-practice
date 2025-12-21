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
                                                5. Default password/permissions /Umask

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
                  4 => it will create a group and add the userinto the group those config also    from /etc/login.defs if
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

   A group is collection of users, Instead of giving permission to indiviadual , we can combined group of people into one group and create a group and give the  permission, SO i will be easier as well as decrease administrative workload.

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


# User Profiles :-

 When we create a user account the user environment files or profile files will be copied from /etc/skel directory to every user home directory
  
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


           Commands: ln -s destination source => softlink ; link -s file1 file2

                     ln destination source => hardlink ; ln file1 file2


# Bouns Tip:- apropos keyword, If Stuck at anywhere and you are not able remember related command so that time you can use it
>> Example you stuck at while doing lvm => so use apropos lvm it will give related command by checking the man pages similar like man -k

#Sudo User
       
       A sudo user is nothing but a non-root user only, But he as the previlages to run as root, By using the sudo infornt of command so that it will execute a command
       as root user.

           How to give sudo access to a normal user ?

           There is a sudo access control file => /etc/sudoers

           Here you can add Under Root , IF you want add the user
           
>> root  ALL=(ALL:ALL) ALL  
       
           user: root

          from any host: ALL

          can run as any user: (ALL:ALL)

           can run any command: ALL

        but must authenticate normally if needed

>> root  ALL=(ALL:ALL) NOPASSWD: ALL

       NO password required.
               
               There is a time limit once you enter password then it will store 5 min/15 min based on linux type. till the time it won't ask password after it will ask to enter.
>> /etc/sudoers.d/ Directory ?
               It contains additional sudo rules seperate from the main. Files inside this directory define who can run which commands as root.
      

#                    Process Management 
>>
    Process:- A process is compiled source code that is currently running on the system.

    PID: All the process have a process id or PID 

    PPID :- Every process has a parent process. The child process is often started by the parent    process      

    Init/systemd : The first process which will started by the kernal itself, So techincally init/systemd has no PPID because this is the only Parent for all the process id's.

    The first process id in linux 1 that is nothing but init/systemd .. upto RHEL6 init is the first process id and from RHEL7 systemd.

    Kill:- When a process stops running, the process dies, When you want a process to die, You can kill it.

    Daemon:- A background service process which start at system startup and keep running forever are called daemon process or daemons, These proces never die. sshd, httpd, crond ---

    Zombie:- When a process is killed but still shows up on system and some utilize memory spaces those process are zombie and we can't kill it becuase they already process, So we have to kill the parent process of the zombie
   
   ===========
>> Some Important Process Management commands:-

    ps :- It displays current console process
         
      └─>>> ps                                                                                                                                       
      PID TTY          TIME CMD
     3909505 pts/4    00:00:00 zsh
     4002326 pts/4    00:00:00 ps

    pgrep :- It will give procces id of a process

     pgrep pcoip-server
     pgrep chrome
     pgrep bash ----
    
>>  kill :- To kill the process with process PID
     
      kill -l (To show the list of kill signals)

      Actually kill command will send a signal to process, We have total 64 Killsignels are there, By default when you run "kill PID" -15 signal will run that is
      nothing but it send the SIGTERM signal to the process, It is very safe to use and recommended , Why ?
         
         The process is asked to stop

                                  The process can catch the signal

                                It can:

                                       Clean up temporary files

                                       Close open files

                                       Save state

                                       Gracefully shut down

                                       The process may ignore or delay the signal

         Then We can use kill -9 PID, -9 is nothing but forcefully stop/ "It will send the process SIGKill signal"

           What happens

                          The kernel kills the process immediately

                          The process:

                                     Cannot clean up

                                    Open files, locks, and shared memory may be left in a bad state

                                    Using SIGKILL can cause:

                                                            Corrupted files

                                                            Incomplete database writes

                                                            Orphaned shared memory

                                                            Unreleased locks

                                                            That’s why it’s called the “nuclear option”

    pkill - To Kill the process with process name instead of using PID

    killall : It will send the kill signals to all the process based on their state. we can use killall top, killall chrome  like..

      generally mostly I am using for terminate a specifc users all process - by using -u
                                                                killall -u $USERNAME

      Interview question;- What happens when you press Ctrl+C or Ctrl+Z ?
          Ctrl+C call the Interput (SIGINT) which is kill signal 2 and  kill the process. ==> Kill signal 2 ,SIGINT
          Ctrl+Z call the SIGSTP kill signal 20 which will suspended the job in this case it stop the process


>> Most Commonly Used ps command options to troubleshoot:-   

   ps aux | less / ps -aux | less

   here :- 
    -a = all proccess of all users
    -u  = show the list of usernames
    -x = no terminal process nothing but daemons
   
   Example output:-
  
     USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
     root         1  0.0  0.1 170892  12456 ?        Ss   Jan18   0:03 /usr/lib/systemd/systemd
     root       412  0.0  0.0      0      0 ?        S    Jan18   0:00 [kthreadd]
     root       815  0.1  0.5 512348  43872 ?        Sl   Jan18   1:25 /usr/bin/dockerd
     mysql     1245  1.8  6.2 2487352 512384 ?       Sl   Jan18  45:12 /usr/sbin/mysqld
     nginx     1532  0.0  0.2 126540  18240 ?        S    Jan18   0:12 nginx: worker process
     sav      1764804 26.5  0.0      0      0 ?      Z    17:15  82:30 [xstudio.bin] <defunct>
     premm    2216681  0.0  0.0      0      0 ?      Zs   20:55   0:00 [chrome] <defunct>
     user1    2281103  0.3  1.2 945612 102340 pts/2  Sl+  22:10   0:15 /usr/bin/python3 app.py
     user1    2281720  0.0  0.0 222036   1112 pts/2  S+   22:25   0:00 ps aux

    User:- Process OWner
    PID: process id
    %CPU : cpu usage
    % MEM: memory usage
    VSZ :- Virtual Memory SIZE
    RSS:- Resident Set Size (Actual RAM / Physical Mem)
    TTY : Temrinal
    STAT:- Status of the process
    START: when it is started
    TIME : cpu consumption time
    COMMAND: What is the command

## Ps Command Options:-
>>
    PS -A => List information of all process.
       -e  => Lists every process on th system
       -f => generates full listing
       -l => generates long list
       -u => List particular user process
       -g => to list group using proces
       -o - to get the specfic options.
  
  Interview How to Indentify Zombie process and how to kill ?

  => You can find by simple running Top Command
  => You can find ps -aux | grep defunct
  => The most useful command to find zombie process along with PPID
      ps -A -o ppid,pid,stat | grep Z
      By above command you can find and kill the parent process then it will kill zombie also


>> 
   ## How to increase prority value of a process

   First We need to understand - If prority value is high then process has low priorty to cpu time allocation
                                if prority value is low then the process has high prority to use cpu time allocation
    Nice value :- it starts from negative -20 to postive 19
                 When nice value increased prority value will low
                 When Nice value decreases prority will increase
      
      When you ran Top
                    
                    prority value - 20 in this case nice value 0 that means normal
                    prority value - 0 in this case nice is is 19

  When You are starting a process so you can use - nice -n 4 $Command 
                  for giving negative nice value needs sudo => sudo nice -n -4 $Command
    If You want to change the prority of already running process then use - renice

        => renice -n 8 Processname/ProcessID
        ==> sudo renice -n -8 Processname/ProcessID
        
>>        TOP  CoMMand

      Top command real-time monitorng command and widely used :-

      By default Top command will give the details of USER PID cpu utlization , mem  prority and nice values , Load average system uptime so on

       It refresh the data every 3 sec by reading the files from /proc and we can change the intervel time by pressing d and give what intervell we need

       Every Usefull options :- h for help, P for CPU USAGE, M = MEM usage, k for kill , r for renice

>> 
    How to Make a process a background process ?
    By simply using end of the command & Ampersend simple it will make as backgroup job/process
    to see background jobs/process => Command is jobs
    if you want make it forground - fg %1 ==> here one is the job id
    if you want you can kill CTRL+C

>>>>>> Tip:- If you want see detail process structure use pstree | more >>>

#################################################################################################################################################################

| Symbol     | Effect                                            |
| ---------- | ------------------------------------------------- |
| `#`        | H1 header (`# Title`)                             |
| `##`       | H2 header                                         |
| `###`      | H3 header                                         |
| `>`        | Blockquote (`> This is a quote`)                  |
| `` ` ``    | Inline code (` `code` `)                          |
| ` `        | Code block (triple backticks for multi-line code) |
| `-` or `*` | Bullets (`- Item` or `* Item`)                    |
| `1.`       | Numbered list                                     |

==========================================

# Disk Management

 ` To see the detials of the disk drives with internal partition details/existing partitions => fdisk -l `

 ` lsblk will show detail partitions along with mounted points `

 - add the disk to the system
    * then check with lsblk 
    * create the partition with => fdisk $diskname ( fdisk /dev/sdb)
    * If you want to see existing partion => then press p => print
    * want to create new - n
    * if we are using old MBR partition table it designed to create 4 Primary partitions only and then we can only created extended nothning but logical
    * New partiton GPT there is no need to create only 4 primary partitions,so go with default - press p or enter it will select primary
    * Here You need to select partition size => +100M (MB), +2G (2gb)
    * press the w to write the changes
    * if you press h it will give help options
    * run the command:- partprobe to update the partition with out reboot
    ### Now partition has been created successfully

  `` To Write the data in the partitions you should format it, Means you need to give the filesystem.  

>        Disk → Partition → Filesystem → Directory > Files


        Till the partition you have raw storage , OS doesn't know where to write the date and how to keep it.

        Actually what filesystem formating does :- Writing filesystem metadata:

                                                                              superblock

                                                                              inode tables

                                                                              allocation maps

                                                                              journals (if supported)

                                                            Defining rules for:

                                                                                how files are stored

                                                                                how free space is tracked

                                                                                how directories work

                                                                                crash recovery

      After formatting, the partition becomes usable storage. ``

      To make the filesystem to the partion command is  => mkfs.ext4 /dev/sdb1(partitionname) - genereal purpose usually giving for /root, /home good performance)
                                                           mkfs.xfs /dev/sdb2  - High performance , used to storage large and media file high I/O operations
                                                          
                                                
       
      to writed the date we need directory and mount it with our partition

      * create a directory - mkdir chandu
      * Mount => mount /dev/sdb1 chandu (temp)
      * To make it permanent - edit fstab

       <device>    <mountpoint> <fs>  <options>  <dump> <fsck>

      /dev/sdb1      /user_data  xfs   defaults    0       0            => DeviceName - Not recommended

      UUID=2f9c1f32-1a3b-4c91-a5d1-1e7a8e3f6a12  /user_data  xfs  defaults 0 0  => With UUID 

      LABEL=USER_DATA  /user_data  xfs  defaults 0 0 ==> LABEL

      How to set a label ?
      ext4/ext3/ext2 => sudo e2label /dev/sdb1 USER_DATA
            to check => e2label /dev/sdb1

             xfs => sudo xfs_admin -L USER_DATA /dev/sdb1
       to check => sudo xfs_admin -l /dev/sdb1

 ` Label name should be unique and  there is no space and don't _  and for ext* 16 characers & for xfs you can use 12 chars. `

  #
   if you want unmount = use umount /dev/sdb1 mountpoint
        to umount all -> umount -a
    forcefully umount => umount -fl path
    to check which user is accessing - fuser -cu Mountpoint
    to kill user forcfully => fuser -ck Mountpoint

## ==================== Logical Volume Management =================================================================


lvm is nothing but logical volume management..lvm is used to create ,delete and resize the file systems dynimacally .
lvm implementation supports physical storage grouping, logical volume resizing and data migration..
one of the nice feature in lvm is logical volume resizing ,we can increase the logical volume ,sometime without any downtime and 
we can migrate data from failing hard disk creating mirrors and snapshot.
 ------------------------------------------LVM-------------------------

componenets of lvm in linux:----------
> 1) Physical volunme(PV):- it is the standard partitions that you add to the lvm.normally, a physical volume is a standard primary or logical partition with the hex code 8e.
> 2) physical extend(PE):- it is a chunk of disk space. every PV is called into a number of equal sized pes.
> 3) volume group(VG):- it is composed as a group of pv's & lv's it is the organizational group for LVM
>  4) logical volume(LV):- is composed f a group of le's.you can format & mount any file system on an lv.
the size of these lv's can easily be increased or decreased as per the reuirements
>  5) logical extent(LE):- it is also a chunk of disk space. every LE is maped to a specific PE
 
####################STEPS TO CREATE LVM #################################
Add a hard disck and scan it using command as #echo "---" /sys/class/scsi_host/host0/scan
(echo invited commas --- invited comma close  slash sis slash class slash skasi underscroll slash host slash host0 slash scan)
                                                                                            
1) we need to see the disk proporties using fdisk -l
2) then we need to creat physical volume group using pv create /dev/sda
3) then we need to creat volum group using vg create vgo1 /dev/sda
4) then we need to creat logical volume group using Lv create -L size -n Lva vgo1 
5) Then we need to make a file system using mkfs.ext4 /dev/vgo1/Lvo1
6) we need to creat a directory using  mkdir /LVM
7) then we need to mount the file system using mount /dev/vgo1/Lvo1/Lvm
8) then we need to check file system using  df -hT
9) then we need to turn on mount using mount -a 
10) to mount the file system permentally using vi /etc/fstad
###################### LVM REDUCE ##########################				
Ans) When we reduce the LV before we must unmount that  (If filesystem is not mounted we can't see filesin the mounted dir)
( whenever we are   going to dismount first we shoutdown the application by using filesystem) then defragment the file system 
( defragment is the process of repairing the filesystem ) by using e2fsck -d -D >Lvname< -f. 
In the next we first reduce the filesystem then after we can able to reduce the lv . 
by using resize2fs command u able to reduce the file system . Before going to execute resize2fs command 
we have to check properly otherwise we must lost the data . 
 then we can reduce  by using lv reduce command. then  we have to mount .
 
 
 Five Steps : need a downtime to reduce the filesystem ( We can reduce the ext234 file but we cant reduce xfs)
 
   1.shoutdown the application using the file system
   2.dismount # the filesystem
   3. defragement the file system # e2fsck -d -D >Lvname< -f
   4.reduce the file system # resize2fs /dev/vgdata/oravol0 12 G
   5.reduce the logical volume # lv reduce -L 12G lvname
   6. mount back the file system
   
   
   => without e2fsck and resixe we can use -r flag 
   
   
  
######################STEPS TO CREAT LVM #################################
step1=> first bring disk or partition to lvm control
 pvcreate diskname
step2=> then we need to convert to vggroup or volume group 
      vgcreate vgname disk or partition( vgcreate vgdata /dev/sdd)
	  
step3 => then we convert the vgroup to logical volume
  based on size---->
                    lvcreate -L 4G vgdata(vgname) => default name it takes
	                lvcreate -L 4G vgdate(vgname) -n lv1 => for specific name
					lvcreate -L +1G vgdata(vgname) 
					
  based on extends-->vgcreate vgchandu -s 8M /dev/sdd
                    lvcreate -l 3 /dev/vgchnadu/lv1clear


#########################How to extend LVM#####################################
1) we need to check volume group details using comand VGS
2) Lvextend -L +10GB /dev/vgo1/Lvo1
4) resize 2fs /dev/vgo1/Lvo1
5) df -hT

---------------------rename lv----
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/partial_output
https://bugzilla.redhat.com/show_bug.cgi?id=860338

######################HOW TO RECOVER ACCIDENTAL DELETION LVM ##########################################
################# how to recover lv from accedental deletion ##############################
umount and remove lv for testing purpose on test server..
step1: run vgcfgrestore --list vgname    ( we get data from /etc/lvm/archive)
step 2: select the specific which has description has before executing the lvremove command file
step 3: vgcfgrestore -f filename vgname
step 4: check lvs showing deleted lv but it is in non -active state to confirm run lvscan
step 5: active the lv by using lvchange -ay lvname...
completed the process u have recoverd deletd lvm .
#########################HOW TO MIGRATE DATA FROM ONE DISK TO ANOTHER DISK ######################
STEP 1:  We need another disk with respecitive size 
step 2: extend the volume group => vgextend vgname newdisk
step3 : check the dependencis using dmsetup dep lvname
step 4: pvmove -n lvname olddisk to newdisk 
step 5: data migration completd but in pvs it still showing old disk 
step 6 :vgreduce vgname old diskname then pv remove...
note : we cant do this process on /boot or /root filesystems

###############split###################
The following example splits off the new volume group smallvg from the original volume group bigvg.
# vgsplit bigvg smallvg /dev/ram15
  Volume group "smallvg" successfully split from "bigvg"
  https://access.redhat.com/documentation/en-us/red_hat_entcleerprise_linux/4/html/cluster_logical_volume_manager/vg_move
################Linux troubleshooting############3
lvm dumpconfig => to check config information
lvs -v,pvs -a, dmsetup info -p, lvdump



      





 






  

                    
 
                                                                                                        