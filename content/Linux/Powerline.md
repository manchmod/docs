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

- meslo slashed (LG S)
- GoMono
- Cousine
- Droid Sans Mono Slashed ***

## add powerline to tmux

https://powerline.readthedocs.io/en/latest/usage/other.html#tmux-statusline

```
# add to .tmux.conf
run-shell "powerline-daemon -q"
source "/usr/local/lib/python2.7/site-packages/powerline/bindings/tmux/powerline.conf"

# if necessary run (on linux this will be in .local/bin with the python modules)
powerline-config tmux setup

```

## config locations

https://powerline.readthedocs.io/en/latest/configuration/local.html#powerline-script-overrides


```
# get powerline install location
pip show powerline-status | grep Location | awk '{print $2}'

# main config
$powerline/powerline/config_files/themes/tmux 

# tmux theme
$powerline/powerline/config_files/config.json
``` 

## add powerline to zshrc

get installation root
```
pip show powerline-status
```

```
# add to zshrc
. /usr/local/lib/python2.7/site-packages/powerline/bindings/zsh/powerline.zsh
```
