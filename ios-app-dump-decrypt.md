---
description: Dump and Decrypt IOS apps with a Jailbroken iPhone
---

# IOS App Dump / Decrypt

### overview

[https://ivrodriguez.com/decrypting-ios-applications-ios-12-edition/](https://ivrodriguez.com/decrypting-ios-applications-ios-12-edition/)

[https://medium.com/@felipejfc/the-ultimate-guide-for-live-debugging-apps-on-jailbroken-ios-12-4c5b48adf2fb](https://medium.com/@felipejfc/the-ultimate-guide-for-live-debugging-apps-on-jailbroken-ios-12-4c5b48adf2fb)

```text
brew install usbmuxd
git clone https://github.com/AloneMonkey/frida-ios-dump
cd frida-ios-dump
python2 -m virtualenv .
source bin/activate
pip install -r requirements.txt
```

```text
iproxy 2222 22
./dump.py [APPNAME]
```

### frida-ios-dump

[https://github.com/AloneMonkey/frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)

### ssh over usb \(libusb\)

[https://iphonedevwiki.net/index.php/SSH\_Over\_USB](https://iphonedevwiki.net/index.php/SSH_Over_USB)

### frida

[https://www.frida.re/docs/ios/\#with-jailbreak](https://www.frida.re/docs/ios/#with-jailbreak)

### python2 virtualenv

[https://help.dreamhost.com/hc/en-us/articles/215489338-Installing-and-using-virtualenv-with-Python-2](https://help.dreamhost.com/hc/en-us/articles/215489338-Installing-and-using-virtualenv-with-Python-2)

