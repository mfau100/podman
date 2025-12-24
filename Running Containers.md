### Containers

### Logging into a registry.

[student@test ~]$ podman login registry.redhat.io
Username: rhn-gps-mfau
Password:
Login Succeeded!

### Verifying the logged in user.

[student@test ~]$ podman login registry.redhat.io --get-login
rhn-gps-mfau
[student@test ~]$

### Shows the a lot of info including the registries.

[student@test rhcsa9]$ podman info

..........................................................................................................
### Demo: Building an image from a Containerfile.  A text file with instructions to build a container image.

### In SVV's rhcsa9 repo.

[student@test ~]$ cd rhcsa9/
[student@test rhcsa9]$ pwd
/home/student/rhcsa9

### Changed SVV's Containerfile to reflect the file in the video.

[student@test rhcsa9]$ cat Containerfile
FROM registry.access.redhat.com/ubi8/ubi:latest
RUN dnf install nmap -y
CMD ["/usr/bin/sleep", "30"]

[student@test rhcsa9]$ pwd
/home/student/rhcsa9

[student@test rhcsa9]$ cd

[student@test ~]$ cp rhcsa9/Containerfile .

[student@test ~]$ vim Containerfile

### New file. The "-y" feeds the yes answer for the install, otherwise it will error, because container files aren't meant to be interactive.

[student@test ~]$ cat Containerfile
FROM registry.access.redhat.com/ubi8/ubi:latest
RUN dnf install nmap -y
CMD ["/usr/bin/nmap", "-sn", "192.168.0.0/24"]

[student@test ~]$ podman images
REPOSITORY  TAG         IMAGE ID    CREATED     SIZE
[student@test ~]$

### Creating the image "-t" is the tag and the "." is the location of the container image file.

[student@test ~]$ podman build -t mymap .
STEP 1/3: FROM registry.access.redhat.com/ubi8/ubi:latest
Trying to pull registry.access.redhat.com/ubi8/ubi:latest...
Getting image source signatures
Checking if image destination supports signatures
Copying blob 5490bafbf8c0 done   |
Copying config 81cb3ff275 done   |
Writing manifest to image destination
Storing signatures
STEP 2/3: RUN dnf install nmap -y
Updating Subscription Management repositories.
Unable to read consumer identity
subscription-manager is operating in container mode.

This system is not registered with an entitlement server. You can use subscription-manager to register.

Red Hat Enterprise Linux 8 for x86_64 - AppStre  23 MB/s |  78 MB     00:03
Red Hat Enterprise Linux 8 for x86_64 - BaseOS   30 MB/s | 116 MB     00:03
Red Hat Universal Base Image 8 (RPMs) - BaseOS  809 kB/s | 735 kB     00:00
Red Hat Universal Base Image 8 (RPMs) - AppStre 3.8 MB/s | 3.7 MB     00:00
Red Hat Universal Base Image 8 (RPMs) - CodeRea 260 kB/s | 188 kB     00:00
Dependencies resolved.
================================================================================
 Package     Arch    Version            Repository                         Size
================================================================================
Installing:
 nmap        x86_64  2:7.92-2.el8_10    rhel-8-for-x86_64-appstream-rpms  5.9 M
Installing dependencies:
 libibverbs  x86_64  48.0-1.el8         rhel-8-for-x86_64-baseos-rpms     404 k
 libpcap     x86_64  14:1.9.1-5.el8     rhel-8-for-x86_64-baseos-rpms     169 k
 nmap-ncat   x86_64  2:7.92-2.el8_10    rhel-8-for-x86_64-appstream-rpms  242 k

Transaction Summary
================================================================================
Install  4 Packages

Total download size: 6.7 M
Installed size: 26 M
Downloading Packages:
(1/4): libpcap-1.9.1-5.el8.x86_64.rpm           403 kB/s | 169 kB     00:00
(2/4): nmap-ncat-7.92-2.el8_10.x86_64.rpm       527 kB/s | 242 kB     00:00
(3/4): libibverbs-48.0-1.el8.x86_64.rpm         2.4 MB/s | 404 kB     00:00
(4/4): nmap-7.92-2.el8_10.x86_64.rpm            8.4 MB/s | 5.9 MB     00:00
--------------------------------------------------------------------------------
Total                                           9.5 MB/s | 6.7 MB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Installing       : libibverbs-48.0-1.el8.x86_64                           1/4
  Installing       : libpcap-14:1.9.1-5.el8.x86_64                          2/4
  Installing       : nmap-ncat-2:7.92-2.el8_10.x86_64                       3/4
  Running scriptlet: nmap-ncat-2:7.92-2.el8_10.x86_64                       3/4
  Installing       : nmap-2:7.92-2.el8_10.x86_64                            4/4
  Running scriptlet: nmap-2:7.92-2.el8_10.x86_64                            4/4
  Verifying        : nmap-2:7.92-2.el8_10.x86_64                            1/4
  Verifying        : nmap-ncat-2:7.92-2.el8_10.x86_64                       2/4
  Verifying        : libpcap-14:1.9.1-5.el8.x86_64                          3/4
  Verifying        : libibverbs-48.0-1.el8.x86_64                           4/4
