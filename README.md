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
cd vagrant-docker
cd provisioning
echo "  - name: copy oracle\n    copy: src=./config/oracle dest=/home/vagrant/ owner=vagrant" >> vagrant.yml
cd config
git clone https://github.com/oracle/docker-images.git oracle
cd ../..
vagrant provision
vagrant ssh
vagrant@localhost ~/$ cd oracle/OracleDatabase/dockerfiles
vagrant@localhost ~/$ mv /path/to/binary/linuxx64_12201_database.zip 12.2.0.1/
vagrant@localhost ~/$ ./buildDockerImage.sh -v 12.2.0.1 -e -i
```
And docker run.
```
vagrant@localhost ~/$ sudo docker run -p 1521:1521 --name ora oracle/database:12.2.0.1-ee
```

# Trouble shoot

### If there is not enough space available in the docker container, you must increase Docker container size limit.

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
Open `/lib/systemd/system/docker.service`,
```
...
ExecStart=/usr/bin/dockerd --storage-opt dm.basesize=20G
...
```
and restart docker service.
```
sudo systemctl daemon-reload
sudo service docker restart
```

### docker does not start with an error of iptables
you attempt an error similar to the following.
```
$ sudo systemctl start docker
Job for docker.service failed because the control process exited with error code. See "systemctl status docker.service"
```
you can check the error log with the following.
```
$ sudo journalctl -xe
```

```
-- Unit docker.service has begun starting up.
2600  6月 07 10:03:42 localhost dockerd[3887]: time="2017-06-07T10:03:42.332563149Z" level=info msg="libcontainerd: new containerd process, pid: 3890"
2601  6月 07 10:03:43 localhost dockerd[3887]: time="2017-06-07T10:03:43.355875972Z" level=warning msg="devmapper: Usage of loopback devices is strongly discouraged for production use. Please use `--storage-opt dm.thinpooldev` or use >
2602  6月 07 10:03:43 localhost dockerd[3887]: time="2017-06-07T10:03:43.387532312Z" level=warning msg="devmapper: Base device already exists and has filesystem xfs on it. User specified filesystem  will be ignored."
2603  6月 07 10:03:43 localhost dockerd[3887]: time="2017-06-07T10:03:43.392868323Z" level=error msg="[graphdriver] prior storage driver \"devicemapper\" failed: devmapper: Base device size cannot be smaller than 26.84 GB"
2604  6月 07 10:03:43 localhost dockerd[3887]: time="2017-06-07T10:03:43.393031778Z" level=fatal msg="Error starting daemon: error initializing graphdriver: devmapper: Base device size cannot be smaller than 26.84 GB"
2605  6月 07 10:03:43 localhost systemd[1]: docker.service: main process exited, code=exited, status=1/FAILURE
2606  6月 07 10:03:43 localhost systemd[1]: Failed to start Docker Application Container Engine.
2607 -- Subject: Unit docker.service has failed
2608 -- Defined-By: systemd
2609 -- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
2610 ---
2611 -- Unit docker.service has failed.
2612 ---
2613 -- The result is failed.
2614  6月 07 10:03:43 localhost systemd[1]: Unit docker.service entered failed state.
2615  6月 07 10:03:43 localhost systemd[1]: docker.service failed.
```

line:2603
```
level=error msg="[graphdriver] prior storage driver \"devicemapper\" failed: devmapper: Base device size cannot be smaller than 26.84 GB"
```
workaround is - rm -rf /var/lib/docker & restart docker(sometimes host reboot needed)

# Ref
https://github.com/oracle/docker-images/tree/master/OracleDatabase  
http://qiita.com/lethe2211/items/0bb493fa93a0088cfac9  
https://bobcares.com/blog/docker-container-size/
