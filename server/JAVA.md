# Java

## Debian

`/etc/apt/sources.list`

```
# jessie-backports allows newer software to be installed
deb http://http.us.debian.org/debian/ jessie-backports main
deb-src http://http.us.debian.org/debian/ jessie-backports main
```


```
sudo apt-get install -t jessie-backports openjdk-8-jdk
```

## Ubuntu 18.04

```
sudo apt-get install openjdk-8-jdk
```
```
vi /etc/enviroment
JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
source /etc/environment
```


## Maven

```
cd /usr/local
wget http://www-eu.apache.org/dist/maven/maven-3/3.5.4/binaries/apache-maven-3.5.4-bin.tar.gz
sudo tar xzf apache-maven-3.5.4-bin.tar.gz
sudo ln -s apache-maven-3.5.4 apache-maven
sudo vi /etc/profile.d/apache-maven.sh

export JAVA_HOME=${JAVA_HOME}
export M2_HOME=/usr/local/apache-maven
export MAVEN_HOME=/usr/local/apache-maven
export PATH=${M2_HOME}/bin:${PATH}

source /etc/profile.d/apache-maven.sh
```
## Systemd Java 

```
[Unit]
Description=PreTrack application
After=syslog.target

[Service]
User=app
ExecStart=/usr/lib/jvm/java-8-openjdk-amd64/bin/java -jar /home/app/workspace/backend/target/backend-0.0.1-SNAPSHOT.jar
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl enable service
sudo systemctl daemon-reload
```