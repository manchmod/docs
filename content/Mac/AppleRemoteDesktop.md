---
title: "Apple Remote Desktop"
date: 2019-12-15T13:18:17-04:00
draft: false
---

|Date Added|Description|Link|
|:---|:---|---|
|2020-1-30| enabling screensharing | https://www.techrepublic.com/article/how-to-enable-screen-sharing-on-macs-via-terminal/ | 
|2020-1-30| enabling ARD | https://support.apple.com/en-us/HT201710 | 

## sample commands
```
# restart agent
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -restart -agent

# enable agent, including menu item
sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -activate -configure -allowAccessFor -allUsers -privs -all -clientopts -setmenuextra -menuextra yes

# enable screensharing
sudo defaults write /var/db/launchd.db/com.apple.launchd/overrides.plist com.apple.screensharing -dict Disabled -bool false

```

