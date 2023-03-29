To run the command at once:
java -jar jenkins-cli.jar -s http://localhost:8080 build my-python-job -p message="DevOps is great"






# instruction on how to run the container. 
1. Building docker image following the command:
> docker build -t myjenkins-blueocean:2.332.3-1 .
2. Creating network called "Jenkins"
   > docker network create Jenkins
   
   
3.Running container in UBUNTU 20.0.0.4
  > docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1
  
  
  
4.Get password for Jenkins web.
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
 Accessing http://localhost:8080/
 
 
5.inserting the code that we get in section 4


6.Installing Jenkins CLI on ubuntu 20.0.04
curl -L https://github.com/jenkins-zh/jenkins-cli/releases/latest/download/jcli-linux-amd64.tar.gz|tar xzv
sudo mv jcli /usr/local/bin/


Points to take into consideration:

Storage : Backup and Disaster Recovery: It is important to regularly backup Jenkins data and configurations to ensure that data is not lost in case of a disaster.
Security : It is important to ensure that only authorized users have access to the system Jenkins allows you to configure SSL/TLS encryption to secure communication between the Jenkins server and clients to prevent unauthorized access.
Maintainability : Scalability and Performance: As your Jenkins system grows, it is important to ensure that it remains scalable and performs well. This includes considerations such as storage capacity, network bandwidth, and server hardware.
Example for docker-compose.yaml : 
storage - In the example below there is a local driver and NFS  share in diffrent location 
192.168.1.100  in /mnt/nfs/db-data for backup purposes.
security - Both services have secrets defined app service has a secret named db_password. 
The db service also has a secret named db_password.
Maintainability -  named volumes makes it easier to manage the volumes and share it across containers this making the volume more  maintainable.

file : docker-compose.yaml 
version: "3.8"

services:
  app:
    image: myapp:latest
    volumes:
      - data:/app/data
    environment:
      - DB_USER=myuser
      - DB_PASSWORD=mypassword
    secrets:
      - db_password

  db:
    image: mysql:latest
    volumes:
      - db-data:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
    secrets:
      - db_password

volumes:
  data:
    driver: local
    driver_opts:
      type: nfs
      o: addr=192.168.1.100,rw
      device: ":/mnt/nfs/data"

  db-data:
    driver: local
    driver_opts:
      type: nfs
      o: addr=192.168.1.100,rw
      device: ":/mnt/nfs/db-data"

secrets:
  db_password:
    external: true


