---
{"dg-publish":true,"permalink":"/work/s4-msass-development/"}
---


## Install postgres with docker

```bash
# create & run Postgres docker version
docker run --name my-postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 -d postgres:15 


# docker exec to it & connect to DB
psql -U postgres -d postgres -h 127.0.0.1 -W

#Create database thingsboard;
create database thingsboard;

# Drop table if update sql
drop database thingsboard



```


## S4Msass run in Linux 
```bash
# Install
mvn clean install -Dlicense.skip=true -DskipTests -e
cd ./application/target
dpkg -i thingsboard.deb

# Config DB
vim /usr/share/thingsboard/conf/thingsboard.conf

# Add to conf file
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=sql
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/thingsboard export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=postgres

# Run install script
cd /usr/share/thingsboard/bin/install
./install.sh --loadDemo

# Start thingsboard service
service thingsboard start

# show system log 
tail /var/log/thingsboard/thingsboard.log -f

# Uninstall for update
dpkg -l | grep thingsboard
dpkg -r thingsboard
dpkg -P thingsboard
# Make sure clear 
dpkg -l | grep thingsboard


```

## S4Msass develop environment for Mac

```bash

# NOT work
# mvn clean compile -Dlicense.skip=true

# For Installation
mvn clean install -Dlicense.skip=true -DskipTests -e
# or
mvn clean install -DskipTests -Dlicense.skip=true

# Using external  Postgres DB
# TODO


# copy /thingsboard/dao/src/main/resources/sql to /thingsboard/application/src/main/data/sql
# If it's the first time to init DB
cp -r dao/src/main/resources/sql application/src/main/data/

# run ThingsboardInstallApplication in IDE
# run ThingsboardServerApplication in IDE


# modify DB url if needed
vim application/src/main/resources/thingsboard.yml
datasource: 
  url: "${SPRING_DATASOURCE_URL:jdbc:postgresql://localhost:5432/thingsboard}"
  
# run UI
cd ui-ngx
yarn install
ng serve --open

```


## S4M Sass Simulator

### S4M Saas Create Tenant

1. Login with system admin account
2. Create Tenant
3. Copy tenant number

### Simulator 

- Push device info to server (Exec simulator with 'Flag=0' )
   Enter tenant No (copy from website)
   Get accessToken back

- Send attribute to server (Exec simulator with 'Flag=1' & modify access token)

- Send telemetry (Exec simulator with 'Flag=2')




Note

-   **系统管理员**: sysadmin@thingsboard.org / sysadmin
-   **租户管理员**: tenant@thingsboard.org / tenant
-   **客户**: customer@thingsboard.org / customer


Some Opt

```bash
select * from attribute_kv where attribute_type = 'CLIENT_SCOPE';

```


