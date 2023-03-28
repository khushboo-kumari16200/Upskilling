## Important Link
https://notes.dhrona.net/p/li_mar_2023

Day 1 presentations: https://files.chandrashekar.info/li1.pdf

Day 2 presentations: https://files.chandrashekar.info/li2.pdf

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

sudo tail -f /var/log/audit/audit.log
<img width="1390" alt="image" src="https://user-images.githubusercontent.com/106802147/227868242-bae43749-da99-4ef7-b9cc-d2aeb4e6cad8.png">

loadpin, you can not load arbitrary 
<img width="914" alt="image" src="https://user-images.githubusercontent.com/106802147/227891528-7144986a-60dd-4154-8108-b2cb27f4c657.png">

<img width="804" alt="image" src="https://user-images.githubusercontent.com/106802147/227891902-45390e6d-5862-4076-85cd-6bf4f6626c1b.png">

<img width="713" alt="image" src="https://user-images.githubusercontent.com/106802147/227887306-7ff144ce-e015-43d5-b1da-3187b89dc4bc.png">

if you see something then, you have LSM
<img width="304" alt="image" src="https://user-images.githubusercontent.com/106802147/227892623-ada508e1-6dbc-43bc-9bb0-b5724c3a6b0e.png">

LIST OF LSM 

<img width="683" alt="image" src="https://user-images.githubusercontent.com/106802147/227892818-99c6d2fe-8c3d-44b1-a69b-2f5584ca7600.png">
appmaror is turned off on instructor system

<img width="476" alt="image" src="https://user-images.githubusercontent.com/106802147/227893809-0ac4a7fc-ab87-4ab4-bb07-9f306aa057b1.png">

<img width="709" alt="image" src="https://user-images.githubusercontent.com/106802147/227894563-a9745086-6ee0-44cf-ab3d-5bd9f73aeda4.png">

when package is installed, multiple files are kept in compartments 

confined and unconfined 

unconfined runs without restriction, 


<img width="684" alt="image" src="https://user-images.githubusercontent.com/106802147/227897231-f8808130-31aa-4a33-b0fb-632130ae60d9.png">

<img width="757" alt="image" src="https://user-images.githubusercontent.com/106802147/227897757-68e50e25-7430-44d5-82cf-2df323e15ece.png">

<img width="761" alt="image" src="https://user-images.githubusercontent.com/106802147/227898618-f55abf4d-764e-4aa0-be46-1db7c069963b.png">

<img width="749" alt="image" src="https://user-images.githubusercontent.com/106802147/227900196-44d87918-d425-419b-a203-a194214d1324.png">


<img width="751" alt="image" src="https://user-images.githubusercontent.com/106802147/227899956-14295660-8f11-45af-bd08-e9af2e04dfae.png">

selinux user

<img width="697" alt="image" src="https://user-images.githubusercontent.com/106802147/227900703-d6477fe1-56d1-4939-b9ee-80d0ad275786.png">

<img width="702" alt="image" src="https://user-images.githubusercontent.com/106802147/227900772-114c15a0-436f-4564-81f1-83df957e8b08.png">

`ls -lZ` - context: user role type msl level

<img width="868" alt="image" src="https://user-images.githubusercontent.com/106802147/227901283-d11024b5-3f8a-4154-b515-4db5bb202917.png">

<img width="979" alt="image" src="https://user-images.githubusercontent.com/106802147/227901767-8a5d4233-99f2-406a-a741-df2e469300a6.png">

<img width="829" alt="image" src="https://user-images.githubusercontent.com/106802147/227906903-b0a75b7a-63c0-4c4b-92e9-6ebc21ad265b.png">

Smack LSM

<img width="744" alt="image" src="https://user-images.githubusercontent.com/106802147/227907308-37de41bc-5ff5-475a-98ca-0a69f25ac28d.png">

WHEN WE get access vector cache error, how to setup new policy

