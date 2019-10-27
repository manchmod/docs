---
title: "Wireshark SSL Handshake"
date: 2019-10-27T13:52:21-04:00
draft: false
---

### ssl handshake searches in wireshark

```
ssl.handshake.extensions_server_name == "graph.facebook.com" 
ssl.handshake.extensions_server_name != "graph.facebook.com" && ssl.handshake.extensions_server_name != "itunes.apple.com" && ssl.handshake.type ==1

```

