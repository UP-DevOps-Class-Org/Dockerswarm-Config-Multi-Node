
![telegram-cloud-photo-size-5-6222212696623919957-y](https://user-images.githubusercontent.com/34917417/214479834-84e2a5e9-2962-4f74-93c0-4aac0f649593.jpg)

# Dockerswarm-Config-Multi-Node

## Installation

**Create 2 nodes in AWS EC2 and Install Docker engine in each node**


***Note: My EC2 instance is Linux Debian***

**1. Install packages to allow apt to use a repository**
```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
**2. Add Docker’s official GPG key**
```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
**3. Use the following command to set up the repository:**
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

**4. Install Docker Engine**
```
sudo apt-get update
sudo chmod a+r /etc/apt/keyrings/docker.gpg
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
**5. Use the following command to check docker is installed**
```
docker ps 
```

## Swarm configuration
**Use the following command to Initialize a swarm**
```
root@swarm-node-1:~# docker swarm --help
root@swarm-node-1:~# docker swarm init
```
`Swarm initialized: current node (gt2ktjb5g97sa3wle3ha9w8zz) is now a manager.`

**Use the following command from another Node(EC2 instance) to add a worker to this swarm**
```
root@swarm-node-2:~# docker swarm join --token SWMTKN-1-4vx8iuatnhry0bzxdk049t6ygnccglw1hxe68781tueeiwowad-95gmdgg3irfrt2rwjhvmde5le 172.31.54.79:2377
```
**To add a manager to this swarm, run follow the instructions**
```
root@swarm-node-2:~# docker swarm join-token manager
```
***If Another Node already joined use following command to check available nodes***
```
root@swarm-node-1:~# docker node ls
```
<img width="929" alt="Screenshot 2023-01-25 at 12 26 26 pm" src="https://user-images.githubusercontent.com/34917417/214486957-87782c25-4509-42fe-b919-43fa3468ac35.png">

**Manual Copy project directory to swarm-node-1 host**
```
(base) kivsithvothy@Kivs-MBP Demo-Projects % scp -r Deployment root@54.157.141.39:/opt/                                               
```
**Check host IP**

```
curl ifconfig.me
```

**Use the following command to start deploy swarm services**

```
root@swarm-node-1:~# docker login nexus.sithvothykiv.site:8888 
root@swarm-node-1:~# cd /opt/Deployment/
root@swarm-node-1:/opt/Deployment# ./start.sh  or root@swarm-node-1:~# docker stack deploy node --compose-file docker-compose.yml --with-registry-auth

root@swarm-node-1:/opt/Deployment# docker service ls 
```
<img width="1363" alt="Screenshot 2023-01-27 at 11 36 01 am" src="https://user-images.githubusercontent.com/34917417/215010310-98d713ac-0b49-413d-958e-0197f6932e9d.png">

***Another node, Called swarm-node-2 Use follwing command to check running services***

```
root@swarm-node-2:~# docker ps 
```
<img width="1651" alt="Screenshot 2023-01-27 at 11 38 56 am" src="https://user-images.githubusercontent.com/34917417/215010600-36c76f35-71e6-45aa-97f2-af0e97e47858.png">