<img width="749" alt="image" src="https://user-images.githubusercontent.com/106802147/227908176-4c4f48d3-b57c-4494-9689-9f6013e50625.png">

<img width="1237" alt="image" src="https://user-images.githubusercontent.com/106802147/227913873-3c641aa3-f3f9-4c51-972c-412c8926abd2.png">

<img width="743" alt="image" src="https://user-images.githubusercontent.com/106802147/227914247-ca98631d-f03f-42be-a143-1e6d706c8412.png">

<img width="739" alt="image" src="https://user-images.githubusercontent.com/106802147/227914925-b38e9062-e92f-4c85-acc6-5eab702e04dc.png">


`strings /bin/ls` -> extract string from binary

`strings /dev/mem`

<img width="608" alt="image" src="https://user-images.githubusercontent.com/106802147/227917056-bf6f63de-f6e3-4326-9db4-6cbbc8d60c00.png">

once lockdown enabled, no going back (but try system restart)

<img width="574" alt="image" src="https://user-images.githubusercontent.com/106802147/227918293-287d95fe-3e75-4889-8dcf-18e41e9c76c5.png">

`lsmod` - list loaded module

<img width="682" alt="image" src="https://user-images.githubusercontent.com/106802147/227919059-2a50f68f-4b4c-4e43-b738-e627893f2161.png">

untrusted module can not be loaded when confidentiality is enabled.

<img width="701" alt="image" src="https://user-images.githubusercontent.com/106802147/227919189-4c748f14-ed19-44a8-9981-21f12adcddf2.png">

<img width="672" alt="image" src="https://user-images.githubusercontent.com/106802147/227919871-bb8b0c5b-72f6-45be-9a87-c2210bf4c532.png">

safely switch to user account, as switched user account can access everything that this had access too.

<img width="680" alt="image" src="https://user-images.githubusercontent.com/106802147/227921256-ddbd977c-9b73-43d3-9d13-e029fa26e572.png">


<img width="702" alt="image" src="https://user-images.githubusercontent.com/106802147/227921914-a595420c-54d8-46fb-8ba1-14cd8fac53ef.png">

`ifconfig`

<img width="528" alt="image" src="https://user-images.githubusercontent.com/106802147/227922712-a171350c-c810-4b74-b0dc-08fa029c37e5.png">

`ip addr show`

<img width="637" alt="image" src="https://user-images.githubusercontent.com/106802147/227923084-1055efbe-8e1a-4a4c-bc92-a4889e8a77ee.png">

add ip
<img width="574" alt="image" src="https://user-images.githubusercontent.com/106802147/227918293-287d95fe-3e75-4889-8dcf-18e41e9c76c5.png">

`ip route` 

`ip neigh`

below layer 3 
`ip link`

IProute2 commands cheatsheet: https://www.baturin.org/docs/iproute2/

`ss` - socket

tcp socket
`ss -t`

<img width="1097" alt="image" src="https://user-images.githubusercontent.com/106802147/227924378-dc540f48-3d19-4a5f-bb04-34d5e84c4d26.png">

<img width="726" alt="image" src="https://user-images.githubusercontent.com/106802147/227924593-54107360-d84c-4e5f-8410-716dfd621380.png">

## Day 2

<img width="675" alt="image" src="https://user-images.githubusercontent.com/106802147/228125311-51dd55e9-585c-4794-b0cc-7e30504aec32.png">

<img width="680" alt="image" src="https://user-images.githubusercontent.com/106802147/228125466-46cfe423-71b1-4b78-a548-554ea47fcdcf.png">

task structure in 

<img width="517" alt="image" src="https://user-images.githubusercontent.com/106802147/228125936-250035c5-fd30-4b53-a5c5-c0c71b502c7c.png">

important register details like program counter, base pointer etc are stored in thread_info, used while context switch

<img width="525" alt="image" src="https://user-images.githubusercontent.com/106802147/228126152-1fd8659b-95da-4b5e-97bb-823b7b435ede.png">

