---
layout  : wiki
title   : bash tips
summary : 
date    : 2019-10-21 13:32:07 +0900
updated : 2020-01-21 15:43:36 +0900
tag     : bash linux
toc     : true
public  : true
parent  : Tips
latex   : false
---
* TOC
{:toc}


# Misc tips
### Close Lid & Sleep
- [src](https://itsfoss.com/ubuntu-close-lid-suspend/)
### Trash in linux
```
rm -rf ~/.local/share/Trash/*
```
#### using trash-cli
```
sudo apt install trash-cli
trash-empty
```
### vscode tips
- ctrl+tab : switching tabs
- ctrl+p : shows recently opened files
- disable certain vim commands:
```
  "vim.handleKeys": {
        "<C-w>": false
    }
```
- Ctrl+shift+V: markdown preview - toggle
- Ctrl+K+V: markdown preview on the side
    - don't work well due to Vim binding
    - Just use C-Shft-P toggle and type preview on the side
- [more markdown on vscode](https://code.visualstudio.com/docs/languages/markdown#_markdown-preview)
- [Window Management](https://code.visualstudio.com/docs/getstarted/keybindings#_editorwindow-management)
- swtich between splitted window: Ctrl+1, Ctrl+2
- Toggle minimap option
### rsync
```
rsync -avzh jihyunyang@mio.mines.edu:/[some directory] ~/[some my local directory]
```
- [rsync explanation](https://www.tecmint.com/rsync-local-remote-file-synchronization-commands/)
- somehow, I still don't know how to send from remote to local.
- how local can know my ip address and so on?
### Python
- Null equivalent: None
### grep
- ls -al | grep [smthing]
- and it is case sensitive

### how to use crontab
- [submit a job in crontab](https://awc.com.my/uploadnew/5ffbd639c5e6eccea359cb1453a02bed_Setting%20Up%20Cron%20Job%20Using%20crontab.pdf)
- e.g) Setting an alarm

### naviagating in Terminal
- [source](https://askubuntu.com/questions/423529/how-to-efficiently-switch-between-several-terminal-windows-using-the-keyboard)
- Alt + Tilde(Can be moved in same application too)
- <C+shift+T>: new tab
- <alt+num>: moving tab
- <C+pgdn>: also moving tab. toggling
### Find pattern containing files
- [source](https://stackoverflow.com/questions/16956810/how-do-i-find-all-files-containing-specific-text-on-linux)
```
grep -rnw '/path/to/somewhere/' -e 'pattern'
```
### Adding Hibernation shortcut
- Test in terminal
- systemtcl suspend -i
### linux - Watch & tail
```
watch -n 5 -d cat log.txt
```
- exit by <C-C>
### linux - symbolic link
```
ln -s [orgin] [name of symlink]
```

### Navigating + Previous command
- "ctrl p"
- [src](https://www.thegeekstuff.com/2008/08/15-examples-to-master-linux-command-line-history)
- [src2](https://unix.stackexchange.com/questions/147563/how-do-i-repeat-the-last-command-without-using-the-arrow-keys)
- [src3](https://www.linux.com/tutorials/fabulous-bash-navigation-shortcuts/)
```
Up     Ctrl+P
Down   Ctrl+N
Left   Ctrl+B
Right  Ctrl+F
Home   Ctrl+A
End    Ctrl+E
Delete Ctrl+D
Clean  Ctrl+I or clear
```

```
One word back Alt+b
One word fwd  Alt+f
Exit Graphical Sesssion Ctrl+Alt+F1
```

- replay history: e.g) !1626
- cutting and pasting
```
Yank   Ctrl+Y
Delete   Ctrl+U
```


### Disable vscodeVim
- edit settings.json

### open new terminal from vscode 
- ctrl shift c
### set new volume from terminal
```
amixer sset 'Master' 50%
```
### Fixed Schedule
- [src](https://www.iwillteachyoutoberich.com/blog/time-management-how-an-mit-postdoc-writes-3-books-a-phd-defense-and-6-peer-reviewed-papers-and-finishes-by-530pm/)

### update
```
sudo apt-get update && sudo apt-get upgrade 
sudo apt-get dist-upgrade
```
### bash prompt howto
- [src](http://tldp.org/HOWTO/Bash-Prompt-HOWTO/index.html)
- counting files
```
ls -l | wc -l
```
