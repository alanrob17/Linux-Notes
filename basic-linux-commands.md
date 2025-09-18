# Basic Linux commands

## cat

Cat will scroll off the screen. To stop this happening use.

```bash
    cat .bashrc | less
```

This will show the first page of information and then allow you to use the arrow keys, Page Up or Page Down or use the Home or End keys. Type ``q`` to quit.

## chmod

The chmod command sets the file permissions flags on a file or folder. The flags define who can read, write to or execute the file. When you list files with the ``-l`` (long format) option you'll see a string of characters that look like

> -rwxrwxrwx

If the first character is a ``-`` the item is a file, if it is a ``d`` the item is a directory. The rest of the string is three sets of three characters. From the left, the first three represent the file permissions of the **owner**, the middle three represent the file permissions of the **group** and the rightmost three characters represent the permissions for **others**. In each set, an ``r`` stands for *read*, a ``w`` stands for **write**, and an ``x`` stands for **execute**.

If the ``r``, ``w``, or ``x`` character is present that file permission is granted. If the letter is not present and a ``-`` appears instead, that file permission is not granted.

One way to use ``chmod is`` to provide the permissions you wish to give to the owner, group, and others as a 3 digit number. The leftmost digit represents the **owner**. The middle digit represents the **group**. The rightmost digit represents the **others**. The digits you can use and what they represent are listed here:

> 0: No permission      
> 1: Execute permission     
> 2: Write permission       
> 3: Write and execute permissions      
> 4: Read permission        
> 5: Read and execute permissions       
> 6: Read and write permissions     
> 7: Read, write and execute permissions

I create a new text file named ``example.txt``.

It will have these permissions.

> -rw-r--r-- 1 alanr alanr 0 Sep 10 14:34 examples.txt

Each group has different permissions. I want to change this so owner (user) and group have read and write permissions and leave others to have read permissions.

```bash
    chmod 664 example.txt
```

Will change ``example.txt`` permissions to.

> -rw-rw-r-- 1 alanr alanr 0 Sep 10 14:34 examples.txt

### Symbolic mode

 The  format of a symbolic mode is ```[ugoa...][[-+=][perms...]...]```, where perms is either zero or more letters from the set ``rwxXst``, or a single letter from the set ``ugo``.  Multiple symbolic modes can be given, separated  by commas.

A combination of the letters ``ugoa`` controls which users' access to the file will be changed: the user who owns it (``u``), other users in the file's group (``g``), other users not in the file's group (``o``), or all  users  (``a``).   If none of these are given, the effect is as if (``a``) were given, but bits that are set in the umask are not affected.

The operator **+** causes the selected file mode bits to be added to the existing file mode bits of each  file; **-** causes them to be removed; and **=** causes them to be added and causes unmentioned bits to be removed except that a directory's unmentioned set user and group ID bits are not affected.

Modify all permissions to read-write.

```bash
    chmod a+rw example.txt
```

Where ``a`` means all.

I could also remove permissions with.

```bash
    chmod a+r-w example.txt
```

This removes the write permissions but allows all users to read the text file.

If I decide I want to restrict write permissions for others I can do this.

```bash
    chmod ug+rw,o+r-w example.txt
```

Returns.

> -rw-rw-r-- 1 alanr alanr 76 Sep 10 17:51 example.txt

## chown

The ``chown`` command allows you to change the **owner** and **group** owner of a file. Listing our ``example.txt`` file with ls -l we can see *alanr* *alanr* in the file description. The first of these indicates the name of the file owner, which in this case is the user *alanr*. The second entry shows that the name of the group owner is also *alanr*. Each user has a default group created when the user is created. That user is the only member of that group. This shows that the file is not shared with any other groups of users.

You can use ``chown`` to change the owner or group, or both of a file. You must provide the name of the owner and the group, separated by a **:** character. You will need to use **sudo**. To retain dave as the owner of the file but to set mary as the group owner, use this command:

```bash
    sudo chown root:root example.txt
```

Returns.

> -rw-rw-r-- 1 root root 0 Sep 10 14:34 example.txt

