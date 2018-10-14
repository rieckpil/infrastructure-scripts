## Debian Jessie (8.X)

* https://www.tecmint.com/install-postgresql-on-debian-and-ubuntu/

```
touch /etc/apt/sources.list.d/pgdg.list
deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt install postgresql-9.6  
```

Data directory: `/var/lib/postgresql/9.6/main`
Config directory: `/etc/postgresql/9.6/main`

```
listen_adress="*"
host    all             all             0.0.0.0/0            md5
```


## Ubuntu 18.04

```
 sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
 wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
 apt-get install postgresql-9.6

 sudo su - postgres
 psql
 \password postgres
 ```