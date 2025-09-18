# Install and update software with APT package manager

We use a package manager to install and update software tools on our system. Ubuntu use the ``APT`` (Advanced Package Tool) package manager.

Your Linux system has an index of packages you can install.

To see the list of packages available.

```bash
    sudo apt list
```

To see a list of installed packages.

```bash
    sudo apt list --installed
```

To view package details.

```bash
    sudo apt policy nano
```

Returns.

```bash
nano:
  Installed: 7.2-2ubuntu0.1
  Candidate: 7.2-2ubuntu0.1
  Version table:
 *** 7.2-2ubuntu0.1 500
        500 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 Packages
        500 http://security.ubuntu.com/ubuntu noble-security/main amd64 Packages
        100 /var/lib/dpkg/status
     7.2-2build1 500
        500 http://archive.ubuntu.com/ubuntu noble/main amd64 Packages
```

To search for a software package.

```bash
    apt search tree |less
```

This brings back way too much information so we pipe it into ``less``.

Returns.

```bash
...
tre-command/noble 0.4.0-5 amd64
  Tree command, improved

tree/noble 2.1.1-2ubuntu3 amd64
  displays an indented directory tree, in color

tree-ppuzzle/noble 5.3~rc16+dfsg-9build2 amd64
  Parallelized reconstruction of phylogenetic trees by maximum likelihood
...
```

You can find more information with.

```bash
    apt show tree
```

Returns.

```bash
Package: tree
Version: 2.1.1-2ubuntu3
Priority: optional
Section: universe/utils
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Florian Ernst <florian@debian.org>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 111 kB
Depends: libc6 (>= 2.38)
Homepage: http://oldmanprogrammer.net/source.php?dir=projects/tree
Task: xubuntu-desktop, lubuntu-desktop, ubuntu-mate-core, ubuntu-mate-desktop, ubuntu-budgie-desktop-minimal, ubuntu-budgie-desktop, ubuntu-budgie-desktop-raspi, ubuntu-unity-desktop, ubuntucinnamon-desktop-minimal, ubuntucinnamon-desktop-raspi
Download-Size: 47.1 kB
APT-Sources: http://archive.ubuntu.com/ubuntu noble/universe amd64 Packages
Description: displays an indented directory tree, in color
 Tree is a recursive directory listing command that produces a depth indented
 listing of files, which is colorized ala dircolors if the LS_COLORS environment
 variable is set and output is to tty.
```

To install the package.

```bash
    sudo apt install tree
```

To run ``tree``.

```bash
    tree ~/
```

To set a depth level.

```bash
    tree -L 2 /etc
```

## To upgrade your current packages

Is a two step process.

```bash
    sudo apt update -y
```

And

```bash
    sudo apt upgrade -y
```

## Remove an installed package

```bash
    sudo apt remove tree
```

This removes an installed package, but may leave configuration files behind. To Remove a package and its associated configuration files.

```bash
    sudo apt purge tree
```

Remove packages that were automatically installed to satisfy dependencies but are no longer needed.

```bash
    sudo apt autoremove
```

Reinstall a package if its installation is corrupted.

```bash
sudo apt reinstall tree
```

## Add a package repository to apt

In this example we will add the repository for Opera web browser.

This is a two step process. First you have to add the GPG key to ensure authenticity of your packages.

```bash
curl -fsSL https://deb.opera.com/archive.key | gpg --dearmor | sudo tee /usr/share/keyrings/opera.gpg > /dev/null
```

Finally, we’ll add the Opera Browser’s APT repository to our system:

```bash
sudo apt-add-repository 'https://deb.opera.com/archive.key'
```

Now, update your apt repository index to reflect the changes in the new repository. This is done with the following apt update command:

```bash
    sudo apt update
```

Now install the software with the following command:

```bash
    sudo apt install opera-stable
```

## List all dependencies of a Package

Run this command.

```bash
    sudo apt depends tree
```

Returns.

```bash
    tree
      Depends: libc6 (>= 2.38)
```

## Search for Broken Dependencies on Ubuntu

If you want to see if you have broken dependencies run.

```bash
    sudo apt-get check nano
```

Returns in my case.

> Reading package lists... Done     
> Building dependency tree... Done      
> Reading state information... Done

## Search for location of packages

You can find the location of a particular package by using the ``dpkg`` tool which is the backend of the package manager.

```bash
    sudo dpkg -L nano
```

This returns a large amount of information including where the documentation and man pages are located.

> /.        
> /etc      
> /etc/nanorc       
> /usr      
> /usr/bin      
> /usr/bin/nano     
> ...

## Clean old repository lists from APT Package Manager

You can remove old, unused repositories from Ubuntu with.

```bash
    sudo apt autoclean
```

Returns.

> Reading package lists... Done     
> Building dependency tree... Done      
> Reading state information... Done     
> Del landscape-client 24.02-0ubuntu5.3 [124 kB]        
> Del landscape-common 24.02-0ubuntu5.3 [93.0 kB]

This was the result when I ran it.
