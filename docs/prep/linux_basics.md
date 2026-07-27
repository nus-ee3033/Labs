# Linux Command-Line Basics

If this is your first time using a terminal, don't worry — you only need a small set of commands to survive this course. This page covers what you'll actually use in Labs 1–6 and the ROS2 lectures.

!!! info
    You'll mostly be working inside Ubuntu Desktop, everything below applies there.

## Why command line at all?

Linux and ROS2 tools (`ros2 run`, `colcon build`, `rosdep`, etc.) don't always have a full graphical interface. The terminal *is* the interface. Once you get comfortable with a dozen commands, it becomes much faster than clicking through folders.

## Opening a terminal

- **Ubuntu Desktop / VM**: look for a terminal icon, or right-click the desktop → "Open Terminal Here"
- **WSL on Windows**: open PowerShell and type `wsl`, or launch the "Ubuntu" app from the Start menu

## Navigating the file system

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

!!! question "Why does `ros2` say 'command not found' sometimes?"
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

- [Ubuntu's official command-line tutorial](https://ubuntu.com/tutorials/command-line-for-beginners)
- Search any error message you see — Linux/ROS2 error messages are usually specific enough to find a fix quickly
