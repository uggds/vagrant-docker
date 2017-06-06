# USAGE

```
vagrant up
vagrant ssh
```

# use Oracle Database

Download the binary file of Oracle Database here.  
http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html

```
git clone https://github.com/uggds/vagrant-docker.git
cd vagrant-docker/provisioning/config
git clone https://github.com/oracle/docker-images.git oracle
cd oracle/OracleDatabase/dockerfiles
mv /path/to/binary/linuxx64_12201_database.zip 12.1.0.2/
./buildDockerImage.sh -v 12.1.0.2 -e -i
```
