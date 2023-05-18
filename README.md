# Dockers 

1. Docker is an open-source platform that allows developers to automate the deployment and management of applications within lightweight, portable containers

2.   Containers are isolated environments that package an application and its dependencies, including the runtime, libraries, and system tools, into a single unit

3. This enables applications to run consistently across different environments, such as development, testing, and production, without being affected by differences in underlying infrastructure.

4. Docker containers are lightweight, start quickly, and consume fewer resources compared to traditional virtual machines.

## Pre-Requisites

1. Basic knowledge of Linux and development, and basics of SSH concepts
2. Hardware requirements - 
* Laptop/Desktop with a minimum of 8GB RAM, i5 + processors
* OS : Windows / Linux
* Open Internet Connectivity
* Putty or SSH on participant systems
3. Docker Desktop. Can be installed from `https://docs.docker.com/engine/install/`

## Fundamental Concepts 

Let us look at some of the key components in dockers first- 

1. Docker image: A Docker image is a read-only template that contains everything needed to run an application, including the code, runtime, system tools, libraries, and dependencies. Images are built from a set of instructions called Dockerfiles.

2. Docker container: A Docker container is a runnable instance of a Docker image. It is an isolated environment that encapsulates the application and its dependencies, making it portable and consistent across different environments.

3. Dockerfile: A Dockerfile is a text file that contains instructions to build a Docker image. It specifies the base image, configuration, dependencies, and commands required to create the image.

4. Docker Hub: Docker Hub is a public registry where you can find pre-built Docker images shared by the Docker community. It allows developers to share, distribute, and pull Docker images easily.

5. Docker Compose: Docker Compose is a tool that allows you to define and manage multi-container Docker applications. It uses a YAML file to define the services, networks, and volumes required for your application, making it easier to set up and manage complex application architectures.

### Architectures

Monolithic and microservice architectures are two different approaches to structuring and deploying applications. While Docker is a technology that can be used with both approaches, it is often associated with microservices due to its benefits in managing and deploying containerized services. 

1. Monolithic Approach:
In a monolithic architecture, an application is built as a single, self-contained unit. All components of the application, such as the user interface, business logic, and data acess layer, are tightly coupled and run within a single process or executable. The entire application is deployed as a single unit, and scaling or modifying individual components independently can be challenging.



2. Microservice Approach:
In a microservice architecture, an application is divided into a collection of small, loosely coupled services. Each service represents a specific functionality or business capability and runs independently in its own container. These services communicate with each other through well-defined APIs, usually using lightweight protocols like HTTP or message queues.


