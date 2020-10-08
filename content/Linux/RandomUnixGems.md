+++
title = "Random Unix Gems"
description = "Random Unix Gems"
+++


#### Generate Unique MAC address for host

[GiantDorks Bash Function](http://giantdorks.org/alain/how-to-generate-a-unique-mac-address/)
```bash
GetMAC()
{
  if [ -n "$1" ]; then
    OID="00:16:3e"
    RAND=$(echo $1 | md5sum | sed 's/\(..\)\(..\)\(..\).*/\1:\2:\3/')
    echo "$OID:$RAND"
  else
    echo "ERROR: please supply hostname to create MAC address from, e.g.:"
    echo "       $FUNCNAME myhost"
  fi
}
```
[CommandLineFu OneLiner](https://www.commandlinefu.com/commands/view/6632/generate-a-unique-mac-address-from-an-ip-address)
```
echo 00:16:3e$(gethostip 10.1.2.11 | awk '{ print tolower(substr($3,3)) }' |sed 's/.\{2\}/:&/g' )
```


#### Iterate across a list of items in a file

read a list of IP addresses from a file and do a dig reverse look up

```
 while read ip; do dig +noall +answer -x $ip; done < ip-list.txt
```

#### find and xargs

couldnt write a better tut myself...

[everythingcli find/xargs](https://www.everythingcli.org/find-exec-vs-find-xargs/)

##### ss command (netstat replacements)

```
ss -aunp  (udp with process and all info)

ss -ltpn (tcp listeners with processes) 

ss -ltpan (tcp with all and processe info )

```
