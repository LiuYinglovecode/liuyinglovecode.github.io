---
title: check nginx error log
date: 2019-11-06 08:25:15
tags:
- nginx
- docker
- error
categories:
- nginx
cover: ../../images/articles/nginx.png
---

### check error log

``` bash
$ docker exec -it mirrorname bash
cd /var/log/nginx/
/var/log/nginx#
cat error.log
```
![Alt text](../../images/nginx.png)
