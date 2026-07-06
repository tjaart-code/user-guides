---
title: Bash Terminal Commands
---

## Quick Reference
```sh
sha256sum filename

python3 ./lichess-bot.py -v -u

sudo docker compose up -d

openssl rand -hex 32

# View local & universal time
timedatectl

# Show the total file size of folder
du -sh foldername

# Count the number of lines
wc -l filename

# Monitor the CPU's core temperatures
sensors
watch sensors # live monitoring

# Root access
sudo su # root access on Ubuntu
sudo -i # root access on Ubuntu
su root # root access on other distros

# Restart & Shutdown
sudo init 6 # restart
sudo init 0 # shutdown
systemctl reboot
systemctl poweroff

# Services
sudo systemctl restart apache2
sudo service php8.0-fpm restart # for updating php.ini file

# System utilities
sudo apt-get update --fix-missing
xset m default # mouse speed
units # converter

```

---

## Docker Compose

```bash
sudo docker compose up -d
sudo docker compose down
# Navigate to your project folder containing your docker-compose.yml
sudo docker compose pull

# GUI with DOCKGE
cd /opt/dockge
sudo docker compose up -d
http://localhost:5001

```

---

## PC Stats / Info

* **TTY Graphical Interface Toggle:** `Ctrl+Alt` with `F1`/`F7` (Ubuntu = `F7`, RHEL = `F1`)
* `man <command>` - Know more about what a command can do
* **Flags:** `-s` means summary, `-h` means human readable

```bash
lspci # pc stats
cat /etc/issue.net # show ubuntu version
uname -a # prints all system information
lsb_release -a # prints Linux version information

history -c # clear all of the past commands
pwd # print working directory
ls -l # list down the directories & files of the current directory
fdisk -l # show partitions
lsblk # list of partition drives
df # displays filesystem disk space usage for all mounted partitions
free -m -h # view free memory in MB (human-readable)
lshw # Hardware Lister

```

---

## LAMP: Linux, Apache, MariaDB, PHP

### Service Management

```bash
# Apache
sudo systemctl start|stop|restart|reload|status apache2

# MariaDB
sudo systemctl start|stop|restart|status mariadb
sudo mariadb -u root -p

# Check Apache config for errors
sudo apache2ctl configtest

# Enable/disable Apache modules
sudo a2enmod <module>
sudo a2dismod <module>

# Enable/disable Apache sites
sudo a2ensite <site>.conf
sudo a2dissite <site>.conf
```

### MariaDB / MySQL Configuration

```bash
sudo mysql -u root -p # login to mariaDB/mySQL

```

```sql
SHOW DATABASES;
DROP DATABASE my_database;
DROP DATABASE IF EXISTS my_database;

SELECT User,Host FROM mysql.user; -- show users
DROP USER 'userName'@'localhost';
SELECT database(); -- see/select current database
USE new_database; -- use this database

CREATE DATABASE my_databaseDB;
CREATE DATABASE IF NOT EXISTS my_databaseDB;
CREATE USER user@localhost;
SET PASSWORD FOR user@localhost= PASSWORD("password");
GRANT ALL ON my_database.* TO 'user'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;

```

### Joomla Installation via CLI

```bash
wget [https://github.com/joomla/joomla-cms/releases/Joomla_3.9.12-Stable-Full_Package.zip](https://github.com/joomla/joomla-cms/releases/Joomla_3.9.12-Stable-Full_Package.zip)

sudo mkdir -p /var/www/html/my_website
unzip -q Joomla_3.9.12-Stable-Full_Package.zip -d /var/www/html/my_site

sudo chown -R www-data:www-data /var/www/html/my_site/
sudo chmod -R 755 /var/www/html/my_site/

```

---

## PostgreSQL

```bash
sudo -u postgres psql # access the database

```

---

## SSH / FTP

```bash
ssh root@41.185.91.26
ssh root@example.com # IP address version
ssh domainuser@example.com # domain FTP user version

```

---

## Network & Processes

### Network Interfaces & Diagnostics

```bash
sudo systemctl restart NetworkManager

# View Public IP
curl ipv4.icanhazip.com
wget -qO - [https://api.ipify.org](https://api.ipify.org)
dig +short myip.opendns.com @resolver1.opendns.com

sudo iwlist wlan0 freq # available frequencies
iwlist wlan0 scan

ifconfig
iwconfig
sudo lshw -class network # view details of network/card
ip addr # system's network interfaces
route -n # list kernel routing tables
ip link show # view mac addr
hostname -I # view my IP add

# NetworkManager Client
nmcli # controlling NetworkManager
nmcli dev show | grep DNS # view DNS servers
nmcli connection show
resolvectl status

```