Now if I go to add text to this file I can't because only **root** has read-write access.

# curl

The ``curl`` command is a tool to retrieve information and files from Uniform Resource Locators (URLs) or internet addresses.

```bash
    curl https://raw.githubusercontent.com/torvalds/linux/master/kernel/events/core.c -o core.c
```

This command retrieves the file for us. Note that you need to specify the name of the file to save it in, using the ``-o`` (output) option. If you do not do this, the contents of the file are scrolled rapidly in the terminal window but not saved to your computer.

## df

The ``df`` command shows the size, used space, and available space on the mounted filesystems of your computer.

Two of the most useful options are the ``-h`` (human readable) and ``-x`` (exclude) options. The human-readable option displays the sizes in Mb or Gb instead of in bytes. The exclude option allows you to tell ``df`` to discount filesystems you are not interested in. For example, the ``squashfs`` pseudo-filesystems that are created when you install an application with the snap command.

```bash
    df -h -x squashfs
```

## diff

The ``diff`` command compares two text files and shows the differences between them. There are many options to tailor the display to your requirements.

The ``-y`` (side by side) option shows the line differences side by side. The ``-w`` (width) option lets you specify the maximum line width to use to avoid wraparound lines. The two files are called *alpha1.txt* and *alpha2.txt* in this example. The ``--suppress-common-lines`` prevents diff from listing the matching lines, letting you focus on the lines which have differences.

```bash
diff -y -W 70 alpha1.txt alpha2.txt --suppress-common-lines
```

## echo

The ``echo`` command prints (echoes) a string of text to the terminal window.

The command below will print the words "A string of text" on the terminal window.

```bash
    echo A string of text
```

The ``echo`` command can show the value of environment variables, for example, the ``$USER``, ``$HOME``, and ``$PATH`` environment variables. These hold the values of the name of the user, the user's home directory, and the path searched for matching commands when the user types something on the command line.

```bash
    echo $USER
```

## find

Use the ``find`` command to track down files that you know exist if you can't remember where you put them. You must tell ``find`` where to start searching from and what it is looking for. In this example, the **.** matches the current folder and the ``-name`` option tells find to look for files with a name that matches the search pattern. Use ``-iname`` for case insensitive searches.

You can use wildcards, where ``*`` represents any sequence of characters and ``?`` represents any single character. We're using ``*.cs`` to match any file name containing the sequence "ones." This would match words like bones, stones, and lonesome.

```bash
    find . -iname *.cs
```

You can limit your search results by specifying the **type** you are searching for. **f** for files or **d** for directory.

```bash
    find . -type d  -iname Debug
```

Returns.

> ./source/dotnetapp/bin/Debug      
> ./source/dotnetapp/obj/Debug      
> ./source/app/node_modules/istanbul-lib-source-maps/node_modules/debug     
> ...

# finger

The ``finger`` command gives you a short dump of information about a user, including the time of the user's last login, the user's home directory, and the user account's full name.

```bash
    finger alanr
```

Returns.

```bash
Login: alanr                            Name:     
Directory: /home/alanr                  Shell: /bin/bash      
On since Wed Sep 10 14:14 (AEST) on pts/1   2 hours 9 minutes idle        
     (messages off)       
No mail.      
No Plan.
```

# free

The ``free`` command gives you a summary of the memory usage with your computer. It does this for both the main Random Access Memory (RAM) and swap memory. The ``-h`` (human) option is used to provide human-friendly numbers and units. Without this option, the figures are presented in bytes.

```bash
    free -h
```

Returns.

```bash
               total        used        free      shared  buff/cache   available      
Mem:            15Gi       439Mi        14Gi       3.0Mi       437Mi        14Gi      
Swap:          4.0Gi          0B       4.0Gi
```

## grep

https://www.geeksforgeeks.org/linux-unix/grep-command-in-unixlinux/

## groups

The ``groups`` command tells you which groups a user is a member of.

```bash
    groups alanr
```

Returns.

> alanr : alanr adm dialout cdrom floppy sudo audio dip video plugdev netdev docker ollama sales

## Creating a tar archive