Installed products updated.

Installed:
  libibverbs-48.0-1.el8.x86_64         libpcap-14:1.9.1-5.el8.x86_64
  nmap-2:7.92-2.el8_10.x86_64          nmap-ncat-2:7.92-2.el8_10.x86_64

Complete!
--> 6db5e12bd3db
STEP 3/3: CMD ["/usr/bin/nmap", "-sn", "192.168.0.0/24"]
COMMIT mymap
--> 69929b87129c
Successfully tagged localhost/mymap:latest
69929b87129c91f355fa0c816fbc65b82e075a52fb03b5c226d764cd39204bdc
[student@test ~]$

### Now we have 2 images "mymap" and a ubi8 image which was needed to build the image.

[student@test ~]$ podman images
REPOSITORY                           TAG         IMAGE ID      CREATED             SIZE
localhost/mymap                      latest      69929b87129c  About a minute ago  637 MB
registry.access.redhat.com/ubi8/ubi  latest      81cb3ff275b0  10 hours ago        205 MB

### Running containers.

podman run  (Run one default command)
podman run ubi8 sleep (To run an alternative command as a command line argument)
podman run -it
podman ps
podman ps -a
podman inspect
podman inspect container (To find the primary command)
podman search
podman stop
podman build
podman images
podman pull
podman exec (Executes a command in a running container)
podman rm
podman logs

[student@test ~]$ podman search ubi
NAME                                 DESCRIPTION
registry.redhat.io/ubi8              Provides the latest release of Red Hat Unive...
registry.redhat.io/ubi9              Provides the latest release of Red Hat Unive...
registry.redhat.io/ubi10             Provides the latest release of Red Hat Unive...
registry.redhat.io/ubi7/ubi          Provides the latest release of the Red Hat U...
registry.redhat.io/ubi8/ubi          Provides the latest release of the Red Hat U...
registry.redhat.io/ubi7              Provides the latest release of the Red Hat U...
registry.redhat.io/ubi9/ubi          Provides the latest release of Red Hat Unive...
registry.redhat.io/ubi10/ubi         Provides the latest release of Red Hat Unive...
registry.redhat.io/ubi7/ubi-init     Provides the latest release of the Red Hat U...
registry.redhat.io/ubi8/ubi-minimal  Provides the latest release of the Minimal R...
registry.redhat.io/ubi8/ubi-init     Provides the latest release of the Red Hat U...
registry.redhat.io/ubi8-init         Provides the latest release of the Red Hat U...
registry.redhat.io/ubi7-minimal      Provides the latest release of the minimal R...
registry.redhat.io/ubi8/ubi-micro    Provides the latest release of Micro Univers...
registry.redhat.io/ubi8-micro        Provides the latest release of Micro Univers...
registry.redhat.io/ubi9/ubi-minimal  Provides the latest release of the Minimal R...
registry.redhat.io/ubi9-minimal      Provides the latest release of the Minimal R...
registry.redhat.io/ubi9-init         Provides the latest release of the Red Hat U...
registry.redhat.io/ubi10-init        Provides the latest release of the Red Hat U...
registry.redhat.io/ubi10/ubi-init    Provides the latest release of the Red Hat U...
registry.redhat.io/ubi7/ubi-minimal  Provides the latest release of the minimal R...
registry.redhat.io/ubi8-minimal      Provides the latest release of the minimal R...
registry.redhat.io/ubi7-init         Provides the latest release of the Red Hat U...
registry.redhat.io/ubi9/ubi-init     Provides the latest release of the Red Hat U...
registry.redhat.io/ubi9/ubi-micro    Red Hat Universal Base Image 9 Micro
docker.io/hclcnlabs/ubi
docker.io/chauhanr/ubi
docker.io/containercraft/ubi
docker.io/cloudctl/ubi
docker.io/startx/ubi
docker.io/sysflowtelemetry/ubi       Universal base images for the SysFlow projec...
docker.io/sneubi/ubi                 experimental repo
docker.io/myage/ubi
docker.io/dgholmes/ubi
docker.io/geosx/ubi
docker.io/jekjek10/ubi
docker.io/ahurtado2192/ubi
docker.io/songdongsheng/ubi
docker.io/aaronstrouse1/ubi          testing repo
docker.io/naz513/ubi
docker.io/vrsfactory/ubi             A personal UBI image based on Red Hat UBI Mi...
docker.io/pluizetto/ubi              UBI images
docker.io/blackinches/ubi
docker.io/mamis/ubi
docker.io/opsmxdev/ubi
docker.io/bmason42/ubi
docker.io/allsmooth/ubi              My short description
docker.io/montavista/ubi
docker.io/vmartico/ubi
docker.io/acaprari/ubi
[student@test ~]$
..................................................................
### Logged into registry.redhat.io.

