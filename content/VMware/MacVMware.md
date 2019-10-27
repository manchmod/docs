---
title: "Mac VMware"
date: 2019-10-27T15:06:15-04:00
draft: false
---

## govc
```
brew install govmomi/tap/govc
```

## mac powershell

```
brew tap caskroom/cask 
brew cask install powershell
```

## PowerCLI

```
Install-Module -Name VMware.PowerCLI -Scope CurrentUser
Get-Command -Module *VMWare*
```