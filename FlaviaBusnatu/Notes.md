# Linux
## ls
This is the list command. It allows us list the content of the directory.  

The colorized list of content  
- ls --color=no  
- ls --color=yes  

`clear` - to clear the screen  

What we pass after the command - **these are arguments**.  
- '-' one dash informs the system that we will pass one letter argument, like 'l'  
- '--' two dashes means that argument will contain more than one letter. Most commonly it will be an english word.  

`ls -l` -> l means long listing format.  
`ls -s` ->  shows the short list of files and allocated space  
ex: drwx------3 root root 4096 Jul 22 10:19 snap  
   permissions   number of hard links       Owner     Group       Size    date and time of last modification   file name  

`ls -a` -> listed all the files(+hidden files)    
- .  means the current directory of the user.  
- .. means parent directory.  

`ls -A` ->will show all files, except the . and ..(almost all)  

## Sorting

-t -> sorts files with the last modification time, newest files come first.  
-tc -> last metadata modification time- permissions change, location of the file, etc  
-lS -> sorts files by size, largest are going first.  
-lh  -> printed the size of the files not in bytes but in K, M, or G.  
        h use the powers of 1024. So, 1K is a 1 powered by 1024.  

`ls --format=commas` will print the files separated by commas = ls -m  
`ls -lQ` prints the filenames in quotes  
--time-style changes the way how the date is formated in long format  

ls -al --author prints the username of the creator of the file  
ls -ald prints directories only  
ls -ali prints inodes  
ls -alR recursively prints all subdirectories  
ls -alr prints list in the reversed order  

## MAN  
q -> quit  
man 8 ls -> go to the page 8 for ls  
man -f ls -> to see what sections/pages we have available  
man -f intro -> shows us multiple sections available  
whatis ls shows short description of the functionality = man -f ls  

## Work with directories  
`mkdir filename` -> to create a directory  
`mkdir testdir{1..10}` -> create 10 directories, starting with testdir1 to testdir10
`mkdir f1 f2 f3 ....`   
`mkdir -p parentdir/childdir{01..100}` ->with -p we allowed the system to create parent directory and Under the parentdir we created 100 files, from childdir001 to childdir100  
`cd filename` -> In order to change the directory  
`pwd` ->This command shows your current work directory.  
`cd ..` ->We can come back to the previous directory  

There is a built-in variable called **$HOME**. This variable contains the path to the current user home directory  
**cd $HOME = cd ~ = cd**

`rmdir filename` -> to remove a directory  
`rmdir testdir{1..10}`

**!we can remove parent directory only when it is empty**  
```
rmdir -p maindir/childdir
rm filename -> to remove a file
```
```
rmdir parentdir/*
rmdir parentdir
```

`rm - rf dir` 
- -r means go recursively through directories (and treat everything as file)  
- -f - force. Another words, do not ask, assume the user knows what he is doing.

## FILES  

`touch filename` ->to create an empty file  
`touch my{01..100}file`  

ls my*file -> * means any string  

`touch try1 try2 try01`  
ls try? ->means any single character(try1, try2)  

## VIM  
vim newfile -> opened a vim editor  
In order to start typing anything in editor (enter insert mode), hit i    
type....  
hit ESC key to enter the command mode, and type :wq  
If you changed something in the file and you don't want to save the changes, press :q!.  

## PIPES  
The pipes and redirections are used to send (or retrieve) some information sent from one command or script to another command or script. It works on files too.  
The structure looks liKe this:  
command1 | command2 | command3

`cat numbers.txt` ->print the file  
`cat numbers.txt | wc -l `-> print the number of lines in the file  
It is important to remember, uniq always works best with sort.  And sort is first.  
*cat numbers.txt | sort | uniq | wc -l*

`ls -al > directorylist.txt` ->redirects all output from the left side of the sign, to the file on the right side of the sign
'>' will create file (if not exists) and rewrite all data as fresh file. '>>' behaves similarly, however, if there is any content in the fill, this redirection will add theoutput of the command on the end of existing content.

`head testfile` ->It we want to print first lines of the file, we can use head. By default head shows 10 lines.

`head -n2 testfile` -> print only 2 lines

`tail testfile` -> tail does exactly the same thing as heead does, but from the end of the file.

`tree sourcedir` ->It recursively shows the content of the directory.  
`cp source target` -> copy existing file to the new filename or location  
`mv sourcedir/one movedfiles` -> to move the file from one place to another

