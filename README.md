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
```
ubuntu@Linux4n6:~$ cat /var/log/auth.log.1 |grep net-tools
Apr 17 15:54:52 tryhackme sudo:   ubuntu : TTY=pts/0 ; PWD=/home/ubuntu ; USER=root ; COMMAND=/usr/bin/apt-get install net-tools
ubuntu@Linux4n6:~$
```
## Task 7 Log files
```
ubuntu@Linux4n6:~$ cat /var/log/syslog* |grep hostname
Jun  1 20:30:34 Linux4n6 systemd[1]: systemd-hostnamed.service: Succeeded.
Apr 17 00:01:30 tryhackme dbus-daemon[539]: [system] Successfully activated service 'org.freedesktop.hostname1'
Apr 17 00:01:30 tryhackme NetworkManager[540]: <info>  [1650153690.6203] hostname: hostname: using hostnamed
Apr 17 00:01:30 tryhackme NetworkManager[540]: <info>  [1650153690.6204] hostname: hostname changed from (none) to "tryhackme"
Apr 17 00:01:31 tryhackme systemd-hostnamed[638]: Changed host name to 'ip-10-10-51-7'
Apr 17 00:02:05 tryhackme systemd[1]: systemd-hostnamed.service: Succeeded.
Apr 17 04:40:12 tryhackme systemd-resolved[490]: Using system hostname 'tryhackme'.
Apr 17 04:40:12 tryhackme kernel: [    3.511370] systemd[1]: Set hostname to <tryhackme>.
Apr 17 04:40:12 tryhackme dbus-daemon[539]: [system] Activating via systemd: service name='org.freedesktop.hostname1' unit='dbus-org.freedesktop.hostname1.service' requested by ':1.3' (uid=100 pid=443 comm="/lib/systemd/systemd-networkd " label="unconfined")
Apr 17 04:40:12 tryhackme dbus-daemon[539]: [system] Successfully activated service 'org.freedesktop.hostname1'
Apr 17 04:40:12 tryhackme NetworkManager[540]: <info>  [1650170412.7584] hostname: hostname: using hostnamed
Apr 17 04:40:12 tryhackme NetworkManager[540]: <info>  [1650170412.7585] hostname: hostname changed from (none) to "tryhackme"
Apr 17 04:40:13 tryhackme systemd-hostnamed[607]: Changed host name to 'ip-10-10-51-7'
Apr 17 04:40:47 tryhackme systemd[1]: systemd-hostnamed.service: Succeeded.
Apr 17 15:50:17 tryhackme systemd-resolved[502]: Using system hostname 'tryhackme'.
Apr 17 15:50:17 tryhackme kernel: [    2.877851] systemd[1]: Set hostname to <tryhackme>.
Apr 17 15:50:17 tryhackme dbus-daemon[582]: [system] Activating via systemd: service name='org.freedesktop.hostname1' unit='dbus-org.freedesktop.hostname1.service' requested by ':1.3' (uid=100 pid=457 comm="/lib/systemd/systemd-networkd " label="unconfined")
Apr 17 15:50:18 tryhackme dbus-daemon[582]: [system] Successfully activated service 'org.freedesktop.hostname1'
Apr 17 15:50:18 tryhackme NetworkManager[583]: <info>  [1650210618.1019] hostname: hostname: using hostnamed
Apr 17 15:50:18 tryhackme NetworkManager[583]: <info>  [1650210618.1019] hostname: hostname changed from (none) to "tryhackme"
Apr 17 15:50:18 tryhackme systemd-hostnamed[643]: Changed host name to 'ip-10-10-178-149'
Apr 17 15:50:52 tryhackme systemd[1]: systemd-hostnamed.service: Succeeded.
Apr 17 21:00:59 Linux4n6 systemd-resolved[491]: Using system hostname 'Linux4n6'.
Apr 17 21:00:59 Linux4n6 kernel: [    2.728250] systemd[1]: Set hostname to <Linux4n6>.
Apr 17 21:00:59 Linux4n6 dbus-daemon[539]: [system] Activating systemd to hand-off: service name='org.freedesktop.hostname1' unit='dbus-org.freedesktop.hostname1.service' requested by ':1.2' (uid=100 pid=442 comm="/lib/systemd/systemd-networkd " label="unconfined")
Apr 17 21:00:59 Linux4n6 dbus-daemon[539]: [system] Successfully activated service 'org.freedesktop.hostname1'
Apr 17 21:00:59 Linux4n6 NetworkManager[542]: <info>  [1650211259.9857] hostname: hostname: using hostnamed
Apr 17 21:00:59 Linux4n6 NetworkManager[542]: <info>  [1650211259.9857] hostname: hostname changed from (none) to "Linux4n6"
Apr 17 21:01:00 Linux4n6 systemd-hostnamed[599]: Changed host name to 'ip-10-10-178-149'
Apr 17 21:01:33 Linux4n6 systemd[1]: systemd-hostnamed.service: Succeeded.
Jun  1 20:29:57 Linux4n6 systemd-resolved[512]: Using system hostname 'Linux4n6'.
Jun  1 20:29:57 Linux4n6 kernel: [    2.977103] systemd[1]: Set hostname to <Linux4n6>.
Jun  1 20:29:57 Linux4n6 dbus-daemon[591]: [system] Activating via systemd: service name='org.freedesktop.hostname1' unit='dbus-org.freedesktop.hostname1.service' requested by ':1.3' (uid=100 pid=466 comm="/lib/systemd/systemd-networkd " label="unconfined")
Jun  1 20:29:58 Linux4n6 dbus-daemon[591]: [system] Successfully activated service 'org.freedesktop.hostname1'
Jun  1 20:29:58 Linux4n6 NetworkManager[592]: <info>  [1654097398.3030] hostname: hostname: using hostnamed
Jun  1 20:29:58 Linux4n6 NetworkManager[592]: <info>  [1654097398.3030] hostname: hostname changed from (none) to "Linux4n6"
Jun  1 20:29:58 Linux4n6 systemd-hostnamed[655]: Changed host name to 'ip-10-10-53-56'
ubuntu@Linux4n6:~$ 
```
## Task 8 Conclusion
[Linux Forensics Cheatsheet - deploy](
