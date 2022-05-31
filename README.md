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