The ``tar`` command in Linux is a powerful utility used for archiving and compressing files and directories. The name "tar" stands for "tape archive," reflecting its original purpose of writing data to tape drives. However, it has evolved to become a versatile tool for managing file collections in various formats.

### Commonly used options with tar command

| Options | Description |
|---|---|
| **-c** | Create a new archive file. |
| **-x** | Extract files from an archive. |
| **-v** | Verbosely list files processed. |
| **-f** | Specify the name of the archive file. |
| **-t** | List the contents of an archive without extracting. |
| **-z** | Compress the archive using gzip. |
| **-j** | Compress the archive using bzip2. |
| **-J** | Compress the archive using xz. |
| **--exclude** | Exclude files or directories from the archive. |

### Creating a tar Archive

To create a tar archive of a directory named ``examples``, you can use the following command:

```bash
    tar -cvf examples.tar examples
```

This command creates a tar archive named ``examples.tar`` containing the contents of the ``examples`` directory. The ``-c`` option is for creating an archive, ``-v`` enables verbose output, and ``-f`` specifies the name of the archive file.

### Extracting a tar Archive

To extract the contents of a tar archive named ``examples.tar``, you can use the following command:

```bash
    tar -xvf examples.tar
```

This command extracts the contents of the ``examples.tar`` archive into the current directory. The ``-x`` option is for extracting files, ``-v`` enables verbose output, and ``-f`` specifies the name of the archive file.

### Creating a Compressed tar Archive

To create a compressed tar archive using gzip, you can use the following command:

```bash
    tar -czvf examples.tar.gz examples
```

This command creates a gzip-compressed tar archive named ``examples.tar.gz`` containing the contents of the ``examples`` directory. The ``-z`` option enables gzip compression.

### Extracting a Compressed tar Archive

To extract the contents of a gzip-compressed tar archive named ``examples.tar.gz``, you can use the following command:

```bash
    tar -xzvf examples.tar.gz
```

This command extracts the contents of the ``examples.tar.gz`` archive into the current directory. The ``-x`` option is for extracting files, ``-z`` enables gzip decompression, ``-v`` enables verbose output, and ``-f`` specifies the name of the archive file.

## Creating a gzip archive

https://www.geeksforgeeks.org/linux-unix/gzip-command-linux/

The ``gzip`` command in Linux is a vital tool for compressing files and reducing their sizes. It employs the **DEFLATE** compression algorithm, which makes it highly effective for compressing large files or preparing data for transfer over networks.

There are other archiving formats but ``gzip`` compresses the best.

| Options | Description |
|---|---|
| **-f** | Forcefully compress a file even if a compressed version with the same name already exists. |
| **-k** | Compress a file and keep the original file, resulting in both the compressed and original files. |
| **-L** | Display the gzip license for the software. |
| **-r** | Recursively compress all files in a folder and its subfolders. |
| **-v** | Display the name and percentage reduction for each file compressed or decompressed. |
| **-d** | Decompress a file that was compressed using the gzip command. |

### Basic Compression using gzip Command in Linux

To compress a file named ``mydoc.txt``, the following command can be used:

```bash
    gzip mydoc.txt    
```

This command will create a compressed file named ``mydoc.txt.gz`` and remove the original ``mydoc.txt`` file.

If you don't want to remove the original file, use the ``-k`` option:

```bash
    gzip -k mydoc.txt    
```

This command will create a compressed file named ``mydoc.txt.gz`` while keeping the original ``mydoc.txt`` file intact.

### How to Decompress a gzip File in Linux

The basic syntax of the ``gzip`` command for decompressing a file is as follows:

```bash
    gzip -d filename.gz
```

This command decompresses the specified ``gzip`` file, leaving the original uncompressed file intact.

### Recursively Compressing Files using gzip Command

To compress all files in a directory and its subdirectories, the ``-r`` option can be used:

```bash
    gzip -r examples
```

This will compress all files into separate .gz files in the ``examples`` directory and its subdirectories. A far better way to do this is to create a tar archive and then compress that.

```bash
    tar -cvf examples.tar examples
    gzip examples.tar
```