![Arch image](https://www.openlegacy.com/hubfs/Picture1.webp)

### Containers 

Containers in Docker are lightweight, isolated environments that package an application and its dependencies, including the runtime, libraries, and system tools, into a single unit. They provide a consistent and reproducible environment for applications to run, regardless of the underlying infrastructure. Containers allow for efficient resource utilization, fast startup times, and easy portability, making them a popular choice for application deployment and management.

In the following slides, you will learn more about how to create and use containers 

#### Pull an image 

Containers are based on Docker images, which serve as the starting point for creating containers. You can either pull an existing image from a Docker registry like Docker Hub or build your own custom image using a Dockerfile.

To pull an image, you can use the `docker pull` command, specifying the image name and tag.

Steps: 

1. Launch a terminal or command prompt on your machine.
2. Ensure that Docker is installed and running by typing `docker version` in the terminal. This command should display the Docker version information if Docker is properly installed.
3. Pull the image: To pull an image, use the docker pull command followed by the image name and, optionally, the tag. The general syntax is as follows: <br>
`docker pull <image name>:<tag>`<br>
For example to pull the latest version of the official Nginx web server image, the code is <br>
`docker pull nginx:latest`

Docker will initiate the image download process from the default registry, which is Docker Hub.

4. Monitor the pull process: Docker will display the progress as it pulls the image. It will indicate the download status and the layers being retrieved. The time taken for the pull will depend on the size of the image and your network speed.

5. Verify the pulled image: Once the pull process is complete, you can verify that the image is available locally by using the <br> `docker images` command. It will display a list of all the images available on your machine, including the pulled image.

#### Building an image 

To build a Docker image, you need to create a Dockerfile that defines the instructions and configurations for building the image.

1. Create a Dockerfile: Start by creating a text file named "Dockerfile" (no file extension) in a directory of your choice. This file will contain the instructions for building the Docker image. A Dockerfile typically consists of a series of commands and directives.

2. Specify the base image: In the Dockerfile, specify the base image upon which your image will be built. The base image provides the starting point for your image and usually includes an operating system and pre-installed dependencies. You can specify the base image using the `FROM` directive. For example, `FROM ubuntu:latest` sets the base image as the latest version of Ubuntu.

3. Add files and dependencies: Use the `COPY` or `ADD` directives in the Dockerfile to include files, scripts, or dependencies from your local machine into the image. These files will be available within the container when it is running. For example,  <br>`COPY app.py /app` copies the file app.py from the local directory to the /app directory within the image.

4. Run commands: Use the RUN directive to execute commands within the image during the build process. These commands can be used to install packages, set up configurations, or perform other tasks required for your application. For example,<br>` RUN apt-get update && apt-get install -y python3`  installs Python 3 within the image.

5. Expose ports (optional): If your application requires network communication, you can use the `EXPOSE` directive to specify which ports should be exposed from the container. For example, `EXPOSE 8080` exposes port 8080 from the container.

6. Define the default command: Use the `CMD` or `ENTRYPOINT` directive to specify the command that should be executed when a container is created from the image. This command defines the primary process running within the container. For example, <br>`CMD ["python3", "app.py"]` sets the default command to run the app.py file using Python 3.

7. Build the image: Once the Dockerfile is created, navigate to the directory containing the Dockerfile in your terminal or command prompt. Then use the `docker build` command followed by the desired options and the path to the directory. For example, `docker build -t myimage:latest`<br> builds the image with the tag "myimage" and the latest version.

8. Use the built image: After the build process completes successfully, you can use the built image to create containers. You can run containers using the `docker run` command, specifying the image name and any additional options or configurations.

#### Docker BUILD cheatsheat 

|Command	                                                          |                 Explanation                     |
| ---------------------------------                                         |          ---------------------------------                   
|`docker build`	                                                      |  Builds an image from a Dockerfile located in the current -directory|                                                             
|`docker build https://github.com/docker/rootfs.git#container:docker`  |Builds an image from a remote GIT repository|
|`docker build -t imagename/tag`	                                   |    Builds and tags an image for easier tracking|
|`docker build https://yourserver/file.tar.gz`	                       |Builds an image from a remote tar archive|
|`docker build -t image:1.0 -<<EOFFROM busyboxRUN echo "hello world"EOF`|	Builds an image via a Dockerfile that is passed through STDIN|

#### Create a container 

Once you have the desired image, you can create a container from it using the `docker create` or `docker run` command. The docker create command creates a container without starting it, while docker run creates and starts a container. Specify the image name, desired container name (optional), and any additional configuration options. For example, <br> `docker run --name mycontainer nginx:latest` will create and start a container named "mycontainer" based on the Nginx image.

You will find a list of container interactions listed in the following slide

#### Container interactions cheat sheet

|Command	|Explanation|
|-----------|-------------|
|docker start container |	Starts a new container|
|docker stop container	| Stops a container|
|docker pause container	|Pauses a container|
|docker unpause container|	Unpauses a container|
|docker restart container|	Restarts a container|
|docker wait container	|Blocks a container|
|docker export container|	Exports container contents to a tar archive|
|docker attach container|	Attaches to a running container|
|docker wait container	|Waits until the container is terminated and shows the exit code|
|docker commit -m “commit message” -a “author” container username/image_name: tag	|Saves a running container as an image|
|docker logs -ft container	|Follows container logs|
|docker exec -ti container script.sh	|Runs a command in a container|
|docker commit container image	|Creates a new image from a container|
|docker create image	|Creates a new container from an image|


#### Container Inspection Commands 

Sometimes, you need to inspect your containers for quality assurance or troubleshooting purposes. These commands help you get an overview of what different containers are doing: 

|Command |	Explanation|
|---------|-------------|
|docker ps	|Lists all running containers|
|docker -ps -a	|Lists all containers|
|docker diff container	|Inspects changes to directories and files in the container filesystem|
|docker top container	|Shows all running processes in an existing container|
|docker inspect container |	Displays low-level information about a container|
|docker logs container	|Gathers the logs for a container|
|docker stats container	|Shows container resource usage statistics|


#### Containers vs VMs 

Now that we have a basic idea about Dockers, Images and Containers, let us look at what the main difference between containers and vms are. 

![Differences](https://raw.githubusercontent.com/collabnix/dockerlabs/master/beginners/docker/images/vm-docker4.png)

