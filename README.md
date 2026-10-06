# 🐧 Linux Practical Examination

A hands-on Linux practical exam performed on an **Ubuntu server (AWS EC2 instance)** through the terminal. It has **12 questions** that move from basic file handling to users, permissions, package management, web servers, processes, text searching and networking.

## 📖 About the Exam

| Area | Questions | What is tested |
|------|-----------|----------------|
| File system basics | Q1 – Q3 | Creating, copying, moving, deleting and reading files and directories |
| Users and groups | Q4 – Q5 | Creating users and groups and checking their details |
| Permissions | Q6 – Q7 | `chmod`, `chown`, `chgrp` and numeric permissions |
| Software and services | Q8 – Q9 | Installing Apache, managing it with `systemctl`, hosting a custom webpage |
| Process management | Q10 | Finding, inspecting, stopping and restarting processes |
| Search and networking | Q11 – Q12 | `grep`, `sort`, `find`, IP, routing, `ping`, `curl`, open ports |

**Environment:** Ubuntu Linux, AWS EC2, Apache 2.4.66, Bash
**Working directory:** `~/LinuxExam`

---

## Q1 – Basic File Operations
Created a working directory `LinuxExam`, moved into it, and created three empty files (`student.txt`, `course.txt`, `result.txt`). Confirmed the location and listed the contents.

```bash
mkdir LinuxExam
cd LinuxExam
touch student.txt course.txt result.txt
pwd
ls -l
```

## Q2 – File Management
Copied `student.txt` to a backup, renamed `course.txt`, deleted `result.txt`, created three folders, and moved the backup into `Backups`. `ls -R` shows the final structure.

```bash
cp student.txt student_backup.txt
mv course.txt linux_course.txt
rm result.txt
mkdir Documents Backups Scripts
mv student_backup.txt Backups/
ls -R
```

## Q3 – File Content Operations
Added 5 lines of student info to `student.txt` and 5 lines of course info to `linux_course.txt`. Displayed them with `cat`, the first 3 lines with `head`, the last 2 with `tail`, and counted lines, words and characters with `wc`.

```bash
cat student.txt
cat linux_course.txt
head -n 3 student.txt
tail -n 2 student.txt
wc -l student.txt
wc -w student.txt
wc -m student.txt
```

## Q4 – User Management
Created the user `student01`, set a password, and verified the account. Displayed its UID, GID and home directory, then switched to the account and confirmed the username with `whoami`.

```bash
getent passwd student01
id student01
getent passwd student01 | cut -d: -f6
sudo su - student01
whoami
exit
```

## Q5 – Group Management
Created the group `linuxbatch` and added `student01`. Created a second user `student02` with a password, added it to the group, and listed all group members.

```bash
sudo groupadd linuxbatch
sudo usermod -aG linuxbatch student01
groups student01
sudo useradd -m -s /bin/bash student02
sudo passwd student02
sudo usermod -aG linuxbatch student02
getent group linuxbatch
```

## Q6 – File Permissions
Created `project.txt` and set permissions to `754` (owner `rwx`, group `r-x`, others `r--`). Then changed the mode to `640` and changed the owner and group to `student01:linuxbatch`, verifying each step.

```bash
touch project.txt
chmod 754 project.txt
ls -l project.txt
chmod 640 project.txt
sudo chown student01:linuxbatch project.txt
ls -l project.txt
```

## Q7 – Permission Challenge
Created three directories with different access rules: `public` (everyone, `755`), `private` (owner only, `700`) and `shared` (owner and group members, `770`, group `linuxbatch`). Verified with `ls -ld`.

```bash
mkdir public private shared
chmod 755 public
chmod 700 private
chmod 770 shared
sudo chgrp linuxbatch shared
ls -ld public private shared
```

## Q8 – Package Management
Updated the package list, installed the Apache web server, and checked its version (Apache/2.4.66). Started the service, checked its status (`active (running)`), and enabled it to start at boot.

```bash
sudo apt update
sudo apt install apache2 -y
apache2 -v
sudo systemctl start apache2
sudo systemctl status apache2 --no-pager
sudo systemctl enable apache2
systemctl is-enabled apache2
```

## Q9 – Apache Web Server Configuration
Found the document root (`/var/www/html`) and created a custom `index.html` showing the student name, roll number, course name and the heading "Linux Practical Examination". Restarted Apache and verified the page with `curl -i` (HTTP 200 OK) and in the browser using the server's public IP.

```bash
cd /var/www/html
sudo nano index.html
sudo systemctl restart apache2
curl -i http://localhost
```

## Q10 – Process Management
Listed running processes, found the Apache PIDs with `pgrep`, and displayed the main process details with `ps`. Stopped Apache and confirmed it was `inactive`, then started it again and confirmed it was `active`.

```bash
ps aux
top
pgrep -a apache2
ps -fp $(pgrep -o apache2)
sudo systemctl stop apache2
systemctl is-active apache2
sudo systemctl start apache2
systemctl is-active apache2
```

## Q11 – Search and Text Processing
Created `students.txt` with 10 records, then searched it with `grep` (normal, case-insensitive and count). Sorted the records, and used `find` to locate a file by name and to list all `.txt` files in `LinuxExam`.

```bash
grep "Rahul" students.txt
grep -i "rahul" students.txt
grep -ic "rahul" students.txt
sort students.txt
find . -name "student.txt"
grep -i "a" students.txt
find . -type f -name "*.txt"
```

## Q12 – Linux Networking
Displayed the hostname, IP address, network interfaces and routing table. Tested connectivity with `ping` and `curl`, resolved a domain name to its IP, and listed listening ports (SSH on 22, Apache on 80, MariaDB on 3306).

```bash
hostname
hostname -I
ip addr
ip route
ping -c 4 google.com
curl -I https://www.google.com
getent ahostsv4 google.com
sudo ss -tulnp
sudo ss -tulnp | grep ':80 '
curl -I http://localhost
```

---

## ✅ Summary
This exam demonstrates practical skills in file handling, user and group administration, permission control, installing and running a web server, process management, text searching and network troubleshooting on Linux.
