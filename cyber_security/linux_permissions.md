# Understand how a multiuser system operates and the role of the root user

## Overview

In the past, when computers were large and expensive, it was necessary for more than one person at a time to use a single computer. To maximize limited compter resources, the Unix OS was created to be a multiuser system. Linux is also a multiuser OS and has a protocol for controlling user access to the various files and processes on the operating system.


There are two main types of users on a Linux OS: system and regular. The systems users, with names like *bin*, *sys*, and *mail*, run processes in the background; in other words, processes which don't require any interaction with a user. The regular users are the humans that have separate accounts and interact with the system. A list of users on the system can be shown with the following command:

```bash
$ cat /etc/passwd
```
This list could have many hundreds of users so the output won't be shown here. While the file is named **passwd** the actual encrypted user passwords are stored in a different file. All users can view the `/etc/passwd` file but only users with certain permissions can edit the file.


### The root user

In Windows and macOS, the account with the permission to alter system settings and change permissions for other users, sometimes called accounts, is called the administrator. In Linux, this account is called the *root* user.

We reviewed the Linux file system earlier and explored the root directory  (`/`) as top directory of the whole file structure. The `/` directory should not be confused with the `root` user's home directory  which is `/root`. Each regular user has a home directory where personal files live, in addition to configuration files for that account.

Let's explore a `/home` directory, starting at cyberuser's home directory.

```bash
cyberuser@mycomputer $ whoami
cyberuser
cyberuser@mycomputer $ pwd
/home/cyberuser

# Change to the root directory
cyberuser@mycomputer $ cd /
cyberuser@mycomputer $ ls
bin    dev   initrd.img      lib64       mnt   root  snap  tmp  vmlinuz
boot   etc   initrd.img.old  lost+found  opt   run   srv   usr  vmlinuz.old
cdrom  home  lib             media       proc  sbin  sys   var
```

This is a standard listing for the `/` directory. In addition to all of the necessary Linux operating system directories, there is a directory for `root` in `/`. What happens when we try to `cd` into that directory?

```bash
cyberuser@mycomputer $ cd root
bash: cd: root: Permission denied
```

As we are not the `root` user, we don't have permission to even view files in the `root` user's home directory. In Linux, we can change to another user, if we know the credentials for that user. Using the command `su` or *substitute user*, we can switch to any other user if we have the credentials for that account. The syntax for this command is `su - username`. Here's an example to switch to the user *batman*:

```bash
cyberuser@mycomputer $ su - batman
Password: batmas_password_typed_here
batman@mycomputer $
batman@mycomputer $ whoami
batman
```
When the `-` is used with `su`, you actually become that user and your current directory will be that user's. If the `-` is left off, you will become that user but will stay in the directory where you issued the `su` command.

### Superuser
The `su` command is sometimes referred to as *super user* because it is often used to switch to `root` or the *superuser*. If the `-` is left off, the default is to switch to the superuser. Here's an example for how to do that:

```bash
cyberuser@mycomputer $ su
Password: root_password_typed_here
mycomputer / #
mycomputer / # whoami
root
```

When we log in as root, which we confirmed with the `whoami` we can see that the bash prompt has changed from `$` to a `#` and the user name is no longer listed in front of the computer name (mycomputer). What's in the root directory?

```bash
mycomputer / # cd root
mycomputer / # pwd
/root
mycomputer ~ # ls
50_linuxmint.cfg.bak  grub.bak  grub.cfg.bak  setup-chroot.sh
```
The directory the user is currently is is shown by the `/` for the root directory and `~` for the home directory. When we print the working directory (`pwd`) it displays `/root`


As the root user, you now have complete control over the system. This can be a dangerous position to be in in terms of the integrity of your system. Only advanced users should attempt to change anything on the system while the superuser. To return to the previous user, type `exit`:

```bash
mycomputer / # exit
cyberuser@mycomputer $ whoami
cyberuser
cyberuser@mycomputer $ pwd
/
cyberuser@mycomputer $ cd
cyberuser@mycomputer $ pwd
/home/cyberuser
```


## Follow Along

Using the online Linux terminal (need link here? can we use links within the body of the content?) we will practice logging as the root (superuser) and exploring the file system.

* identify the home directory, change to the `/` directory and then try to go into the `/root` directory
* change to the su and try again
* cd to different directories, ls to see what files are there

## Challenge

Using your local computer, see if you have root / administrator privileges. Switch to the superuser account and explore the system. **DO NOT CHANGE ANY SETTINGS**


## Additional Resources

