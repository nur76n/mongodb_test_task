# MongoDB Test Task


### Install vim
```
apt install nano
```

### Setting up timezone
```
dpkg-reconfigure tzdata
```
### Uninstall all conflicting packages
```
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
### Install docker engine
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Verify docker engine installation
```
sudo docker run --rm hello-world
```


### Create mongodb compose file
```
mkdir mongodb && cd mongodb
```
```
nano compose.yml
```

### MongoDB `compose.yml` file

```
version: '3.1'

services:

  mongodb1:
    container_name: mongodb1
    image: mongo:7.0.2-jammy
    environment:
      MONGO_INITDB_ROOT_USERNAME: root1
      MONGO_INITDB_ROOT_PASSWORD: password1
  mongodb2:
    container_name: mongodb2
    image: mongo:7.0.2-jammy
    environment:
      MONGO_INITDB_ROOT_USERNAME: root2
      MONGO_INITDB_ROOT_PASSWORD: password2
```	  
	  


### Start containers and ensure that containers are running
```
docker compose up -d

docker ps
```

### Connect to the mongodb1 container and connect to the mongodb2's shell

#### variant 1
```
docker exec -it mongodb1 mongosh "mongodb://root2@mongodb2/admin"

db.runCommand( {
   dbStats: 1      
} )

exit
```

#### variant 2:
```
docker exec -it mongodb1 /bin/bash

mongosh "mongodb://root2@mongodb2"

show dbs;

exit

exit
```

