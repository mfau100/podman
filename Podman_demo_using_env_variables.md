### Podman demo using environment variables.  Running this as a regular user student.

```
[student@test ~]$ podman run --name mydb registry.redhat.io/rhel9/mariadb-105
Trying to pull registry.redhat.io/rhel9/mariadb-105:latest...
Error: unable to copy from source docker://registry.redhat.io/rhel9/mariadb-105:latest: initializing source docker://registry.redhat.io/rhel9/mariadb-105:latest: unable to retrieve auth token: invalid username/password: unauthorized: Please login to the Red Hat Registry using your Customer Portal credentials. Further instructions can be found here: https://access.redhat.com/RegistryAuthentication

[student@test ~]$ podman login registry.redhat.io
Username: rhn-gps-mfau
Password:
Login Succeeded!
```

### It fails to run, because it's missing required parameters.

```
[student@test ~]$ podman run --name mydb registry.redhat.io/rhel9/mariadb-105
Trying to pull registry.redhat.io/rhel9/mariadb-105:latest...
Getting image source signatures
Checking if image destination supports signatures
Copying blob d9ce76bfbf66 skipped: already exists
Copying blob b427d0ef72cc done   |
Copying blob 208dfedcaf81 done   |
Copying config fcde646cab done   |
Writing manifest to image destination
Storing signatures
Warning: Can't detect cpu quota from cgroups
Warning: Can't detect cpuset size from cgroups, will use nproc
Warning: Can't detect cpu quota from cgroups
Warning: Can't detect cpuset size from cgroups, will use nproc
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
[student@test ~]$
```

### The container exited with a code "1".

```
[student@test ~]$ podman ps -a
CONTAINER ID  IMAGE                                        COMMAND               CREATED        STATUS                    PORTS       NAMES
ed9fe2f0285d  registry.redhat.io/ubi9:latest               sleep 3600            10 hours ago   Exited (0) 292 years ago              ubi9-container
d390b5997c56  localhost/mymap:latest                       /usr/bin/nmap -sn...  10 hours ago   Exited (1) 10 hours ago               breezy
9d29c5daa855  localhost/mymap:latest                       /usr/bin/nmap -sn...  2 hours ago    Exited (1) 2 hours ago                test
599318d2170d  registry.redhat.io/rhel9/mariadb-105:latest  run-mysqld            7 minutes ago  Exited (1) 7 minutes ago  3306/tcp    mydb
```

### Checking the logs, shows certain parameters are required.

```
[student@test ~]$ podman logs mydb
Warning: Can't detect cpu quota from cgroups
Warning: Can't detect cpuset size from cgroups, will use nproc
Warning: Can't detect cpu quota from cgroups
Warning: Can't detect cpuset size from cgroups, will use nproc
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
[student@test ~]$
```

### Using skopeo to get more info on the container.

```
[student@test ~]$ skopeo inspect docker://registry.redhat.io/rhel9/mariadb-105
-bash: skopeo: command not found
[student@test ~]$ sudo dnf install skopeo
[sudo] password for student:
Updating Subscription Management repositories.
Last metadata expiration check: 2:17:58 ago on Fri 19 Dec 2025 06:53:44 AM PST.
Dependencies resolved.
=========================================================================================================================================================================================================================================
 Package                                        Architecture                                   Version                                                    Repository                                                                Size
=========================================================================================================================================================================================================================================
Installing:
 skopeo                                         x86_64                                         2:1.20.0-2.el9_7                                           rhel-9-for-x86_64-appstream-rpms                                         8.2 M

Transaction Summary
=========================================================================================================================================================================================================================================
Install  1 Package

Total download size: 8.2 M
Installed size: 28 M
Is this ok [y/N]: y
Downloading Packages:
skopeo-1.20.0-2.el9_7.x86_64.rpm                                                                                                                                                                          14 MB/s | 8.2 MB     00:00
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                     14 MB/s | 8.2 MB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                                 1/1
  Installing       : skopeo-2:1.20.0-2.el9_7.x86_64                                                                                                                                                                                  1/1
  Running scriptlet: skopeo-2:1.20.0-2.el9_7.x86_64                                                                                                                                                                                  1/1
  Verifying        : skopeo-2:1.20.0-2.el9_7.x86_64                                                                                                                                                                                  1/1
Installed products updated.

Installed:
  skopeo-2:1.20.0-2.el9_7.x86_64

Complete!
[student@test ~]$
```

