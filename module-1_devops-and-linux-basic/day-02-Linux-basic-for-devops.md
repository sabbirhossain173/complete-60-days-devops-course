## day 2 :  Linux File System & Commands (Hands-on)

### What is Linux File System?
The Linux file system is hierarchical, starting with the root directory /. Everything in Linux — files, folders, devices — starts from /. It's like the 'C: drive' in Windows but more structured.
### 🎯 Show this simple hierarchy:
```sh
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── opt
├── root
├── tmp
├── usr
└── var
```
### 🗣 Quick Explanation of Each:
 - /bin: essential binary commands
 - /home: user directories
 - /etc: system configurations
 - /var: variable files (logs, mail)
 - /root: root user's home
 - /tmp: temporary files

### Hands-On: Basic Navigation Commands 
📺 Show terminal screen and demonstrate:
```sh
pwd          # Print current directory
ls           # List files
ls -l        # Detailed list
cd /etc      # Change directory
cd ~         # Go back to home
mkdir test   # Make directory
touch file1  # Create a file
rm file1     # Remove file
rmdir test   # Remove directory
```
### Hands-On: Viewing & Editing Files
Examples:
```sh
cat /etc/os-release        # View content of a file
less /var/log/syslog       # View file with scroll
vi testfile.txt            # Edit with vi editor
```
```cat``` quickly displays file content. If it's long, use less to scroll. To edit, you can use ```vi```
### Creating Users
```sh
sudo adduser dhaka
sudo passwd dhaka
```
### Switch user and try:
 ```sh
su - dhaka
sudo apt update
```

### Standard User Deletion
The basic command removes the user account and their entry from system files, but by default, it leaves their home directory and mail spool intact.
```sh
sudo userdel <username>
sudo deluser <username> (on Debian/Ubuntu)
```


Deleting the User and Their Home Directory: This is the most common and recommended approach for a complete removal, as it deletes the user's home folder and mail spool along with the account.
```sh
sudo userdel -r <username>
sudo deluser --remove-home <username> (on Debian/Ubuntu)
```

### Hands-On: File Permissions & Ownership
```sh
ls -l file.txt              # See permissions
chmod 755 file.txt          # Change permission
chown user:user file.txt    # Change ownership
```
Linux permissions are powerful. ```chmod``` changes who can read, write, or execute a file. ```chown``` changes the file owner.

###  Explain basic permission structure: rwxr-xr-x
Every file and directory in Linux has a set of permissions that control who can read, write, or execute it.
📄 Basic Format (Using ls -l):
<br>When you run:
```sh
ls -l file.txt
```
You’ll see something like:
```sh
-rwxr-xr--  1 user group  1024 Jul 19 10:00 file.txt
```
Let’s break it down:
```sh
 -rwxr-xr--
 │ │ |  │ 
 │ │ │  └── Other (Everyone else)
 │ │ └──── Group
 │ └─────── Owner (User)
 └───────── File type
```
🧩 Explanation:
| Symbol | Meaning                              |
| ------ | ------------------------------------ |
| `-`    | File (can also be `d` for directory) |
| `r`    | Read permission (view content)       |
| `w`    | Write permission (edit/delete)       |
| `x`    | Execute permission (run file)        |
| `-`    | No permission                        |

Example Breakdown: -rwxr-xr--
| Section | Permissions          | Who it Applies To             |
| ------- | -------------------- | ----------------------------- |
| `rwx`   | Read, write, execute | **Owner** (can do everything) |
| `r-x`   | Read, execute        | **Group** (can read & run)    |
| `r--`   | Read only            | **Others** (public access)    |

### 🛠 Changing Permissions
You use the chmod command to change permissions.
<br>
🔧 Symbolic Mode:
```sh
chmod u+x script.sh      # Add execute to user (owner)
chmod g-w file.txt       # Remove write from group
chmod o+r file.txt       # Add read for others
```
🔢 Numeric Mode:
```sh
chmod 755 script.sh
```
Breakdown of 755:
 - 7 = rwx (Owner)
 - 5 = r-x (Group)
 - 5 = r-x (Others)

| Value | Meaning |
| ----- | ------- |
| 0     | ---     |
| 1     | --x     |
| 2     | -w-     |
| 3     | -wx     |
| 4     | r--     |
| 5     | r-x     |
| 6     | rw-     |
| 7     | rwx     |

### 👑 Changing Ownership
```sh
chown user:group file.txt
```

### user delete command
```sh
userdel t2 	        # delete user without remove its directory and file
userdel t2 -r	      # delete user with remove its all directory and file such as /home/t2                     
```

### add user to sudo group
```sh
add this user to sudo group
# usermod -aG sudo rajiv1
```
### without logout and login access sudo
```sh
newgrp sudo
```
### user give permission without passowrd
sudo vi /etc/sudoers
```sh
dhaka ALL=(ALL) NOPASSWD: ALL
```
"Now let’s switch to ```dhaka``` and test if the user can run sudo commands. This is often done when giving limited admin access to DevOps team members.

### Hands-On Test: Restricted vs Sudo User
### Real-World Scenario: Team Setup
In real DevOps teams, you may want to give developers access to deploy scripts but not full root access. You can use groups + restricted sudo rules for this.

### Understanding Sudo and Sudoers
View and safely edit sudoers file:
```sh
sudo vi /etc/sudoers
```
Add:
```sh
dhaka ALL=(ALL) NOPASSWD: ALL
```
```sudo``` lets a user run commands as root. Safely edit the file with ```nano``` or ```vim```. if anything rong — it can break your system.

### Creating and Managing Groups
```sh
sudo groupadd devops
sudo usermod -aG devops rajiv
groups rajiv
```
Groups allow us to assign access to a set of users. Here, we added ```rajiv``` to the ```devops``` group.

### Understanding File Permissions 
Create a file and show:
```sh
touch sample.txt
ls -l sample.txt
chmod 640 sample.txt
chown rajiv:devops sample.txt
```
```chmod``` changes permission. ```640``` means: owner can read/write, group can read, others have no access. ```chown``` changes ownership.


### Practical Scenario: Create User & Set Permissions
Let's create a new user devopsuser, give them a project folder, and make sure only they can access it. This is a common task in DevOps when setting up teams.
```sh
sudo adduser devopsuser
sudo mkdir /opt/project
sudo chown devopsuser /opt/project
sudo chmod 700 /opt/project
```
### File system check 
```sh
df -h
df -hT
```

### ✅ Summary:
| Command | Purpose                 |
| ------- | ----------------------- |
| `ls -l` | View permissions        |
| `chmod` | Change file permissions |
| `chown` | Change file owner/group |