target is informed to run signal handler 
<img width="797" alt="image" src="https://user-images.githubusercontent.com/106802147/228126559-a93c6931-58c7-4022-bc8f-774dd6002793.png">

kernel maintian different stack for every task(process and thread)
<img width="672" alt="image" src="https://user-images.githubusercontent.com/106802147/228126820-33acea05-c1b1-40c7-93ad-fdb977315549.png">

single page - 4k 
kernel stack - various func in kernel executed using this
on_cpu =1 if executing on cpu.

0 to 99 - for real time process, 0 - low priority

-20 -> higher priority 

real time 0 is higher than -20 priority 


<img width="514" alt="image" src="https://user-images.githubusercontent.com/106802147/228127677-fca0b45a-3c62-4611-a5f6-78308a7d5ace.png">

different scheduling method - policy
<img width="516" alt="image" src="https://user-images.githubusercontent.com/106802147/228127787-66614bf9-5c37-4cc5-83a2-6633794d9931.png">

which cpus can execute the process 

mm -> process memory structure maintained by this
<img width="507" alt="image" src="https://user-images.githubusercontent.com/106802147/228128093-ab1756de-95c5-4d6b-9b25-69358c84a1ee.png">

kernel thread does not have user space-> for kernel thread mm pointer will be null.

when context switch-> active_mm , kthread active mm will point to user space mm, used for lazy tlb handling

<img width="724" alt="image" src="https://user-images.githubusercontent.com/106802147/228128524-1a4b357b-0b26-4eb6-8483-dc655b16306e.png">

<img width="710" alt="image" src="https://user-images.githubusercontent.com/106802147/228128638-47fc98e1-a980-4063-a51d-97a7a2f46d53.png">

when process exists, parent process is notified by kernel. 

when process is traced, tracer becomes parent process. 

otherwise parent process is same as current process

<img width="725" alt="image" src="https://user-images.githubusercontent.com/106802147/228128962-219e340a-f5b4-4ac6-97d5-602b7db9e8b7.png">

<img width="1023" alt="image" src="https://user-images.githubusercontent.com/106802147/228129248-25d58026-55b5-4cb0-9745-354e08ed658e.png">

`cat /proc/2872/status` 

<img width="801" alt="image" src="https://user-images.githubusercontent.com/106802147/228129927-835543df-4dbd-4ac8-a0db-cb39c282bad4.png">

execv syscall -> load process

first process pid-1 is responsible for managing user space

<img width="851" alt="image" src="https://user-images.githubusercontent.com/106802147/228130490-78a2343b-185a-470a-9fd9-81782c5e0250.png">

<img width="737" alt="image" src="https://user-images.githubusercontent.com/106802147/228130910-abd193e6-cb01-45c2-97a7-6d7034469479.png">

<img width="545" alt="image" src="https://user-images.githubusercontent.com/106802147/228131219-900e7965-318b-484e-bc91-ec45c4232abd.png">


one way of multi-tasking -> when it create 2 diff process, they run in diff address space

multi-threading  

Native Threading: Kernel manages the threads of the user-space process (a.k.a Kernel-supported-Threading / Light-Weight-Processes -> LWPs). Get benefits of preemptive scheduling. Good for CPU intensive work

User Threading: Threading implementation fully in userspace (governed by a library) - no kernel intervention is required. Relies more on co-operative scheduling. not good for cpu intensive work, good for IO intensive work. Ex: socket programming-> 1000 connections.


