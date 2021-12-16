---
title: "Random Mac Gems"
date: 2019-12-15T13:18:17-04:00
draft: false
---

|Date Added|Description|Link|
|:---|:---|---|
|2019-11-24| hi | https://docs.notch.org | 



#### ip command on MacOS

you can install the ip command on MacOS. see [here](https://docs.notch.org/linux/ipcmd) for more details on how to use it

```
brew install iproute2mac
```

#### disable notarization check in Catalina

```
sudo xattr -dr com.apple.quarantine /Applications/Chromium.app
```

then right click, open

#### Installing Catalina in VMware ESXi

https://www.jomebrew.com/2019/10/macos-catalina-beta-on-vmwwre-esxi-67-u2.html

override serial/hw in vmx file (serial must be valid, and of same HW type)
```
serialNumber = "SERIAL-NUM"
hw.model = "MacPro6,1"
```

#### remount NTFS volume as read write
```
diskutil list
umount /Volumes/SecureKey128
mkdir /Volumes/SecureKey128
mount_ntfs -o rw /dev/disk2s1 /Volumes/SecureKey128
```

#### Fixing Synergy Mouse issue
``` 
# detecting the problem
ioreg -l -w 0 | grep SecureInput

# fixing the issue
pmset displaysleepnow
```