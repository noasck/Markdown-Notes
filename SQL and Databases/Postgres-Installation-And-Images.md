# Installation of PostgreSQL on Ubuntu
--------------
## 1. Enable PostgreSQL Apt Repository *(optional?)*


``` bash
sudo apt-get install wget ca-certificates
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
```

## 2. Installing packages

``` bash
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib

```
Multiple other dependencies will also be installed. PostgreSQL 12 is the latest available version during the last update of this tutorial.

---------------

## 3. Configuring user

So to connect to Postgres server, log in to your system as user postgres and connect the database.
``` bash
sudo su - postgres
psql
```
Now configure PostgreSQL to make is accessible by your normal users. Change your_username with your actual user already created on your Ubuntu system.
``` sql
 CREATE ROLE <your_username> WITH LOGIN CREATEDB ENCRYPTED PASSWORD '<your_password>';
 \q --- exit from postgres
```
Then switch to the user account and run createdb command followed by the database name. This will create a database on PostgreSQL.
``` bash
su - your_username
createdb <my_db>
```
**Ready**


------------------------------

# Installing PostgrSQL with PostGIS and PgAdmin in Docker
### Links
- Crunchy data tutorial: [Read more...](https://blog.crunchydata.com/blog/easy-postgresql-10-and-pgadmin-4-setup-with-docker)
- Article on Medium: [Read more...](https://medium.com/spatial-data-science/how-to-install-postgis-and-pgadmin4-with-docker-easily-3f4cb3551bef)
- High Availability, Load Balancing, and Replication - **Postgres docs** - [Read more...](https://www.postgresql.org/docs/12/high-availability.html)
- How to Deploy PostgreSQL for High Availability - [Read more...](https://severalnines.com/database-blog/how-deploy-postgresql-high-availability)
- **Official** crunchy data examples - [Github repo](https://github.com/CrunchyData/crunchy-containers/tree/master/examples)

## 1) Select Postgres image
- Crunchy PostgreSQL - [On Docker Hub...](https://hub.docker.com/r/crunchydata/crunchy-postgres)
- Crunchy PostgreSQL + PostGIS - [On Docker Hub...](https://hub.docker.com/r/crunchydata/crunchy-postgres-gis)
- Crunchy PostgreSQL + PostGIS **HA** - [On Docker Hub...](https://hub.docker.com/r/crunchydata/crunchy-postgres-gis-ha)

## 2) Run bash-script to setup Docker container
Replace all env's and Hub image version with valid values.

``` python
#!/bin/bash

mkdir postgres
cd postgres

docker volume create --driver local --name=pgvolume
docker volume create --driver local --name=pga4volume

docker network create --driver bridge pgnetwork

cat << EOF > pg-env.list
PG_MODE=primary
PG_PRIMARY_USER=postgres
PG_PRIMARY_PASSWORD=yoursecurepassword
PG_DATABASE=testdb
PG_USER=yourusername
PG_PASSWORD=yoursecurepassword
PG_ROOT_PASSWORD=yoursecurepassword
PG_PRIMARY_PORT=5432
EOF

cat << EOF > pgadmin-env.list
PGADMIN_SETUP_EMAIL=youremail@yourdomain.com
PGADMIN_SETUP_PASSWORD=yoursecurepassword
SERVER_PORT=5050
EOF

docker run --publish 5432:5432 \
  --volume=pgvolume:/pgdata \
  --env-file=pg-env.list \
  --name="postgres" \
  --hostname="postgres" \
  --network="pgnetwork" \
  --detach \
crunchydata/crunchy-postgres:centos7-10.9-2.4.1

docker run --publish 5050:5050 \
  --volume=pga4volume:/var/lib/pgadmin \
  --env-file=pgadmin-env.list \
  --name="pgadmin4" \
  --hostname="pgadmin4" \
  --network="pgnetwork" \
  --detach \
crunchydata/crunchy-pgadmin4:centos7-10.9-2.4.1
```
## 3) Configure via webUI

1. Go to http://localhost:5050/
2. Log into pgAdmin 4 with:
    - Email: youremail@yourdomain.com
    - Password: yoursecurepassword
3. Add a server using:
    - Hostname: postgres
    - Username: yourusername
    - Password: yourpassword
