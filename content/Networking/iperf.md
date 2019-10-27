---
title: "Iperf"
date: 2019-10-27T15:48:57-04:00
draft: false
---

## logfile
```
iperf -o ./iperf.out -f MBytes -c 10.190.184.11 -t 30 -d -y c
```

## bidirectional. 300 seconds
```
iperf -e -f MBytes -c 10.190.184.11 -t 300 -i 1 -d
```
## single direction
```
iperf -e -f MBytes -c 10.190.184.11 -t 300 -i 1 
```
