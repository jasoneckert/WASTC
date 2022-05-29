# Docker Basics
  - Open a Terminal on your Fedora Workstation virtual machine as root
  - `snap install docker` 
  - `docker search busybox | less`
  - `docker pull busybox` (pulls the official busybox Linux container image from Docker Hub)
  - `docker images` (you can remove a downloaded image using "docker rmi imagename")
  - `docker run busybox echo Hello World` (exits upon completion)
  - `docker ps` (note that there are no running containers)
  - `docker ps -a` (note the container run in the past based on the busybox image)
  - `docker run -it busybox sh` (runs a shell in the container for you to interact with)
     - `ls`
     - `ls /etc`
     - `ls /bin`
     - `ps -ef`
     - `exit`
  - `docker ps`
  - `docker ps -a`
  - `docker container prune` (removes all history of stopped containers)
  - `docker search apache | less`  
  - `docker pull httpd`  
  - `docker images`
  - `docker run -d -P --name site1 httpd` (-P exposes a random port on the underlying OS that is mapped to :80 in the container)  
  - `docker port site1`
  - `docker ps`
  - Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIP:port (where port is the exposed port of your site1 container)
  - `docker run -d -P --name site2 httpd`   
  - `docker port site2`
  - `docker ps`
  - `docker exec -it site2 sh`  
     - `cd htdocs`
     - `ls`
     - `cat index.html`
     - `echo “<html><body><h1>Site 2 works!</h1></body></html>” > index.html`
     - `exit`
  - Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIP:port (where port is the exposed port of your site2 container)
  - `docker stop site1`
  - `docker stop site2`
  - `docker ps`
  - `docker ps -a`
  - `docker container prune`
  - `docker ps -a`	   

# How Many Containers Can We Run?!?
  - To see how many comtainers can be run on a single cloud host (versus VMs), create and execute the following shell script (use Cockpit to monitor resources)
  - For added fun, do this on a Raspberry Pi 4 (instructions on how to install Docker on the Raspberry Pi OS Linux distro, see https://dev.to/elalemanyo/how-to-install-docker-and-docker-compose-on-raspberry-pi-1mo)
```  
  COUNTER=1
  while true
  do
    docker run -d -P --name site$COUNTER httpd
    COUNTER=$(expr $COUNTER + 1)
  done
```  
  - When you are satisfied with the number of containers run, stop the shell script and create and run the following shell script to stop the containers
```  
  COUNTER=1
  while true
  do
    docker stop site$COUNTER 
    COUNTER=$(expr $COUNTER + 1)
  done
```

# Building Containers
  - Developers often start with a pre-built container image, add their Web app to it, and then push that image to Docker Hub. 
  - To illustrate this process without creating an actual Web app, we’ll create another container image based on httpd, but with a different webpage, run it locally, and then push it to Docker Hub. 
  - First create a free account on Docker Hub (https://hub.docker.com) and follow the instructions to create a free public repository called webapp. 
  - Next, create an access token (password equivalent for uploading to your Docker Hub repository) by navigating to Account Settings > Security > New Access Token (you’ll need to copy this token and use it as a password when connecting to Docker Hub later on).
  - `mkdir webappcontainer && cd webappcontainer` 
  - `vi index.html` (add the following contents, saving your changes)
     - `<html><body><h1>This is a custom app!</h1></body></html>`
  - `vi Dockerfile` (add the following contents, saving your changes)
     - `FROM httpd`
     - `COPY ./index.html htdocs/index.html`
  - `cd webappcontainer`
  - `docker build -t jasoneckert/webapp .` (replace jasoneckert with your Docker Hub username) 
  - `docker images`  
  - `docker login -u jasoneckert` (paste your Docker Hub access token when prompted)
  - `docker push jasoneckert/webapp:latest`
  - `docker run -d -p 80:80 --name webapp jasoneckert/webapp` 
  - Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIP
  - `docker --help`
  - `docker run --help`
  - `docker exec --help`
