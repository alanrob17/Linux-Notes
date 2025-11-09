# Linux Notes

These are notes on tasks that I have learnt using Linux.

## Disc usage

### Report filesystem space usage: df

To see the current disc where your home directory exists. This command shows information about the space used and available on your filesystem.

``-h`` prints human readable results.

```bash
    df -h /home
```

Returns.

> /dev/nvme0n1p2  228G   24G  194G  11% /

### Estimate file space usage: du

Now that you know how much space is being used, you might want to figure out where that space is being used. This is where the ``du`` command comes in.

```bash
    du -hs ~
```

Returns.

> 12G     /home/alanr

## Renaming hard disks

Open the **Disks** application.

Select the drive partition that you want to change.

On the right-hand side you see the Volume and underneath that you see a black square icon. Click this to unmount the drive.

Click the next icon (additional partition items)and select ``Edit filesystem...``. Now add a name for the partition.

[Disks application](/assets/images/disks.png "Disks application")

In this case I have renamed the partition, **D**.

## Shred

This is the command I use to remove sensitive files.

```bash
    shred -zfu -n 1 file.txt
```

**Note:**

> -z overwrites all bytes with a 0.     
> -f change permissions to allow writing if necessary.      
> -u add a final overwrite with zeros to hide shredding.        
> -n 1 overwrite the file 1 time.

I have created a Bash alias for this command.

Examples.

```bash
    wipe file.txt
    wipe *.txt
```

## Clearing the history

Clear a particular entry.

```bash
    history -d <Line number>
```

Clear current session history.

```bash
    history -c
```

Now close the current terminal and reopen a terminal and you will see your previous history.

**Permanently** clear the history.

```bash
    history -c && history -w
```

An easier way is to delete ``~/.bash_history``.

## Ubuntu App Installation Location

To access your applications from anywhere on Ubuntu, you should place them in specific directories depending on your needs.

### For all users on the system

Install the application in the ``/opt`` directory, which is a standard location for third-party software not managed by the package manager.

If the application is a compiled program, you can create a wrapper script in ``/usr/local/bin`` that runs the executable. This script should be made executable with chmod +x.

The ``/usr/local/bin`` directory is included in the system's default **PATH**, allowing you to run the application by typing its name from any terminal location.

### For only your user

You can place the application files in a folder under your home directory, such as ``~/Applications`` or a hidden folder like ``~/.apps``.

To access it from the terminal, create a wrapper script in ``~/.local/bin`` (which is also in your PATH) or create an alias in your ``~/.bashrc`` file.

This approach **does not** require administrative privileges.

### For desktop integration (menu entries)

To add a menu entry for the application, place a ``.desktop`` file in ``~/.local/share/applications`` for a single user or in ``/usr/local/share/applications`` for all users.

This allows the application to appear in the graphical application menu.

### For applications installed via source code

If you compile an application from source, the standard installation prefix is often ``/usr/local``, which places the executable in ``/usr/local/bin``.

This directory is part of the default **PATH**, enabling easy terminal access.

## Tree command

The ``tree`` command is a recursive directory listing program that produces a depth-indented listing of files.

To install the ``tree`` command.

```bash
    sudo apt install tree
```
To use the ``tree`` command.

```bash
    tree -L 1 /home/alanr
```

This lists the current directory and its subdirectories up to a depth of 1 level from the **/home/alanr** directory.

Returns.

![Tree command](/assets/images/tree-command.jpg "Tree command")

## Package managers

One of the easiest ways to keep your Linux system clean and speedy is to make sure your software packages are up-to-date. Package managers, like ``apt``, ``dnf``, or ``pacman``, are your main tools for installing, updating, and removing software.

## Update your system

Over time, outdated packages can cause system slowdowns, conflicts, or even security holes. Regular updates keep everything running smoothly and ensure you’re using the most optimized versions of your software. On your specific Linux system, run:

```bash
    sudo apt update && sudo apt upgrade -y
```

These commands do two things. They fetch the latest list of available packages from your repositories and install any available updates for the packages you already have. Keeping your system updated boosts performance and ensures you're protected by the latest security patches.

### Remove unused packages

Sometimes, there could be many packages installed on your system that you don't need anymore. It's a good idea to uninstall them. You can list installed packages with this command:

```bash
    apt list --installed
```

This will list all packages installed. You can go though this list to remove packages you don't need with this command.

```bash
    sudo apt remove package-name
```

If you want to completely remove a package along with its system-wide configuration files, use:

```bash
    sudo apt purge package-name
```

As you install and uninstall software, leftover dependencies and unused packages can quietly pile up in your system. These don’t just take up disk space. They can sometimes slow down package management tasks or even cause version conflicts later on. To clean up what you don’t need anymore, you can use:

```bash
    sudo apt autoremove
```

This command scans your system for packages that were automatically installed as dependencies but are no longer required.

### Clean package cache

Every time you install or update software, your package manager saves downloaded files in a cache. Over time, those cached packages can grow to even several gigabytes, especially if you’ve been updating regularly. You can safely clear it with:

```bash
    sudo apt clean
```

## systemctl

A system that takes forever to boot or feels sluggish right after startup often has too many background services running. Some of these are essential, but others can quietly eat up resources. You can list Linux services and check which ones are enabled at startup with:

```bash
    systemctl list-unit-files --state=enabled
```

This lists all the services that launch automatically when your system boots. If you spot something you don’t need starting up every time, you can disable it safely with:

```bash
    sudo systemctl disable service-name
```

To see what’s currently running and their statuses, you can use:

```bash
    systemctl --type=service --state=running
```

By getting rid of unnecessary startup programs, you’ll get faster boot times and a system that feels lighter right from login. Just make sure not to disable anything critical.

## find: search for unused files

Sometimes the biggest space hogs aren’t obvious. Old backups, forgotten ISOs, or log files tucked deep in system folders. The find command is your all-purpose cleanup scout. It can locate files by size, age, or type, helping you decide what’s worth deleting.

To find large files (say, over 500 MB) anywhere on your system, run:

```bash
    sudo find / -type f -size +500M 2>/dev/null
```

The ``2>/dev/null`` part simply hides “permission denied” messages, so your results are easier to read. If you want to find old files you haven’t touched in months, try:

```bash
    find ~/ -type f -mtime +90
```

This lists files in your home directory that haven’t been modified in over 90 days. Used wisely, the find command is one of the most powerful ways to track down old clutter and free up space without installing anything extra.

## ps: list heavy processes

Even with plenty of free space, your system can still feel sluggish if certain programs are eating up too much CPU or memory. That’s where the ps command comes in. It helps you see what’s running and how resource-hungry each process is.

A quick way to spot the top resource users is:

```bash
    ps aux --sort=-%mem | head
```

This lists all active processes, sorts them by memory usage (highest first), and shows the top few lines. For CPU usage instead, just sort by ``%cpu``:

```bash
    ps aux --sort=-%cpu | head
```

You’ll see columns for the process owner, CPU and memory percentages, and the command that started it. If something’s hogging resources, note its PID, then you can end it with:

```bash
    sudo kill PID # Replace PID with the actual number shown in the list
```
