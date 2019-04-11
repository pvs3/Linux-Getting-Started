# Linux-Getting-Started

TOC:

## 1 Basic commands

- 1.1 ls
- 1.2 cd
- 1.3 pwd
- 1.4 mkdir
- 1.5 cp
- 1.6 rm
- cat
- grep
- which
- echo
- cut
- sort
- type
- stat
- tee
- uname
- id
- who
- chmod
- umask
- setfacl
- getfacl

**cd** : Change dir

```
cd
-> returns to 'home dir' eg: for user 'penguin' = /home/penguin

cd ..
-> moves one dir UP
```

**pwd** : Print (actual)working dir = 'where am I now ?'

```
pwd
/home/penguin
```

**mkdir** : Make dir

```
mkdir -p /home/penguin/AWS/BigData/Mobile
-> -p = make all 'parent dirs if they do not exist !
```

**grep** Global Regular Expression Print

**umask**

Dicates the default file permission set.

How ?

- umask = 0022 (default)
- permissions = 0666 -umask = 0644 (rx-|r--|r--)

### 1.1

### 1.2

### 1.3

## 2 Users

### 2.1 Password file

- file location = /etc/passwd
- stored in ETSI format = 7 fields

<img src="/images/passwd-file.png" width="800px">

example:

<img src="/images/get_passwd_details4user.png" width="800px">

### 2.2 Adding users

```
useradd penguin -m -s /bin/bash -g users
-> -m = create home dir for this user (/home/penguin)
-> -s = default bash for this user
-> -g = belongs to group
```

**Remark:** you have to have permission in order to create users , usually 'root'.

## 3 Files

**Everything is a file in linux !**

### 3.1 Filesystem hierarchy

<img src="images/linux_file_system.png" width="800px">

    - '/' = root dir

    - '/root'   = home dir for root user -> ! not in /home
    - '/bin'    = dir for programs
    - '/boot'   = files required for starting your system -> 'DO NOT TOUCH !!'
    - '/dev'    = contains device files
                    - null  = 'black hole' eg:as stdout for files - /dev/null
                    - tty   = 'teletype' = your terminal window
                    - ttyS0 = serial port (physical device)
                    - pts   = 'pseudo terminal slave' = remote connections eg SSH (no physical device)

                    **remark**
                    - c = 'Character device' -> eg see tty
                    - b = 'block device' = acts as a disk
    - '/etc'    = Everything To Configure -> contains mainly configuration files (eg passwd file, ...)
    - '/home'   = dir for working dir for users. eg: /home/penguin or /home/user2
    - '/lib'    = Libraries
    - '/lib64'  = 64 - bit libraries
    - '/media'  = external plugable media (mainly storage) eg USB drives
    - '/mnt'     = mount = for manualy mounting drives. Today not used very much (? except Dockert ?)
    - '/opt'    = for installation of add- software.
                    **remark: ** there is a subtile difference between /opt and /usr/local
    - '/proc'   = virtual dir - created every launch- with info about your computer: CPU, kernel, etc
    - '/run'    = (new dir) where system processes write temp data -> DO NOT TOUCH !!
    - '/sbin'   = dir for 'superuser' programs (eg fdisk)
    - '/usr'    = was home dir for users in the 'early days'. Now mish-mash of directory with: apps,libs,docs,wallpaper,icons, etc
    - '/srv'    = dir for servers. eg: /srv/www (with html for your site) or /srv/ftp
    - '/sys'    = virtual dir - contains info about connected devices -> DO NOT TOUCH !!
    - '/tmp'    = tempory dir mainly used by runnings applications
    - '/var'    = mainly used for log files

**Remark : ** - '~' = short for home dir, for user 'penguin' this is: /home/penguin

<img src="/images/standard-unixfilesystem-hierarchy.png" width="800px">

### 3.2 File security

    - first char = file type
    - permission Classes (user, group, Other users)
        - read / write / execute (r/w/x)
        - ther is a subtile difference between 'Other' and 'All -> see Chmod 3.4
    ```
                |  r w x | r w x | r w x
    file type   |  user  | group | Other
        x       |  x x x | x x x | x x x
    ```