### Network Traffic Monitoring

```bash
bmon # bandwidth monitor and rate estimator
sudo iftop # data flowing through individual socket connections
iptraf # individual connections and data flow amount between hosts
tcptrack # captures packets and calculates bandwidth stats
sudo tcptrack -i eth0/wlan0
vnstat # network traffic monitor (background daemon)
nload # monitor incoming and outgoing traffic separately
nethogs wlan0

netstat -tunap
netstat -i
ss -t -a
ss -a -t -o state established

```

### DNS & Domain Lookups

```bash
whois websiteName
host <IP address>
dig <IP address>
nslookup <IP address>
nslookup –type=mx <domain name> # check for valid email addresses

```

### Nmap Scanning

```bash
man nmap
nmap -A -T4 scanme.nmap.org

```

### Process & Resource Monitoring

```bash
top # real-time active processes ordered list
htop # advanced interactive Linux process monitoring tool
sudo iotop # monitor real-time Disk I/O and processes
sudo iotop -o # only show processes or threads actually doing I/O
lsof # list open files

```

---

## SQLMap

```bash
sqlmap -u "[http://your-target-url.com/vulnerable-parameter?param=value](http://your-target-url.com/vulnerable-parameter?param=value)" --dbs

```

---

## Time-Warrior

```bash
timew --view current state
timew tags # view used tags
timew start "my activity"
timew start myActivity
timew stop
timew continue
timew summary
timew summary myActivity
timew track 9:00 - 11:00 'Outline the tutorial topics' # add forgotten time to log

```

---

## Task-Warrior

```bash
task add "Buy milk" # add a task
task list # see pending tasks
task 1 done # mark task 1 as complete
task 1 modify priority:H # set priority

```

---

## Anti-Virus (ClamAV)

```bash
sudo systemctl stop clamav-freshclam
sudo freshclam # update clamav anti-virus
sudo systemctl start clamav-freshclam

clamscan -r /home # check files in all users home directories
clamscan -r / # scan entire computer

```

---

## OpenVPN

```bash
sudo apt-get install openvpn network-manager-openvpn network-manager-openvpn-gnome

sudo systemctl start openvpn@server
sudo systemctl status openvpn@server

```

---

## Sendmail

```bash
echo "This is a test email." | sendmail tjaart@localhost

```

```text
sendmail user@example.com
From: me@myhost.com
To: user@example.com
Subject: Your subject here

Hi, this is my message, and I'm sending it to you!
.

```

---

## Easter Eggs

```bash
ls /usr/share/calendar/
calendar -f /usr/share/calendar/calendar.pagan -A 365
calendar -f /usr/share/calendar/calendar.music -A 365
calendar -f /usr/share/calendar/calendar.computer -A 365
calendar -f /usr/share/calendar/calendar.history -A 365
calendar -f /usr/share/calendar/calendar.world -A 365

```

---

## Ownership & Groups

```bash
sudo chown owner:group /home/user/filename # change owner and group
sudo chown owner: /home/user/filename # change owner
sudo chown :group /home/user/filename # change group

groups username
groups # view groups
users # view users
compgen -u # view users
compgen -g # view groups

```

---

## File Permissions

```bash
sudo chmod 665 filename
sudo chmod u+x go=rx filename

```

| Octal | Binary | File Mode |
| --- | --- | --- |
| 0 | 000 | `---` |
| 1 | 001 | `--x` |
| 2 | 010 | `-w-` |
| 3 | 011 | `-wx` |
| 4 | 100 | `r--` |
| 5 | 101 | `r-x` |
| 6 | 110 | `rw-` |
| 7 | 111 | `rwx` |

* **Context Legend:** `u` = user/owner, `g` = group, `o` = Other/wOrld
* **Permission Legend:** `r` = read, `w` = write, `x` = execute

### Quick Syntax Examples

* `u+x`: Add execute permission for the owner
* `u-x`: Remove execute permission from the owner
* `+x` or `a+x`: Add execute permission for the owner, group, and world
* `o-rw`: Remove read and write permission from anyone besides the owner and group owner
* `go=rw`: Set the group owner and anyone besides the owner to have read and write permission
* `u+x,go=rx`: Add execute permission for the owner and set the permissions for the group and others to read and execute

---

## Folders & Files Management

```bash
rm -r folder # Remove a folder and its content recursively
mkdir folder # Create a new folder
rmdir folder # Remove an empty folder
rm filename # Remove said filename
cp -i /file/location/fileName /newFile/location/folder/ # copy and ask if to replace
> filename # create blank new file

du -sh folder # show the total file size of folder
wc -l file # count the number of lines

subl /filename # open file with Sublime-Text

```

