# qems
IDQ QEMS docker runtime

# Prerequisite
## 3rd party library
Not included here for confidentiality
 * QEMS_Admin.war
 * QEMS_API.war

Two files must be copied into ```qems/``` folder

## ```.env``` file
Not included here for confidentiality due to password information<br>
The following variables must be defined
```
TZ=Asia/Seoul
DB_ROOT_PWD=<mariadb root password>
DB_NAME=<database name to create>
DB_USER=<database user name>
DB_USER_PWD=<database user password>
WEBHOME=/usr/local/tomcat
APIHOME=/usr/local/tomcat_api
```