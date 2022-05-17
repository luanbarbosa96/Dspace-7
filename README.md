# Dspace-7
Installation of D-Space

apt-get install tomcat9
apt-get install openjdk-11-jre 
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.zip
nano /etc/environment

JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"
JRE_HOME="/usr/lib/jvm/java-11-openjdk-amd64/jre"
CATALINA_HOME="/usr/share/tomcat9"
CATALINA_BASE="/var/lib/tomcat9"
TOMCAT_HOME="/usr/share/tomcat9"        
JAVA_OPTS="-Xms2048m -Xmx7168m -Dfile.encoding=UTF-8"
CATALINA_OPTS="server -Xms384M -Xmx512M"

export PATH=/opt/apache-maven-3.8.5/bin:$PATH

apt-get install ant
apt-get install postgresql-11 postgresql-contrib-11 libpostgresql-jdbc-java -y
nano /etc/postgresql/11/main/pg_hba.conf
      trust
      md5

sudo service postgresql restart
sudo -u postgres psql
        postgres=# create database dspace;
        postgres=# create user dspace with encrypted password 'dspace';
        postgres=# grant all privileges on database dspace to dspace;
 \q

psql -U postgres -d dspace
         CREATE EXTENSION pgcrypto;
\q

apt-get install perl
sudo useradd -m dspace
sudo passwd dspace
mkdir /dspace
chown dspace /dspace
mkdir /build
chmod -R 777 /build

cd /build
wget https://github.com/DSpace/DSpace/archive/refs/tags/dspace-7.1.zip
unzip dspace-7.1.zip

cd DSpace-dspace-7.1/
mvn -U package
apt-get install git
mvn -U package

cd /build/DSpace-dspace-7.1/dsapce/target/dspace-installer/
ant fresh_install


#Configure Tomcat
/etc/tomcat9/Catalina/localhost/server.xml
xmlui.xml

<?xml version='1.0'?>
<Context
docBase="/dspace/webapps/server"/>

chown tomcat:tomcat /dspace/*

/dspace/bin/dspace databse migrate
/dspace/bin/dspace create-administrator
/etc/init.d/tomcat9 restart

http://localhost:8080/server
