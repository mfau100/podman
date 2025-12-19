[root@test ~]# sudo podman run --rm --network host mymap

Error: ^C
[root@test ~]# podman images
REPOSITORY                            TAG         IMAGE ID      CREATED         SIZE
localhost/mynmap2                     latest      21f369a228f4  12 minutes ago  637 MB
registry.access.redhat.com/ubi8/ubi   latest      81cb3ff275b0  19 hours ago    205 MB
registry.redhat.io/rhel9/mariadb-105  latest      fcde646cabf0  3 days ago      441 MB
registry.redhat.io/rhel9/httpd-24     latest      b80ea316ac5a  3 days ago      314 MB
registry.redhat.io/ubi9               latest      d60669a3ae08  2 weeks ago     218 MB
registry.redhat.io/ubi9/nginx-126     latest      e8ec8e9cad12  2 weeks ago     338 MB
registry.redhat.io/ubi8/httpd-24      latest      695884b876f6  3 weeks ago     445 MB
docker.io/library/busybox             latest      08ef35a1c3f0  14 months ago   4.68 MB
[root@test ~]# sudo podman run --rm --network host localhost/mynmap2
Starting Nmap 7.92 ( https://nmap.org ) at 2025-12-19 15:39 UTC
dnet: Failed to open device enp0s3
QUITTING!
[root@test ~]# getenforce
Enforcing
[root@test ~]# grep AVC /var/log/audit/audit.log | tail
type=AVC msg=audit(1765992435.900:28): avc:  denied  { sys_admin } for  pid=774 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1765992435.900:28): avc:  denied  { sys_admin } for  pid=774 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1765992435.900:28): avc:  denied  { sys_admin } for  pid=774 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1765992435.900:28): avc:  denied  { sys_admin } for  pid=774 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0
[root@test ~]# journalctl | grep sealert | tail
Dec 17 09:27:22 test setroubleshoot[896]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 09:27:22 test setroubleshoot[896]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 09:27:22 test setroubleshoot[896]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 09:27:22 test setroubleshoot[896]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 13:08:01 test setroubleshoot[893]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 13:08:01 test setroubleshoot[893]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 13:08:01 test setroubleshoot[893]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 13:08:01 test setroubleshoot[893]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 13:08:01 test setroubleshoot[893]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
Dec 17 13:08:01 test setroubleshoot[893]: SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability. For complete SELinux messages run: sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
[root@test ~]# sealert -l b3fd506e-47b0-43d3-8123-e2985c1ce8b4
SELinux is preventing /usr/libexec/switcheroo-control from using the sys_admin capability.

*****  Plugin catchall (100. confidence) suggests   **************************

If you believe that switcheroo-control should have the sys_admin capability by default.
Then you should report this as a bug.
You can generate a local policy module to allow this access.
Do
allow this access for now by executing:
# ausearch -c 'switcheroo-cont' --raw | audit2allow -M my-switcheroocont
# semodule -X 300 -i my-switcheroocont.pp


Additional Information:
Source Context                system_u:system_r:switcheroo_control_t:s0
Target Context                system_u:system_r:switcheroo_control_t:s0
Target Objects                Unknown [ capability ]
Source                        switcheroo-cont
Source Path                   /usr/libexec/switcheroo-control
Port                          <Unknown>
Host                          test
Source RPM Packages           switcheroo-control-2.4-4.el9.x86_64
Target RPM Packages
SELinux Policy RPM            selinux-policy-targeted-38.1.65-1.el9.noarch
Local Policy RPM              selinux-policy-targeted-38.1.65-1.el9.noarch
Selinux Enabled               True
Policy Type                   targeted
Enforcing Mode                Enforcing
Host Name                     test
Platform                      Linux test 5.14.0-611.13.1.el9_7.x86_64 #1 SMP
                              PREEMPT_DYNAMIC Sat Nov 29 05:13:16 EST 2025
                              x86_64 x86_64
Alert Count                   30
First Seen                    2025-12-17 07:36:00 PST
Last Seen                     2025-12-17 13:07:53 PST
Local ID                      b3fd506e-47b0-43d3-8123-e2985c1ce8b4

Raw Audit Messages
type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0


type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0


type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0


type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0


type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0


type=AVC msg=audit(1766005673.771:27): avc:  denied  { sys_admin } for  pid=773 comm="switcheroo-cont" capability=21  scontext=system_u:system_r:switcheroo_control_t:s0 tcontext=system_u:system_r:switcheroo_control_t:s0 tclass=capability permissive=0


type=SYSCALL msg=audit(1766005673.771:27): arch=x86_64 syscall=setsockopt success=yes exit=0 a0=3 a1=1 a2=1a a3=7fff00f9dcf0 items=0 ppid=1 pid=773 auid=4294967295 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=(none) ses=4294967295 comm=switcheroo-cont exe=/usr/libexec/switcheroo-control subj=system_u:system_r:switcheroo_control_t:s0 key=(null)

Hash: switcheroo-cont,switcheroo_control_t,switcheroo_control_t,capability,sys_admin

[root@test ~]#


### Implementing the fix.

[root@test ~]# ausearch -c 'switcheroo-cont' --raw | audit2allow -M my-switcheroocont
******************** IMPORTANT ***********************
To make this policy package active, execute:

semodule -i my-switcheroocont.pp

[root@test ~]# semodule -X 300 -i my-switcheroocont.pp

### Per my BFF this is not a safe. I didn't copy the output, but take the fix with a grain of salt.  It didn't work anyway...see below.

[root@test ~]# sudo podman run --rm --network host localhost/mynmap2
Starting Nmap 7.92 ( https://nmap.org ) at 2025-12-19 15:47 UTC
dnet: Failed to open device enp0s3
QUITTING!
