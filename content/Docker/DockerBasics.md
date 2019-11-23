---
title: "Docker Basics"
date: 2019-10-27T15:06:15-04:00
draft: false
---

## running images 

NOTE: order of arguments matters, imagename must be at the end.

```
docker run -p 7777/7777 -d <imagename>

NOT 

docker run <imagename> -d -p 7777/7777
```

you will get an extremely unhelpful error message like if you do not
```
docker: Error response from daemon: OCI runtime create failed: container_linux.go:346: starting container process caused "exec: \"-p\": executable file not found in $PATH": unknown.
```

#### run docker image in background
```
docker run -d <image>
```
#### run docker image mapping a port
```
docker run -p 80:80/tcp
```

#### run command (shell) in running container
```
docker exec -it <container_id> /bin/bash
```

#### run command (shell) in image
```
docker run -it <image> /bin/bash
```


## building images 

#### build docker image from dockerfile in CWD with tag
```
docker build . -t <tag>
```

#### delete all local docker images / cache
```
docker system prune -a
```




### RUN vs CMD vs ENTRYPOINT

https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/

