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
