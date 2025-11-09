# FZF fuzzy searcher

[FZF Github website](https://github.com/junegunn/fzf)

[FZF Documentation](https://junegunn.github.io/fzf/getting-started/)

## Installation

``fzf`` is usually installed on Linux but it is an older version of ``fzf``.

Run.

```bash
	fzf --version
```

If the version is lower than 0.48.0 uninstall ``fzf`` with.

```bash
	sudo apt remove fzf
```

Now install it with.

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
 ~/.fzf/install --all
```

This will install the latest version and add the following key bindings into ``.bashrc``.

```bash
# Set up fzf key bindings and fuzzy completion
[ -f ~/.fzf.bash ] && source ~/.fzf.bash
```

Now add a different layout to ``fzf``. Add this before the keybindings.

```bash
# Set up fzf key bindings and fuzzy completion
set FZF_DEFAULT_OPTS "--layout=reverse --border=bold --border=rounded --margin=3% --color=dark"
[ -f ~/.fzf.bash ] && source ~/.fzf.bash
```

## Key bindings

**Alt-C** -- fuzzy directory search

**Ctrl-T** -- fuzzy filename search

**Ctrl-R** -- fuzzy history search

## Run a command using fuzzy search

The first command is.

```bash
	fzf
```

This will recursively search for all files in your folder structure.

You can type the name of the file you are looking for and this will reduce the number of files you are seeing in the list.

``fzf`` gives us back the filename we were searching for.

Getting the filename back is not too useful. A better way to do this is

```bash
	nano $(fzf --preview='batcat --color=always {}')
```


Type in ``nano`` at the command line.

> nano

Then.

Ctrl-T

This will show you the fuzzy file listing and you can select a file. When you hit enter it will leave the full command on the file line, e.g.

```bash
	nano .bashrc
```

Hit enter again and it will run the selected file in nano.

Another way to do this is.

```bash
	nano $(fzf)
```

Select your file and it will run it in batcat.

Or 

```bash
	nano $(fzf)
```

## Select a folder

You can use ``**`` to find a directory. For example.

```bash
	cd /etc/**<TAB>
```

This will open ``fzf`` at the root of the /etc/ directory. Now you can type to get to exactly where you want to go.

Or you can do a ``cd **<TAB>`` in your current directory and it will do a list of all sub directories and allow you to select one.

## Batch commands in ``fzf``

command ls -a | fzf | xargs -I{} batcat {}

find . -type f | fzf --multi | xargs -I{} head -n 10 {}

fzf --preview 'cat {}'

command ls -a *.cs | fzf --preview 'cat {}'

command ls -a *.cs | fzf --preview 'batcat --color=always {}'

fzf --preview 'batcat --colors=always {}'

## Killing processes

```bash
	kill -9 **
```

After you type this hit the ``Tab`` to bring up a list of your processes.

Search for the process you want to kill.

## Search a Git repository for commits

Go into a repository and run.

```bash
git log --oneline | fzf --preview 'git show --name-only {1}'
```

dosen't work
export FZF_DEFAULT_COMMAND='ag --hidden --ignore .git -l -g ""'

## Check an alias

```bash
type gcom
```

Returns.

> gcom is aliased to `git log --oneline | fzf --preview "git show --name-only {1}"'

## Create an alias and use it

```bash
alias pf='fzf --preview="less {}" --bind shift-up:preview-page-up,shift-down:preview-page-down'
```

Add this to ``.bashrc`` and run.

```bash
	source .bashrc
```

Now run.

```bash
	nano $(pf)
```

This will open fzf and allow you to move through the files. On the right hand side you will see the current file text.

You can use Shift-PageUp or Shift-PageDown to move through the file a page at a time.

## Using Vim

You can open the file list in Vim and select a file to edit.

```bash
	vim .
```

To quit Vim.

Esc
:wq

## Search through a list of man pages

```bash
	man -k . | fzf
```
Use this command to open the selected man page.

```bash
	man -k . | fzf | awk '{print $1}' | xargs -r man
```

## Limiting the depth of your fzf search

```bash
	find . -maxdepth 1 | fzf
```

Will only search the current directory.

If I use this command on ``/mnt/g/Training``.

```bash
	find . -maxdepth 2 | fzf
```

It will only find the Training provider and a list of its courses.

An example.

> ./Dometrain/Dometrain - Deep Dive C#
