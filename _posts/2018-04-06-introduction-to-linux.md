---
layout: post
title: Introduction to Linux
date: 2018-04-06 14:34:59 +0800
categories: ['Linux']
tags: ['Linux']
disqus_identifier: 48067408406029811070672091991462348188
---

- TOC
{:toc}

### Multics/Unix
- Multics
    - an influential early **time-sharing** operating system
    - MIT, GE, **Bell Labs**
- Unix
    - Ken Thompson
        - The designer and implementer of the original Unix
        - Since 2006, Thompson has worked at Google, where he co-invented the **Go** programming language.
- C and Unix
    - Dennis Ritchie: the creator of the **C** programming language
    - In 1972, Unix was rewritten in the higher-level language C
    - Ritchie was found dead on October 12, 2011, at the age of 70 at his home in Berkeley Heights, New Jersey, where he lived alone. First news of his death came from his former colleague, **Rob Pike**. The cause and exact time of death have not been disclosed.

re: [https://en.wikipedia.org/wiki/Multics]( https://en.wikipedia.org/wiki/Multics)

re: [https://en.wikipedia.org/wiki/History_of_Unix](https://en.wikipedia.org/wiki/History_of_Unix)

re: [https://en.wikipedia.org/wiki/Ken_Thompson](https://en.wikipedia.org/wiki/Ken_Thompson)

re: [https://en.wikipedia.org/wiki/Dennis_Ritchie#C_and_Unix](https://en.wikipedia.org/wiki/Dennis_Ritchie#C_and_Unix)

### BSD & System Ⅴ & Mach

- BSD: Berkeley Software Distribution
    - A branch of Unix, Berkeley Unix
    - FreeBSD, OpenBSD, NetBSD
    - Darwin, macOS, iOS, watchOS, tvOS
- System Ⅴ
    - Solaris
- Mach <small>microkernel</small>
- Berkeley **socket**s
- Bill Joy & Chuck Haley
    - **vi** text editor
    - Sun Microsystems, Inc
    - **Java** programming language

<small>*Linux: I'm coming …*</small>

re: [https://en.wikipedia.org/wiki/Berkeley\_Software\_Distribution](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution)

re: [https://en.wikipedia.org/wiki/History\_of\_the\_Berkeley\_Software\_Distribution](https://en.wikipedia.org/wiki/History_of_the_Berkeley_Software_Distribution)
re: [https://en.wikipedia.org/wiki/UNIX\_System\_V](https://en.wikipedia.org/wiki/UNIX_System_V)

re: [https://en.wikipedia.org/wiki/Berkeley\_sockets](https://en.wikipedia.org/wiki/Berkeley_sockets)


### Unix philosophy

![Swiss Knife](/assets/intro-linux/swiss-knife.jpg)
![Thompson and Ritchie](/assets/intro-linux/thompson-ritchie.jpg)

- The Unix philosophy, originated by Ken Thompson, is a set of cultural norms and philosophical approaches to **minimalist**, **modular** software development.
- The Unix philosophy favors **composability** as opposed to **monolithic** design.
- Peter H. Salusin A Quarter-Century of Unix (1994):
    - Write programs that **do one thing and do it well**.
    - Write programs to **work together**.
    - Write programs to handle **text streams**, because that is a universal interface.

re: [https://en.wikipedia.org/wiki/Unix_philosophy](https://en.wikipedia.org/wiki/Unix_philosophy)

### GNU/Linux/Linux distribution

![Linux](/assets/intro-linux/gnu.png)
![Linux](/assets/intro-linux/linux.png)

- Debian <sub>apt deb</sub>
    - Ubuntu
- Fedora, Red Hat. <sub>yum rpm</sub>
    - Red Hat Enterprise Linux (RHEL)
        - CentOS
- Android

re: [https://en.wikipedia.org/wiki/Linux](https://en.wikipedia.org/wiki/Linux)

re: [https://en.wikipedia.org/wiki/Linux_distribution](https://en.wikipedia.org/wiki/Linux_distribution)

### Debian on VM

![Debian](/assets/intro-linux/debian.jpg)

#### Oracle VirtualBox/VMware Player

- Download VirtualBox:
    - [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- Download VMware Workstation Player:
    - [https://my.vmware.com/en/web/vmware/free#desktop_end_user_computing/vmware_workstation_player/12_0](https://my.vmware.com/en/web/vmware/free#desktop_end_user_computing/vmware_workstation_player/12_0)

- Getting Debian:
    - [https://www.debian.org/distrib/](https://www.debian.org/distrib/)
    - [https://www.debian.org/mirror/list](https://www.debian.org/mirror/list)

- Download CentOS:
    - [https://www.centos.org/download/](https://www.centos.org/download/)
    - [https://www.centos.org/download/mirrors/](https://www.centos.org/download/mirrors/)

#### APT: Advanced Package Tool

```sh
# apt-get update    # yum makecache
Hit:1 https://deb.nodesource.com/node_8.x stretch InRelease
Hit:2 https://download.docker.com/linux/debian stretch InRelease
...
Reading package lists... Done
# apt-cache search nginx    # yum search
nginx-small, powerful, scalable web/proxy server
# apt-cache madison nginx   # yum --showduplicates search nginx
nginx| 1.13.3-1~bpo9+1 | http://mirrors.163.com/debian stretch-backports/main amd64 Packages
nginx| 1.10.3-1+deb9u1 | http://mirrors.163.com/debian stretch/main amd64 Packages
# apt-get install nginx     # yum install nginx
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
...
```

re: [https://en.wikipedia.org/wiki/APT\_\(Debian\)](https://en.wikipedia.org/wiki/APT_\(Debian\))

#### Shell/SSH/Putty/Xshell

![Shell](/assets/intro-linux/shell.png)

```sh
$ touch hello.sh
$ cat <<EOF
> #!/bin/bash
>
> echo "Hello GNU/Linux!"
> EOF
#!/bin/bash
echo "Hello GNU/Linux!"
$ /bin/bash hello.sh
Hello GNU/Linux!
$ chmod+x hello.sh
$ ./hello.sh
Hello GNU/Linux!
$
```

re: [https://en.wikipedia.org/wiki/Unix\_shell](https://en.wikipedia.org/wiki/Unix_shell)

re: [https://en.wikipedia.org/wiki/Bash\_\(Unix\_shell\)](https://en.wikipedia.org/wiki/Bash_(Unix_shell))

re: [https://en.wikipedia.org/wiki/Secure_Shell](https://en.wikipedia.org/wiki/Secure_Shell)

#### Bash -GNU Bourne-Again SHell

- **Bash** is an **sh**-compatible command language interpreter that executes commands read from the standard input or from a file.
- Bash also incorporates useful features from the Kornand C shells (**ksh** and **csh**).
- A *login shell* is one whose first character of argument zero is a -, or one started with the --login option.
- An *interactive shell* is one started without non-option arguments (unless -s is specified) and without the -c option whose standard input and error are both connected to terminals (as determined by isatty(3)), or one started with the -i option. **PS1** is set and $- includes i if bash is interactive, allowing a shell script or a startup file to test this state.
- When you log in on a text console, or through SSH, or with su -, you get an *interactive login shell*.
- When you start a shell in a terminal in an existing session (screen, X terminal, Emacsterminal buffer, a shell inside another, etc.), you get an *interactive, non-login shell*.

**Bash Startup Files**

![Bash Startup Files](/assets/intro-linux/bash-startup-files.png)

re: [https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)

re: [https://unix.stackexchange.com/questions/38175/difference-between-login-shell-and-non-login-shell](https://unix.stackexchange.com/questions/38175/difference-between-login-shell-and-non-login-shell)

re: [https://askubuntu.com/questions/879364/differentiate-interactive-login-and-non-interactive-non-login-shell](https://askubuntu.com/questions/879364/differentiate-interactive-login-and-non-interactive-non-login-shell)

**Subshell**

- A *subshell* is a child process (fork-exec) launched by a shell (or shell script).
- The *source* command can be used to load any functions file into the current shell script or a command prompt.

```sh
$ cat hello.sh
#!/bin/bash
greeting="Hello GNU/Linux!"
$ ./hello.sh
$ echo $greeting
...
$ . ./hello.sh   # source ./hello.sh
$ echo $greeting
Hello GNU/Linux!
```

re: [https://en.wikipedia.org/wiki/Fork%E2%80%93exec](https://en.wikipedia.org/wiki/Fork%E2%80%93exec)

re: [https://en.wikibooks.org/wiki/A_Quick_Introduction_to_Unix/Shells_and_subshells](https://en.wikibooks.org/wiki/A_Quick_Introduction_to_Unix/Shells_and_subshells)

re: [http://www.tldp.org/LDP/abs/html/subshells.html](http://www.tldp.org/LDP/abs/html/subshells.html)

**ENVIRONMENT**

- When a program is invoked it is given an array of strings called the **environment**. This is a list of name-value pairs, of the form name=value.
- The shell provides several ways to manipulate the environment. On invocation, the shell scans its own environment and creates a parameter for each name found, automatically marking it for export to *child processes*. Executed commands inherit the environment. The **export** and **declare -x** commands allow parameters and functions to be added to and deleted from the environment. If the value of a parameter in the environment is modified, the new value becomes part of the environment, replacing the old. The environment inherited by any executed command consists of the shell's initial environment, whose values may be modified in the shell, less any pairs removed by the **unset** command, plus any additions via the export and declare -x commands.

```sh
$ greeting="Hello GNU/Linux!"
$ echo $greeting
Hello GNU/Linux!
$ vi hello.sh
$ cat hello.sh
#!/bin/bash
echo $greeting
$ ./hello.sh
$ exportgreeting
$ ./hello.sh
Hello GNU/Linux!
```
#### File/File System

- "On a UNIX system, everything is a file; if something is not a file, it is a process."
- Sorts of files
    - `-` Regular file
    - `d` Directory
    - `l` Link
    - `c` Character device
    - `s` Socket
    - `p` Named pipe
    - `b` Block device

```sh
# mkdirmydir
# touch myfile
# ln -sf myfilemylink
# ls -l
total 4
drwxr-xr-x 2 root root4096 Sep 10 17:10 mydir
-rw-r--r--1 root root0 Sep 10 17:10 myfile
lrwxrwxrwx1 root root6 Sep 10 17:10 mylink-> myfile
# ls -l /var/run/*.sock
srw-rw----1 root docker0 Sep 10 11:01 /var/run/docker.sock
# ls -l /dev/sda1
brw-rw----1 root disk 8, 1 Sep 10 11:01 /dev/sda1
# ls -l /dev/tty1
crw--w----1 root tty4, 1 Sep 10 11:01 /dev/tty1
```

```sh
/tmp$ df-h
Filesystem      Size    Used    Avail     Use%   Mounted on
udev            986M       0    986M        0%   /dev
tmpfs           200M    6.1M    194M        4%   /run
/dev/sda1        19G    9.9G    7.9G       56%   /
tmpfs           998M       0    998M        0%   /dev/shm
tmpfs           5.0M       0    5.0M        0%   /run/lock
tmpfs           998M       0    998M        0%   /sys/fs/cgroup
tmpfs           200M       0    200M        0%   /run/user/1000
/tmp$ df. -h
Filesystem      Size    Used    Avail     Use%   Mounted on
/dev/sda1        19G    9.9G    7.9G       56%   /
/tmp$ touch foobar
/tmp$ ls -lifoobar
262826 - rw-r--r--1 x x 0 Sep 11 21:01 foobar
```

```sh
/tmp# mkdir mydir   # make a directory
/tmp# cd mydir/     # change to mydir/
/tmp/mydir# pwd     # print the work directory
/tmp/mydir
/tmp/mydir# touch myfile    # make a file
/tmp/mydir# ls      # list the currentydirectory contents
myfile
/tmp/mydir# mv myfile myfile2   # rename myfileto myfile2
/tmp/mydir# rm myfile2          # remove myfile2
/tmp/mydir# cd ..               # change to parent dir
/tmp# rm -r mydir/              # remove mydir
/tmp# ls
/tmp#
/tmp# cat <<EOF > foo
> Hello GNU/Linux!
> EOF
/tmp# cat foo
Hello GNU/Linux!
/tmp# cp foo bar    # copy foo to bar
/tmp# ls
bar foo
/tmp# cat bar
Hello GNU/Linux!
/tmp# mkdir mydir
/tmp# mv foo bar mydir/     # move foo bar to mydir/
/tmp# ls
mydir
/tmp# cp -a mydir/ mydir2   # copy mydir/ to mydir2
/tmp# ls
mydir mydir2
```

```sh
/tmp# cat <<EOF > hello.sh
> #!/bin/bash
> echo "Hello GNU/Linux!"
> EOF
/tmp# ls -l
total 4
-rw-r--r-- 1 root root 36 Sep 10 17:34 hello.sh
/tmp# chmod +x hello.sh
/tmp# ls -l
total 4
-rwxr-xr-x 1 root root 36 Sep 10 17:34 hello.sh
/tmp# ./hello.sh
Hello GNU/Linux!
/tmp# stat -c "%a" hello.sh
755
/tmp# chmod 777 hello.sh
/tmp# ls -l
total 4
-rwxrwxrwx 1 root root 36 Sep 10 17:34 hello.sh
/tmp# tail -n 1 hello.sh
echo "Hello GNU/Linux!"
/tmp# head -n 1 hello.sh
#!/bin/bash
/tmp# cat hello.sh
#!/bin/bash
echo "Hello GNU/Linux!"
```

```
/bin    Common programs, shared by the system, the system administrator and the users.
/boot   The startup files and the kernel, vmlinuz.
/dev    Contains references to all the CPU peripheral hardware, which are represented as files with special properties.
/etc    Most important system configuration files are in /etc, this directory contains data similar to those in the Control Panel in Windows
/home   Home directories of the common users.
/lib    Library files, includes files for all kinds of programs needed by the system and the users.
/mnt    Standard mount point for external file systems, e.g. a CD-ROM or a digital camera.
/opt    Typically contains extra and third party software.
/proc   A virtual file system containing information about system resources.
/root   The administrative user's home directory.
/sbin   Programs for use by the system and the system administrator.
/tmp    Temporary space for use by the system, cleaned upon reboot, so don't use this for saving any work!
/usr    Programs, libraries, documentation etc. for all user-related programs.
/var    Storage for all variable files and temporary files created by.
```

re: [http://www.tldp.org/LDP/intro-linux/html/sect_03_01.html](http://www.tldp.org/LDP/intro-linux/html/sect_03_01.html)

re: [https://en.wikipedia.org/wiki/File_system](https://en.wikipedia.org/wiki/File_system)

#### Process

```sh
$ ps -f -C nginx
UID        PID  PPID  C STIME TTY          TIME CMD
root      3972  3957  1 16:18 ?        00:00:00 nginx: master process nginx -g daemon off;
systemd+  4012  3972  0 16:18 ?        00:00:00 nginx: worker process
```

```sh
$ ps aux|grep nginx
root      3972  0.3  0.2  32424  4872 ?        Ss   16:18   0:00 nginx: master process nginx -g daemon off;
systemd+  4012  0.0  0.1  32912  2492 ?        S    16:18   0:00 nginx: worker process
x         4026  0.0  0.0  12784   948 pts/2    S+   16:19   0:00 grep --color=auto nginx
```

```sh
$ ps -f -u www-data
UID        PID  PPID  C STIME TTY          TIME CMD
www-data  6631  6630  0 21:43 ?        00:00:00 nginx: worker process
www-data  6632  6630  0 21:43 ?        00:00:00 nginx: worker process
```

```sh
$ pstree -p
systemd(1)─┬─acpid(422)
           ├─agetty(435)
           ├─cron(354)
           ├─dbus-daemon(360)
           ├─dhclient(477)
           ├─irqbalance(358)
           ├─ntpd(446)───{ntpd}(455)
           ├─rsyslogd(359)─┬─{in:imklog}(403)
           │               ├─{in:imuxsock}(402)
           │               └─{rs:main Q:Reg}(404)
           ├─sshd(353)───sshd(1360)───sshd(1369)───bash(1370)───pstree(3168)
           ├─systemd(1362)───(sd-pam)(1363)
           ├─systemd-journal(197)
           ├─systemd-logind(417)
           └─systemd-udevd(211)
```

```sh
# pstree -p
init(1)─┬─acpid(2155)
        ├─auditd(2052)─┬─audispd(2054)───{audispd}(2055)
        │              └─{auditd}(2053)
        ├─dhclient(2001)
        ├─events/0(4)

        ├─klogd(2087)
        ├─ksoftirqd/0(3)
        ├─kthread(6)─┬─aio/0(245)
        │            ├─ata/0(498)

        ├─smartd(2338)
        ├─sshd(2244)───sshd(2395)───sshd(2402)───bash(2403)───su(2435)───bash(2436)───pstree(2519)
        ├─syslogd(2084)
        └─udevd(575)
```

#### User/Group

```sh
$ touch foo
$ sudo touch bar
$ ls -l
total 0
-rw-r--r-- 1 root root 0 Sep 11 21:57 bar
-rw-r--r-- 1 x    x    0 Sep 11 21:57 foo
$ chown x bar
chown: changing ownership of 'bar': Operation not permitted
$ sudo chown x bar
$ ls -l
total 0
-rw-r--r-- 1 x root 0 Sep 11 21:57 bar
-rw-r--r-- 1 x x    0 Sep 11 21:57 foo
$ sudo chgrp x bar
$ ls -l
total 0
-rw-r--r-- 1 x x 0 Sep 11 21:57 bar
-rw-r--r-- 1 x x 0 Sep 11 21:57 foo$
```

```sh
$ ps -f -C nginx,ps
UID        PID  PPID  C STIME TTY          TIME CMD
root      6630     1  0 21:43 ?        00:00:00 nginx: master process nginx
www-data  6631  6630  0 21:43 ?        00:00:00 nginx: worker process
www-data  6632  6630  0 21:43 ?        00:00:00 nginx: worker process
x         6872  3315  0 22:02 pts/1    00:00:00 ps -f -C nginx,ps
$ sudo touch foobar
[sudo] password for x:
$ echo 'Hello GNU/Linux!' > foobar
-bash: foobar: Permission denied
$ ls -l foobar
-rw-r--r-- 1 root root 0 Sep 12 13:56 foobar
$ su
Password:
# echo 'Hello GNU/Linux!' > foobar
# exit
exit
$ id
uid=1000(x) gid=1000(x) groups=1000(x),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),108(netdev),999(docker)
$ sudo grep '%sudo' /etc/sudoers
%sudo   ALL=(ALL:ALL) ALL$ 
```

```sh
~# cd ~xianyu
-su: cd: ~xianyu: No such file or directory
~# useradd -s /bin/bash -u 2000 -c "Hello GNU/Linux!" -m xianyu
~# cd ~xianyu/
/home/xianyu# grep 'xianyu' /etc/passwd
xianyu:x:2000:2000:Hello GNU/Linux!:/home/xianyu:/bin/bash
/home/xianyu# id xianyu
uid=2000(xianyu) gid=2000(xianyu) groups=2000(xianyu)
/home/xianyu# su - xianyu
~$ pwd
/home/xianyu
~$ exit
logout
/home/xianyu# cd
~# userdel -r xianyu
userdel: xianyu mail spool (/var/mail/xianyu) not found
~# cd ~xianyu
-su: cd: ~xianyu: No such file or directory
```

#### File System Permissions

```
Symbolic Notation   Numeric Notation    English
----------          0000                no permissions
-rwx------          0700                read, write, & execute only for owner
-rwxrwx---          0770                read, write, & execute for owner and group
-rwxrwxrwx          0777                read, write, & execute for owner, group and others SECURITY RISK
---x--x--x          0111                execute
--w--w--w-          0222                write
--wx-wx-wx          0333                write & execute
-r--r--r--          0444                read
-r-xr-xr-x          0555                read & execute
-rw-rw-rw-          0666                read & write
-rwxr-----          0740                owner can read, write, & execute; group can only read; others have no permissions
```

re: [https://en.wikipedia.org/wiki/File_system_permissions](https://en.wikipedia.org/wiki/File_system_permissions)

re: [https://en.wikipedia.org/wiki/Umask](https://en.wikipedia.org/wiki/Umask)

#### Signal <small>(IPC=Inter-process communication)</small>

```sh
$ kill -l
 1) SIGHUP          2) SIGINT(Ctrl-C)      3) SIGQUIT(Ctrl-\)    4) SIGILL            5) SIGTRAP
 6) SIGABRT         7) SIGBUS              8) SIGFPE             9) SIGKILL          10) SIGUSR1
11) SIGSEGV        12) SIGUSR2            13) SIGPIPE           14) SIGALRM          15) SIGTERM
16) SIGSTKFLT      17) SIGCHLD            18) SIGCONT           19) SIGSTOP(Ctrl-Z)  20) SIGTSTP
21) SIGTTIN        22) SIGTTOU            23) SIGURG            24) SIGXCPU          25) SIGXFSZ
26) SIGVTALRM      27) SIGPROF            28) SIGWINCH          29) SIGIO            30) SIGPWR
31) SIGSYS         34) SIGRTMIN           35) SIGRTMIN+1        36) SIGRTMIN+2       37) SIGRTMIN+3
38) SIGRTMIN+4     39) SIGRTMIN+5         40) SIGRTMIN+6        41) SIGRTMIN+7       42) SIGRTMIN+8
43) SIGRTMIN+9     44) SIGRTMIN+10        45) SIGRTMIN+11       46) SIGRTMIN+12      47) SIGRTMIN+13
48) SIGRTMIN+14    49) SIGRTMIN+15        50) SIGRTMAX-14       51) SIGRTMAX-13      52) SIGRTMAX-12
53) SIGRTMAX-11    54) SIGRTMAX-10        55) SIGRTMAX-9        56) SIGRTMAX-8       57) SIGRTMAX-7
58) SIGRTMAX-6     59) SIGRTMAX-5         60) SIGRTMAX-4        61) SIGRTMAX-3       62) SIGRTMAX-2
63) SIGRTMAX-1     64) SIGRTMAX 
```

#### Standard IO/Pipeline

```sh
$ ls -l /dev/std*
lrwxrwxrwx 1 root root 15 Sep 12 09:28 /dev/stderr -> /proc/self/fd/2
lrwxrwxrwx 1 root root 15 Sep 12 09:28 /dev/stdin -> /proc/self/fd/0
lrwxrwxrwx 1 root root 15 Sep 12 09:28 /dev/stdout -> /proc/self/fd/1
$ ls -l /proc/self/fd/{0,1,2}
lrwx------ 1 x x 64 Sep 12 20:15 /proc/self/fd/0 -> /dev/pts/0
lrwx------ 1 x x 64 Sep 12 20:15 /proc/self/fd/1 -> /dev/pts/0
lrwx------ 1 x x 64 Sep 12 20:15 /proc/self/fd/2 -> /dev/pts/0
```

```sh

$ cat hello.sh
#!/bin/bash
echo "Hello GNU/Linux!"
foo bar
$ ./hello.sh
Hello GNU/Linux!
./hello.sh: line 3: foo: command not found
$ ./hello.sh 2> err 1> out
$ cat err
./hello.sh: line 3: foo: command not found
$ cat out
Hello GNU/Linux!
$ ./hello.sh > allblue 2>&1
$ cat allblue
Hello GNU/Linux!
./hello.sh: line 3: foo: command not found
$ cat < out | xargs echo
Hello GNU/Linux!
```

re: [https://en.wikipedia.org/wiki/Standard\_streams](https://en.wikipedia.org/wiki/Standard_streams)

re: [https://en.wikipedia.org/wiki/Redirection\_\(computing\)](https://en.wikipedia.org/wiki/Redirection_(computing))

![Standard IO/Pipeline](/assets/intro-linux/io-pipeline.png)

- In Unix-like computer operating systems, a pipeline is a sequence of processes chained together by their standard streams, so that the output of each process (stdout) feeds directly as input (stdin) to the next one.
- The concept of pipelines was championed by **Douglas McIlroy** at Unix's ancestral home of **Bell Labs**, during the development of Unix, shaping its toolbox philosophy.
- Each process takes input from the previous process and produces output for the next process via standard streams. Each "\|" tells the shell to connect the standard output of the command on the left to the standard input of the command on the right by an **inter-process communication** mechanism called an (anonymous) pipe, implemented in the operating system. Pipes are **unidirectional**; data flows through the pipeline from left to right.

re: [https://en.wikipedia.org/wiki/Pipeline\_%28Unix%29](https://en.wikipedia.org/wiki/Pipeline_%28Unix%29)

```sh
$ man 1 man | less
$ git rebase –-help | more
```
#### Vim: God of the text editors / Emacs: a text editor of the Gods

- Two modes: **command mode** and **insert mode**

- Commands that switch the editor to insert mode
    ```
    a will append: it moves the cursor one position to the right before switching to insert mode
    i will insert
    o will insert a blank line under the current cursor position and move the cursor to that line.
    ```
- Moving through the text

    ```
    h to move the cursor to the left
    l to move it to the right
    k to move up
    j to move down
    ```
- Basic operations

    - `n dd` will delete n lines starting from the current cursor position.
    - `n dw` will delete n words at the right side of the cursor.
    - `x` will delete the character on which the cursor is positioned
    - `:n` moves to line n of the file.
    - `:w` will save (write) the file
    - `:q` will exit the editor.
    - `:q!` forces the exit when you want to quit a file containing unsaved changes.
    - `:wq` will save and exit
    - `:w newfile` will save the text to newfile.
    - `:wq!` overrides read-only permission (if you have the permission to override permissions, for instance when you are using the root account.
    - `/astring` will search the string in the file and position the cursor on the first match below its position.
    - `:1, $s/word/anotherword/g` will replace word with anotherword throughout the file.
    - `yy` will copy a block of text.

re: [https://vim.org](https://vim.org)

*~/.vimrc*

```vimrc
" Display line number
set number

" Disable VIM swap and backup
set nobackup
set nowritebackup
set noswapfile

" Indenting source code
set expandtab
set tabstop=4
set shiftwidth=4

autocmd FileType make setlocal noexpandtab
autocmd FileType js   set shiftwidth=2

" UTF-8
set encoding=utf-8
set fileencoding=utf-8
set fileencodings=ucs-bom,utf-8,chinese
set ambiwidth=double

" syntax
syntax on

set showmatch

" backspace

set backspace=indent,eol,start
```
