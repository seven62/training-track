# Basic Unix Usage

## History

### Early On
- developed my ATT employees at Bell Labs (1969-70)
- MULTICS project too large, cancelled
- side project continued, single user "Unics"
- multi-user support, became "Unix"

### Assembly to C  
- rewritten in C language (1972)
- C allowed hardware independence

### Unix Spreading (1975)
- free liscensing and source
- corporations, university, govt

### Branching Out (1977- )
- open source - BSD, Linux
- closed source - Solarix, HP/UX
- mixed source - macOS

#### macOS
- BSD + NeXTSTEP + Apple = Darwin
- Finder / System Pref interface with Unix
- primary weapon = Terminal

#### Terminal
- why?  all this sexy hardware and sticky slicky GUI for what?
- command line access to Unix
- location: /Applications/Utilities/Terminal
```command + space
``` to launch Spotlight, type "Terminal"
- understanding the prompt:  ```host:currentdir user$
```
- basic command structure - command option argument
``` banner -w 50 hello
```

#### Man Pages  <-----------------
- there is a manual for every system command
- access by: "man command_your_using"
- space - next page
- f / b - forward / back
- q - quit
- /searchstring - searching!

#### Kernel / Shell
- Kernel - core of OS
- gives time / memory to Applications
- macOS - Mach kernel
- Shell - outer layer of OS
- user interaction (Terminal)
- sends req to kernel
- macOS default is Boure Again SHell
- show current - ```echo $0
```

## Filesystem
- hierarchical "tree"
- case sensitive
- /// FORWARD /// slashes
- file extensions "don't matter"
- hidden files - begin with a "."
- absolute vs. relative path

### Navigation
- working dir - your current position in the filesystem "pwd"
- listing files / contents - "ls"
- cd - change directory

### Organization
/ root
/bin binaries, programs
/sbin system biniaries, system programs
/dev devices ie input devices
/etc system configurations
/home user directories
/lib libraries of code
/tmp temporary
/var various, files system uses
/usr
/usr/... user apps, tools, libraries
macOS /Applications - programs
macOS /Library - libraries of code
macOS /Network - networked devices
macOS /System - macOS
macOS /Users - user home directories
macOS /Volumes - mounted volumes

## Working with Files / Dirs  
### Naming Files  
- case sensitive (capitals are dumb)
- spaces are dumb (important_file.txt)
- extensions are nice, not required
- avoid characters (/\.#$ etc)

### Creating / Reading Files
- touch command
- text editor of choice (vim, nano, emacs)
- cat, more, less, head tail
- mkdir (-p -v)
- mv - move files / folders
- mv - *also* renames files: `mv current.name new.name`  
- cp - copy file / dir
- rm / rmdir - remove (delete) file / dir

#### Aliases / Links
- macOS finder alias do not work like unix links!
- hard links - `ln filetolink hardlink`
- hard links do not break if source file is moved / deleted  
- symbolic links `ln -s filetolink symlink`
- symlinks do break

#### Searching
- Spotlight and Unix
- find command:  `find path expression`

















# Appendix

## Important Aliases
- . - "here" or current working directory
- ~ - current user's home directory
- .. - up one level from current directory

## Commands - Getting Started
- pwd - print working directory
- ls - list contents
- cd - change directory
- "cd -" - toggle to your previous directory
- pushd / popd - create dir stack
- man -k / apropos - search whatis DB for string
- touch - create a new (empty) file
- tree - displays dir contents in tree view
- cat - outputs contents of file on screen
- more - like cat, one page at a time
- less - less > more
- head - display lines from beginning of file
- tail - display lines from end of file (-f follow, awesome for logs)
- mkdir - make directory
- mv - move / rename files
- cp - copy file / dir
- rm / rmdir - remove (delete) file / dir
- ln - hard link
- ln -s - symbolic link  
- find - search for things








## Keyboard Shortcuts
TAB - command OMGautomatic completion
up arrow / down arrow - access previous command history
CMD + K - clear terminal screen
CTRL + A - move cursor to start of line
CTRL + E - move cursor to end of line
CTRL + U - clear all text left of cursor
OPT + click - move cursor to click point
