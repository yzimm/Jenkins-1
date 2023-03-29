# Jenkins-101

How to run the conatainer :
1. Building doccker image following the command:
> docker build -t myjenkins-blueocean:2.332.3-1 .
Creating network called "jenkins"
> docker network create jenkins
Running container in UBUNTU 20.0.0.4
> docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1
