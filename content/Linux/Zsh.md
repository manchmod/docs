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
