+++
title = "XCode 11 LLDB Python 2"
description = "Dump and Decrypt IOS apps with a Jailbroken iPhone"
+++

### Mac Xcode 11 LLDB Python2 config

fixes lldb so that python2 is used (so fG!'s lldbinit works')

```
defaults write com.apple.dt.lldb DefaultPythonVersion 2
```

https://github.com/facebook/chisel/issues/262