Try cassendra
```bash
# config
export DATABASE_ENTITIES_TYPE=sql
export DATABASE_TS_TYPE=cassandra
export CASSANDRA_URL=127.0.0.1:19042
export SPRING_JPA_DATABASE_PLATFORM=org.hibernate.dialect.PostgreSQLDialect
export SPRING_DRIVER_CLASS_NAME=org.postgresql.Driver
export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:15432/thingsboard
export SPRING_DATASOURCE_USERNAME=postgres
export SPRING_DATASOURCE_PASSWORD=xuffei






#docker status
$ docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS                                                                            NAMES
fcafb0ff3932   cassandra     "docker-entrypoint.s…"   29 minutes ago   Up 29 minutes   7000-7001/tcp, 7199/tcp, 9160/tcp, 0.0.0.0:19042->9042/tcp, :::19042->9042/tcp   cassandra
8f29aad14921   postgres:15   "docker-entrypoint.s…"   7 days ago       Up 38 minutes   0.0.0.0:15432->5432/tcp, :::15432->5432/tcp                                      my-postgres

# check cassandra
docker exec -it cassandra bash
cqlsh
use thingsboard;
desc tables;

# result
cqlsh:thingsboard> desc tables;
ts_kv_cf  ts_kv_partitions_cf
cqlsh:thingsboard> select * from ts_kv_cf;
 entity_type     | entity_id                            | key                            | partition     | ts            | bool_v | dbl_v | json_v | long_v | str_v
-----------------+--------------------------------------+--------------------------------+---------------+---------------+--------+-------+--------+--------+---------
 API_USAGE_STATE | 05359790-dd94-11ed-8620-0f4a384ba42f |            jsExecutionApiState | 1680307200000 | 1681786333065 |   null |  null |   null |   null | ENABLED
 API_USAGE_STATE | efacdb00-dd92-11ed-8620-0f4a384ba42f |                  emailApiState | 1680307200000 | 1681785867440 |   null |  null |   null |   null | ENABLED
 API_USAGE_STATE | 6a4628e0-dd92-11ed-8a00-dbf2847f8f32 |                    smsApiState | 1680307200000 | 1681785643630 |   null |  null |   null |   null | ENABLED
 API_USAGE_STATE | 05359790-dd94-11ed-8620-0f4a384ba42f |               jsExecutionLimit | 1680307200000 | 1681786333065 |   null |  null |   null |      0 |    null
 API_USAGE_STATE | 6a4628e0-dd92-11ed-8a00-dbf2847f8f32 |                  emailApiState | 1680307200000 | 1681785643630 |   null |  null |   null |   null | ENABLED
           ASSET | 0b3e1630-dd94-11ed-8620-0f4a384ba42f |                     failedMsgs | 1680307200000 | 1681786343126 |   null |  null |   null |      0 |    null
 API_USAGE_STATE | 6a4628e0-dd92-11ed-8a00-dbf2847f8f32 |              transportApiState | 1680307200000 | 1681785643630 |   null |  null |   null |   null | ENABLED
 API_USAGE_STATE | 05359790-dd94-11ed-8620-0f4a384ba42f |             createdAlarmsLimit | 1680307200000 | 1681786333065 |   null |  null |   null |      0 |    null
           ASSET | 0b3e1630-dd94-11ed-8620-0f4a384ba42f |                    timeoutMsgs | 1680307200000 | 1681786343126 |   null |  null |   null |      0 |    null
 API_USAGE_STATE | efacdb00-dd92-11ed-8620-0f4a384ba42f |            jsExecutionApiState | 1680307200000 | 1681785867440 |   null |  null |   null |   null | ENABLED
 API_USAGE_STATE | 6a4628e0-dd92-11ed-8a00-dbf2847f8f32 |                     emailLimit | 1680307200000 | 1681785643630 |   null |  null |   null |      0 |    null
 API_USAGE_STATE | efacdb00-dd92-11ed-8620-0f4a384ba42f |                     dbApiState | 1680307200000 | 1681785867440 |   null |  null |   null |   null | ENABLED
           ASSET | 0b3e1630-dd94-11ed-8620-0f4a384ba42f |                      tmpFailed | 1680307200000 | 1681786343126 |   null |  null |   null |      0 |    null
           ASSET | 2f13abb0-dd94-11ed-8620-0f4a384ba42f |                 successfulMsgs | 1680307200000 | 1681786403298 |   null |  null |   null |      1 |    null
           ASSET | 2f13abb0-dd94-11ed-8620-0f4a384ba42f |                     tmpTimeout | 1680307200000 | 1681786403298 |   null |  null |   null |      0 |    null
           ASSET | 0b3e1630-dd94-11ed-8620-0f4a384ba42f |                      totalMsgs | 1680307200000 | 1681786343126 |   null |  null |   null |      9 |    null
 API_USAGE_STATE | 05359790-dd94-11ed-8620-0f4a384ba42f |                     dbApiState | 1680307200000 | 1681786333065 |   null |  null |   null |   null | ENABLED
```


#s4m 
#thingsboard
