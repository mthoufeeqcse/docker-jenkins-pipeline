# docker-jenkins-pipeline
Jenkins Docker pipeline to deploy a HTML page in dockerized tomcat

The idea is to run a container socat in parallel to jenkins container. The socat container mounts the /var/run/docker.run 
and listens at port 2375.

From jenkins using docker-pipeline plugin and passing the docker URI to communicate to port 2375. 
This way we can invoke docker inside jenkins docker container.

Example:
withDockerServer([uri: 'tcp://10.118.56.21:2375']) { }

# Steps to be executed: 

1. run socat container :
docker run -d -v /var/run/docker.sock:/var/run/docker.sock -p 2375 verb/socat:alpine tcp-listen:2375,reuseaddr,fork unix:/var/run/docker.sock

2. run jenkins container :
docker run --name pipeline-jenkins -p 8080:8080 -p 50000:50000 -v /var/jenkins_home -v pipeline-jenkins
