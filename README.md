# QEMS docker
IDQ QEMS(v1.5) docker runtime
## main features
 * mariadb 10.3
 * tomcat 8.5.38: to run two tomcat instances in one container

# Prerequisite
## docker
 * docker >= 19.x.x
 * docker-compose >= 1.25.x (version 3)
## 3rd party library
Not included here for confidentiality
 * QEMS_Admin.war
 * QEMS_API.war

> Two files must be copied into ```qems/``` folder

## ```.env``` file
Not included here for confidentiality due to password information<br>
The following variables must be defined and ```.env``` must exist in the top
folder (the same location as ```docker-compose.yml```)

```
TZ=Asia/Seoul
DB_ROOT_PWD=<mariadb root password>
DB_NAME=<database name to create>
DB_USER=<database user name>
DB_USER_PWD=<database user password>
WEBHOME=/usr/local/tomcat
APIHOME=/usr/local/tomcat_api
```

# Running
## start EMS
```
docker-compose up -d
```
## shutdown EMS
```
docker-compose down
```
## connect to EMS
@Browser, ```http://<ip>:8081```
## Caveat
> QEMS runs smoothly from the second launch !

Interestingly, DB cannot accept request at the first launch causing HTTP 404 error.<br>
Shut down after DB initialization then restart and QEMS will run without a hitch.

**check DB initialization at first launch**

Stop and restart once confirming the last line, "```...load completed...```"
```
$ docker logs qdb
Initializing database


PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
To do so, start the server, then issue the following commands:
.
.
.
Database initialized
MySQL init process in progress...
2020-06-22  6:06:07 0 [Note] mysqld (mysqld 10.3.17-MariaDB-1:10.3.17+maria~bionic) starting as process 108 ...
2020-06-22  6:06:07 0 [Note] InnoDB: Using Linux native AIO
.
.
.
2020-06-22  6:06:09 0 [Note] mysqld: ready for connections.
Version: '10.3.17-MariaDB-1:10.3.17+maria~bionic'  socket: '/var/run/mysqld/mysqld.sock'  port: 0  mariadb.org binary distribution
Warning: Unable to load '/usr/share/zoneinfo/leap-seconds.list' as time zone. Skipping it.
2020-06-22  6:06:29 10 [Warning] 'proxies_priv' entry '@% root@c3c35f990b3e' ignored in --skip-name-resolve mode.

/usr/local/bin/docker-entrypoint.sh: running /docker-entrypoint-initdb.d/qems_setup.sql


2020-06-22  6:06:58 0 [Note] mysqld (initiated by: unknown): Normal shutdown
.
.
.
2020-06-22  6:06:59 0 [Note] mysqld: Shutdown complete

MySQL init process done. Ready for start up.

2020-06-22  6:07:00 0 [Note] mysqld (mysqld 10.3.17-MariaDB-1:10.3.17+maria~bionic) starting as process 1 ...
2020-06-22  6:07:00 0 [Note] InnoDB: Using Linux native AIO
.
.
.
2020-06-22  6:07:02 0 [Note] mysqld: ready for connections.
Version: '10.3.17-MariaDB-1:10.3.17+maria~bionic'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution
2020-06-22  6:07:04 0 [Note] InnoDB: Buffer pool(s) load completed at 200622  6:07:04
```