This creates a single compressed file named ``examples.tar.gz``.

### Force Compression Using gzip Command

In cases where the compressed file already exists, the ``-f`` option forcefully overwrites it:

```bash
    gzip -f example.txt
```

This command compresses ``example.txt``, overwriting any existing ``example.txt.gz`` file.

### Compressing Multiple Files Using gzip Command

``Gzip`` can compress multiple files simultaneously by providing their names as arguments:

```bash
    gzip file1.txt file2.txt file3.txt
```

This command compresses "file1.txt," "file2.txt," and "file3.txt" individually.

### Compressing a set of files in a directory

I have a number of ``.sh`` scripts in a directory as well as other files. I want to compress all of the ``.sh`` files into an archive file named scripts.

```bash
    tar -czf scripts.tar.gz *.sh
```

To check the files in the archive use.

```bash
    tar -tzf scripts.tar.gz
```

## Compressing with zip

``zip`` is installed on Ubuntu. We can use this when we are sending files across Linux, MacOS and Windows.

```bash
    zip -r examples.zip examples/
```

This command creates a zip archive named ``examples.zip`` containing the contents of the ``examples`` directory. The ``-r`` option is for creating an archive recursively, including all files and subdirectories within the specified directory.

### Extracting a zip Archive

To extract the contents of a zip archive named ``examples.zip``, you can use the following command:

```bash
    unzip examples.zip
```

This command extracts the contents of the ``examples.zip`` archive into the current directory.

I can extract these to another directory with the ``-d`` option.

```bash
    unzip examples.zip -d extracted
```

## head

The head command gives you a listing of the first 10 lines of a file. If you want to see fewer or more lines, use the ``-n`` (number) option. In this examples, we use head with its default of 10 lines. We then repeat the command asking for only five lines.

Print the first 10 lines.

```bash
    head examples.txt
```

Print the first 5 lines.

```bash
    head -n5 examples.txt
```

## less

The ``less`` command allows you to view files without opening an editor. It's faster to use, and there's no chance of you inadvertently modifying the file. With less you can scroll forward and backward through the file using the Up and Down Arrow keys, the PgUp and PgDn keys and the Home and End keys. Press the Q key to quit from less.

```bash
    less .bashrc
```

**Note:** it is better to use ``less`` than ``cat`` for large files. Using ``cat`` will scroll the whole file whereas with ``less`` you only see the first page of the file.

You can also pipe the output from other commands into ``less``. To see the output from ``ls`` for a listing of your entire hard drive, use the following command:

```bash
    ls -R / | less
```

While you are looking through the content you can search for text using the ``/`` (backslash) followed by the search term. You can go backwards with the ``?`` and add the search term.

## Redirection

Most of the redirection we have seen is output to the screen. We have also seen output through pipes to other commands. 

Text in a shell travels through on of three streams.

0. Standard Input (``stdin``) - this is usually the keyboard (keyboard or text input)
1. Standard Output (``stdout``) - this is usually the screen (text output)
2. Standard Error (``stderr``) - this is usually the screen (error messages)

### Output to stdout

```bash
    ls 1> filelist.txt
```

This command lists the files in the current directory and redirects the output (``stdout``) to a file named ``filelist.txt``. The ``1>`` operator specifies that the standard output (file descriptor 1) should be redirected to the specified file.

### Output to stderr

```bash
    ls non_existent_file 2> errorlog.txt
```

Returns.

> ls: cannot access 'non_existent_file': No such file or directory

This command attempts to list a non-existent file, which generates an error message. The ``2>`` operator redirects the standard error (file descriptor 2) to a file named ``errorlog.txt``.

### Output to both stdout and stderr

```bash
    ls existing_file non_existent_file > output.txt 2>&1
```

**Note:** I can leave of the ``1`` in front of the ``>`` redirection operator because ``stdout`` is the default.

This command lists both an existing file and a non-existent file. The standard output is redirected to ``output.txt``, and the standard error is also redirected to the same file using ``2>&1``.

### Multiple redirections

Redirect both standard output and standard error to different files.

