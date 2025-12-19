### How to debug a container.  Can't curl the container on port 8080 and the firewall is open.

```
[student@test ~]$ curl localhost:8080
curl: (56) Recv failure: Connection reset by peer

[student@test ~]$ podman exec -it 0e27f8bd2030 bash
```

### The normal tools didn't work...ss, netstat, ip a, find commands didn't work.

```
[root@0e27f8bd2030 /]# ss -tulpn
bash: ss: command not found

[root@0e27f8bd2030 /]# ll /var/log/nginx16/
total 0
-rw-r--r--. 1 root root 0 Dec 19 18:08 error.log

[root@0e27f8bd2030 /]# cat /var/log/nginx16/error.log

[root@0e27f8bd2030 /]# netstat
bash: netstat: command not found

[root@0e27f8bd2030 /]# ip a

[root@0e27f8bd2030 ~]# find / -name nginx.conf
```

### I found this nginx command and it shows the syntax to check the conf file and reveals it's location.

```
[root@0e27f8bd2030 ~]# nginx -t
nginx: the configuration file /opt/rh/nginx16/root/etc/nginx/nginx.conf syntax is ok
nginx: configuration file /opt/rh/nginx16/root/etc/nginx/nginx.conf test is successful
```

### Great command to view the conf file.  The "^\s*" allows any leading spaces or tabs.
### Below this container is using port 80.

```
[root@0e27f8bd2030 ~]# grep -Ev '^\s*($|#)' /opt/rh/nginx16/root/etc/nginx/nginx.conf
user  nginx;
worker_processes  1;
error_log /proc/self/fd/2;
pid        /opt/rh/nginx16/root/var/run/nginx/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       /opt/rh/nginx16/root/etc/nginx/mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log /proc/self/fd/1  main;
    sendfile        on;
    keepalive_timeout  65;
    include /opt/rh/nginx16/root/etc/nginx/conf.d/*.conf;
    server {
        listen       80;
        server_name  localhost;
        location / {
            root   /opt/rh/nginx16/root/usr/share/nginx/html;
            index  index.html index.htm;
        }
        error_page  404              /404.html;
        location = /40x.html {
            root   /opt/rh/nginx16/root/usr/share/nginx/html;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   /opt/rh/nginx16/root/usr/share/nginx/html;
        }
    }
}
```

### The curl confirms port 80 works.

```
[root@0e27f8bd2030 ~]# vi /opt/rh/nginx16/root/etc/nginx/nginx.conf

[root@0e27f8bd2030 ~]# curl localhost:80
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Red Hat Enterprise Linux</title>
```

### This works from the host level using the hostname.

```
[student@test ~]$ podman ps
CONTAINER ID  IMAGE                                           COMMAND               CREATED      STATUS      PORTS                          NAMES
0e27f8bd2030  registry.redhat.io/rhscl/nginx-16-rhel7:latest  nginx -g daemon o...  2 hours ago  Up 2 hours  0.0.0.0:8080->80/tcp, 443/tcp  thirsty_gauss
[student@test ~]$ hostname
test
[student@test ~]$ ping test
PING test (192.168.0.112) 56(84) bytes of data.
64 bytes from test (192.168.0.112): icmp_seq=1 ttl=64 time=0.055 ms
^C
--- test ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.055/0.055/0.055/0.000 ms
[student@test ~]$ curl test:8080
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Red Hat Enterprise Linux</title>
```

### Using the 127.0.0.1 address works too.

```
[student@test ~]$ curl 127.0.0.1:8080
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
    <head>
        <title>Test Page for the Nginx HTTP Server on Red Hat Enterprise Linux</title>
```

### I don't know why localhost doesn't work, but I'll work on that another day.

```
[student@test ~]$ curl localhost:8080
curl: (56) Recv failure: Connection reset by peer
[student@test ~]$
[student@test ~]$ getent hosts localhost
::1             localhost localhost.localdomain localhost6 localhost6.localdomain6
[student@test ~]$ vim /etc/hosts
[student@test ~]$ cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.0.252 dns
192.168.0.62 shi
```