- [How to use su in Linux](https://linoxide.com/how-tos/understand-linux-su-command-function-with-example/)# Understand how to use `sudo` and edit the `sudoers` file

## Overview

It is not always necessary to use `su` to become a different user, especially if you only need access to a few commands and not the other user's entire environment. The `sudo` command can then be used to issue commands with the priviliges of another user, including root. The name stands for *substitute user do* but is usually referred to as *superuser do* since most uses of `sudo` are for root priviliges.

Issuing `sudo` commands doesn't require the user to input another user's password. Instead, there is a list of users and their associated priviliges in the `/etc/sudoers` file. If you have `sudo` privileges for the command you are issuing, then it will go through. The first time you use `sudo` in your terminal, you input your password; after that, you can use `sudo` in that sessions without needing to enter the password again.

The syntax for `sudo` is `sudo command`. If no user name is given, the default is the root user. We can see in the following example that the `whoami` command was issued as root:

```bash
cyberuser@mycomputer ~ $ sudo whoami
[sudo] password for cyberuser: 
root
cyberuser@mycomputer ~ $ sudo whoami
```
The `/etc/sudoers` file can only be edited by root and requires the use of a special tool called `visudo`. This program allows this file to be safely edited. A screenshot of the `visudo` output is shown below.

![Sudo screenshot](../../images/sudoScreen.png)

The important lines are `root    ALL=(ALL:ALL) ALL` and `%sudo   ALL=(ALL:ALL) ALL`. The first shows us that root has ALL privileges; the second that any user in the sudo group also has ALL privileges. If you needed to give a user only certain priviliges, then you would add the following line to file:

```bash
batman ALL=(ALL:ALL) cat, less
```
Here, we have given `batman` privileges for just two commands: `cat` and `less`. 


## Follow Along

Let's go over an example for what would be added to the `/etc/sudoers` file for a few examples. This will include:

* give users the ability to create directories but input their password when deleting directories
* give a certain user privileges for commands relating to configuring the network ?


## Challenge

**Students will not edit the sudoers file on their own computer (if it has one)**. We will find an alternative like a VM or online terminal.

## Additional Resources

- [The sudo command](https://www.unixtutorial.org/commands/sudo)
- [How to use visudo](https://www.unixtutorial.org/how-to-use-visudo)# Understand the permission levels of r+w+x and how to check permissions for files and user groups


## Overview

All files in Linux are owned by a single user and a single group and every file has permissions associated with it determining who can view, edit, and run that file. The three permission levels are denoted by:

* **r** (read)
* **w** (write)
* **x** (execute)

As mentioned above, there are also permission classes, which determines who has what permission level:

* **user** - Permissions apply to the owner of the file.
* **group** - Permissions apply to the group who owns the file. If the user is also in the group, the user permissions apply.
* **other** - Permissions for all other users.

A better way to understand the permissions is to list of files with the long view option `ls -l`:

```bash
$ ls -l
drwxr-xr-x  3 cyberuser mygroup   4096 Mar 12 17:09 Documents
drwxr-xr-x  4 cyberuser mygoup    4096 Jun 27 07:02 lambda_cybersec
-rw-r--r--  1 cyberuser mygroup 169445 Jul  1 16:56 ls_output.txt
drwxr-xr-x 11 cyberuser mygroup   4096 Mar 18 20:05 myblog
-rw-r--r--  1 cyberuser mygroup    161 Jul  1 17:26 mynewfile.txt
```
On the left is the information about the permission for each file. There is a lot of information here so we'll break it down with the following graphic.

(image not included)

The parts that we are interested in are the file mode, and who the user and groups are. The mode on the left indicates the permissions for the user, group, and other (everyone else).

(image not included)

The file type is indicated on the left: d for directory and blank for a regular file. In the following example, the directory has read-write-execute permission for the user, and read-execute permission for the group and others.

```bash
drwxr-xr-x  4 cyberuser mygoup    4096 Jun 27 07:02 lambda_cybersec
```

## Follow Along

We'll open a terminal and explore our local file system. This will include going over examples of permissions for directories, files, and the groups on the system.

## Challenge

You will explore your own files (*or maybe use a VM for this exercise?*) and find examples of:

* what other users are on the system
* what group you are in
* what other groups are on your system


## Additional Resources

- [Linux file permissions](https://www.booleanworld.com/introduction-linux-file-permissions/)# Know how to use `chmod` and`chown` to change permissions and how permissions are represented numerically

## Overview

We've already covered permission levels and classes. When managing both your own files and files on another computer, it will be necessary to change permissions for certain users and groups.

Using the `chmod` command, which stands for change mode, we can change permissions for any file or directory. The sytax for the `chmod` command is:

```
chmod options permissions file name
```

The `chmod` command can be used two way: symbolically or numerically.

Using the letters for **u**ser, **g**roup, and **o**ther and the permission levels **r**ead, **w**rite, and e**x**ecute, the file permissions are changed like this:

```bash
$ chmod u=rwx, g=rx, o=r myfile.txt

```
 
 This command will change the permissions of the file so the user has read+write+execute permission, the group can read+execute, and others can only read the file. This assumes the file is executable; it has to be of the right type in order to do this.
 
The `chmod` command can also be used in octal-mode, where numbers between 0-7 are used to represent the permission levels. The above command in octal-mode would look like:

```bash
$ chmod 754 myfile.txt

```
where

* 4 stands for **r**ead
* 2 stands for **w**rite
* 1 stands for e**x**ecute
* 0 stands for no permission

The digits represent the **ugo** permission classes: 7 corresponds to the permission for the **u**ser, the 5 for the **g**roup, and the 4 for **o**ther. The graphic below summarizes the symbolic and octal-modes.

(image not included)

## Follow Along

We will create a few files and change the permissions with both symbolic and octal notation. When using `chmod`, the more you practice the easier it will be to remember the shortcut octal-modes.

## Challenge

Use both `chmod` and learn about another command `chown` that works in a similar way. Change the permissions on files using both methods.

## Additional Resources

- [Linux file permissions](https://www.booleanworld.com/introduction-linux-file-permissions/)
- [More linux permissions (might not include in final version)](https://www.digitalocean.com/community/tutorials/an-introduction-to-linux-permissions)