### Skopeo shows a lot of info.  Just pasted the relavent info below, the usage line shows how to run it.
### Skopeo fetches this info without pulling it.  Podman inspect needs to pull the image before it can show info.

```
[student@test ~]$ skopeo inspect docker://registry.redhat.io/rhel9/mariadb-105

"usage": "podman run -d -e MYSQL_USER=user -e MYSQL_PASSWORD=pass -e MYSQL_DATABASE=db -p 3306:3306 rhel9/mariadb-105"

```

### Removed the failed container and ran it again with a variable.

```
[student@test ~]$ podman rm mydb
mydb
```

### Remember to put the environment values before the image.  The container was run without "-d" and it runs in the foreground.

```
[student@test ~]$ podman run --name mydb -e MYSQL_ROOT_PASSWORD=password registry.redhat.io/rhel9/mariadb-105
Warning: Can't detect cpu quota from cgroups
Warning: Can't detect cpuset size from cgroups, will use nproc
Warning: Can't detect cpu quota from cgroups
Warning: Can't detect cpuset size from cgroups, will use nproc
=> sourcing 20-validate-variables.sh ...
=> sourcing 25-validate-replication-variables.sh ...
=> sourcing 30-base-config.sh ...
---> 17:17:01     Processing basic MySQL configuration files ...
=> sourcing 60-replication-config.sh ...
=> sourcing 70-s2i-config.sh ...
---> 17:17:01     Processing additional arbitrary  MySQL configuration provided by s2i ...
=> sourcing 40-paas.cnf ...
=> sourcing 50-my-tuning.cnf ...
---> 17:17:01     Initializing database ...
---> 17:17:01     Running mysql_install_db ...


PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
To do so, start the server, then issue the following command:

'/usr/bin/mariadb-secure-installation'

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the MariaDB Knowledgebase at https://mariadb.com/kb

Please report any problems at https://mariadb.org/jira

The latest information about MariaDB is available at https://mariadb.org/.

Consider joining MariaDB's strong and vibrant community:
https://mariadb.org/get-involved/

---> 17:17:03     Starting MySQL server with disabled networking ...
---> 17:17:03     Waiting for MySQL to start ...
2025-12-19 17:17:03 0 [Note] Starting MariaDB 10.5.29-MariaDB source revision c461188ca6ad6ec3a54201eb87ebd75797d296df server_uid yYBSVFFJD5/tejOJX8yKoR2cpoA= as process 64
2025-12-19 17:17:03 0 [Note] InnoDB: Uses event mutexes
2025-12-19 17:17:03 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2025-12-19 17:17:03 0 [Note] InnoDB: Number of pools: 1
2025-12-19 17:17:03 0 [Note] InnoDB: Using crc32 + pclmulqdq instructions
2025-12-19 17:17:03 0 [Note] mysqld: O_TMPFILE is not supported on /var/tmp (disabling future attempts)
2025-12-19 17:17:03 0 [Note] InnoDB: Using Linux native AIO
2025-12-19 17:17:03 0 [Note] InnoDB: Initializing buffer pool, total size = 33554432, chunk size = 33554432
2025-12-19 17:17:03 0 [Note] InnoDB: Completed initialization of buffer pool
2025-12-19 17:17:03 0 [Note] InnoDB: 128 rollback segments are active.
2025-12-19 17:17:03 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2025-12-19 17:17:03 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2025-12-19 17:17:03 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2025-12-19 17:17:03 0 [Note] InnoDB: 10.5.29 started; log sequence number 45079; transaction id 20
2025-12-19 17:17:03 0 [Note] Plugin 'FEEDBACK' is disabled.
2025-12-19 17:17:03 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/data/ib_buffer_pool
2025-12-19 17:17:03 0 [Warning] 'user' entry 'root@c6ef77e2ba30' ignored in --skip-name-resolve mode.
2025-12-19 17:17:03 0 [Warning] 'proxies_priv' entry '@% root@c6ef77e2ba30' ignored in --skip-name-resolve mode.
2025-12-19 17:17:03 0 [Note] InnoDB: Buffer pool(s) load completed at 251219 17:17:03
2025-12-19 17:17:03 0 [Note] Reading of all Master_info entries succeeded
2025-12-19 17:17:03 0 [Note] Added new Master_info '' to hash table
2025-12-19 17:17:03 0 [Note] /usr/libexec/mysqld: ready for connections.
Version: '10.5.29-MariaDB'  socket: '/tmp/mysql.sock'  port: 0  MariaDB Server
---> 17:17:04     MySQL started successfully
This installation of MariaDB is already upgraded to 10.5.29-MariaDB.
There is no need to run mysql_upgrade again.
You can use --force if you still want to run mysql_upgrade
---> 17:17:04     Setting password for MySQL root user ...
---> 17:17:04     Initialization finished
=> sourcing 40-datadir-action.sh ...
---> 17:17:04     Running datadir action: upgrade-warn
---> 17:17:04     MySQL server version check passed, both server and data directory are version 10.5.
=> sourcing 50-passwd-change.sh ...
---> 17:17:04     Setting passwords ...
---> 17:17:04     Shutting down MySQL ...
2025-12-19 17:17:04 9 [Warning] 'user' entry 'root@c6ef77e2ba30' ignored in --skip-name-resolve mode.
2025-12-19 17:17:04 9 [Warning] 'proxies_priv' entry '@% root@c6ef77e2ba30' ignored in --skip-name-resolve mode.
2025-12-19 17:17:04 0 [Note] /usr/libexec/mysqld (initiated by: root[root] @ localhost []): Normal shutdown
2025-12-19 17:17:04 0 [Note] InnoDB: FTS optimize thread exiting.
2025-12-19 17:17:04 0 [Note] InnoDB: Starting shutdown...
2025-12-19 17:17:04 0 [Note] InnoDB: Dumping buffer pool(s) to /var/lib/mysql/data/ib_buffer_pool
2025-12-19 17:17:04 0 [Note] InnoDB: Buffer pool(s) dump completed at 251219 17:17:04
2025-12-19 17:17:04 0 [Note] InnoDB: Removed temporary tablespace data file: "ibtmp1"
2025-12-19 17:17:04 0 [Note] InnoDB: Shutdown completed; log sequence number 45091; transaction id 21
2025-12-19 17:17:04 0 [Note] Event Scheduler: Purging the queue. 0 events
2025-12-19 17:17:04 0 [Note] /usr/libexec/mysqld: Shutdown complete
---> 17:17:05     Cleaning up environment variables MYSQL_USER, MYSQL_PASSWORD, MYSQL_DATABASE and MYSQL_ROOT_PASSWORD ...
---> 17:17:05     Running final exec -- Only MySQL server logs after this point
2025-12-19 17:17:05 0 [Note] Starting MariaDB 10.5.29-MariaDB source revision c461188ca6ad6ec3a54201eb87ebd75797d296df server_uid yYBSVFFJD5/tejOJX8yKoR2cpoA= as process 1
2025-12-19 17:17:05 0 [Note] InnoDB: Uses event mutexes
2025-12-19 17:17:05 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2025-12-19 17:17:05 0 [Note] InnoDB: Number of pools: 1
2025-12-19 17:17:05 0 [Note] InnoDB: Using crc32 + pclmulqdq instructions
2025-12-19 17:17:05 0 [Note] mysqld: O_TMPFILE is not supported on /var/tmp (disabling future attempts)
2025-12-19 17:17:05 0 [Note] InnoDB: Using Linux native AIO
2025-12-19 17:17:05 0 [Note] InnoDB: Initializing buffer pool, total size = 33554432, chunk size = 33554432
2025-12-19 17:17:05 0 [Note] InnoDB: Completed initialization of buffer pool
2025-12-19 17:17:05 0 [Note] InnoDB: 128 rollback segments are active.
2025-12-19 17:17:05 0 [Note] InnoDB: Creating shared tablespace for temporary tables
2025-12-19 17:17:05 0 [Note] InnoDB: Setting file './ibtmp1' size to 12 MB. Physically writing the file full; Please wait ...
2025-12-19 17:17:05 0 [Note] InnoDB: File './ibtmp1' size is now 12 MB.
2025-12-19 17:17:05 0 [Note] InnoDB: 10.5.29 started; log sequence number 45091; transaction id 20
2025-12-19 17:17:05 0 [Note] Plugin 'FEEDBACK' is disabled.
2025-12-19 17:17:05 0 [Note] InnoDB: Loading buffer pool(s) from /var/lib/mysql/data/ib_buffer_pool
2025-12-19 17:17:05 0 [Note] InnoDB: Buffer pool(s) load completed at 251219 17:17:05
2025-12-19 17:17:05 0 [Note] Server socket created on IP: '::'.
2025-12-19 17:17:05 0 [Warning] 'user' entry 'root@c6ef77e2ba30' ignored in --skip-name-resolve mode.
2025-12-19 17:17:05 0 [Warning] 'proxies_priv' entry '@% root@c6ef77e2ba30' ignored in --skip-name-resolve mode.
2025-12-19 17:17:05 0 [Note] Reading of all Master_info entries succeeded
2025-12-19 17:17:05 0 [Note] Added new Master_info '' to hash table
2025-12-19 17:17:05 0 [Note] /usr/libexec/mysqld: ready for connections.
Version: '10.5.29-MariaDB'  socket: '/var/lib/mysql/mysql.sock'  port: 3306  MariaDB Server
```