[root@test ~]# podman login registry.redhat.io
Username: rhn-gps-mfau
Password:
Login Succeeded!

### This container will sleep for an hour in the foreground.

[root@test ~]# podman run --name ubi9-container registry.redhat.io/ubi9 sleep 3600
Trying to pull registry.redhat.io/ubi9:latest...
Getting image source signatures
Checking if image destination supports signatures
Copying blob d9ce76bfbf66 skipped: already exists
Copying config d60669a3ae done   |
Writing manifest to image destination
Storing signatures

### From a 2nd terminal.

login as: student
student@192.168.0.112's password:
Activate the web console with: systemctl enable --now cockpit.socket

Last login: Thu Dec 18 21:49:23 2025 from 192.168.0.205
[student@test ~]$ sudo -i
[sudo] password for student:

### From a 2nd terminal I can see the container running.

[root@test ~]# podman ps
CONTAINER ID  IMAGE                           COMMAND     CREATED        STATUS        PORTS       NAMES
305ef5814262  registry.redhat.io/ubi9:latest  sleep 3600  5 minutes ago  Up 5 minutes              ubi9-container
[root@test ~]#

###  It took a little bit for the container to stop. SVV said normally it will try a graceful stop for 10 seconds and if that doesn't work podman will send a SIGKILL it's like "kill -9".

[root@test ~]# podman stop 305ef5814262
WARN[0010] StopSignal SIGTERM failed to stop container ubi9-container in 10 seconds, resorting to SIGKILL
305ef5814262
[root@test ~]#

### Back on the primary terminal the container has stopped.

[root@test ~]# podman run --name ubi9-container registry.redhat.io/ubi9 sleep 3600
Trying to pull registry.redhat.io/ubi9:latest...
Getting image source signatures
Checking if image destination supports signatures
Copying blob d9ce76bfbf66 skipped: already exists
Copying config d60669a3ae done   |
Writing manifest to image destination
Storing signatures
[root@test ~]#

### Ran the same container with a different name...cool

[root@test ~]# podman run --name sleepy registry.redhat.io/ubi9 sleep 3600
.............................................................
podman run busybox  (Cloud image - minimal linux image)

### Notice the busybox has "exited 0" meaning it successfully completed.  SVV said the command it ran "sh" is not connected to standard-out, so it immediated exited, which is normal.

[root@test ~]# podman ps -a
CONTAINER ID  IMAGE                                    COMMAND               CREATED         STATUS                     PORTS                                     NAMES
1297b7ccfecf  registry.redhat.io/ubi8/httpd-24:latest  /usr/sbin/httpd -...  21 hours ago    Exited (1) 21 hours ago    0.0.0.0:8080->80/tcp, 8080/tcp, 8443/tcp  myhttpd
8633d5a257e8  registry.redhat.io/ubi8/httpd-24:latest  /usr/bin/run-http...  21 hours ago    Exited (0) 292 years ago   0.0.0.0:8080->80/tcp, 8080/tcp, 8443/tcp  myhttpd2
305ef5814262  registry.redhat.io/ubi9:latest           sleep 3600            7 hours ago     Exited (137) 7 hours ago                                             ubi9-container
e5864df37b46  registry.redhat.io/ubi9:latest           sleep 3600            7 hours ago     Exited (137) 7 hours ago                                             sleepy
d1a55ffe9cbf  docker.io/library/busybox:latest         sh                    11 minutes ago  Exited (0) 11 minutes ago                                            priceless_feynman

### This mariadb pod (remember to specify the exact container name), exited with an error "1", because it needs more info to run.  Kind of like you tried to run "podman run mariadb" and this failed, because it needed more info.

[root@test ~]# podman run registry.redhat.io/rhel9/mariadb-105
Trying to pull registry.redhat.io/rhel9/mariadb-105:latest...
Getting image source signatures
Checking if image destination supports signatures
Copying blob d9ce76bfbf66 skipped: already exists
Copying blob 208dfedcaf81 skipped: already exists
Copying blob b427d0ef72cc done   |
Copying config fcde646cab done   |
Writing manifest to image destination
Storing signatures
=> sourcing 20-validate-variables.sh ...
You must either specify the following environment variables:
  MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
  MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
  MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
  MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
