# Xargs

> [!IMPORTANT]
> Before diving into the deep , we have to know the difference between pipe(|) command & xargs

### Pipe
```
  Pipe => This will pass previous command StdOutput to next command Standard Input
         cat filename | grep word
         Here cat is the command1 and what ever output comes from it , the pipe command passing to grep (command2) as StdInput
```
### Xargs
```
   Xargs - It will take the previous command standard Output and Pass as the second command StandardArgument
   Command1 |Xargs Command2
   └─>>> cat list 
    sebv
    apun
    tmat
    crol
    josd
    kcon
    mej
    ttri
 ─>>> cat list |xargs dnwho     # here form the cat command it takes input and passing as argumnets   

    login:        biab
    email:        biab@dneg.com
    name:         Bianca Bonatto
    status:       active
    site:         Vancouver
    location:     w4a
    room:         -
    seat:         -
    machine:      mtlws2057.vanmac87.vanthin258
    dept:         company_na_cs
    homedir show: TRAINVFX

 ------

    login:        sebv
    email:        sebv@dneg.com
    name:         Sebastian Vazquez
    status:       active
    site:         Vancouver
    location:     w4a
    room:         -
    seat:         -
    machine:      mtlws2065
    dept:         company_na_cs
    homedir show: TRAINVFX
```
> [!NOTE]
> In Above what happen if you want to see you can see by using -t flag in xargs
> When I use -t ; xargs prints what it is doing , first the output added as dnwho argumnets
> -t -n1 => will show the execution of 1 by one argumnet

```
        └─>>> cat list |xargs -t dnwho 
        dnwho biab sebv apun tmat crol josd kcon mej ttri 
        -----

        login:        biab
        email:        biab@dneg.com
        name:         Bianca Bonatto
        status:       active
        site:         Vancouver
        location:     w4a
        room:         -
        seat:         -
        machine:      mtlws2057.vanmac87.vanthin258
        dept:         company_na_cs
        homedir show: TRAINVFX

        ------

        login:        sebv
        email:        sebv@dneg.com
        name:         Sebastian Vazquez
        status:       active
        site:         Vancouver
        location:     w4a
        room:         -
        seat:         -
        machine:      mtlws2065
        dept:         company_na_cs
        homedir show: TRAINVFX

        ------

        login:        apun
        email:        -
        name:         Anuradha Pundir <RETIRED>
        status:       inactive
        site:         Vancouver
        location:     w4a
        room:         230
        seat:         05
        machine:      mtlmac535
        dept:         vfx_na_prod
        homedir show: -
    ```


        └─>>> cat list |xargs -t -n1 
        dnwho biab 
        ------

        login:        biab
        email:        biab@dneg.com
        name:         Bianca Bonatto
        status:       active
        site:         Vancouver
        location:     w4a
        room:         -
        seat:         -
        machine:      mtlws2057.vanmac87.vanthin258
        dept:         company_na_cs
        homedir show: TRAINVFX

        [1 result]
        dnwho sebv 
        ------

        login:        sebv
        email:        sebv@dneg.com
        name:         Sebastian Vazquez
        status:       active
        site:         Vancouver
        location:     w4a
        room:         -
        seat:         -
        machine:      mtlws2065
        dept:         company_na_cs
        homedir show: TRAINVFX

        [1 result]
        dnwho apun 
        ------

        login:        apun
        email:        -
        name:         Anuradha Pundir <RETIRED>
        status:       inactive
        site:         Vancouver
        location:     w4a
        room:         230
        seat:         05
        machine:      mtlmac535
        dept:         vfx_na_prod
        homedir show: -

        [1 result]
        dnwho tmat
```
============================
### 
  By Default xargs - will take output from previous command and arrange in a one line with spaces, If you can tell howmany lines you want by using -n
```
    └─>>> cat list |xargs 
    biab sebv apun tmat crol josd kcon mej ttri
    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |xargs |xargs 
    biab sebv apun tmat crol josd kcon mej ttri
    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |xargs |xargs -n2 
    biab sebv
    apun tmat
    crol josd
    kcon mej
    ttri
    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |xargs |xargs -n3
    biab sebv apun
    tmat crol josd
    kcon mej ttri
    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |xargs |xargs -n5                                                                                                                              [Wed, Jun 03 - 16:05:49]
    biab sebv apun tmat crol
    josd kcon mej ttri
```
### First Of all
Why we are using xargs command? 
By default some commands won't take output from previous commands:- we differniate as below

===Stream Based -> Awk, grep, sed, sort & wc -- 
The above commands will work with pipe fine - cat filename |grep/awk/sed/sort/uniq/wc

===Argument Based -> echo,ls, mv, cp --
This arg based won't accept pipe , bcz pipe will give the std input from previous command
so result will be - 
```
    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |ls 
    '='   10           analysis.sh    bilal.sh          cla.sh           dnalias.sh       kiranmai.sh   myif.sh   statemets.sh   test      testapple
    1    aanchal      anotherefile   casestatemts.sh   dirvish          duplicat.sh      learn         rough     sydws.sh       test123   testforun.sh
    1]   accessible   applog         casex             dirvishping.sh   findmachine.sh   list          scripts   systemlog      test143   winmachin.sh
    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |echo 

    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |xargs echo 
    biab sebv apun tmat crol josd kcon mej ttri
    ┌──(vcse@chnws5245)-[/user_data/Practice]
    └─>>> cat list |xargs echo
```    
# Place Holder 
Placeholder (-I{}): Is a temporary box or bucket. it stores the previous command output in the buket and when we call it paste as one by one arguments
Why it is critical: Many commands (like cd, whichsys, or user scripts) are hardcoded to accept only one single target argument at a time. If you pass them a cluster of inputs at once, they will crash. The placeholder solves this by creating a controlled loop.
```
    └─>>> for i in `cat set|awk '{print $5}'`; do ws -w "$i" ;done | grep -i "Z230" | awk -F, '{print $1}' |xargs -t -I{} whichsys --site all {} 
    whichsys --site all chnws433  # it works here
    chnws433,Vivek B,214tm,HP-Z230/EL8-64,1x4core 3.30GHz-E3-1230 v3,Quadro K2000,31809,4-of-4,SGH349PZQM,-,-,Workstation,Deployed
    whichsys --site all chnws439 
    chnws439,Rahul Patnaik,225tm,HP-Z230/EL8-64,1x4core 3.30GHz-E3-1230 v3,Quadro K2000,31809,4-of-4,SGH349PZRR,-,-,Workstation,Deployed
    whichsys --site all chnws281 
    chnws281,,208tm,HP-Z230/EL8-64,1x4core 3.30GHz-E3-1230 v3,Quadro K2000,31907,4-of-4,SGH349PZS0,-,-,Workstation,Deployed
    ┌──(vcse@chnws5245)-[/u/gupr] # It fials here
    └─>>> for i in `cat set|awk '{print $5}'`; do ws -w "$i" ;done | grep -i "Z230" | awk -F, '{print $1}' |xargs whichsys --site all\
    whichsys: Only one argument can be used for search. 

    it works here 
    # cat list |xargs dnwho -=> why ? think
``` 