## TOP  
### FIRST LINE  
**top - 19:38:28 up 2 days, 20:47, 0 users, load average: 0.52, 0.58, 0.59**

*top* - program name  
*19:38:28* - current hour  
*up 2 days, 20:47* - uptime( the time from last start of the system)  
*0 users* - number of active users  

`who` ->  to show who is logged in  

*load average*: 0.52, 0.58, 0.59 ->We see here three numbers. They are representing the load average for the system in last 1, 5 and 15 minutes. These shows the average number of processes running and waiting for CPU time.

`w` -> for the first line like in top

### SECOND LINE  
**Tasks: 6 total, 1 running, 5 sleeping, 0 stopped, 0 zombie -> information about processes in our system**

*total* - shows all processes in the system  
*running* - currently active processes. It means, these processes are using CPU right now  
*sleeping* - process is waiting for something. It may be I/O operation for example.  
*stopped* - Stopped processes (for example by ctrl+z)  
*zombie* -  It is a process which had finish his job but still has entry in the process table. In simple way, these processes are waiting for exit().  

### THIRD LINE  
**%Cpu(s): 13.9 us, 9.5 sy, 0.0 ni, 76.3 id, 0.0 wa, 0.4 hi, 0.0 si, 0.0 st**

 -> This line shows the CPU(s) utilization, splitted to specific types

*us* - user - All user processes are combined in this number. So, our sessions too.  
*sy* - system - processes owned by system (kernel)  
*ni* - nice -  allows us to change the priority of the process.   The standard value for processes is 0 , but we can modify it from 19 (lowest) to -20 (highest) priority.This statistic here shows all processes with the niceness set abow 0  
*id* - idle - idle time means that the system is bored and do nothing  
*wa* - iowait - the number represents the time (which is a subset of idle time) when the process is waiting for input/output operation  
*hi* - hardware interrupts. These are physical interrrupts from hardware and are handled by CPU itself.  
*si* - software interrupts. These are generated by software and are handled by kernel.  
*st* - steal time -This number represents the time "stealed" from the virtual machine by hypervisor. Another words, how long our system needs to wait for resources from hypervisor  

### Fourth and fifth lines  
**MiB Mem :  16217.5 total,   6184.9 free,   9808.7 used,    224.0 buff/cache**    
**MiB Swap:  49152.0 total,  48436.2 free,    715.8 used.   6278.3 avail Mem**  

buff/cache is a combine value of buffer memory, used by kernel and cache, memory by page cache.  

available simply means that the new starting program, application, etc can use max this size of memory for its to be run.

## PROCESSES LIST  
 PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                      
**652 root      20   0 1800688  43276  30296 S   0.3   2.1   0:00.62 containerd**

*PID* - Process ID number. It is unique number of the process in the system.  
*USER* - process' owner. The process is started by this user.  
*PR* - default priority of the process, scheduled by kernel when process was started.  
*NI* - nice. Shows the value, if nice was performed against the process.  
*VIRT* - total amount of memory used by the process.  
*RES* - RAM memory used by process.  
*SHR* - amount of memory shared with other processes.  
*S* - process state (we discussed it above).  
*%CPU* - what amount of available CPU is used by the process.  
*%MEM* - like for CPU, but this value represents memory usage.  
*TIME+* - total time of CPU usage by the process.  
*COMMAND* - quite obvious, this process is executed.  

How to modify the default view  
- 1 ->The CPU information in line 3 is expanded to single cores.  
We switch here between the unified view and detailed view.  
- M ->switch to order by memory usage  
- N-> the list sorted by PID  
- T ->order by running time  
- P ->order by CPU usage  

By default our processes list contains the tasks. We can switch it to threads by pressing *H* (and come back to processes by pressing the same key).  
By pressing *c* we can change between simple name of the command and the full path  
*v* and *V* shows the tree of processes, instead simple list  

u ->is used, we can select one user to list his processes.  
  ->we can search through processes. o is case-insensitive and O is case-sensitive.  
s->  we can change the refresh rate(default is 3 seconds)  
k-> press k and provide the PID of the process to kill  
w ->If many settings were changed and we wish to keep them for longer time  
  ->the settings will be written in the toprc file in the home directory  


## PS

ps -> Process status  
we can see these processes, which are related to this specific session only(we see the session itself, and processes executed at this exact moment by this session)  

