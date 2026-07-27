# Linux Command-Line Basics

If this is your first time using a terminal, don't worry — you only need a small set of commands to survive this course. This page covers what you'll actually use in Labs 1–6 and the ROS2 lectures.

!!! info
    You'll mostly be working inside Ubuntu (either in a VM or via WSL). Everything below applies there.

## What actually happens when you type a command

When you type something into the terminal and hit enter, you're running a program with that name. `ls`, for example, is just a small program whose job is to list the contents of your current folder. Try it now:

```bash
ls
```

Most programs accept **parameters** (also called flags or options) that change how they behave — think of them as instructions you're giving the program about what you want. There are two styles:

- a single `-` followed by one-character parameters, e.g. `-l`
- a double `--` followed by full-word parameters, e.g. `--all`

For example:

```bash
ls -l
```

The `l` parameter tells `ls` to print its output in **long format** (permissions, size, modified date, etc.) instead of just names. Try it and compare with plain `ls`.

You can also stack several single-character parameters together after one `-`. The `a` parameter tells `ls` to show **all** files, including hidden ones (any file or folder whose name starts with a `.`):

```bash
ls -la
```

Now you should see extra entries like `.bashrc` or `.config` that weren't visible before — these are hidden by default because a leading `.` tells Linux "don't show me unless asked."

!!! tip
    This `-flag` / `--flag` pattern isn't unique to `ls` — almost every command-line tool in this course, including `ros2` and `colcon`, works the same way (e.g. `colcon build --symlink-install`).

### Finding out what a command does: `man`

Every built-in command comes with a manual page. If you forget what a command or parameter does, don't guess — look it up:

```bash
man ls
```

This opens a scrollable manual (use the arrow keys, press `q` to quit). It lists every available parameter for that command with an explanation. This is often faster than searching the web, and it always matches the exact version installed on your machine.

!!! tip
    Not every command has a `man` page (e.g. some `ros2` subcommands don't). In those cases, try adding `-h` or `--help` instead, e.g. `ros2 pkg -h`.

## Why command line at all?

Linux and ROS2 tools (`ros2 run`, `colcon build`, `rosdep`, etc.) don't always have a full graphical interface. The terminal *is* the interface. Once you get comfortable with a dozen commands, it becomes much faster than clicking through folders.

## Opening a terminal

- **Ubuntu Desktop / VM**: look for a terminal icon, or right-click the desktop → "Open Terminal Here"
- **WSL on Windows**: open PowerShell and type `wsl`, or launch the "Ubuntu" app from the Start menu

## Navigating the file system

### A few symbols you'll see everywhere

Before the commands, it helps to know what these shorthand symbols mean when they appear in a path:

| Symbol | Meaning |
|---|---|
| `/` | The **root** of the entire file system — everything on the machine lives somewhere under `/`. Also used as a separator between folder names, e.g. `home/user/Documents`. |
| `~` | Your **home folder** (shorthand for something like `/home/yourusername`). Typing `cd ~` always takes you home, no matter where you are. |
| `.` | The **current** folder you're in. Mostly used when running something in the current directory, e.g. `./my_script.sh`. |
| `..` | The **parent** folder, one level up from where you are. `cd ..` moves you up one level. |

!!! tip
    You can chain `..` to go up multiple levels: `cd ../..` moves up two folders. And `~/Documents` means "the Documents folder inside my home folder," regardless of your current location.

### Commands

| Command | What it does |
|---|---|
| `pwd` | Print working directory — "where am I?" |
| `ls` | List files in the current folder |
| `ls -la` | List files, including hidden ones, with details |
| `cd foldername` | Move into a folder |
| `cd ..` | Move up one folder |
| `cd ~` or `cd` | Go to your home folder |
| `cd -` | Go back to the previous folder |

!!! tip
    Press **Tab** while typing a path to auto-complete it. This saves you from typos and is the single most useful habit to build early.

## Working with files and folders

```bash
mkdir my_folder          # create a folder
touch my_file.txt        # create an empty file
cp file.txt copy.txt     # copy a file
mv file.txt newname.txt  # rename or move a file
rm file.txt              # delete a file
rm -r my_folder          # delete a folder and its contents
```

!!! danger
    `rm` does **not** move files to a trash bin, they are **deleted immediately**. Double-check the filename before pressing enter, especially with `rm -r`.

## Viewing file contents

```bash
cat file.txt      # print the whole file
less file.txt     # scroll through a long file (press q to quit)
nano file.txt     # simple text editor inside the terminal
```

## Permissions and `sudo`

Some commands (usually installing software) require admin rights:

```bash
sudo apt update
sudo apt install ros-humble-turtlesim
```

`sudo` will ask for your password. Nothing appears on screen as you type it — that's normal, just type and press enter.

If you see `Permission denied`, it usually means the command needs `sudo`, or the file isn't marked executable (see below).

```bash
chmod +x my_script.sh   # make a file executable
```

## Running programs and stopping them

| Shortcut | Effect |
|---|---|
| `Ctrl + C` | Stop the currently running program |
| `Ctrl + Z` | Pause the current program |
| `command &` | Run a command in the background |
| ↑ / ↓ arrows | Scroll through your command history |


## Multiple terminals

Many labs (e.g. Lecture 3's `turtle_teleop_key` example) ask you to open several terminals at once — one per node. This is completely normal in ROS2: each terminal runs one program and they talk to each other in the background. Keep track of what's running in each one.

## The one ROS2-specific habit to build

Every **new** terminal needs to "source" ROS2 before any `ros2` command will work:

```bash
source /opt/ros/humble/setup.bash
```

and, inside your workspace, also:

```bash
. install/setup.bash
```

??? tip "Why does `ros2` say 'command not found' sometimes?"
    Almost always because this terminal hasn't been sourced yet. Run the two commands above and try again.

## Quick reference cheat sheet

??? tip "Click to expand: most-used commands at a glance"
    ```bash
    pwd                      # where am I
    ls -la                   # list files
    cd foldername            # move into folder
    cd ..                    # move up
    mkdir name               # new folder
    touch file.txt           # new file
    cp a.txt b.txt           # copy
    mv a.txt b.txt           # rename/move
    rm file.txt              # delete file
    rm -r folder             # delete folder
    cat file.txt             # print file
    sudo apt install pkgname # install software
    chmod +x script.sh       # make executable
    Ctrl+C                   # stop program
    ```

## Where to go for more

- [Ubuntu's official command-line tutorial](https://ubuntu.com/desktop/docs/en/latest/tutorial/the-linux-command-line-for-beginners/)
- Search any error message you see — Linux/ROS2 error messages are usually specific enough to find a fix quickly
