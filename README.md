# Docker for Absolute Beginners

## Docker Overview

Image - An Image is a package or a template which is used to craete one or more containers.

Containers - These are running instances of images that are isolated that have their own environment and set of processes.

With docker, the dev team can now have the docker file with all their requirements then this Docker file will be used to create an image for their application.
This image can now on any host with docker installed in it and the ops team can deploy this image in production.

## Getting started with docker

link: <https://docs.docker.com/engine/install/ubuntu/>

OS requirements
To install Docker Engine, we need the 64-bit version of one of these Ubuntu versions:

Ubuntu Kinetic 22.10
Ubuntu Jammy 22.04 (LTS)
Ubuntu Focal 20.04 (LTS)
Ubuntu Bionic 18.04 (LTS)

Uninstall old versions:

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Install using the repository

#### Set up the repository

* Update the apt package index and install packages to allow apt to use a repository over HTTPS:

 ```shell
 sudo apt-get update
 sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

* Add Docker’s official GPG key:

 ```shell
 sudo mkdir -p /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
 ```

* Use the following command to set up the repository:

 ```shell
 echo \

  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] <https://download.docker.com/linux/ubuntu> \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### Get the docker engine

* Update the apt package index:

 ```shell
 sudo apt-get update
 ```

* Install Docker Engine, containerd, and Docker Compose.

 ```shell
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
 ```

* Check installation:

 ```shell
 sudo docker run hello-world
 ```

But the Docker engine needs root access otherwise everytime we have to use sudo. Thus we gave to create the docker group and add your user:
link: <https://docs.docker.com/engine/install/linux-postinstall/>

* Create the docker group.

```shell
sudo groupadd docker
```

* Add your user to the docker group.

```shell
sudo usermod -aG docker $USER
```

* Log out and log back in so that your group membership is re-evaluated.

```shell
newgrp docker
```

* Verify that you can run docker commands without sudo.

```shell
docker run hello-world
```

## Basic Docker Commands

1. Run - Used to run a container from an image
   eg. ```docker run ngnix``` -- this will run an instance of the nginx application on the docker host
   if the image does not exist then docker will pull the image down from the docker hub , but only for once.

2. ps - The docker ps command lists all running containers and some basic information about them.
   eg. ```docker ps``` -- gives the container id, the name of the image, current status and the name of the conatainer.
   eg. ```docker ps -a``` -- gives a list of all containers be it running or not

3. stop - Stops the running container. If we dont know the name of the container then we can do docker ps to get the name.
   eg. ```docker ps``` -- gives the name of the running container say, "silly_samet"
   eg. ```docker stop silly_samet```

4. rm - Removes a container permanently. If it prints the name back then its ok
   eg. ```docker rm silly_samet```
   eg. ```docker ps -a``` -- this will show that silly_samet is no longer running or even on the list.

5. images - Gives  alist of images on the host
   ```docker images```
   ```docker images -q``` -- Returns a list of all image IDs, one per line.

6. rmi - Removes the images. We have to ensure that no containers are running of of that image before attempting to remove the image.
   We must stop any dependant container to be able to delete an image.
   eg. ```docker rmi nginx``` -- Removes the nginx image
   eg. ```docker rmi $(docker images -q)``` -- Removes all images at once

7. pull - Pulls the image without running it.
   eg. ```docker pull ubuntu``` -- Pulls the ubuntu image and stores it on our host.

8. exec - Executes a command in a running container.
   eg. first lets run a container
   ```docker run -d ubuntu sleep 180``` -- Ubuntu runs and sleeps for 180 secs giving us time to execute commands in it. This runs in detached mode so that we can use the terminal.
   ```docker ps``` -- Will show the the name of the running container , say holo_halibili
   ```docker exec holo_halibili cat /etc/hosts``` -- Will do a cat and print the contents of the /etc/hosts file

9. detach and attach - Lets us run the container in the foreground or the background
   eg. ```docker run -d  kodekloud/simple-webapp``` -- will run the container in the background
   eg. ```docker run kodekloud/simple-webapp``` -- will run the container in the foreground and will not take any command unless we manually exit the container
   eg. ```docker attach <conatainer-id>``` -- This will re attach the container if we have run it earlier in the detached mode.

10. -it - Say we do ```docker run centos```, this will actually lead to container shutting down. So if we want to kep it running, we need to keep a process running in it. Thus we can do: ```docker run -it centos bash```. This will run bash inside the container, thus keeping it alive and also the ```-it``` will allow us to automatically login into the container when we run it and get to the bash.

