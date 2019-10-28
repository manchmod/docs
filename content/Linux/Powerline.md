+++
title = "Powerline"
description = "Powerline configuration"
+++

## install powerline

https://powerline.readthedocs.io/en/latest/

(ensure you are installing this into the right pip place)

```
pip install powerline-status
```

## install patched powerline fonts 

https://github.com/powerline/fonts

meslo slashed (LG S)


## add powerline to zshrc

get installation root
```
pip show powerline-status
```

```
# add to zshrc
. /usr/local/lib/python2.7/site-packages/powerline/bindings/zsh/powerline.zsh
```

## add powerline to tmux

https://powerline.readthedocs.io/en/latest/usage/other.html#tmux-statusline

```
# add to .tmux.conf
source "/usr/local/lib/python2.7/site-packages/powerline/bindings/tmux/powerline.conf"
```