---

## Archive Extraction

```bash
tar -xvf Package_Name.tar
tar xvzf Package_Name.tar.gz
tar xvjf Package_Name.tar.bz2

gunzip File_Name.gz
7z x Package_name.7z
unzip Package_name.zip -d /path/path/folder

```

---

## Flatpak

```bash
flatpak run org.gimp.GIMP # run flatpak application
flatpak list # list installed applications
flatpak uninstall applicationID
flatpak uninstall --unused
flatpak update # update all applications
flatpak update -v # fix install errors

```

---

## Package Installation & Compilation

```bash
./configure
make
sudo make install

sudo grub-install --root-directory=/mnt /dev/sda

sh fileName.sh
bash fileName.sh

dpkg --list # installed packages
dpkg --status <package name> # determine if package is installed
apt-cache show <package name> # installed package information
dpkg --get-selections | grep -v deinstall # list of installed programs
sudo dpkg -i <package name> # Installing .deb packages

sudo apt-get install -f # install missing dependencies

apt-get remove/purge <program name> # remove/delete program
apt-get -u upgrade # list upgradeable packages

sudo aptitude purge packagename # remove package and configuration files
rpm -Uvh package.rpm # Install an rpm package

```

---

## PPA Repositories

```bash
apt policy # list PPA Repositories
ls /etc/apt/sources.list.d # list PPA Repositories

sudo add-apt-repository --remove ppa:PPA_REPOSITORY_NAME/PPA
sudo apt-get install ppa-purge
ppa-purge ppa:PPA_REPOSITORY_NAME/PPA

```

---

## Formatting USB Storage

```bash
lsblk # list of partition drives
df -h

sudo umount /dev/sdc1
sudo mkfs.vfat /dev/sdc1 # for vfat*
sudo mkfs.ntfs /dev/sdc1 # for ntfs
sudo mkfs.ext4 /dev/sdc1 # for ext4

```

---

## Package Manager Lock Errors (Shitty Error Solved)

```bash
# Find and kill the process holding the lock
ps -e | grep -e apt -e adept | grep -v grep 

sudo dpkg --configure -a # help with the configuration error
sudo rm /var/cache/apt/archives/lock

```

---

## Aircrack-ng (Wi-Fi Security)

```bash
airmon-ng check kill
airmon-ng start wlan0 # monitor mode, receive all network traffic
airodump-ng mon0 # capture packets
airodump-ng -c 9 --bssid DC:EE:06:B4:14:2E -w dump mon0 # capture only one AP (bssid, channel 9)

# Cracking
aircrack-ng -b DC:EE:06:B4:14:2E dump-01.cap -w ../folder # cracking WEP passwords
aircrack-ng dump-01.cap -w /folder/folder/filename.txt # filename.txt is the wordlist

sudo ./wifite.py

```

* Reference: [Null-Byte Wi-Fi Hacking Guide](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-getting-started-with-aircrack-ng-suite-wi-fi-hacking-tools-0147893/)

---

## John the Ripper

```bash
# Extract hash from office document
python office2john.py fileName.txt > newFile.txt

# Bruteforce hash file using a wordlist
john --wordlist=wordlistfile/myWordlist.txt hash-file.txt

```

---

## Basic Math Operations

Using the `expr` command line tool:

```bash
expr 134 + 11 # add
expr 134 - 11 # subtract
expr 134 / 11 # divide
expr 134 \* 11 # multiply (note the \ escape before *)
expr 134 % 11 # remainder

# Logical comparisons (Yes=1, No=0)
expr 134 \> 11 # Is 134 bigger than 11?
expr 134 = 11 # Does 134 equal 11?

```

### Variable Calculations Example

```bash
age=33
expr $age = 33 # returns 1 (yes)

age=33
age=`expr $age + 1`
echo $age # outputs 34

```

* Reference: [Vitux Guide on CLI Math](https://vitux.com/how-to-do-basic-math-in-linux-command-line/)

---

## Joomla CMS Tweaks & Workarounds

### Fix Chmod Verification Error

Open the file `libraries/vendor/joomla/filesystem/src/File.php`

Find this line of code:

```php
if (!Path::canChmod($file))

```

Change it to:

```php
if (!Path::canChmod($file) && false)

```

### Remove Deprecation Notices from phpMyAdmin

Add the following configuration code to your `config.inc.php`:

```php
$cfg['SendErrorReports'] = 'never';

```

### Misc/Uncategorized Hash Signatures

* open = `$2y$10$TrqVEcvwTowz0ojvsffUFukN7iC2Mm6gevviCA6UEpG6XLzgufo86`