Optional Settings:
  MYSQL_LOWER_CASE_TABLE_NAMES (default: 0)
  MYSQL_LOG_QUERIES_ENABLED (default: 0)
  MYSQL_MAX_CONNECTIONS (default: 151)
  MYSQL_FT_MIN_WORD_LEN (default: 4)
  MYSQL_FT_MAX_WORD_LEN (default: 20)
  MYSQL_AIO (default: 1)
  MYSQL_KEY_BUFFER_SIZE (default: 32M or 10% of available memory)
  MYSQL_MAX_ALLOWED_PACKET (default: 200M)
  MYSQL_TABLE_OPEN_CACHE (default: 400)
  MYSQL_SORT_BUFFER_SIZE (default: 256K)
  MYSQL_READ_BUFFER_SIZE (default: 8M or 5% of available memory)
  MYSQL_INNODB_BUFFER_POOL_SIZE (default: 32M or 50% of available memory)
  MYSQL_INNODB_LOG_FILE_SIZE (default: 8M or 15% of available memory)
  MYSQL_INNODB_LOG_BUFFER_SIZE (default: 8M or 15% of available memory)

For more information, see https://github.com/sclorg/mariadb-container
[root@test ~]# podman ps -a
CONTAINER ID  IMAGE                                        COMMAND               CREATED         STATUS                     PORTS                                     NAMES
1297b7ccfecf  registry.redhat.io/ubi8/httpd-24:latest      /usr/sbin/httpd -...  21 hours ago    Exited (1) 21 hours ago    0.0.0.0:8080->80/tcp, 8080/tcp, 8443/tcp  myhttpd
8633d5a257e8  registry.redhat.io/ubi8/httpd-24:latest      /usr/bin/run-http...  21 hours ago    Exited (0) 292 years ago   0.0.0.0:8080->80/tcp, 8080/tcp, 8443/tcp  myhttpd2
305ef5814262  registry.redhat.io/ubi9:latest               sleep 3600            7 hours ago     Exited (137) 7 hours ago                                             ubi9-container
e5864df37b46  registry.redhat.io/ubi9:latest               sleep 3600            7 hours ago     Exited (137) 7 hours ago                                             sleepy
d1a55ffe9cbf  docker.io/library/busybox:latest             sh                    17 minutes ago  Exited (0) 17 minutes ago                                            priceless_feynman
67e96933f707  registry.redhat.io/rhel9/mariadb-105:latest  run-mysqld            42 seconds ago  Exited (1) 41 seconds ago  3306/tcp                                  sleepy_shtern

### The mariadb pod needs parameters defined to run.  These are environment variables that are site specific.

[root@test ~]# podman logs 67e96933f707
=> sourcing 20-validate-variables.sh ...
You must either specify the following environment variables:
  MYSQL_USER (regex: '^[a-zA-Z0-9_]+$')
  MYSQL_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
  MYSQL_DATABASE (regex: '^[a-zA-Z0-9_]+$')
Or the following environment variable:
  MYSQL_ROOT_PASSWORD (regex: '^[a-zA-Z0-9_~!@#$%^&*()-=<>,.?;:|]+$')
Or both.
Optional Settings:
  MYSQL_LOWER_CASE_TABLE_NAMES (default: 0)
  MYSQL_LOG_QUERIES_ENABLED (default: 0)
  MYSQL_MAX_CONNECTIONS (default: 151)
  MYSQL_FT_MIN_WORD_LEN (default: 4)
  MYSQL_FT_MAX_WORD_LEN (default: 20)
  MYSQL_AIO (default: 1)
  MYSQL_KEY_BUFFER_SIZE (default: 32M or 10% of available memory)
  MYSQL_MAX_ALLOWED_PACKET (default: 200M)
  MYSQL_TABLE_OPEN_CACHE (default: 400)
  MYSQL_SORT_BUFFER_SIZE (default: 256K)
  MYSQL_READ_BUFFER_SIZE (default: 8M or 5% of available memory)
  MYSQL_INNODB_BUFFER_POOL_SIZE (default: 32M or 50% of available memory)
  MYSQL_INNODB_LOG_FILE_SIZE (default: 8M or 15% of available memory)
  MYSQL_INNODB_LOG_BUFFER_SIZE (default: 8M or 15% of available memory)

For more information, see https://github.com/sclorg/mariadb-container
..................................................................................................................................