- PID - this is the process id
- TTY - Terminal associated with the process
- TIME - total time of CPU usage
- CMD - the command which is running
- STAT - It means state of the process

**STATUSES**
- D - uninterruptible sleep (usually IO)  
- I - Idle kernel thread  
- R - running or runnable (on run queue)  
- S - interruptible sleep (waiting for an event to complete)  
- T - stopped by job control signal  
- t - stopped by debugger during the tracing  
- X - dead (should never be seen)  
- Z - defunct ("zombie") process, terminated but not reaped by its parent  

Some statuses may have the second letter. Let's list the most important  

- < - high-priority (not nice to other users)  
- N - low-priority (nice to other users)  
- s - is a session leader  
- l - is multi-threaded  
- '+' - is in the foreground process group  

`ps -A` ->list all processes  
`ps -ef` ->when someone wants to determine the PID of the process  
`ps aux` ->shows the most important info, like PID, status and resources usage    
to see the hierarchy of processes ->`ps aux --forest` OR `ps tree`  

## ALIASES  
Aliases give us a great possibility to "shape" command line to our needs  
alias lh='ls -alh' ->We create an alias called lh and when we invoke it, it should execute the ls -alh command. Now we execute lh.  
**!!this alias is available in this session only**  
`alias` ->shows all defined aliases in the session.  
`unalias lh` -> to remove aliases  

To make an alias permanent
```  
echo "alias lh='ls -alh'" >> .bashrc  
source ./.bashrc
```
## USERS

`whoami`->shows the user we are logged in  
`id` ->to learn this information, not only about current user, but any user in the system  
(he User ID (UID), primary group ID (GID) and other groups to which user belongs to)

`UID` ->an unique User Identifier, which identify this user through the system and determine which resources this user can access  
- 0 is reserved for root.  
- 1-99 are reserved for predefined accounts (like games, mail, www-data,sys,bin and more)  
- 100-999 reserved for system and administrative accounts.
1000+ are reserved for users  

`GID` ->as the same meaning, but for group. A group is an abstract which combines many users in similar entity (for the same privileges, purpose, actions, etc)  
- 0 is reserved for root.
- 1-99 are reserved for system and application use.
- 100+ are for users.

We have 4 files involved in configuration of users
1. passwd  
  ->It is located, like most of config files, in /etc

  The structure of this file is as follows:
  **username:password:UID:GID:description:homedir:shell**

`cat /etc/shells` -> to see the available shells

2. shadow  
This file contains sensitive information about the user, like password, and other configurations  
3. group  
  contains information about groups  
  The structure is as follows:  

  - group name  
  - password  
  - GID  
  - users belong to the group  

4. gshadow  
This file contains encrypted passwords for groups.  

which show us where the executable is located  
file gives us information about the object  

`adduser` uses useradd when creates user.aadduser is interactive.  
 It asks for password for user and other information  
 ```
useradd testuser1  
adduser testuser2  
```
password is not set for testuser1  
description is added for testuser2  
testuser1 has sh set and testuser2 - bash as shell.  
Well, testuser1 has the home directory defined, but it is not created.  

Let's go through most important arguments, which we should consider to use during create of new user.  
- -d - create home directory in specified location, if we want to change
- -m - create the home directory
- -p - password
- -s - provide shell
- -c - comments, or description of the account

Change the password  
```
grep testuser3 /etc/shadow
passwd testuser3
```
usermod testuser3 -g testuser2  
we modified the primary group for testuser3. Right now it is testuser2  
We can use GID also, not just name:

usermod testuser3 -g 1001  

usermod testuser3 -aG testuser1  
we added the secondary group 1001  

usermod -d we can create a home directory  
usermod testuser3 -d /home/newdir -m  

usermod testuser3 -s /bin/sh ->to modify the shell  

userdel testuser1-> to delete a user  
userdel -rf testuser3 ->In order to remove the files  

## History in command line  

`history`  
!6 ->executa a 6a comanda din history  
history 3 ->will show 3 last commands stored in history.  
history -c ->to clear history  
set +o history -> to disable history  
If we wish to disable history for current user:  

echo 'set +o history' >> ~/.bashrc  

And for all users in the system:  

echo 'set +o history' >> /etc/profile  

## Elevate privileges
When we log in to Linux we are "normal" user. With limited access to programs, directories, etc.  
Root is a master of the system. Root can do everything. Simply, it is an admin  
su ->to use another user  
su - student1  

Let's check student1 is sudo is configured for him.  
su - student1  
sudo ps  


