# simple-node-js-react-npm-app

This repository is for the
[Build a Node.js and React app with npm](https://jenkins.io/doc/tutorials/build-a-node-js-and-react-app-with-npm/)
tutorial in the [Jenkins User Documentation](https://jenkins.io/doc/).

The repository contains a simple Node.js and React application which generates
a web page with the content "Welcome to React" and is accompanied by a test to
check that the application renders satisfactorily.

The `jenkins` directory contains an example of the `Jenkinsfile` (i.e. Pipeline)
you'll be creating yourself during the tutorial and the `scripts` subdirectory
contains shell scripts with commands that are executed when Jenkins processes
the "Test" and "Deliver" stages of your Pipeline.

## Docker Setup

```
docker network create jenkins

docker build -t myjenkins-blueocean:1.1 .

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
 --volume "$HOME":/home \
 myjenkins-blueocean:1.1
```

OR

```
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
 --publish 3000:3000 \
 --publish 2376:2376 \
 docker:dind
```

1. Open Jenkins dashboard at http://localhost:8080
2. Display Jenkins logs to get admin password

```
docker logs jenkins-blueocean
```

3. Install suggested plugins.
4. Create admin user.

```
docker start jenkins-blueocean jenkins-docker
docker stop jenkins-blueocean jenkins-docker
```
