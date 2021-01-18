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
