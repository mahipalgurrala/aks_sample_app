# **WORKING WITH CONTAINERS**
---

### **Activity 1 - Docker Compose Installation/Setup**

1. Install/setup Docker Compose:
   
    Update Repositories and Packages -

      ```bash
      sudo yum update
      sudo yum upgrade
      ```

    Install curl utilities If not installed -

      ```bash
      sudo yum install curl
      ```

    Download Docker Compose -

      ```bash
      sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
      ```

    Change the file permissions to make the software executable -

      ```bash
      sudo chmod +x /usr/local/bin/docker-compose
      ```

    Check by running the below command whether  Docker Compose is working -

      ```bash
      docker-compose
      ```
      
---

### **Activity 2 - Searching, Pulling, and Displaying local images**

---

1. Log in to the Docker Hub account, explore different docker images by typing image name in the search explorer. Try image like: java, python, nginx, alpine, accenture/adop-nginx etc.

2. To list the different flags available with search use the below command -

```bash
docker search --help
```

3. Use the below command to search for an image using the docker cli –

```bash
# docker search <term>
docker search adop
```

4. To list all sub-commands available with the **`docker image`** management command use -

```bash
docker image --help
```

5. To pull an image from a registry (default is `hub.docker.com`) use –

```bash
# New Way
# docker image pull <IMAGE-NAME>[:<TAG>]
docker image pull alpine

# Old Way
# docker pull <IMAGE-NAME>[:<TAG>]
docker pull busybox:1.28.1
```

**NOTE**: If tag is not specified, default is always `latest`.

6. To display list of all local images use -

```bash
# New Way
docker image ls

# Old Way
docker images
```

---

### **Activity 03 - Executing an image and listing containers**

---

1. To list all sub-commands available with the **`docker container`** management command use –

```bash
docker container --help
```

2. To view all options available to work with **`run`** use the below command -

```bash
docker container run --help
```

3. To execute a simple docker image use the below command –

```bash
# New Way
docker container run hello-world
# Old Way
docker run hello-world
```

**NOTE**: The hello-world image is not pulled again when the command is executed second time because it is cached locally.

4. To create a container from an existing image (i.e. alpine) and execute a command inside it use - 

```bash
docker container run alpine echo "Hello, World"
```

**NOTE**: After the command execution, the container exits irrespective of the outcome.

5. To list the directory of alpine image by running `ls` command inside a container use -

```bash
docker container run alpine ls
```

6. To list the processes running inside container of alpine image use `ps` command with arguments `-ef` -

```bash
docker container run alpine ps -ef
```

**NOTE**: Ideally, there should only be a single process running inside a container.

7. To list all available options with the **`container ls`** sub-command use –

```bash
docker container ls --help
```

8. To list all running containers use –

```bash
# New Way
docker container ls

# Old Way
docker ps 
```

9. To list all running and stopped containers use the option `-a` or `--all` with `ls` –

```bash
# New Way
docker container ls -a

# Old Way
docker ps -a
```

**NOTE**: When you run a docker container without using the `--name` option, the docker engine assigns a name to that container. The name is comprised of an adjective and a famous personality last-name, joint by an underscore (_).

10. Try out the below commands with different options. 

```bash
# display all the containers including exited one
docker container ls -a 
# display only the containers id including exited one
docker container ls -aq
# display the last container started 
docker container ls -l 
# display only the id of last container started
docker container ls -lq 
```

11. The `--filter` option can be used to filter out the containers based on a key=value –

```bash
# docker container ls --filter key=value
docker container ls --filter status=exited
```

