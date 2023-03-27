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

<img width="701" alt="image" src="https://user-images.githubusercontent.com/106802147/227849569-0868ee07-105c-4ce0-b285-26ce6705a638.png">

## Extended attribute
<img width="383" alt="image" src="https://user-images.githubusercontent.com/106802147/227850332-adfdfc86-dc00-4b94-9575-a9f8b2d8f23a.png">

`man chattr`

`lsattr`. 
`chattr +a a.txt`. -> append only bit
with even as root user, i can not overwrite, but modify. log rottater will flip this bit, rotate and chnage back the bit, 

`chattr +i a.txt` -> immutability  
`chattr -R +i dir1` 

<img width="701" alt="image" src="https://user-images.githubusercontent.com/106802147/227851659-82e73591-2748-4e9c-ac0d-4c941cbdc15d.png">

`map 7 capabilities`

<img width="712" alt="image" src="https://user-images.githubusercontent.com/106802147/227855039-9aa1cebb-4af9-4f10-a892-afbd189c9dd5.png">

<img width="754" alt="image" src="https://user-images.githubusercontent.com/106802147/227855595-3ba2f2ec-8bd6-4edd-bf31-d2905c9ef305.png">

Bounding -> subset of permitted capabilities.

<img width="337" alt="image" src="https://user-images.githubusercontent.com/106802147/227857961-65417584-552f-44c9-8d2f-49072bdb9188.png">

<img width="458" alt="image" src="https://user-images.githubusercontent.com/106802147/227858520-4ae653d2-b546-4b27-9c35-78162aba6b73.png">

<img width="634" alt="image" src="https://user-images.githubusercontent.com/106802147/227858907-29c915fc-1ad6-4105-89e1-ba62be53de5c.png">

Apply on program

<img width="705" alt="image" src="https://user-images.githubusercontent.com/106802147/227859863-c75b0676-d426-4fb4-91f0-3a87f3826f62.png">

<img width="758" alt="image" src="https://user-images.githubusercontent.com/106802147/227859684-5f814fa6-53c7-466f-a6e3-4750df5a97f1.png">

setcap - set file capibilities

<img width="650" alt="image" src="https://user-images.githubusercontent.com/106802147/227860424-bde90473-0501-4a30-b33d-9b4504734740.png">

<img width="756" alt="image" src="https://user-images.githubusercontent.com/106802147/227860602-fc264573-d6f5-450f-b0c9-a84889bff654.png">


`pscab` - capabilities of process

`getcap` for program

<img width="693" alt="image" src="https://user-images.githubusercontent.com/106802147/227862293-6cc8f738-8d56-4f5f-939a-4bce1eff158b.png">

kernel space audit thread
<img width="526" alt="image" src="https://user-images.githubusercontent.com/106802147/227862673-2b55532e-972b-492b-a94d-ce9e92b94078.png">

syslog is for user space, various kernel subsytem uses kauditd

`dpkg -d audit` should have entry for audit to work

<img width="703" alt="image" src="https://user-images.githubusercontent.com/106802147/227863467-73b92bac-98da-4722-8f5d-49ccc2c5662f.png">

<img width="723" alt="image" src="https://user-images.githubusercontent.com/106802147/227863494-067791ad-0a21-4fdf-9614-37ba34017100.png">

how to know if kernel is built with particular configuration like CONFIG_AUDIT: 
sudo modprobe configs



how to know if kernel is built with particular configuration like CONFIG_AUDIT: 

`dpkg -d audit` should have entry for audit to work

<img width="724" alt="image" src="https://user-images.githubusercontent.com/106802147/227864842-0a343092-b625-4502-b923-7c3931889a11.png">

<img width="870" alt="image" src="https://user-images.githubusercontent.com/106802147/227864616-edab2144-c953-4146-9084-208a437383d1.png">

for redhat based
<img width="650" alt="image" src="https://user-images.githubusercontent.com/106802147/227864956-3964b165-3035-427a-82b0-8f8e9b9a3ff7.png">

Ubuntu
<img width="531" alt="image" src="https://user-images.githubusercontent.com/106802147/227865298-2fcc2c15-e1cc-49d8-8d0f-36a641c4e02f.png">

<img width="673" alt="image" src="https://user-images.githubusercontent.com/106802147/227865863-1fae4413-2e89-4b34-a6a7-2d57a45eb34a.png">

`sudo aureport`

<img width="969" alt="image" src="https://user-images.githubusercontent.com/106802147/227866931-67048f73-e073-4bb3-8709-92ebcd4f7c01.png">

watch on file using -w  
`sudo auditctl -h`
<img width="673" alt="image" src="https://user-images.githubusercontent.com/106802147/227867717-aa1aa99c-1915-4f46-8d67-ab2880968265.png">

sudo tail -f /etc/audit/audit.log
<img width="1390" alt="image" src="https://user-images.githubusercontent.com/106802147/227868242-bae43749-da99-4ef7-b9cc-d2aeb4e6cad8.png">