11. rm - This will remove the conatiners from the host, given they are not running
   eg. ```docker rm <container id>```
   eg. ```docker rmi $(docker ps -a -q)``` -- Removes all the non runnning containers.

* Say we wanted to run a docker container from an ubuntu image:

```shell
docker run ubuntu
```

It will run a container of the image and exits immedeately. We can verify and see that the new container is in a exited stage by doing:

```shell
docker ps
docker ps -a
```

Unlike VMs, containers are not meant to host an operating system. Containers are meant to run a specific task or a process. Once the tak is complete the container exits. A container lives only as long as the process inside it. Thus since ubuntu is just image of an operating system it is used as the base image for other applications and there is no process or application running in it by default.

## LAB Links

link: <https://udlabs.kodekloud.com/courses/udemy-labs-docker-for-the-absolute-beginner/>
Coupon Code: udemystudent1118

## Docker run commands

1. Say we do ```docker run redis``` , this will run the latest version of redis. But if we want to run a different version of redis we specify that in this way: ```docker run redis:4.0``` this is basically called a tag. Thus in the first command without any tag specifies, docker will consider the default tag "latest".

2. STDIN - Say we have an application "./app.sh". This will ask for a name in the prompt, where we have to enter our name and based on what we enter (say akash) we will see: "Hello and Welcome Akash!". But problem comes if we containerize and run : ```docker run kodekloud/simple-prompt-docker``` . We only get : "Hello and welcome" because the container ran in a non interactive mode. In order to make it run in an intercative mode : ```docker run -i kodekloud/simple-prompt-docker``` This will map the standard input of our host to the docker container. But still we will not see the prompt asking the name although we will be able to enter the name and get the desired output. Now we can also attach to the terminal of the container using: ```docker run -it kodekloud/simple-prompt-docker``` . This will now show the prompt that would ask for a name.

3. PORT Mapping

   ![Image for port mapping](portmappingimage1.png)

   Now say we run a simple web application in a docker container in our docker host:

   ```shell
   docker run kodekloud/webapp
   ```

   We will be getting an output like this : ```Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)```
   This says that the application is listening on port 5000 , but what IP should be used?
   1. By using the internal IP that the container get assigned by default eg. 172.17.0.2 . This is an internal IP an is only accessible within the docker host. So if we open a browser from within the host we can go to: <http://172.17.0.2:5000>.
   We can check what IP is assigned to a container by doing the following.

      ```shell
      docker ps 
      docker inspect <container_id>
      ```

   2. Now since this is an internal IP users outside the docker host cannot access this IP.
   For this we could use the IP of the docker host eg. 192.168.1.7. Again for this to work we will have to map the port inside the docker container to a free port of the docker host.
   eg. If we want the users to access my application through port 80 on the docker host then we could map port 80 of the host to port 5000 on the container like this:

      ```shell
      docker run -p 80:5000 kodekloud/webapp
      docker run -p <hostport>:<containerport> <image>
      ```

      No we will be able to access the application using: <http://192.168.0.1:80>
      All traffic to the port 80 of host will be redirected to port 5000 of the docker container.

   We cannot map to the same port of the docker host more than once.

4. Volume Mapping

   It is the way buy which data is persisted in a docker container.
   For eg. lets say that we were to run a mysql container.

   ```shell
   docker run mysql
   ```

   When databases and tables are created the data is stored within the container say in the path ```/var/lib/mysql``` inside the docker container which has its own isolated file systesm and any changes to the files happens within the container.

   Now say we want to delete this container and remove it.

   ```shell
   docker stop mysql
   docker rm mysql
   ```

   As soon as we do it the data indside it gets blown away.
   So if we want to persist data we will map a directory outside the container on the docker host to directory inside the container.

   ```shell
   docker run -v /opt/datadir:var/lib/mysql mysql
   docker run -v <path on host>:<path on container>
   ```

   Note: In order to see this happen, when we restart the container, it should be given or mapped to the same directory.

5. Container Logs

   In order to see the logs of a container we ran in the background we do:

   ```shell
   docker logs <container_name>
   ```

### Docker Images

#### How to create own images ?

Here we will take example of a simple webapplication made in flask.
Link : <https://github.com/mmumshad/simple-webapp-flask>

1. Start with an OS like Ubuntu
2. Update the source repositories using "apt" command
3. Install dependencie using the "apt" command
4. Install python dependencies using the "pip" command
5. Copy the source code to a loaction like : "/opt"
6. Run the webserved using the "flask" command

Keeping all the above instructions in mind we will create a Dockerfile :

