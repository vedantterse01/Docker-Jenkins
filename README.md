# JENKINS SETUP USING DOCKER 
<ol>

## <li> Create a docker network

```bash
sudo docker network create jenkins-network
```
</li>

## <li> CREATE JENKINS CONTAINER 
```bash
sudo docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home --name jenkins-master --network jenkins-network -u root jenkins/jenkins:lts
```

### NOW visit the [localhost:8080](http://localhost:8080/)
## You will be prompted for the password 
![alt text](images/image.png)
### FOR PASSWORD 
```bash
sudo docker logs jenkins-master
```
![alt text](images/pass.png)
</li>

## SELECT > install suggested plugins
![alt text](images/installation.png)

wait for some time .. until the installation is done 

## CREATE THE ADMIN USER 
![alt text](images/user.png)


![alt text](images/url.png)



## <li> CREATE SLAVE NODE
### CLICK ON SETTINGS 
![alt text](images/settings.png)

### CLICK ON SETUP AGENT 
![alt text](images/setup-agent.png)

![alt text](images/newnode.png)

### FILL THE DETAILS 

![alt text](images/details1.png)

![alt text](images/details2.png)

#### FILL THE ENVIRONMENT VARIABLES

![alt text](images/details3.png)

![alt text](images/image-1.png)

### CLICK > SAVE 

![alt text](images/page.png)
#### CLICK ON THE > DOCKER-AGENT

### COPY THE > SECRET KEY 
![alt text](images/secret.png)

### CLICK ON CONFIGURE and SCROLL DOWN 
#### ADD AN ENVIRONMENT VARIABLE 
 ![alt text](images/envsecret.png)
 #### CLICK SAVE 

</li>

## <li> CREATE AGENT CONTAINER

```bash 
sudo docker run -d --name jenkins-agent -e JENKINS_URL=http://jenkins-master:8080/ -e JENKINS_AGENT_NAME=docker-agent -e JENKINS_SECRET=<secret-key> --network jenkins-network -u root jenkins/inbound-agent
```

### get inside the container 
```bash
sudo docker exec -it jenkins-agent bash
```
### NOW ENTER THE COMMANDS 
```bash
curl -sO http://host.docker.internal:8080/jnlpJars/agent.jar
```

```bash
java -jar agent.jar -url http://host.docker.internal:8080/ -secret <secret-key> -name "docker-agent" -webSocket -workDir "/home/jenkins/agent"
```
</li>

## <li> CLICK ON THE BUILT-IN-NODE
![alt text](images/page.png)

## CONFIGURE > NUMBER OF EXECUTORS  | SET IT TO > 0 
</li>
</ol>