+++
title = "Zsh"
description = "Zsh configuration"
+++

## install oh-my-zsh (yolo!)

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## redo new user setup
```
autoload -U zsh-newuser-install
zsh-newuser-install -f
```

## fix compinit errors for sudo 

https://github.com/robbyrussell/oh-my-zsh/issues/6835

## add autocomplete and syntax highlighting to oh my zsh

https://gist.github.com/dogrocker/1efb8fd9427779c827058f873b94df95

```
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

in zshrc

plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```
