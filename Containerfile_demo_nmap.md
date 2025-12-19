### Create an image from a Containerfile.

```
[student@test ~]$ cat Containerfile
FROM registry.access.redhat.com/ubi8/ubi:latest
RUN dnf install nmap -y
CMD ["/usr/bin/nmap", "-sn", "192.168.0.0/24"]
```
```
[student@test ~]$ sudo -i
[root@test ~]# podman images
REPOSITORY                            TAG         IMAGE ID      CREATED        SIZE
registry.redhat.io/rhel9/mariadb-105  latest      fcde646cabf0  3 days ago     441 MB
registry.redhat.io/rhel9/httpd-24     latest      b80ea316ac5a  3 days ago     314 MB
registry.redhat.io/ubi9               latest      d60669a3ae08  2 weeks ago    218 MB
registry.redhat.io/ubi9/nginx-126     latest      e8ec8e9cad12  2 weeks ago    338 MB
registry.redhat.io/ubi8/httpd-24      latest      695884b876f6  3 weeks ago    445 MB
docker.io/library/busybox             latest      08ef35a1c3f0  14 months ago  4.68 MB
```
```
[root@test ~]# vim Containerfile
[root@test ~]# podman build -t mynmap2 .
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

Red Hat Enterprise Linux 8 for x86_64 - BaseOS   33 MB/s | 116 MB     00:03
Red Hat Enterprise Linux 8 for x86_64 - AppStre  33 MB/s |  78 MB     00:02
Red Hat Universal Base Image 8 (RPMs) - BaseOS  1.8 MB/s | 735 kB     00:00
Red Hat Universal Base Image 8 (RPMs) - AppStre 7.6 MB/s | 3.7 MB     00:00
Red Hat Universal Base Image 8 (RPMs) - CodeRea 587 kB/s | 188 kB     00:00
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
(1/4): libpcap-1.9.1-5.el8.x86_64.rpm           638 kB/s | 169 kB     00:00
(2/4): libibverbs-48.0-1.el8.x86_64.rpm         1.3 MB/s | 404 kB     00:00
(3/4): nmap-ncat-7.92-2.el8_10.x86_64.rpm       2.4 MB/s | 242 kB     00:00
(4/4): nmap-7.92-2.el8_10.x86_64.rpm             13 MB/s | 5.9 MB     00:00
--------------------------------------------------------------------------------
Total                                            15 MB/s | 6.7 MB     00:00
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
  Verifying        : libpcap-14:1.9.1-5.el8.x86_64                          1/4
  Verifying        : libibverbs-48.0-1.el8.x86_64                           2/4
  Verifying        : nmap-2:7.92-2.el8_10.x86_64                            3/4
  Verifying        : nmap-ncat-2:7.92-2.el8_10.x86_64                       4/4
Installed products updated.

Installed:
  libibverbs-48.0-1.el8.x86_64         libpcap-14:1.9.1-5.el8.x86_64
  nmap-2:7.92-2.el8_10.x86_64          nmap-ncat-2:7.92-2.el8_10.x86_64

Complete!
--> f27e9ca0a7c5
STEP 3/3: CMD ["/usr/bin/nmap", "-sn", "192.168.0.0/24"]
COMMIT mynmap2
--> 21f369a228f4
Successfully tagged localhost/mynmap2:latest
21f369a228f42e030dc57b12eafc3a505bed538f0a2975929f364cb21e55a15f
```

### Failed to run.
1. Normally, a container runs in its own network namespace:
 - Virtual interfaces (veth)
 - NATed traffic
 - No direct access to host NICs like enp0s3
2. With host networking, the container:
 - Shares the host’s network namespace
 - Sees the same interfaces as the host
 - Uses the same IP addresses
 - Has no network isolation
```
[root@test ~]# podman run localhost/mynmap2:latest
Starting Nmap 7.92 ( https://nmap.org ) at 2025-12-19 15:28 UTC
Couldn't open a raw socket. Error: Operation not permitted (1)
```
### Added 2 parameters and now it works.
```
[root@test ~]# podman run --cap-add=NET_RAW --cap-add=NET_ADMIN localhost/mynmap2:latest
Starting Nmap 7.92 ( https://nmap.org ) at 2025-12-19 15:28 UTC
Nmap scan report for Docsis-Gateway (192.168.0.1)
Host is up (0.0061s latency).
Nmap scan report for Pixel-10-Pro (192.168.0.95)
Host is up (0.58s latency).
Nmap scan report for test (192.168.0.112)
Host is up (0.0011s latency).
Nmap scan report for mfau-thinkpadx1carbon5th (192.168.0.119)
Host is up (0.0094s latency).
Nmap scan report for 192.168.0.139
Host is up (0.0061s latency).
Nmap scan report for iPhone (192.168.0.160)
Host is up (0.033s latency).
Nmap scan report for DESKTOP-1QNV5JU (192.168.0.199)
Host is up (0.0032s latency).
Nmap scan report for DESKTOP-86T43M8 (192.168.0.205)
Host is up (0.0054s latency).
Nmap scan report for 192.168.0.250
Host is up (0.11s latency).
Nmap done: 256 IP addresses (9 hosts up) scanned in 14.05 seconds
```
### Nmap needs host networking.

1. This does:
- ARP requests
- ICMP echo requests
- Raw socket operations

2. These require:
- Access to the real network interface
- CAP_NET_RAW and CAP_NET_ADMIN

3. Rootless containers cannot do this, even with --cap-add, because:
- Kernel blocks raw socket access in user namespaces
- Network namespace is virtualized