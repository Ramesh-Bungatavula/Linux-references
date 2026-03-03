# 🚀 Linux Commands for file search & for more productivity 

![Linux](https://img.shields.io/badge/Platform-Linux-blue?logo=linux)
![Shell](https://img.shields.io/badge/Shell-Bash-green)
![License](https://img.shields.io/badge/License-MIT-yellow)
![Maintained](https://img.shields.io/badge/Maintained-Yes-brightgreen)

------------------------------------------------------------------------
## 📚 Table of Contents

- 📁 [Directory & Navigation](#-directory--navigation)
- 🔎 [Finding Files & Directories](#-finding-files--directories)
- 📄 [File Content Processing](#-file-content-processing)
- 📦 [Copying & Synchronizing Files](#-copying--synchronizing-files)
- 🚀 [Other Day-to-Day Useful Commands](#-other-day-to-day-useful-commands)
- 🔥 [Command Chaining](#-command-chaining)

------------------------------------------------------------------------

## 📁 Directory & Navigation

| Command        | Description                                   | Example         |
|---------------|-----------------------------------------------|-----------------|
| `ls`          | List files and directories                    | `ls`            |
| `ls -la`      | List all files including hidden with details  | `ls -la`        |
| `ls -R`       | Recursively list all subdirectories           | `ls -R`         |
| `pwd`         | Show current directory path                   | `pwd`           |
| `cd folder/`  | Change directory                              | `cd projects/`  |

------------------------------------------------------------------------

## 🔎 Finding Files & Directories

| Command | Description |
|---------|-------------|
| `find . -type d` | List directories in the current directory |
| `find . -type f` | List files in the current directory |
| `find . -type f -name "sample.txt"` | Find a file by exact name and print the full path |
| `find . -type f -name "*.txt"` | Find all files with `.txt` extension |
| `ls -R \| grep "sample.txt"` | Search for a filename in current and subdirectories |
| `locate sample.txt` | Quickly search for a file using the indexed database |

------------------------------------------------------------------------

## 📄 File Content Processing

| Command | Description |
|---------|-------------|
| `cat file.txt` | Display file contents |
| `find . -name "*.txt" \| xargs cat` | Show contents of all `.txt` files |
| `find . -name "*.txt" \| xargs cat \| grep "ERROR"` | Search for `"ERROR"` inside all `.txt` files |
| `... \| sort -k4` | Sort output by the 4th column |
| `... \| uniq -f3` | Show unique lines while ignoring the first 3 fields |
| `... > errors.log` | Redirect output to `errors.log` instead of terminal |


------------------------------------------------------------------------

### About `xargs`

Converts standard input into arguments for another command.

``` bash
find . -name "*.txt" | xargs cat
```

------------------------------------------------------------------------

## 📦 Copying & Synchronizing Files

| Command | Description |
|---------|-------------|
| `find . -name "*.log" -exec cp {} ./backup \;` | Copy all `.log` files to the `backup` directory |
| `find . -name "*.txt" -exec rsync -R {} ./backup \;` | Copy `.txt` files while preserving directory structure |
| `rsync -av source/ destination/` | Efficiently synchronize files and directories |

------------------------------------------------------------------------

### About `rsync`

Efficiently copies and syncs files by transferring only changed data.

------------------------------------------------------------------------

## 🚀Other day-to-day useful Commands 

| Command | Description |
|---------|-------------|
| `history` | Show command history |
| `!!` | Repeat last command |
| `!42` | Execute command number 42 from history |
| `Ctrl + R` | Reverse search command history interactively |
| `watch -n 2 df -h` | Run `df -h` every 2 seconds |
| `alias ll='ls -la'` | Create a shortcut (alias) for a command |
| `du -sh *` | Show size of directories/files in current path |
| `df -h` | Display disk usage in human-readable format |
| `free -h` | Show memory usage in human-readable format |
| `top` | Real-time process monitoring |
| `htop` | Enhanced interactive system monitor (if installed) |
| `chmod +x script.sh` | Make a script executable |
| `grep -r "text" .` | Search for text recursively in current directory |
| `tar -czvf archive.tar.gz folder/` | Compress a directory into a `.tar.gz` archive |
| `unzip file.zip` | Extract a `.zip` archive |
------------------------------------------------------------------------

## 🔥 Command Chaining

| Pattern | Description |
|---------|-------------|
| `command1 && command2` | Run the second command only if the first succeeds |
| `command1 \|\| command2` | Run the second command only if the first fails |
| `command1 \| command2` | Pipe output of the first command into the second |
| `command > file` | Redirect output to a file (overwrite) |
| `command >> file` | Append output to a file |

``` bash
mkdir project && cd project
```

------------------------------------------------------------------------

------------------------------------------------------------------------

# License

MIT License
