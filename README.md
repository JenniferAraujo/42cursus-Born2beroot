# 42cursus-Born2beroot
## :books: About the project
This project aims to create a virtual machine. Then, at the end of this project, you will be able to set up your own operating system while implementing strict rules.

## Score
  ![born2beroot](https://user-images.githubusercontent.com/106436743/213870697-91744e74-5d66-44ea-ac59-6e3c24d21b5b.jpg)

## :warning: Mandatory  
This project consists of having you set up your first server by following specific rules:
* Your virtual machine cannot have a graphical interface
* You must choose as an operating system either the latest stable version of Debian (no testing/unstable), or the latest stable version of Rocky.
* You must create at least 2 encrypted partitions using LVM.
* A SSH service will be running on port 4242 only. For security reasons, it must not be possible to connect using SSH as root.
* You have to configure your operating system with the UFW (or firewalld for Rocky) firewall and thus leave only port 4242 open.
* The hostname of your virtual machine must be your login ending with 42 (in my case "jede-ara42".
* You have to implement a strong password policy: 
  * Expire every 30 days.
  * The minimum number of days allowed before the modification of a password will be set to 2.
  * The user has to receive a warning message 7 days before their password expires.
  * Your password must be at least 10 characters long, with an uppercase letter, a lowercase letter, and a number. Also, it must not contain more than 3
consecutive identical characters.
  * The password must not include the name of the user.
  * The following rule does not apply to the root password: The password must have at least 7 characters that are not part of the former password.
  * Your root password has to comply with this policy.
  
* You have to install and configure sudo following strict rules.
* In addition to the root user, a user with your login as username has to be present.
* This user has to belong to the user42 and sudo groups.
* Authentication using sudo has to be limited to 3 attempts in the event of an incorrect password.
* A custom message of your choice has to be displayed if an error due to a wrong password occurs when using sudo.
* Each action using sudo has to be archived, both inputs and outputs. The log file has to be saved in the /var/log/sudo/ folder.
* The TTY mode has to be enabled for security reasons.
* For security reasons too, the paths that can be used by sudo must be restricted.
  Example: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
* You have to create a simple script called monitoring.sh. It must be developed in bash.

* The script will display the following information on all endpoints every 10 minutes.
  * The architecture of your operating system and your kernel version.
  * The number of physical processors.
  * The number of virtual processors.
  * The RAM currently available on your server and its utilization rate in percentage.
  * The memory currently available on your server and its utilization rate in percentage.
  * The current utilization rate of your processors as a percentage.
  * The date and time of the last reset.
  * Whether LVM is active or not.
  * The number of active connections.
  * The number of users using the server.
  * Your server's IPv4 address and its MAC (Media Access Control) address.
  * The number of commands executed with the sudo program.
  
## :footprints: Steps
I followed the guide from [gemartin99/Born2beroot-Tutorial](https://github.com/gemartin99/Born2beroot-Tutorial), and it really helped me. So if that's not enough, I recommend it.

### :desktop_computer: Create virtual machine
* Your virtual machine must be created in the 'sgoinfre' folder to avoid future installation problems.
* I chose the debian operating system because it is highly recommended for users who are new to system administration as it is easier to install and configure than Rocky.

#### :wrench: Configuring the Vm in VirtualBox:
  * Name: Born2beroot.
  * Type of OS: Linux.
  * Version: Debian (64-bit).
  * Memory Size: 1024 MB.
  * Hard disk: Create a virtual hard disk file.
  * File size: 8.00 GB.
  * Hard disk file type: VDI (VirtualBox Disk Image).
  * Storage on physical hard disk: Dynamically allocated.
  
#### Partitioning and Base System

* Hostname: jede-ara42
* Domain: [empty]
* RootPassword: [password]
* Username: jede-ara
* UserPassword: [password]

* Use the option "Guied - use entire disk and set up encrypted LVM" because the subject asked for it
* Choose the disk that we want to partition
* Choose the third option "Separate /home, /var, and /tmp partitions"
* Click in "cancel" when appears that will delete the data from the disk

* Encryption Passphrase: [password]

* Choose that it is not necessary to install extra media
* Leave the HTTP empty
* Put that you don't want to participate in the "package usage survey"
* Remove all the software options
* Install GRUB boot in the hard disk and choose the option "/dev/sda(ATA_VBOX_HARDDISK)"

### :wrench: Configuring a virtual machine
* Insert the encrypted password
* Enter in the user you created
* To enter in the root user you use "su" and put the password

#### :busts_in_silhouette: Installing sudo and configuring users
 * `<apt install sudo>`: to install the necessary packages
 * `<sudo reboot>` : restart the machine to apply the changes
 * `<sudo -V>`: will show you the version of sudo
 * `<sudo adduser {your_user}>`: it must show that your user already exists
 * `<sudo addgroup user42>`: create a group called "user 42"

  :mag: GID is the group identifier, is an abbreviation of GroupID
	
* `<sudo adduser {your_user} {group}>`: we will add the user to the group
* `<sudo adduser {your_user} {sudo}>`: add sudo group too
* `<getent group {user42}>`: seeing if the user is in the group
* `<getent group {sudo}>`: seeing if the user is in the group
* ´<sudo apt update>`: Updates the package sources with their new versions in the repositories
  
#### :satellite: Installation and configuration SSH
  :mag: SSH is remote access to a server by a safe channel, in which all the information will be encrypted

*`<sudo apt install openssh-server>`: installing OpenSSH which is the main connectivity tool for remote session initiation with the SSH protocol
* `<sudo service ssh status>`: to verify if the installation sucedded (it musta appear as active)
* `<nano /etc/ssh/sshd_config>`: We will have to access a file called "sshd_config", for this we insert the path 
  - Change the "#Port 22" to "Port 4242"
  - Change the "#PermitRootLogin prohibit-password" to "PermitRootLogin no"
