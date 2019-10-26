---
layout  : wiki
title   : some troubleshooting
summary : 
date    : 2019-10-21 13:32:07 +0900
updated : 2019-10-21 15:43:36 +0900
tag     : bash linux
toc     : true
public  : true
parent  : Tips
latex   : false
---
* TOC
{:toc}

### lazygit config
- [src](https://stackoverflow.com/questions/19595067/git-add-commit-and-push-commands-in-one)

### clipboard confusion: ctrl v or shfit ins
- [src](https://askubuntu.com/questions/26655/how-do-you-know-when-to-use-shiftinsert-vs-ctrl-v-vs-right-click-paste-to-paste)
- [archwiki-src](https://wiki.archlinux.org/index.php/clipboard)

### Hidipi TroubleShooting
- [Spotify](https://community.spotify.com/t5/Desktop-Linux/Linux-client-barely-usable-on-HiDPI-displays/td-p/1067272)

### ouput of date: for post file
- [date in linux](https://stackoverflow.com/questions/18458839/how-to-get-the-current-date-and-time-in-the-terminal-and-set-a-custom-command-in)
- date +%Y-%m-%d-%H:%M:%S.

### navigating on Terminal
- [source](https://www.howtogeek.com/howto/ubuntu/keyboard-shortcuts-for-bash-command-shell-for-ubuntu-debian-suse-redhat-linux-etc/)
    - <C-A> : go to beginning
    - <C-E> : go to the end

- Terminal and clipboard: Xclip
    - [source](https://stackoverflow.com/questions/5130968/how-can-i-copy-the-output-of-a-command-directly-into-my-clipboard)
    - usage: 
    ```
    cat file | xclip -selection clipboard
    ```
    - this is pretty long, let's make some shortcut
    - another answer: add alias to your ~/.bashrc
    - add cclip, clipp

### markdown newline:
- shift + Enter


### deep break
- [source](http://calnewport.com/blog/2016/09/14/on-deep-breaks/)
- solves my questions about the pomodoro break
- so coffee, cleaning or reading.
- non addictive DEEP BREAK

### Toggle VScode
- [source](https://github.com/Microsoft/vscode/issues/52735)
- settings cycler
- edit settings json
- then edit keybinding
- keybindings.json (Ctrl+K Ctrl+S)

### VSCODE vim troubleShooting
- add false tag in settings.json


