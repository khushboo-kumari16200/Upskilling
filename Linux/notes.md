## Important Link
https://notes.dhrona.net/p/li_mar_2023

## Linux Filesystem Security

<img width="772" alt="image" src="https://user-images.githubusercontent.com/106802147/227838789-6aac24d3-cc78-4a7a-b13a-5acd08ec1981.png">

<img width="828" alt="image" src="https://user-images.githubusercontent.com/106802147/227838959-07e8c7cf-ffe9-4736-a157-7a667887379c.png">

security enhanced linux - for security. 

Linux security models - when hooks created at process, network etc layers in linux.

<img width="722" alt="image" src="https://user-images.githubusercontent.com/106802147/227839877-f7722e48-4ac7-46de-8df3-6a97acc83a48.png">

UPG Scheme - user, private, group scheme
whenever you create user account, user by default will belongs to its private grp.
everyone would have his private space

`id`, `ls -l`, `cat /etc/passwd` - maintain user record but not password (c),  

`cat /etc/shadow` - store password, no access to anyone but password can by modified by passwd or accessed by sudo

How sudo or passwd elevate permission 
`which sudo` gives /usr/bin/sudo

`ls -l /usr/bin/sudo` -> ---s--x--x -> s is set uid bit, has elevated permission, allow users to launch program that run with elevated permission -> elevate permission when needed

`passwd` to change current user password

sudo chmod u+s /bin/cat -> now cat can access any file. but don't do this as it is dangerous
`id -G`, `id -Gn` - enumerate grp by name 

`cat > testfile.txt` - user ownership as chandra and grp also as chandra

`chown` - only root user can change ownership of file.

`chgrp wheel testfile.txt` -  can change grp if he is member of that grp

to switch the primary grp of logged in user. 
`newgrp wheel` - switch to grp wheel

`umask` - reverse permission applied 

`umask 077` - evoke everything from grp and others
`mkdir abc`, `ls -ld abc`

`chmod go+x .` - execute permission on directory is required to traverse into that directory

to create, delete, rename and change permission of file - you need write permission on folder.

ls -ld /tmp
t is at end -> sticky bit, world file accessible, /tmp is used by program and not by us,

chmod +t . -> enable sticky bit -> extended mode bit, to delete, you must be owner of file or folder, 

child process inherit parent process privilege like uid, gid 

root has uid=0 and is super user, check is disabled for root user, can access any system call(eg: change ownership)

`vi /etc/sudoers`

`man sudoers`

<img width="700" alt="image" src="https://user-images.githubusercontent.com/106802147/227848835-4756d8c6-1974-454f-8057-cad70c1c6bf0.png">

To manage ACL, `man getfacl`, `getfacl .`, for every file access, multiple rule check, so *slow*.  

