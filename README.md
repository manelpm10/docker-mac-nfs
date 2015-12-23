#### Setting NFS for your virtual machine
In order to speed your projects, it's recommended to use NFS to share the files. Just run the following lines, changing MY_HOME_DIR for your local machine home dir (aka $HOME):

```
echo "# Docker
\"$HOME\" -alldirs -mapall=$(whoami) -network 192.168.99.0 -mask 255.255.255.0" | sudo tee -a /etc/exports

sudo nfsd checkexports && sudo nfsd restart

docker-machine ssh default "sudo mkdir -p $HOME && sudo mount -t nfs -o noatime,soft,nolock,vers=3,udp,proto=udp,rsize=8192,wsize=8192,namlen=255,timeo=10,retrans=3,nfsvers=3 -v 192.168.99.1:$HOME $HOME"

docker-machine ssh default mount

docker-machine ssh default

echo '#!/bin/sh' | sudo tee /var/lib/boot2docker/bootlocal.sh && sudo chmod 755 /var/lib/boot2docker/bootlocal.sh

echo "sudo mkdir -p MY_HOME_DIR && sudo mount -t nfs -o noatime,soft,nolock,vers=3,udp,proto=udp,rsize=8192,wsize=8192,namlen=255,timeo=10,retrans=3,nfsvers=3 -v 192.168.99.1:MY_HOME_DIR MY_HOME_DIR" | sudo tee -a /var/lib/boot2docker/bootlocal.sh
```