student2 ALL=(ALL:ALL) NOPASSWD:ALL  ->sudo without password   

As mentioned, /var/log is the default location in all Linux systems for logs  
syslog->The main system log. Contains all important information about the system and applications.  
auth.log->Contains information about authorizations. All user login attempts (with information if successful or not), logout, password changes, remote logins and use of sudo  
dmesg->is a kernel ring buffer, not the log.It allows us to interact with kernel and get information by querying bootup messages.

kern.log->Stores Kernel messages  
boot.log-> information about started services, applications, disks configurations and so on.  
lastlog	-> Contains information abount last logins  
faillog	-> use this file with faillog utility. Logs fails, like login failures  
wtmp.log->Contains login infomration. However, it doesn't show information similar to lastlog, but used by other utilities, like who  
dpkg.log->Contains data about packages management - install, remove, update, etc  
cat /var/log/dpkg.log ->how to check a log  
logger "This is a test message" -> to push a message in log  

## STREAMS  
There is a command line with prompt waiting for interacting with you. This interaction is executed by two streams - output and input.

First stream is called standard input.In short, we use the name STDIN.Its file descriptor is 0.  
When STDIN is waiting for instructions passed to it, standard output displays responses from the system (in the simplest scenario).   

Its file descriptor is 1 and we name it STDOUT. 

The third stream is somehow special. It is standard error, with 2 as file descriptor. We call it STDERR.in this way we can differentiate the actions taken when output from our scripts are correct or there are some errors.

cat notexists.txt 2> errorfile ->to redirect an error  

/dev/null ->a virtual file to which we can write(we cannot read)- like a void  

cat notexists.txt 1> catfile 2> errorfile ->we directed STDOUT to catfile and if we had an error we write it to errorfile  

cat notexists.txt > capture.txt 2>&1 ->capture STDOUT and STDERR in the same file  
This uses the &> redirect instruction. This instruction allows you to tell the shell to make one stream got to the same destination as another stream  

## CRONTAB  
cron is a service responsible for control and execution of scheduled tasks. It is started when system is booting and works till the system is shut down.  
So, to conclude this part, cron is a service which controls multiple crontabs  

crontab is simply a list of tasks or commands which are scheduled to be executed on specific date and time.  
/etc/crontab -> the system wide crontab.all users can read, but only admins can edit this file.  
In this directory we have a few more directories.  
/etc/cron.daily  
/etc/cron.hourly  
/etc/cron.monthly  
/etc/cron.weekly  
These directories contain scripts which are executed... daily, hourly, monthly or weekly.  

We have four anacron command executed with different scheduling. An anacron is a scheduler too. Unlike the cron, anacron doesn't assume that the machine is run continuously without stops.  

Example:  
1 17 7 4 2 /usr/bin/ls >/dev/null 2>&1 ->a crontab  
-minute ->it is 1 minute after specified hour.  
-hour ->defines hour of execution(17)  
-day of the month(7)  
-month(apr)  
-weekday
in our example, system will execute the command at 17:1 on 7th of April. But only if it will be Tuesday.  
If we set day/month, we generally not set weekday. The same in oposite direction.  
If we want to keep every in any of position, we use *(So, if we set 15 * as two first elements, it means: execute 15 minutes after every hour)  
-user - we do not set user for user related crontabs, it doesn't make any sense, but for system wide crontab this possibility is valuable  
-command  

/var/spool/cron/crontabs ->In this directory user's crontabs are stored  
Each entry in crontab is called cronjob.  
crontab -l -> To list our jobs  
crontab -e -> opens crontab editor  

stat allows us to get diffeerent type of information than file  
With file we can learn what is the type of the file(like file, directory, package, device etc, and if file type is confirmed, it tries to understand the language)  

stat shows information, which we can clasify like more system-oriented  
- File ->the name of our file  
- Size  
- Blocks ->the size of the file but in blocks  
- IO Block ->size of each block  
- regular file -> the type of the object. We will not get detailed information about the language, only the type, like file, pipe, directory or special file  
- Device -> hex/decimal ID of the device on which object is located  
- Inode -> Inode ID. Inode and Device create the unique identification of the object.  
- Links -> number of hard links created.Every file will have 1, if there is no hard links created. 0 means the file is deleted  
- Access - the permission matrix for the object.  
- Uid and Gid - owner and owner's group identifiers  
- Access shows almost the time of last access to the file.this value will be updated only if is older than Modify timestamp. Modify says about last modification of the content. And Change is about all changes - content and attributes (like permissions, for example)  
- Birth is reserved for the time when file was created, but is not implemented in Linux systems  