example:

<img src="/images/file_example.png" width="800px">

```
                |  r w x | r w x | r w x
    file type   |  user  | group | Other
        -       |  1 1 0 | 1 0 0 | 1 0 0
        -       |    6   |   4   |   4    == chmod 644 == user can read/write and rest can read
```

**remark: ** | 1 1 1 | == 7 => chmod 777 set read/write/execute for user/group/all = max privilage <br> Also called: **symbolic notation** versus **numeric notation**

<img src="/images/file_example_chmod777.png" width="800px">

**first char**

```
* - : regular file
* d : directory
* l : symbilic link
* c : device file of type character (see /dev/tty)
* b : device file of type block (see /dev/sd)
* s : socket
* p : pipe
* D : Door (only on Solaris)
```

### 3.3 Links

2 types:

- 'ln' hardlinks -> They share **Inode** number (? disk-file partition)
- 'ln -s' symbolic links -> They ** DO NOT** share Inode numbers (have different Inode nr)

**Remarks :**

Sharing or not sharing same Inode has major consequences: rem: 'ls -i' gives Inode number

_HardLinks_

- links are 'pointers' to the same Inode
  - file type = '-'
  - Thus if file to which the link refered is deleted -> link stays intact because Inode nr stiil exists
  - if Chmod on file -> this is on Inode -> thus for ALL links

<img src="/images/hard_link.png" width="800px">

_Symbolic Links_

- links point to **different** Inode
  - file type = 'l' = link
  - it only references the absolute path name
  - Thus if file to which the link refered is deleted -> = destroyed -> abs path point to nothing
  - permission of link is always 777 -> Permission of 'destination file' determines what can!!

<img src="/images/symbolic_link.png" width="800px">

### 3.4 Chmod - changing permissions

### 3.4.1 Changing permissions on files

Intro: Difference for files and directories !

    -> for files r w x = evident
    -> for dir:
        - r = you can ls dir
        - w = you can 'cp' files into dir
        - x = you can 'cd' into dir

Remark: You can not have execute -x permission without read -r permission

See 'man chmod'

**Format symbolic mode:** <br>

- ugoa
  - u = user
  - g = group
  - o = other
  - a = all = u+G+o !!! (see remark 3.2 subtile difference)

```
chmod u=rwx myfile.txt          -> sets rwx to user
chmod u-x   myfile.txt          -> prohibits x (execution) for user
chmod u+x   myScript            -> adds x permission tot user
```

<img src="/images/chmod_symbolic.png" width="800px">

**Format numeric mode:** <br>

rwx = \_ \_ \_ in binary

eg:

    - 000 == ---|---|---
    - 400 == 100|000|000 == r--|---|---
    - 644 == 110|100|100 == rw-|r--|r--
    - 775 == 111|111|101 == rwx|rwx|r-x
    - 777 == 111|111|111 == rwx|rwx|rwx

```
chmod u=rwx myfile.txt          -> sets rwx to user
chmod u-x   myfile.txt          -> prohibits x (execution) for user
chmod u+x   myScript            -> adds x permission tot user
```

<img src="/images/chmod_symbolic.png" width="800px">

### 3.4.2 Changing permissions on directories

First line specifies permissions of the actual dir (see 'dot' at the end)

<img src="/images/dir_permissions.png" width="800px">

```
chmod 755 .         -> sets the permissions as above on the actual directory
```

### 3.4.3 ACL

Remark: if cmd not found -> run 'apt-get install acl'

ACL = Access List

-> if applied we see a '.' at the end of the permissions <br> -> enables us to give specific permissions to users

**getfacl**

<img src="/images/getfacl.png" width="800px">

**setfacl**

<img src="/images/setfacl.png" width="800px">
```
setfacl --modify u:pengiun:777 mysecretFile.txt
```
<img src="/images/getfacl.png" width="800px">

Now we see a '+' at the end of the permissions indicating one or more additional 'acl'

<img src="/images/ls_acl.png" width="800px">