```bash
    cp -r myfiles ~/files_backup > success.txt 2> errorlog.txt
```

This command copies the directory ``myfiles`` to a backup location in the user's home directory. The standard output (success messages) is redirected to ``success.txt``, while any error messages are redirected to ``errorlog.txt``.

Redirect standard output and standard error to the same file.

```bash
    cp -r myfiles ~/files_backup > status.txt 2>&1
```

Or.

```bash
    cp -r myfiles ~/files_backup &> status.txt
```

This command copies the directory ``myfiles`` to a backup location in the user's home directory. Both standard output and standard error are redirected to a single file named ``status.txt``.

**Note:** be careful with redirecting files.

```bash
 >filelist.txt
```

This command creates an empty file named ``filelist.txt`` or truncates the file if it already exists. Any existing content in the file will be lost.

### Appending to files

You can append text to a file without overwriting its existing content using the ``>>`` operator.

```bash
    echo "New line of text" >> myfile.txt
```

This command appends the text "New line of text" to the end of the file named ``myfile.txt``. If the file does not exist, it will be created.

## Exploring environment variables and PATH

The shell we are using has a few variables or parameters that affect how the shell works. You can see these variables by using the ``printenv`` command.

```bash
    printenv
```

Or.

```bash
    env
```

This produces a fairly big list of information but there are a few variables to note.

| Variable | Description |
|---|---|
| **HOME** | This variable holds the path to your home directory. |
| **PATH** | This variable holds a list of directories that the shell searches through when you type a command. |
| **USER** | This variable holds your username. |
| **SHELL** | This variable holds the path to the shell you are using. |
| **LANG** | This variable holds the language and locale settings for your system. |
| **PWD** | This variable holds the path to your current working directory. |
| **OLDPWD** | This variable holds the path to your previous working directory. |
| **TERM** | This variable holds the type of terminal you are using. |
| **EDITOR** | This variable holds the path to the default text editor for your system. |
| **HISTSIZE** | This variable holds the number of commands to remember in your command history. |
| **PS1** | This variable holds the primary prompt string, which is displayed when the shell is ready to accept a command. |

You can see the value of a specific variable by using the ``echo`` command followed by the variable name prefixed with a dollar sign (``$``).

```bash
    echo $PATH
```

This is the most important to us. We can run commands like ``ls``, ``cd``, and ``cat`` because the shell looks in the directories listed in the PATH variable to find these commands.

To find out where a command is located use the ``which`` command.

```bash
    which ls
``` 

Returns.

> /usr/bin/ls

### Editing the PATH variable

You can add your own directories to the PATH variable by editing the ~/.bash_profile or ~/.bashrc file. You can use any text editor to do this. Here we use ``nano``.

```bash
    nano ~/.bash_profile
```

This file may or may not exist.

Add a line like this to the end of the file.

```bash
    export PATH=$PATH:/home/alanr/scripts
```

This line adds the directory ``/home/alanr/scripts`` to the existing PATH variable. The ``$PATH`` at the beginning ensures that the current contents of the PATH variable are preserved, and the new directory is appended to it.

## Challenge

We have a set of archived logs in a file named **log.tar.gz**. Extract this and you will find a single file named **auth.log**.

We need to search through this file and find all entries with the text ``Disconnected from invalid user``. This will give us lines of text with suspicious logins.

In this list there are user names and we want to extract a sorted and unique list of users.

### Step 1

Extract the files from **log.tar.gz**.

```bash
    tar -xzf log.tar.gz
```

This extracts the file **auth.log**.

### Step 2

Use grep to extract the lines containing the phrase ``Disconnected from invalid user``.

```bash
    grep "Disconnected from invalid user" auth.log
```

Returns lines in this format.

> 2025-07-16T22:44:16.114657+00:00 linux-pc sshd[312540]: Disconnected from invalid user dayton 80.94.95.15 port 49788 [preauth]

### Step 4

You can see in the result above that the user name is in the 8th column and this is the same for all entries.

Now use pipes and redirection to sort a unique list of names from auth.log with phrase above.