stat -f mybashscript.sh -> -f gives us information about filesystem.  
Namelen inform us about the maximum filename leghth possible to set  

## SOFT AND HARD LINKS

Links ->shortcuts to original destination/file  
  ->pseudo-file  
you created a shortcut to file or directory in Windows system. When you open this shortcut, in fact you open the original location. In linux, when you open the link, the system sees it as object opened from the location where it resides, not the original (or source) location  

ln -> to create both types links  
`ln SOURCE TARGET`  ->to create hard links  
`ln -s SOURCE TARGET` ->to create soft links

exemple:  
lrwxrwxrwx 1 root root 15 May 15 11:20 softlink -> ../source/file1 (a soft link)  
-> l on the beginning of permissions matrix, which informs us clearly that it is the link   
->origin location, or redirection. However we weill call it, the fact is that this shows us the name of the link and where is the origin  

`unlink LINK` -> to delete a link  
This link is soft, what means in this case that link is a pointer to the source  
`rm softlink` ->to remove a soft link  


### Soft links
```
File <------soft link  
  |              |  
  |              |  
inode           inode  
  \  
   Storage  
```
Both files have their own inodes and we can say that inode of link is a shortcut to inode of original file. Only one inode points to the object on storage - original one  
When we remove the original file neither link itself or inode has understanding how to get to the storage.  

### Hard links
```
File       hard link  
   \          /  
       inode  
         |  
       Storage  
```
Both files - original and link, point to the same inode. And this inode points to the object on storage.  
when we remove the original file We remove just the file. Inode stays s long as anything is related to it 

- You can't create a hard link to a directory. It only applies to files  
- If an original file is deleted, the hard link still exists  
- A hard link remains intact when an original file is moved or renamed  
- A file exists until the last hard link is deleted  
- It is not possible to create a hard link between two file systems  
- The content change is reflected in all linked files  

## INODES

An inode is an index node. It serves as a unique identifier for a specific piece of metadata on a given filesystem, and the disk block location of this object  
To check inode of specific file, we use -i argument for ls command  

`blockdev --getbsz /dev/vda `->this way we determined the block size used on the filesystem  

## PERMISSIONS

example:
  - rwx r-x r--

On the beginning we have information about object's type  
- d on the first place of the matrix means directory  
- l in the same place means soft (symbolic) link  
- - means file  
- b - block special file (like hard drive)  
- n - network file  
- p - FIFO  
- s - socket  

These three blocks are related to specific user or users  
- Owner. This is the user who owns the file. Not necessairly the author, but current owner  
- Group. Every file in Linux can have the distinct permissions for one, selected group. By default, this group is set to the owner's group, but it can be changed  
- Others. All other users in the system  

Each group (owner, group and others) is build with three parts  
- r - means Read permission is granted  
- w - means Write permission is granted  
- x - means eXecute permission is granted  

example: drwx------ 2 root root 4096 May 2 10:20 .ssh  
that it is a directory (d) with full access for owner (rwx) and no access for the group and others (---)   

Two methods of setting permissions  
Absolute, or numeric mode uses digits to set permissions in each block. Each block is represented by one digit. Each digit has specific meaning  
- 0 - no access  
- 1 - execute  
- 2 - write  
- 3 - write + execute  
- 4 - read  
- 5 - read + execute  
- 6 - read + write  
- 7 - read + write + execute  

**!!Execute doesn't simply mean execute the file or program, etc. It also means execute the directory, what allows us to enter into the directory.**  

Symbolic mode allows us to work in more easier, more visual way. Here we use operator and the user denotation to define the permission matrix  
- '=' ->Set the whole block and override the previous permissions  
- '-' ->Remove the permission for specific user  
- '+' -Add the permission for specific user  

  - u - owner  
  - g - group  
  - o - other  
  - a- all  

if I want to make it executable for me, I use u+x.f I want to do it for everyone - a+x  

`chmod` -> to change permissions  
chmod permissions objects  

We mentioned earlier, that directory has different default permissions than files(drwxr-xr-x, what means that owner can do everything, but group and other cannot write anything in the directory)  

`chgrp <group> <objects>` ->  In order to change the group  
`chown <owner>:<group> <objects>` ->to change owner
