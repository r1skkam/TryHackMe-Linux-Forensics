[TryHackMe | Linux Forensics](https://tryhackme.com/room/linuxforensics)

# TryHackMe-Linux-Forensics
`Learn about the common forensic artifacts found in the file system of Linux Operating System`

## Task 1 Introduction
## Task 2 Linux Forensics
[TryHackMe | Linux Fundamentals Part 1](https://tryhackme.com/room/linuxfundamentalspart1)

[TryHackMe | Linux Fundamentals Part 2](https://tryhackme.com/room/linuxfundamentalspart2)

[TryHackMe | Linux Fundamentals Part 3](https://tryhackme.com/room/linuxfundamentalspart3)

## Task 3 OS and account information
### OS release information
[$ cat /etc/os-release](https://github.com/r1skkam/TryHackMe-Linux-Forensics/blob/main/os-release)

### User accounts
[$ cat /etc/passwd |column -t -s :](https://github.com/r1skkam/TryHackMe-Linux-Forensics/blob/main/passwd) 

### Group Information
[$ cat /etc/group](https://github.com/r1skkam/TryHackMe-Linux-Forensics/blob/main/group)

### just checking sudo list
[$ sudo -l](https://github.com/r1skkam/TryHackMe-Linux-Forensics/blob/main/sudo%20-l)

### Sudoers List
[$ sudo cat /etc/sudoers](https://github.com/r1skkam/TryHackMe-Linux-Forensics/blob/main/sudoers)

### Login information
[$ sudo last -f /var/log/wtmp](https://github.com/r1skkam/TryHackMe-Linux-Forensics/blob/main/wtmp)

### Authentication logs
[$ cat /var/log/auth.log |tail](https://github.com/r1skkam/TryHackMe-Linux-Forensics/blob/main/auth.log)

## Task 4 System Configuration
### Hostname
```
ubuntu@Linux4n6:~$ cat /etc/hostname      
Linux4n6
ubuntu@Linux4n6:~$ cat /etc/timezone
Asia/Karachi
ubuntu@Linux4n6:~$ netstat -antup |grep 127.0.0.1:5901
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 127.0.0.1:5901          0.0.0.0:*               LISTEN      927/Xtigervnc       
tcp        0      0 127.0.0.1:47680         127.0.0.1:5901          ESTABLISHED -                   
tcp        0      0 127.0.0.1:5901          127.0.0.1:47680         ESTABLISHED 927/Xtigervnc       
ubuntu@Linux4n6:~$ ps -aux |grep Xtigervnc
ubuntu       927  0.7  2.7 336980 109600 ?       S    20:29   0:07 /usr/bin/Xtigervnc :1 -desktop Linux4n6:1 (ubuntu) -auth /home/ubuntu/.Xauthority -geometry 1900x1200 -depth 24 -rfbwait 30000 -rfbauth /home/ubuntu/.vnc/passwd -rfbport 5901 -pn -localhost -SecurityTypes VncAuth
ubuntu      2162  0.0  0.0   3436   720 pts/0    S+   20:45   0:00 grep --color=auto Xtigervnc
ubuntu@Linux4n6:~$ 
```
## Task 5 Persistence mechanisms
```
ubuntu@Linux4n6:~$ cat .bashrc |grep HISTFILESIZE
# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTFILESIZE=2000
ubuntu@Linux4n6:~$ 
```
## Task 6 Evidence of Execution
```
ubuntu@Linux4n6:/home/tryhackme$ sudo su tryhackme
tryhackme@Linux4n6:~$ id
uid=1001(tryhackme) gid=1001(tryhackme) groups=1001(tryhackme)
tryhackme@Linux4n6:~$ apt-get install 
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
tryhackme@Linux4n6:~$ ll
total 20
drwxr-xr-x 2 tryhackme tryhackme 4096 Apr 17 21:04 ./
drwxr-xr-x 4 root      root      4096 Apr 16 20:15 ../
-rw------- 1 tryhackme tryhackme  191 Apr 17 21:04 .bash_history
-rw-r--r-- 1 tryhackme tryhackme 3771 Apr 16 20:15 .bashrc
-rw-r--r-- 1 tryhackme tryhackme  807 Apr 16 20:15 .profile
tryhackme@Linux4n6:~$ cat .bash_history 
ls -a /home/tryhackme/
cd ../tryhackme/
rm -rf .bash_logout 
history -w
ls -a /home/tryhackme/
cd ../tryhackme/
rm -rf .bash_logout 
history -w
cat .bash_history
sudo apt-get install apache2
tryhackme@Linux4n6:~$ 
```