### Stopped the pod.  Control+c didn't work, I had to login another term session to stop it.

```
login as: student
student@192.168.0.112's password:
Access denied
student@192.168.0.112's password:
Activate the web console with: systemctl enable --now cockpit.socket

Last failed login: Fri Dec 19 09:27:20 PST 2025 from 192.168.0.205 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Fri Dec 19 07:07:42 2025 from 192.168.0.205
[student@test ~]$ podman ps
CONTAINER ID  IMAGE                                        COMMAND     CREATED         STATUS         PORTS       NAMES
c6ef77e2ba30  registry.redhat.io/rhel9/mariadb-105:latest  run-mysqld  10 minutes ago  Up 10 minutes  3306/tcp    mydb
[student@test ~]$ podman stop mydb
mydb
[student@test ~]$ podman ps
CONTAINER ID  IMAGE       COMMAND     CREATED     STATUS      PORTS       NAMES
[student@test ~]$ podman ps -a
CONTAINER ID  IMAGE                                        COMMAND               CREATED         STATUS                    PORTS       NAMES
ed9fe2f0285d  registry.redhat.io/ubi9:latest               sleep 3600            10 hours ago    Exited (0) 292 years ago              ubi9-container
d390b5997c56  localhost/mymap:latest                       /usr/bin/nmap -sn...  10 hours ago    Exited (1) 10 hours ago               breezy
9d29c5daa855  localhost/mymap:latest                       /usr/bin/nmap -sn...  2 hours ago     Exited (1) 2 hours ago                test
c6ef77e2ba30  registry.redhat.io/rhel9/mariadb-105:latest  run-mysqld            10 minutes ago  Exited (0) 7 seconds ago  3306/tcp    mydb
[student@test ~]$ podman rm mydb
mydb
[student@test ~]$ podman ps -a
CONTAINER ID  IMAGE                           COMMAND               CREATED       STATUS                    PORTS       NAMES
ed9fe2f0285d  registry.redhat.io/ubi9:latest  sleep 3600            10 hours ago  Exited (0) 292 years ago              ubi9-container
d390b5997c56  localhost/mymap:latest          /usr/bin/nmap -sn...  10 hours ago  Exited (1) 10 hours ago               breezy
9d29c5daa855  localhost/mymap:latest          /usr/bin/nmap -sn...  2 hours ago   Exited (1) 2 hours ago                test
```




