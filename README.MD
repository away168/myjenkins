# Creating my own Jenkins server in Docker

Following instructions [here](https://www.jenkins.io/doc/book/installing/docker/)

Summarized as follows: 

1. Create network

```bash
docker network create jenkins
```

2. Run Docker:Dind in the network

```bash
docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind
```

3. Build Dockerfile (Custom Jenkins with BlueOcean plugin)

```bash
docker build -t myjenkins-blueocean:1.1 .
```

4. Run my Jenkins Server

```bash
docker run \
  --name jenkins-blueocean \
  --rm \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:1.1 
  ```