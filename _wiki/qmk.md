---
layout  : wiki
title   : qmk tutorial
summary :
date    : 2020-01-26 15:43:36 +0900
updated : 2020-01-26 15:43:36 +0900
tag     : tools
toc     : true
public  : true
parent  : tools
latex   : false
comment : false
---
* TOC
{:toc}

## clone qmk github
- find keyboard layout (e.g) Preonic)
- [link](https://github.com/qmk/qmk_firmware/tree/master/keyboards/preonic)
- edit keymap.c (in specific folder)
- make (build) new keymap
- Flash it
    1. first connect the device
    2. push reset button
    3. make via dfu-util

```
make preonic/rev3:default:dfu-util
```
## Troubleshooting
### When Left ALT and GUI swapped
- hold space+alt and plug
-
[LINK](https://www.reddit.com/r/MechanicalKeyboards/comments/70gzhh/help_gui_keys_in_qmk_no_longer_opening_windows/dn317qu/)