```Dockerfile
FROM Ubuntu

RUN apt-get update && apt-get -y install python3 python3-setuptools python3-dev build-essential python3-pip python3-mysqldb
RUN pip3 install flask flask-mysql

COPY app.py /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

Once the Dockerfile is complete we have to build the image and push it to dockerhub in the following way:

```shell
docker build . -f Dockerfile -t akash/my-custom-app
docker push akash/my-custom-app
```

#### Dockerfile

Dockerfile is a text file that is written in a specific format that Docker can understand.
It is in an instruction and argument format.
The instructions say Docker to perform a specific task whehn it is building the image.

1. FROM Ubuntu -- Defines what the base OS should be for the container. Every Docker image must be based of of another image. All Dockerfiles must start with the "FROM" statement.

2. RUN -- This asks the dockers to run a particular command on the base image.

3. COPY -- Copies files from the local system to the docker image. In the example, the code is in the working directory (.) and we will be copying it to loaction "/opt/source-code" inside the docker image.

4. ENTRYPOINT -- Specifies a command that will be run when the image is run as a container.

When docker builds these images it builds them in a layered architecture. Each line of instruction creates a new layer in the docker image with just the changes from the previous layer.

![Image for layerd architecture](LayeredArchitecture.png)

We can see the same if we do:

```shell
docker history <image_name>
```

![Image for layerd architecture](LayeredArchitecture2.png)

All the layers built are cached by docker, so in case if a particular step fails then on rerunning it will reuse the latest build layer in the cache and  continue to build the remaining layers.
The same is true if we were to add addition al steps to the Dockerfile, this way rebuilding our image is faster. Helpful during update of applications.

#### Demo - Creating our own  docker image

We will be using the following code as an application that needs to be deployed
LINK: <https://github.com/mmumshad/simple-webapp-flask>

First we will run and check everything on the local system:

1. Get the base image and create a bash session to make an environment.

   ```shell
      docker run -it ubuntu bash
   ```

   This will create a ubuntu container with the bash terminal session.
   Now inside the terminal we will do the following

   ```shell
   apt-get update
   apt-get install -y python3 python3-setuptools python3-dev build-essential python3-pip python3-mysqldb
   pip3 install flask
   pip3 install flask-mysql
   cat > /opt/app.py
   ```

   Then we copy the code:

   ```python
   import os
   from flask import Flask
   app = Flask(__name__)

   @app.route("/")
   def main():
      return "Welcome!"

   @app.route('/how are you')
   def hello():
      return 'I am good, how about you?'

   if __name__ == "__main__":
      app.run(host="0.0.0.0", port=8080)
   ```

   Again exit the file and type the following in the bash shell

   ```shell
   cd /opt/
   FLASK_APP=app.py flask run --host=0.0.0.0
   ```

   This will deploy the webapp but it will only be accessible by the host system.

2. Thus the Dockerfile is:

   ```Dockerfile

   FROM ubuntu
   RUN apt-get update && apt-get install -y python3 python3-setuptools python3-dev build-essential python3-pip python3-mysqldb
   RUN pip3 install flask flask-mysql
   COPY app.py /opt/
   ENTRYPOINT FLASK_APP=/opt/app.py flask run --host=0.0.0.0 --port=8080

   ```

   Deploy the same by:

   ```shell
   docker build . -t mysimple-webapp:latest
   ```

   Run the container:

   ```shell
   docker run mysimple-webapp
   ```

3. Push the image to docker hub

   We first have to login and then build a tagged image with our username and then push it

   ```shell
   docker login
   docker build . -t akashchakraborty/kodekloud_simple_webapp:pushversion 
   docker push akashchakraborty/kodekloud_simple_webapp:pushversion
   ```

#### Environment Variables

Say there is a single piece of web application written in python "app.py"

![Image for application code](app.py_env_var.png)

In this code there is:

```python
color = 'red'
```

This will set the background color to red. However if we decide to change the color in future, we will have to change the application code.
Thus it is a best practice to move such information out of the application code and into an environment variable:

```python
color = os.environ.get('APP_COLOR')
```

So next time when we run the application we can set an environment variable:

```shell
export APP_COLOR = blue; python app.py
```

To set this environment variable while running a container based on the image of this app, we do:

```shell
docker run -e APP_COLOR=blue <image-name>
```

Now, the question is how do we find the env variable set in a container that is already running. We can do that while doing docker inspect.

![Image for docker inspect environment variable](InspectingEnvVar.png)

NOTE: To know the env field from within a webapp container, run:

```shell
docker exec -it webapp env
```