[REFERENCE LINK - docker container ls](https://docs.docker.com/engine/reference/commandline/container_ls/)

[SUPPORTED FILTERS](https://docs.docker.com/engine/reference/commandline/ps/#filtering)

---

### **Activity 04 - Using a container with terminal**

---

1. To use a container with terminal requires two different options –

i. `-i`, `--interactive` - It keeps STDIN open even if not attached.

ii. `-t`, `--tty` - Allocates a pseudo-TTY

2. To open an interactive pseudo terminal use -

```bash
# New Way

docker container run -it alpine sh

# Old Way

docker run -it alpine sh
```

* Add a new file by using **`touch`** command 
  
* Then list down the directory and files inside the container using **`ls`** command
  
* Type **`exit`** to shutdown and exit the terminal.

**NOTE**: When the terminal exits, then the container is shut down.

```bash
docker container ls -al
```

3. The new container that uses the same image will run with its thin writeable layer. Therefore, the file saved earlier is not present. Try it out! 

```bash
docker container run -it alpine sh

# on the container shell type ls to list directories and files 
ls
```

**NOTE**: To keep the container running rather than shut it down, use the keys combination of (**`ctrl + p + q`**).

4. To exec into container shell once again use -

```bash
# New Way
docker container exec -it <CONTAINER-ID|NAME> sh
# Old Way
docker exec -it <CONTAINER-ID|NAME> sh
```

**NOTE**: Typing the command `exit` does not terminates the container if its shell is accessed using `exec`; therefore, there is no need to use the keys combination of **`ctrl + p + q`**.  

---

### **Activity 05 - Running container in detached mode**

---

1. The information or logs of the container gets displayed directly on the host client, when the container ran without using the **`--detach`** or **`-d`** option. 

```bash
docker container run alpine ping localhost -c 4
```

2. The **`--detach`** or **`-d`** option does not log the container information, and it runs the container in detached mode.

```bash
docker container run --detach alpine ping localhost -c 100
# OR
docker container run -d alpine ping localhost -c 100
```

3. To see the list of running containers use -

```bash
docker container ls
```

4. To attach the client back to the docker container use the attach command –

```bash
# New Way
docker container attach <CONTAINER-ID|NAME>

# Old Way
docker attach <CONTAINER-ID|NAME>
```

[Reference link - docker container attach](https://docs.docker.com/engine/reference/commandline/attach/)

---

### **Activity 06 - Running a web server application container**

---

1. To pull the nginx web server image use -

```bash
docker image pull nginx:alpine
```

2. The `--name` option assigns a name to the container, instead of default name given by the docker engine.

3. The `-P` (capital P) option publishes the container port, which are exposed, to a random port on the host.

4. To run the nginx webserver on host machine published on random port in detached mode use - 

```bash
docker container run --detach --name webserver -P nginx:alpine
```

5. To view the published port use the `container ls` command - 

```bash
docker container ls
```

6. Open a new web browser tab window, and type the URL as given below.

```txt
http://<HOST-IP-ADDRESS>:<PUBLISHED-PORT>
```

**NOTE**: `HOST-IP-ADDRESS` is ther server IP Address received in Email ID. 

7. The `--publish` or `-p` option binds the container ports that are exposed to specific ports on the host machine.

```bash
# <HOST-PORT>:<CONTAINER-PORT>
docker container run --detach --name webserver2 --publish 80:80 nginx:alpine
# OR
docker container run -d --name webserver2 -p 80:80 nginx:alpine
```

**NOTE**: The above command binds port 80 of the container to TCP port 80 of the host machine. The default protocol is TCP unless specified otherwise.

8. We can run more nginx containers with different names and its ports published on different host ports that are vacant.

```bash
docker container run -d --name webserver3 -p 8888:80 nginx:alpine
```

9. To list down the running containers use -

```bash
docker container ls
```

10. Open multiple new web browser tab window, and access the different web server instance running on respective host ports.

---

### **Activity 07 - Debugging a container application**

---

1. To view all available options of the **`container logs`** management command use –

```bash
docker container logs --help
```

2. To fetch the container logs use –

```bash
# New Way
# docker container logs [OPTIONS] <CONTAINER>
docker container logs webserver

# Old Way
# docker logs [OPTIONS] <CONTAINER>
docker logs webserver
```

3. The **`-f`** or **`--follow`** option with logs command helps to follow log output.
 
```bash
docker container logs --follow webserver
```

**NOTE**: If we refresh the home page of the **`webserver`** instance, it outputs new log entries.

4. The **`--tail`** option displays the container logs information from the end depending on number passed as an argument. 

```bash
docker container logs --tail 2 webserver
```

**NOTE**: A negative number or a non-integer, when passed as an argument to **`--tail`** option is invalid. The value is set to default **`all`** in that case.

5. To list all the available options of **`container inspect`** command use –

```bash
docker container inspect --help
```

6. To inspect all available states of the container use –

```bash
# New Way
# docker container inspect <CONTAINER> [CONTAINER …]
docker container inspect webserver

# Old Way
# docker inspect <CONTAINER> [CONTAINER …]
docker inspect webserver
```

7. To get more specific details of the container, use the `--format` option. 

```bash
docker container inspect --format "{{.Config.Image}}" webserver
```

8. To return sub-section of the json use the below command -

```bash
docker container inspect --format "{{json .Config}}" webserver
```

9. Try the **`grep`** command use –

```bash
docker container inspect webserver | grep -i Image
```

---

### **Activity 08 - Stopping, Starting, Killing, and Removing container**

---

1. For a graceful shutting down of container use the stop command -

```bash
# New Way
# docker container stop <CONTAINER>
docker container stop webserver

# Old Way
# docker stop <CONTAINER>
docker stop webserver2
```

2. We can also use the **`kill`** command to stop a running container but it will stop the main entry point process abruptly which may lead to a damaged file system.

```bash
# New Way
# docker container kill <CONTAINER>
docker container kill webserver3

# Old Way
# docker kill <CONTAINER>
```

3. For starting a stopped container use the command –

```bash
# New Way
# docker container start <CONTAINER>
docker container start webserver

# Old Way
# docker start <CONTAINER>
docker start webserver2
```

4. To start a container and attach it to the client use the **`-a`** option with start command

```bash
docker container start -a webserver3
```

You can open a new web browser tab and use the IP address along with container port number which is 8888 in our case. That will log the output.

**NOTE**: Use (**`ctrl + c`**) to stop the running container in attach mode.

5. To filter out exited container use **`--filter`** option -

```bash
docker container ls --filter status=exited
```

6. To remove a stopped container without any error using the command

```bash
# New Way
# docker container rm <CONTAINER>
docker container rm webserver3

# Old Way
# docker rm <CONTAINER>
docker rm <CONTAINER>
```

7. To remove a running container use the **`-f`** or **`--force`** option -

```bash
docker container rm --force <CONTAINER_ID|CONTAINER_NAME> 
```

8. To pass the output of one docker command to another docker command use the **`$`** symbol. To remove all the containers running or stopped use -

```bash
docker container rm -f $(docker container ls -aq)
```

9. Use the **`--rm`** option when creating a new container, which will automatically remove the container when it is stopped.

```bash
docker container run --detach --rm --name webserver --publish 80:80 nginx:alpine
```

10. To stop the container created earlier and verify use -

```bash
docker container stop webserver 
# verify after the container is stopped, it is removed because of --rm option used while running a container
docker container ls -a
```

---

### **Activity 09 - Setting environment variables**

---

1. To set an environment variables that would be used by container use the **`--env`** or **`-e`** option. 
   
```bash
docker container run --rm \
--env SOME_VAR=ValueWithoutSpaces \
-it alpine sh
```

2. To set multiple environment variables you can use **`--env`** or **`-e`** option multiple times in command -

```bash
docker container run --rm \
--env SOME_VAR_ONE=ValueOne \
--env SOME_VAR_TWO="Value With Spaces" \
--env SOME_VAR_THREE=ValueThree \
-it alpine sh
```

3. To echo the different variables value set inside container use -

```bash
echo $SOME_VAR_ONE && echo $SOME_VAR_TWO && echo $SOME_VAR_THREE
```

**NOTE**:  The syntax of **`&&`** is used to join multiple commands.

---

### **Activity 10 - Bind Mounting a container**

---

1. Create a **`my-web-app`** directory and then create a simple HTML file with name as **`index.html`**.

```bash
mkdir ~/my-web-app
vim ~/my-web-app/index.html
```

index.html content below -

```html
<html>
    <body>
        <h1>ATCI : Working with Containers!</h1>
    </body>
</html>
```

2. The **`--volume`** or **`-v`** option is used to bind mount the app directory to web server application.

``` bash
--volume <HOST-PATH> : <CONTAINER-PATH> : [<OTHER-OPTIONS>]
```

```bash
docker container run --rm --detach --name webserver --publish 80:80 \
--volume $(pwd)/my-web-app:/usr/share/nginx/html:ro \
nginx:alpine
```

**NOTE**: we have used **`ro`** for *`readonly`* purpose

3. Open a new web browser tab and type the IP address on the address bar to see the content of your **`index.html`** file that gets mounted inside container html directory.

4. Starting from docker **`17.06`** a new option i.e. **`--mount`** is preferred over **`--volume`** option.

```bash
--mount type=(bind|volume|tmpfs),source=HOST-PATH,destination=CONTAINER-PATH,[options]
OR
--mount type=(bind|volume|tmpfs),source=HOST-PATH,target=CONTAINER-PATH,[options]
OR
--mount type=(bind|volume|tmpfs),src=HOST-PATH,dst=CONTAINER-PATH,[options]
```

```bash
docker container run --rm --detach --name anotherwebserver --publish 8888:80 \
--mount type=bind,src=$(pwd)/my-web-app,dst=/usr/share/nginx/html,readonly \
nginx:alpine
```

5. Open a new web browser tab and type the IP address on the address bar, use the port number 8888 to access the modified **`index.html`** page. 

6. Try updating the file in **`my-web-app`** directory and refresh the browser window to see modified changes.

7. To remove all the containers use -

```bash
docker container rm -f $(docker container ls -aq)
```

---

### **Activity 11 - Volume mounting a container**

---

1. To see a host of available sub commands of **`volume`** management command use -

```bash
docker volume --help
```

2. To create a new volume managed by docker use -

```bash
# docker volume create <VOLUME>
docker volume create my-alpine-volume
```

3. To list the volume created use the command –

```bash
docker volume ls
```

4. To inspect the volume use the command -

```bash
# docker volume inspect <VOLUME>
docker volume inspect my-alpine-volume
```

5. To add this volume to a container use the below command –

To mount the volume **`my-alpine-volume`** into **`/custom_directory`** in the container that could be access using the exec command.

```bash
docker container run --name alpine-one -it \
--mount source=my-alpine-volume,target=/custom_directory \
alpine sh
```

**NOTE**: The **`custom_directory`** is created automatically inside the container to mount the volume.

6. To add a new file inside the **`custom_directory`** use touch command - 

```bash
touch custom_directory/sample.txt
```

7. To list the content of **`custom_directory`** use the command below, then exit using (**`ctrl + p + q`**) -

```bash
ls -al custom_directory
```

8. Create another container that would be mounted on the same volume and we can see the same file inside this new container.

```bash
docker container run --name alpine-two -it \
--mount source=my-alpine-volume,target=/my_directory \
alpine sh
```

9. To view the file created in alpine-one container inside **`my_directory`** use -

```bash
ls -al my_directory
```

10. Add a new file from this running container, say **`another_sample.txt`** inside the **`my_directory`**

```bash
touch my_directory/another_sample.txt
```

11. To come out of the container and keep it running use (**`ctrl + p + q`**)

12. Now, exec into the alpine-one container and check the content of the **`custom_directory`**

```bash
docker container exec -it alpine-one sh
ls -al custom_directory
```

13. To remove a docker volume use the below command -

```bash
# docker volume rm <volume-name>
docker volume rm my-alpine-volume
```

**NOTE**: If you try to remove the volume which is already attached to the container it will throw the error.

14.  First remove all the containers attached to the volume we have to remove –

```bash
docker container rm -f $(docker container ls -aq)
docker volume rm my-alpine-volume
```

**NOTE**: If a container is started with a volume that doesn't exist, docker will automatically create that volume.

---

### **Activity 12 - Creating a network and assigning it to an existing container**

---

1. To create two containers communicating with each other use -

```bash
docker container run --detach --rm --name alpine-one alpine sleep 3600
docker container run --detach --rm --name alpine-two alpine sleep 3600
```

2. To check the status of the container running use -

```bash
docker container ls
```

3. To ping from alpine-one container to alpine-two container just by using container name use -

```bash
docker container exec -it alpine-one ping -c 4 alpine-two
```

4. To see the list of available sub-command use with **`network`** management command use -

```bash
docker network --help
```

5. To create a new network using the bridge driver use the below command -

```bash
# docker network create --driver bridge <NETWORK-NAME>
docker network create --driver bridge my-network
```

6. To see the list of available networks use -

```bash
docker network ls
```

7. To connect the existing container to a network use -

```bash
docker network connect my-network alpine-one
docker network connect my-network alpine-two
```

8. To inspect a network use –

```bash
# docker network inspect <NETWORK-NAME>
docker network inspect my-network
```

**NOTE**: Check that both the containers are connected to this network

9. Now, try to ping from alpine-one container to alpine-two container once again -

```bash
docker container exec -it alpine-one ping -c 4 alpine-two
```

10. To disconnect an existing container from a network use **`disconnect`** command -

```bash
docker network disconnect my-network alpine-one
docker network disconnect my-network alpine-two
docker network inspect my-network
```

11. To remove a network use the command -

```bash
# docker network rm <NETWORK-NAME>
docker network rm my-network
```

12. To delete the containers created use -

```bash
docker container rm -f $(docker container ls -aq)
```

---

### **Case Study - Creating a MySQL server and connecting to it using Adminer (a web based UI to communicate with MySQL Server)**

---

In this case study, we need to set up a new mysql server and using a web ui we should be able to execute sql query on the mysql server.

**HINTS**:

1. Look for the official MySQL image on hub.docker.com. Read the description to understand the different environment variables.

2. To store the data generated by mysql create a volume and mount it to container.

3. Look for the official adminer image on hub.docker.com. Read the description to understand the different environment variables. Look for the specific environment variables to set the name of mysql server.

4. Create a separate network to join the `adminer` and `mysql` server for communication.

---

---

### **Activity 13 - Commit Changes in a Running Container**

---

1. To run a nginx webserver inside a container use - 

```bash
docker container run --rm --detach --name webserver -p 80:80 nginx:alpine
```

2. Open a web browser and access the default web page for nginx webserver, using the private IP address

3. To exec into the webserver container use -

```bash
docker container exec -it webserver sh
```

4. Open the **`index.html`** file, which is located in the path given below -

```bash
vi /usr/share/nginx/html/index.html
```

and edit it like

```html
<body>
    <h1>ATCI - Working with Containers Training!</h1>
</body>
```

**NOTE**:

use the key **`I`** to be in insert mode to edit the file

use esc with **`:wq`** to write and quit from vi

5. After changes, to exit out of the shell use the **`exit`** command. Refresh the browser tab to see the modified changes.

6. To list all the options available with **`commit`** command use -

```bash
docker container commit --help
```

7. To commit changes, and create a new image from a container use:

```bash
# New Way
docker container commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

docker container commit webserver mynginx:1.0

# Old Way
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```


8. To view the image that is just created use -

```bash
docker image ls
```

9. Run a new container webservermynginx with the from the **`mynginx:1.0`** image

```bash
docker container run --rm --detach --name webservermynginx -p 8080:80 mynginx:1.0
```

10. Open a web browser and access the default web page for webservermynginx, using the private IP address

11. To exec into the webservermynginx container use -

```bash
docker container exec -it webservermynginx sh
```

12. Open the **`index.html`** file to see, which is located in the path given below -

```bash
cat /usr/share/nginx/html/index.html
```
Can see the below configuration:

```html
<body>
    <h1>ATCI - Working with Containers Training!</h1>
</body>
```

13. To stop the container use -

```bash
docker container stop webserver
docker container stop webservermynginx
```

### **Activity 14 - Build Image Using Dockerfile**

---

1. To view the different options available with the **`build`** command use -

```bash
docker image build --help
```

2. Create a new directory say **`my-alpine-repo`** and add a new **`Dockerfile`** file inside that directory.

```bash
mkdir ~/my-alpine-repo && cd ~/my-alpine-repo
vim ~/my-alpine-repo/Dockerfile
```

3. Add the below content to the Dockerfile

```Dockerfile
FROM alpine
RUN apk upgrade
RUN apk add vim
RUN apk add tree
```

4. To build an image from **`Dockerfile`** use  -

```bash
# New Way
# docker image build [OPTIONS] PATH | URL | -
docker image build -t my-alpine:1.0 .

# Old Way
# docker build [OPTIONS] PATH | URL | -
```

5. To run a container in interactive mode from the recently created image use -

```bash
docker container run --rm -it --name alpine-demo my-alpine:1.0 sh
```

6. Use the **tree** command to view the directory structure of bin –

```bash
tree bin
```

7. To exit out of the container shell use **`exit`** command. 

8. To create a new file **`Dockerfile-2`** from existing **`Dockerfile`** inside **`my-alpine-repo`** directory use - 

```bash
cp ~/my-alpine-repo/Dockerfile ~/my-alpine-repo/Dockerfile-2
```

9. Modify the content of **`Dockerfile-2`** as shown below -

```Dockerfile
FROM alpine
RUN apk upgrade
RUN apk add vim && apk add tree
```

10. To create the image use **`--file`** or **`-f`** option to specify the path of Dockerfile, if it is different than default file name, which is **`Dockerfile`** -

```bash
docker image build -f Dockerfile-2 -t my-alpine:2.0 .
```

**NOTE**: To build this image, the intermediate cache image layer is used from previous image build. Remember, both the images are build in the same host machine.

11. To avoid using cache for re-building an image, use the **`--no-cache`** option -

```bash
docker image build --no-cache -f Dockerfile-2 -t my-alpine:3.0 .
```

**NOTE**: The **`RUN apk upgrade`** step has executed again and no cache image layer was used.

12.  To view list of options available with **`history`** command use -

```bash
docker image history --help
```

13. To view the history of an image use:

```bash
# New Way
# docker image history [OPTIONS] IMAGE
docker image history alpine

# Old Way
# docker history [OPTIONS] IMAGE
```

14. Observe, some similar image layer history for different images as it was build using the cache image layer. 

```bash
docker image history my-alpine:1.0
docker image history my-alpine:2.0
docker image history my-alpine:3.0
```

**NOTE**: When a image is pushed to the registry only the leaf image i.e. the last layer is uploaded along with its constituent layer that makes up an entire image. When this image is made available and pulled again by another docker host the components that supports build cache are no longer required as the image is effectively read-only and hence the `<missing>` for image id.

---

### **Activity 15 - Using CMD**

---

1. Create a new file say **`Dockerfile-3`** from the existing **`Dockerfile-2`** 

```bash
cp ~/my-alpine-repo/Dockerfile-2 ~/my-alpine-repo/Dockerfile-3
```

2. Modify the content of **`Dockerfile-3`** as shown below -

```Dockerfile
FROM alpine
RUN apk upgrade
RUN apk add vim && apk add tree
CMD ping -c 4 localhost
```

**NOTE**: The CMD instruction has three forms:

1. CMD ["executable","param1","param2"] (exec form, this is the preferred form)
   
2. CMD ["param1","param2"] (as default parameters to ENTRYPOINT)

3. CMD command param1 param2 (shell form)

**NOTE**: There can only be one **`CMD`** instruction in a **`Dockerfile`**. If you list more than one **`CMD`**, then only the last CMD will take effect.

3. To build the image using **`Dockerfile-3`** use -

```bash
docker image build -f Dockerfile-3 -t my-alpine:cmd-shell-form .
```

4. To run a new container from **`my-alpine:cmd-shell-form`** image that exist locally use -

```bash
docker container run --rm --name ping-container my-alpine:cmd-shell-form
```

5. If any command is specified after the image name, it will override the `CMD` specified inside the image.

```bash
docker container run --rm --name ping-container my-alpine:cmd-shell-form echo "Docker is AWESOME"
```

6. Open the **`Dockerfile-3`** and change the CMD to use the exec form as shown below

```Dockerfile
FROM alpine
RUN apk upgrade
RUN apk add vim && apk add tree
CMD ["ping", "-c", "4", "localhost"]
```

**NOTE**: Use double quotes to put the executable and parameters

7. To create the image again use -

```bash
docker image build -f Dockerfile-3 -t alpine:cmd-exec-form .
```

8. To run a new container from **`my-alpine:cmd-exec-form`** image that exist locally use -

```bash
docker container run --rm --name ping-container alpine:cmd-exec-form
```

---

### **Activity 16 - Using ENTRYPOINT**

---

1. Create a new file say **`Dockerfile-4`** from the existing **`Dockerfile-3`** 

```bash
cp ~/my-alpine-repo/Dockerfile-3 ~/my-alpine-repo/Dockerfile-4
```

2. Modify the content of **`Dockerfile-4`** as shown below -

```Dockerfile
FROM alpine
RUN apk upgrade
RUN apk add vim && apk add tree
ENTRYPOINT ping -c 4 localhost
```

**NOTE**: ENTRYPOINT has two forms:

ENTRYPOINT command param1 param2 (shell form)

ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)

3. To build the image using **`Dockerfile-4`** use -

```bash
docker image build -f Dockerfile-4 -t my-alpine:ep-shell-form .
```

4. To run a new executable container from **`my-alpine:ep-shell-form`** image that exist locally use -

```bash
docker container run --rm --name ping-container my-alpine:ep-shell-form
```

5. Open the **`Dockerfile-4`** and change the CMD to use the exec form as shown below

```Dockerfile
FROM alpine
RUN apk upgrade
RUN apk add vim && apk add tree
ENTRYPOINT ["ping", "-c", "4", "localhost"]
```

6. To create the image again use -

```bash
docker image build -f Dockerfile-4 -t alpine:ep-exec-form .
```

7. To run a new container from **`my-alpine:ep-exec-form`** image that exist locally use -

```bash
docker container run --rm --name ping-container alpine:ep-exec-form
```

**NOTE**: If you have created an image using **`ENTRYPOINT`**, then any command passed to the container after the image name, will be considered as a parameter for the executable. Hence, we will get an error. 

---

### **Activity 17 - Using CMD and ENTRYPOINT together**

---

1. Create a new file say **`Dockerfile-5`** from the existing **`Dockerfile-4`** 

```bash
cp ~/my-alpine-repo/Dockerfile-4 ~/my-alpine-repo/Dockerfile-5
```

2. Modify the content of **`Dockerfile-5`** as shown below -

```Dockerfile
FROM alpine
RUN apk upgrade
RUN apk add vim && apk add tree
ENTRYPOINT ["ping", "localhost", "-c"]
CMD ["4"]
```

**NOTE**: **`ENTRYPOINT`** allows to configure a container to run as an executable with ping command, and then pass the number of pings as parameter through **`CMD`**.

3. To build the image using **`Dockerfile-5`** use -

```bash
docker image build -f Dockerfile-5 -t my-alpine:cmd-ep-exec-form .
```

4. To run a new executable container from **`my-alpine:cmd-ep-exec-form`** image that exist locally use -

```bash
docker container run --rm --name ping-container my-alpine:cmd-ep-exec-form
```

5. To run same container with more number of pings, pass additional parameter as integer value which in turn will replace the default CMD value of 4 as expected -

```bash
docker container run --rm --name ping-container my-alpine:cmd-ep-exec-form 10
```

[REFERENCE LINK - HOW CMD AND ENTRYPOINT INTERACT](https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact)

---

### **Activity 18 - Using WORKDIR and COPY**

---

1. Create a new directory say **`my-java-repo`** with an **`app`** directory inside it.

```bash
mkdir -p ~/my-java-repo/app && cd ~/my-java-repo 
```

2. Create a new **`Greeting.java`** file, and add the content given below - 

```bash
vim ~/my-java-repo/app/Greeting.java
```

```java
public class Greeting {
    public static void main(String [] args) {
        System.out.println("Welcome from Containerized Java App!");
    }
}
```

3. Create a new **`Dockerfile`** file inside **`my-java-repo`**, and add the content given below - 

```
vim ~/my-java-repo/Dockerfile
```

```Dockerfile
FROM openjdk:8u181-jdk-slim
WORKDIR java-apps
COPY app/Greeting.java ./
RUN javac Greeting.java && rm -rf Greeting.java
ENTRYPOINT ["java", "Greeting"]
```

4. To build an image from this **`Dockerfile`** use -

```bash
docker image build -t my-java-app:1.0 .
```

5. To run a container using **`my-java-app:1.0`** image, which exists locally now, use -

```bash
docker container run --rm --name java-app my-java-app:1.0
```

**NOTE**: The message is printed on the console and container terminates.

---

### **Activity 19 - Using Multi-Stage Build**

---

1. Create a new file say **`Dockerfile-2`** from the existing **`Dockerfile`**

```bash
cp ~/my-java-repo/Dockerfile ~/my-java-repo/Dockerfile-2 
```

2. Modify the content of **`Dockerfile-2`** as given below -

```bash
# STAGE 1 - To build the Java Application
FROM openjdk:8u181-jdk-slim AS build_container
WORKDIR java-apps
COPY app/Greeting.java ./
RUN javac Greeting.java && rm -rf Greeting.java

# STAGE 2 - To package the Java Application
FROM openjdk:8u181-jre-slim
WORKDIR app
COPY --from=build_container java-apps/ ./
ENTRYPOINT ["java", "Greeting"]
```

3. To build an image from this **`Dockerfile-2`** use -

```bash
docker image build -f Dockerfile-2 -t my-java-app:2.0 .
```

4. To run a container from the image **`my-java-app:2.0`**, which exists locally now, use -

```bash
docker container run --rm --name java-app my-java-app:2.0
```

5. To check the size of different version of my-java-app images use -

```bash
docker image ls | grep -i my-java-app
```

[REFERENCE LINK - Dockerfile](https://docs.docker.com/engine/reference/builder/)

---

### **Activity 20 - Pushing an Image to Public Registry**

---

1. To install git in this node instance use -

```bash
# for ubuntu machine use
sudo apt-get install -y git

# for centos machine use
sudo yum install -y git
```

2. Once git is installed, then clone the repository.

```bash
git clone https://innersource.accenture.com/scm/~sujal.kumar.mitra/working-with-containers-lab-files.git
```

3. Move to the **`lab-files/python-flask-app`** directory inside **`working-with-containers-lab-files`**. 

```bash
cd working-with-containers-lab-files/lab-files/python-flask-app/
```

4. Refer the files in **`python-flask-app`** directory.

```bash
ls -al
```

5. To open the **`Dockerfile`** and modify its configuration use -

```bash
vim Dockerfile
```

6. To build a new docker image use -

```bash
# Don't forget to use the . at the end
docker image build -t python-flask-app:1.0 .
```

7. To see the docker image created just now use -

```cmd
docker image ls
```

8. To run a new container from the docker image created use -

```cmd
docker container run --detach --rm --name py-web-app-1 --publish 8888:80 python-flask-app:1.0
```

9. Open a new browser tab window and use the URL given below to access the application.

```txt
http://<HOST-IP-ADDRESS>:8888
```

10. To stop the running container use -

```cmd
docker container stop py-web-app-1
```

11.  To log in to your account at the docker hub registry use - 

```cmd
docker login
```

**NOTE**: Use your docker hub id and password.

12.  To push the image first `tag` it with repository name (i.e. docker hub id).

```cmd
docker image tag python-flask-app:1.0 <docker-hub-id>/python-flask-app:1.0
docker image tag python-flask-app:1.0 <docker-hub-id>/python-flask-app:latest
```

**NOTE**: The image that is pushed or pulled has the format **`domain/repository/image:tag`**. If not specified, the domain defaults to **`docker.io`**, the repository defaults to **`library`**, and the tag defaults to **`latest`**. 

13. To view all the repository images and its tags available locally use -

```cmd
docker image ls | grep -i python
```

14.  To push the image to the docker hub registry use -

```cmd
docker push <docker-hub-id>/python-flask-app:1.0
docker push <docker-hub-id>/python-flask-app:latest
```

15. Once the image is pushed, you can view the image in your docker hub registered account.

16. To remove the python-flask-app images from the host machine use -

```cmd
docker image rm python-flask-app:1.0 <docker-hub-id>/python-flask-app:latest <docker-hub-id>/python-flask-app:1.0
```

17.  To use the pushed image from docker hub registry use -

```cmd
docker container run --detach --rm --name py-web-app-1 --publish 8888:80 <docker-hub-id>/python-flask-app
```

18. To view the application open a new browser tab window, and use -

```txt
http://<HOST-IP-ADDRESS>:8888
```

19. To stop the running container use -

```cmd
docker container stop py-web-app-1
```

---


### **Activity 21 - Using Docker Compose**

---

1. As we have installed git already in Activity 20, So, clone the repository.

```bash
git clone https://innersource.accenture.com/scm/~sujal.kumar.mitra/working-with-containers-lab-files.git
```

2. Move to the **`lab-files/python-flask-app`** directory inside **`working-with-containers-lab-files`**. 

```bash
cd working-with-containers-lab-files/lab-files/python-flask-app/
```

3. Refer the files in **`python-flask-app`** directory.

```bash
ls -al
```

4. This project directory contains a **`docker-compose.yml`** file which is complete in itself for a good starter wordpress project.

5. In that **`docker-compose.yml`** file that starts your **`frontend`** python application:

6. The **`Dockerfile`** is mapped in **`docker-compose.yml`** file.

7. Now, run **`docker compose up -d`** from your project directory.

8. This runs **`docker compose up`** in detached mode, creates a Docker image **`python-frontend`**.

10. At this point, the container **` python-frontend-container`** should be running on port **`8080`** of your Docker Host.

11. you can use open **`http://localhost:8080`** in a web browser.

12. Shutdown and cleanup

      The command **`docker compose down`** removes the containers and default network, but preserves your WordPress  database.

      The command **`docker compose down --volumes`** removes the containers, default network, and the WordPress database.

---      

**END OF ACTIVITY**

---
