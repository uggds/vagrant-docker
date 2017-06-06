# USAGE

```
vagrant up
vagrant ssh
```

# USE Oracle Database

Download the binary file of Oracle Database here.  
http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html

```
git clone https://github.com/uggds/vagrant-docker.git
cd vagrant-docker/provisioning/config
git clone https://github.com/oracle/docker-images.git oracle
cd oracle/OracleDatabase/dockerfiles
mv /path/to/binary/linuxx64_12201_database.zip 12.2.0.1/
./buildDockerImage.sh -v 12.2.0.1 -e -i
```
---

If there is not enough space available in the docker container, you must increase Docker container size limit.

```
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
checkSpace.sh: ERROR - There is not enough space available in the docker container.
checkSpace.sh: The container needs at least 15 GB available.
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
```

## How to increase Docker container size limit
The minimum size of docker containers is 10 GB and its not possible to decrease it further. But you can increase the docker container size from 10 GB it to a higher value, say 20 GB, with these steps:
1. Stop the Docker daemon after taking backup of existing containers and images.
2. Reset the Docker default directory.
3. Start Docker service with the parameter ‘dm.basesize’ set for the new value for Docker container size limit.

```
dockerd --storage-opt dm.basesize=20G
```
The newly created containers will now have their size limits set to 20 GB.

### incase of using systemd
Open `/lib/systemd/system/docker.service`.
```
...
ExecStart=/usr/bin/dockerd --storage-opt dm.basesize=20G
...
```
and run
```
sudo systemctl daemon-reload
sudo service docker start
```