```bash
grep "Disconnected from invalid user" auth.log | awk '{print $8}' | sort -f -u > users.txt
```

### Step 5

Count the number of illegal user names.

```bash
    wc -l users.txt
```

Returns.

> 467 users.txt

There a 467 unique illegal users.

## Find information about your Linux distribution

Look for information about your system with.

```bash
    ls -l /etc/*release
```

Returns

> -rw-r--r-- 1 root root 104 Aug  2 00:21 /etc/lsb-release      
> lrwxrwxrwx 1 root root  21 Aug  2 00:21 /etc/os-release -> ../usr/lib/os-release

### /etc/lsb-release

> DISTRIB_ID=Ubuntu     
> DISTRIB_RELEASE=24.04     
> DISTRIB_CODENAME=noble        
> DISTRIB_DESCRIPTION="Ubuntu 24.04.3 LTS"

### /etc/os-release

> PRETTY_NAME="Ubuntu 24.04.3 LTS"      
> NAME="Ubuntu"     
> VERSION_ID="24.04"        
> VERSION="24.04.3 LTS (Noble Numbat)"      
> VERSION_CODENAME=noble        
> ID=ubuntu     
> ID_LIKE=debian        
> HOME_URL="https://www.ubuntu.com/"        
> SUPPORT_URL="https://help.ubuntu.com/"        
> BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"       
> PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"       
> UBUNTU_CODENAME=noble     
> LOGO=ubuntu-logo

What version of the Linux kernel are we using.

```bash
    uname -a
```

Returns.

> Linux TIGER 6.6.87.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun  5 18:30:46 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux

This information can be useful for troubleshooting.

## Find system hardware and disk information

Find out how much memory a system has.

```bash
    free -h
```

Returns.

```bash
               total        used        free      shared  buff/cache   available
Mem:            15Gi       579Mi        14Gi       3.5Mi       563Mi        15Gi
Swap:          4.0Gi          0B       4.0Gi
```

Find information about your system.

```bash
    lscpu
```

Returns.

```bash
Architecture:             x86_64
  CPU op-mode(s):         32-bit, 64-bit
  Address sizes:          39 bits physical, 48 bits virtual
  Byte Order:             Little Endian
CPU(s):                   16
  On-line CPU(s) list:    0-15
Vendor ID:                GenuineIntel
  Model name:             Intel(R) Core(TM) i7-10700K CPU @ 3.80GHz
    CPU family:           6
...
```

Or you can find the same information in.

```bash
    cat /proc/cpuinfo
```

To find out how much disk space a system has.

```bash
    df -h
```

Returns.

```bash
Filesystem      Size  Used Avail Use% Mounted on
none            7.8G     0  7.8G   0% /usr/lib/modules/6.6.87.2-microsoft-standard-WSL2
none            7.8G  4.0K  7.8G   1% /mnt/wsl
drivers         930G  457G  473G  50% /usr/lib/wsl/drivers
/dev/sdd       1007G  1.6G  955G   1% /
...
```

The most interesting drive is ``/`` because this is our system. This is where our files are kept.

Check how much disk space is used.

```bash
     sudo du -hd1 /
```

Where ``-h`` is human readable and ``d`` is what level of depth to show. In this case it is ``1``. ``/`` is from the root.

You have to use sudo because some of the directories are owned by ``root``.

Returns.

```bash
4.3M    /etc
6.9G    /usr
560K    /run
4.0K    /media
16K     /lost+found
4.0K    /boot
0       /dev
8.0K    /Docker
548M    /var
48K     /tmp
8.0K    /snap
```

We can also check what hardware the system has with.

```bash
    sudo lshw | less
```

We use ``less`` because this pumps out a large amount of information.

To see what is attached to the PCI bus.

```bash
    lscpi
```

To see what is attached to the USB bus.

```bash
    lsusb
```

To find the systems network address.

```bash
    ip a
```

You can see your primary network address from.

```bash
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1420 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:c9:2d:50 brd ff:ff:ff:ff:ff:ff
    inet 172.30.170.164/20 brd 172.30.175.255 scope global eth0
```

Returns.

> 172.30.170.164
