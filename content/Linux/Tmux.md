+++
title = "Tmux"
description = "Tmux configuration"
+++

## quick commands

hit man command (command-b) then

|Command|Description|
|---|---|
|C-o | swap panes|

## cheat sheet

https://gist.github.com/MohamedAlaa/2961058

## tutorial

https://lukaszwrobel.pl/blog/tmux-tutorial-split-terminal-windows-easily/

https://medium.com/@joe_wolfe_21/the-ultimate-terminal-67e93665459d

## dotfile

https://github.com/tangledhelix/dotfiles/blob/master/tmux.conf


## iterm2 keymaps

http://tangledhelix.com/blog/2012/04/28/iterm2-keymaps-for-tmux/


## customizing info

https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/


## iterm scrollback

```
Main> terminal may access clipboard

advanced > scroll wheel sends arrow keys when in alternate screen mode

profile > both save lines to scrollback when…. boxes checked
```

https://dan.carley.co/blog/2013/01/11/tmux-scrollback-with-iterm2/

https://stackoverflow.com/questions/36594420/how-can-i-turn-off-scrolling-the-history-in-iterm2

https://medium.freecodecamp.org/tmux-in-practice-scrollback-buffer-47d5ffa71c93


## vim solarized
[vim, tmux, truecolor](https://medium.com/@dubistkomisch/how-to-actually-get-italics-and-true-colour-to-work-in-iterm-tmux-vim-9ebe55ebc2be)

[solarized in vim](https://fideloper.com/mac-vim-tmux)

``` 
# terminal overrides
set -g default-terminal 'tmux-256color'
set -as terminal-overrides ',xterm*:Tc:sitm=\E[3m'
```

## true color support

[Tmux True Color Support](https://jdhao.github.io/2018/10/19/tmux_nvim_true_color/)

```
# test to ensure color working
awk 'BEGIN{
    s="/\\/\\/\\/\\/\\"; s=s s s s s s s s;
    for (colnum = 0; colnum<77; colnum++) {
        r = 255-(colnum*255/76);
        g = (colnum*510/76);
        b = (colnum*255/76);
        if (g>255) g = 510-g;
        printf "\033[48;2;%d;%d;%dm", r,g,b;
        printf "\033[38;2;%d;%d;%dm", 255-r,255-g,255-b;
        printf "%s\033[0m", substr(s,colnum+1,1);
    }
    printf "\n";
}'
```
## fixing remote unicode rendering
[github issue](https://github.com/tmux/tmux/issues/1257)

```
# alias for unicode
tmux -u 

# or
export LANG=en_US.UTF-8
export LC_CTYPE=en_US.UTF-8
```