early generation('90s) pthread library is user threading library

NPTL -> Native POSIX Threading Library -> pthread library that uses native threading. 

<img width="915" alt="image" src="https://user-images.githubusercontent.com/106802147/228133920-12aeeaec-f05d-4a8b-8dad-444638253b71.png">

THIS Is ntpl way?
<img width="547" alt="image" src="https://user-images.githubusercontent.com/106802147/228134239-8fc5f68b-b8bd-49f5-8d90-b928a458ba0f.png">

<img width="1025" alt="image" src="https://user-images.githubusercontent.com/106802147/228134415-2212b8a1-3f45-4259-8ef6-5b7a3febbd29.png">

deep copy-> all inner structure are copied. every task will have their own inner structure. -> process
 
shallow copy -> share file structure, mm, signal handle, mount etc. -> thread

<img width="792" alt="image" src="https://user-images.githubusercontent.com/106802147/228135082-ed9bf5f3-904b-4348-8727-e7f2237df2ec.png">

<img width="830" alt="image" src="https://user-images.githubusercontent.com/106802147/228135273-8abb86c9-5f18-4527-b951-e438544b8645.png">

user space virtual map structure

pgd_t -> page dir structure -> points to page table, used in virtual to physical address translation

<img width="717" alt="image" src="https://user-images.githubusercontent.com/106802147/228135868-49e37c7b-9d2c-4101-99fd-bc599efd21f7.png">

<img width="826" alt="image" src="https://user-images.githubusercontent.com/106802147/228135296-5ff2c81f-ae19-42ea-b759-e2d329c11a67.png">

<img width="725" alt="image" src="https://user-images.githubusercontent.com/106802147/228136346-3f52fe64-ce2f-40cd-8ccb-86cbf50dc18c.png">

<img width="666" alt="image" src="https://user-images.githubusercontent.com/106802147/228136434-05a47f5a-331d-4f0d-89da-cfed3657ff61.png">


<img width="831" alt="image" src="https://user-images.githubusercontent.com/106802147/228136850-b116e899-4233-45b9-85fa-1ff9bade55fb.png">

<img width="909" alt="image" src="https://user-images.githubusercontent.com/106802147/228137742-722239e3-f416-45aa-bfb0-9ab2a51a5ae2.png">

<img width="733" alt="image" src="https://user-images.githubusercontent.com/106802147/228137963-0702a0f4-d001-4f22-8cea-40bbfaf5593d.png">

<img width="588" alt="image" src="https://user-images.githubusercontent.com/106802147/228142907-cc007cf0-6ac0-4b1f-b9d5-6bbb32968466.png">

if kernel stack is empty, it is not using any syscall, just running in user mode.

<img width="811" alt="image" src="https://user-images.githubusercontent.com/106802147/228143530-297417dd-fe31-4eec-9bf2-ce2f52b5b99d.png">

`gdb -p pid`

`ps x` -> if it has t+ -> means it has been traced

<img width="839" alt="image" src="https://user-images.githubusercontent.com/106802147/228145489-bb08032b-ec6c-4226-9280-4fee08e36ecc.png">

if it is 0, no body is tracing it

<img width="722" alt="image" src="https://user-images.githubusercontent.com/106802147/228146104-89d69eac-7676-4487-9527-4481577982f8.png">

i-> idle 

it task is in s->

<img width="1042" alt="image" src="https://user-images.githubusercontent.com/106802147/228147928-139e4f86-144a-4def-93ee-9299bbbb2579.png">

<img width="755" alt="image" src="https://user-images.githubusercontent.com/106802147/228151458-b1bdad23-18f1-4cad-b193-9fbfa7cddd18.png">

no of faults by process
<img width="734" alt="image" src="https://user-images.githubusercontent.com/106802147/228152021-a8815b7c-2eb5-4b1b-a471-f9e59dba95bc.png">

<img width="729" alt="image" src="https://user-images.githubusercontent.com/106802147/228153359-7b190741-47bf-4ab0-b8e9-2878ce70d806.png">

<img width="587" alt="image" src="https://user-images.githubusercontent.com/106802147/228154435-617e828d-2d8b-496d-8915-626bf07993c2.png">



<img width="722" alt="image" src="https://user-images.githubusercontent.com/106802147/228146104-89d69eac-7676-4487-9527-4481577982f8.png">
