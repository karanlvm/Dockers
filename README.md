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


### Runtime Environments & Engines

1. Runtime Environments: A runtime environment is the underlying software infrastructure required to execute an application. It includes the operating system, system libraries, frameworks, and other dependencies necessary for the application to run. In the context of containers, a runtime environment refers to the environment provided by the container engine, which isolates and manages the execution of containers on the host system.


2. Container Engines: Container engines, also known as container runtimes, are software tools responsible for managing and executing containers. They provide the necessary infrastructure to create, start, stop, and manage containers on the host system. Container engines interact with the host operating system's kernel and utilize features such as cgroups and namespaces to achieve isolation and resource management for containers. Examples of popular container engines include Docker Engine (the original Docker runtime), containerd, and CRI-O.

Container engines offer APIs and command-line interfaces that allow users to interact with containers, manage container images, and configure container networking and storage. They provide mechanisms for container lifecycle management, such as creating, starting, stopping, and removing containers. Container engines also handle resource allocation, security isolation, and networking for containers.

These container engines work in conjunction with container orchestration platforms, such as Docker Swarm, Kubernetes, and Nomad, which provide advanced features for deploying, scaling, and managing containerized applications across clusters of machines.

### Docker Ecosystem 

![ecosys](https://res.cloudinary.com/practicaldev/image/fetch/s--Jne8jX-C--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/i/aojpt8fs65jv79iqg4g2.png)

1. Docker Engine: This is the core component of Docker, responsible for creating and managing containers. It provides the runtime environment to run and isolate applications within containers.

2. Docker Compose: It is a tool that allows defining and running multi-container Docker applications. Docker Compose uses a YAML file to specify the services, networks, and volumes required for the application, making it easier to manage complex multi-container setups.

3. Docker Swarm: Docker Swarm is a native clustering and orchestration solution provided by Docker. It allows you to create and manage a swarm of Docker nodes, forming a distributed cluster where containers can be scheduled and scaled across multiple hosts.

4. Docker Registry: The Docker Registry is a repository for storing and distributing Docker images. It can be a public registry like Docker Hub or a private registry hosted within an organization. Developers can push and pull Docker images to and from the registry.

5. Docker Hub: Docker Hub is a cloud-based public registry that hosts a vast collection of Docker images. It allows developers to discover, share, and collaborate on containerized applications and libraries.

6. Dockerfile: A Dockerfile is a text file that contains a set of instructions for building a Docker image. It specifies the base image, dependencies, environment variables, and commands needed to create a container image.

7. Docker CLI: The Docker command-line interface (CLI) is a command-line tool used to interact with the Docker daemon and manage containers, images, networks, and other Docker resources.

### Docker Components 

![components](https://cdn-images-1.medium.com/max/1600/1*bIQOZL_pZujjrfaaYlQ_gQ.png)

1. Docker Engine: The Docker Engine is the core runtime component of Docker. It is responsible for building, running, and managing Docker containers. It includes a server process called dockerd that runs on the host machine and a command-line interface (CLI) tool called docker that allows users to interact with the Docker Engine.

2. Docker Images: Docker images are the building blocks of containers. An image is a lightweight, standalone, and executable software package that includes everything needed to run an application, including the code, runtime, libraries, and dependencies. Docker images are created from a set of instructions called a Dockerfile, which specifies the steps to build the image.

3. Docker Containers: Containers are the instances of Docker images. They are isolated and lightweight, providing a consistent and reproducible environment for running applications. Containers can be started, stopped, and deleted, allowing for easy application deployment, scaling, and management. Multiple containers can run simultaneously on the same host, each with its own isolated environment.

4. Docker Registry: The Docker Registry is a centralized repository for storing and distributing Docker images. It can be either a public registry, such as Docker Hub, or a private registry hosted within an organization. Docker images can be pushed to a registry to make them accessible to others or pulled from a registry to run them on different hosts.

5. Docker Compose: Docker Compose is a tool for defining and managing multi-container applications. It uses a YAML file to define the services, networks, and volumes required by an application. With Docker Compose, you can start and stop all the services in your application with a single command, making it easier to manage complex applications composed of multiple containers.

6. Docker Swarm: Docker Swarm is Docker's native clustering and orchestration solution for managing a cluster of Docker nodes. It allows you to create a swarm of Docker nodes and deploy services across the swarm. Swarm provides features such as load balancing, service scaling, rolling updates, and fault tolerance, making it easier to manage containerized applications at scale.

### Engine Components 

Docker Engine is the core runtime component of Docker that builds, runs, and manages Docker containers. It consists of several components that work together to facilitate containerization. Here are the main Docker Engine components and their roles:

1. Docker Daemon (dockerd): The Docker Daemon, or dockerd, is a background service that runs on the host machine. It is responsible for managing Docker objects such as containers, images, networks, and volumes. The Docker Daemon listens for Docker API requests and executes them to perform various operations like starting or stopping containers, pulling or pushing images, and managing the overall container lifecycle.

2. Docker Client (docker): The Docker Client is a command-line interface (CLI) tool that allows users to interact with the Docker Daemon. It sends commands and API requests to the Docker Daemon and acts as the primary interface for users to control and manage Docker containers and images. The Docker Client communicates with the Docker Daemon through the Docker API.

3. Docker API: The Docker API (Application Programming Interface) is a RESTful interface that exposes a set of endpoints for interacting with the Docker Daemon. It defines a standardized way to communicate and control Docker resources programmatically. The Docker API allows developers and third-party tools to integrate with Docker and perform various operations like managing containers, images, networks, and volumes.

4. Containerd: Containerd is a high-level container runtime that provides an interface for managing the lifecycle of containers. It is responsible for container execution, image management, and low-level operations like starting, stopping, and deleting containers. Docker Engine uses Containerd as its underlying container runtime, enabling features such as container isolation, resource management, and namespace separation.

### Dataflow 

The dataflow within Docker Engine involves the following steps:

1. Docker Client sends a command or API request to the Docker Daemon using the Docker Client (docker) CLI tool or any other client tool that interacts with the Docker API.

2. The Docker Daemon receives the request and processes it. It performs actions based on the request, such as pulling an image, starting a container, or creating a network.

3. If the request involves working with an image, the Docker Daemon checks if the required image is available locally. If not, it pulls the image from a Docker Registry, such as Docker Hub, and stores it locally in the image cache.

4. Once the necessary image is available, the Docker Daemon creates a container based on the image. It sets up the container's file system, networking, and resource allocation based on the configuration specified in the request.

5. The Docker Daemon starts the container and executes the commands specified in the container's entry point or command. The container runs in an isolated environment with its own filesystem, network namespace, and process namespace.

6. During the container runtime, the Docker Daemon manages the container's lifecycle, monitors its resource usage, and enforces any defined constraints or configurations.

7. If needed, the Docker Client can send additional requests to the Docker Daemon to interact with the running container, such as attaching to its console, inspecting its logs, or modifying its configuration.


This dataflow between the Docker Client and the Docker Daemon allows users to control and manage Docker containers and images, enabling the seamless deployment and execution of applications within isolated and portable containers.


## Intermediate Concepts

### Container Network

Working with container networks involves managing the networking aspects of Docker containers, enabling communication between containers and with external networks. Docker provides several networking options to facilitate connectivity and isolation between containers. Here are the key aspects of working with container networks:

1. Default Bridge Network: When you install Docker, a default bridge network named "bridge" is created automatically. Containers attached to this network can communicate with each other using IP addresses. Docker assigns an IP address to each container on this network, and containers can connect to each other using these IP addresses or container names. However, containers on the default bridge network are isolated from external networks unless port forwarding or network address translation (NAT) is configured.

2. User-Defined Bridge Networks: Docker allows you to create user-defined bridge networks to facilitate communication between containers. Unlike the default bridge network, user-defined networks provide better isolation and control over container connectivity. When you create a user-defined bridge network, Docker automatically assigns a subnet and gateway to it. Containers can be connected to the user-defined network by specifying the network during container creation. Containers on the same user-defined network can communicate with each other using their container names as DNS aliases.

3. Host Network: By using the host network mode, a container shares the host's network stack instead of being isolated in a separate network namespace. This mode gives the container direct access to the host's network interfaces and allows it to use the host's IP address, network ports, and network services. However, this eliminates the network isolation provided by Docker, and containers running in the host network mode can potentially interfere with or collide with the host's network services.

4. Overlay Network: Docker supports overlay networks for connecting containers across multiple Docker hosts in a swarm mode cluster. Overlay networks use the Virtual Extensible LAN (VXLAN) technology to encapsulate network traffic and enable communication between containers running on different hosts. Overlay networks provide a scalable and resilient networking solution for containerized applications distributed across multiple hosts.

5. Network Drivers: Docker allows you to choose different network drivers based on your requirements. The default bridge network and user-defined bridge networks use the bridge driver. Other network drivers include host, overlay, macvlan, and third-party drivers. Each driver provides specific functionalities and capabilities for container networking, such as host network integration, direct host access, or advanced networking features.

6. Network Configuration: When creating or running a container, you can specify the network configuration using Docker CLI or container orchestration tools like Docker Compose or Kubernetes. This includes attaching the container to specific networks, exposing container ports to the host or other containers, setting up port mappings, defining DNS names, and configuring network-related environment variables.


The docker network cheatsheet is attached in the next slide for your reference 


### Network cheatsheet 

|Command  |Explanation|
|---------|------------|
|docker network create networkname	| Creates a new network|
|docker network rm networkname	| Removes a specified network|
|docker network ls	| Lists all networks|
|docker network connect networkname container	|Connects a container to a network|
|docker network disconnect networkname container	|Disconnects a container from a network|
|docker network inspect networkname	| Displays detailed information about a network|

### Docker Volumes 

Docker volumes are a feature that allows you to persist and manage data generated by containers. They provide a way to store and share data between containers, as well as between the host machine and containers. Here's how you can use Docker volumes with commands:

1. Create a Docker Volume: <br>
To create a Docker volume, you can use the `docker volume create` command followed by the desired volume name. For example: <br>
`docker volume create myvolume`

2. List Docker Volumes:
You can list all the Docker volumes on your system using the `docker volume ls` command

3. Inspect Docker Volume:
To get detailed information about a specific Docker volume, use the `docker volume inspect` command followed by the volume name:<br>
`docker volume inspect myvolume`

4. Mount a Docker Volume to a Container:
To use a Docker volume with a container, you can specify it during the container creation using the `-v` or `--mount` flag. The syntax is `source:destination`, where the source is the name of the volume and the destination is the path inside the container where the volume will be mounted. For example: <br>
`docker run -v myvolume:/app/data myimage`

5. Mount a Host Directory as a Docker Volume:
Instead of creating a Docker volume, you can also mount a directory from the host machine as a volume. Use the same syntax as above, but specify the host directory path as the source. For example:<br>
`docker run -v /path/on/host:/app/data myimage`

6. Remove a Docker Volume:
To remove a Docker volume, use the `docker volume rm` command followed by the volume name:
`docker volume rm myvolume`

These commands allow you to create, manage, and remove Docker volumes. By using volumes, you can separate and persist data outside of containers, making it easier to manage and share data between containers or with the host machine.


### Persistent data in Docker Volumes

Persistent data in Docker volumes refers to the ability to store and retain data generated by containers even when the containers are stopped or removed. By using Docker volumes, you can separate the data from the container's file system and ensure its persistence beyond the container's lifecycle.

When a Docker volume is used to store data generated by a container, that data remains intact even if the container is removed or replaced. This is advantageous in scenarios where you want to preserve important or valuable data, such as databases, configuration files, logs, or user uploads.

Persistent data in Docker volumes provides several benefits:

1. Data Separation: Storing data in volumes separates it from the container's filesystem, making it easier to manage, backup, and restore.

2. Data Persistence: Volumes ensure that the data remains available and persistent even when containers are stopped, restarted, or removed. This allows for seamless container updates or replacements without losing valuable data.

3. Data Sharing: Volumes enable data sharing between multiple containers. Several containers can mount the same volume, allowing them to access and modify shared data.

4. Data Portability: Docker volumes offer portability, as they can be easily moved between different host machines or deployed in different environments. This makes it simpler to migrate containers and their associated data.

To achieve persistent data storage using Docker volumes, you need to create a volume and mount it to the desired containers using the `-v` or `--mount` flag during container creation. This allows the containers to read from and write to the volume, ensuring data persistence.

By leveraging persistent data in Docker volumes, you can maintain important information across container lifecycles, facilitate data sharing between containers, and ensure the durability and availability of critical data in your Dockerized applications.


### Port mapping for containers

Port mapping for containers, also known as port forwarding, allows you to expose container ports to the host machine or to other containers, enabling communication with services running inside the containers. It allows external systems to access services running inside the container by mapping a container port to a port on the host machine.

To perform port mapping for containers in Docker, you can use the `-p` or `--publish` flag with the `docker run` command. Here's how you can do it:

1. Map Container Port to Host Port:
Use the `-p` flag to map a container port to a specific port on the host machine. The syntax is `<host-port>:<container-port>`. For example, to map port 8080 of the container to port 8888 on the host: <br>
`docker run -p 8888:8080 myimage` <br>
This command binds the container's port 8080 to the host's port 8888. You can then access the service running in the container by accessing localhost:8888 on the host machine.

2. Map Container Port to a Random Host Port:
If you don't specify a specific host port, Docker will automatically assign a random port on the host machine. Use the `-P` flag to enable this behavior. For example:<br>
`docker run -P myimage`<br>
Docker will assign a random available port on the host and map it to the container's exposed port(s). You can use the docker port command to check the assigned host port.

3. Map Container Port to Another Container:
You can also map container ports to other containers within the same Docker network. Specify the `<container-name>:<container-port>`as the mapping. For example:<br>
`docker run --network mynetwork --name container1 -p 8888:8080 myimage1`
`docker run --network mynetwork --name container2 -p 9999:8080 myimage2`
<br> Here, container1 exposes its port 8080, which is accessible as container1:8080 within the mynetwork network. container2 can access container1's service by using container1:8080 as the hostname and port combination.

Port mapping in Docker allows you to make container services accessible from outside the container, enabling communication with containerized applications. It's a crucial feature when deploying containers in production environments or when services need to interact with each other within a network.

### Dockerfile	Directives

Dockerfile directives are instructions used to build Docker images. They define the steps and configurations required to create a Docker image from a base image. Here are some common Dockerfile directives and their purposes:

1. FROM:
The FROM directive specifies the base image to use as the starting point for the Docker image. It is usually the first instruction in a Dockerfile.<br>
Example: `FROM ubuntu:latest`

2. MAINTAINER (deprecated):
The MAINTAINER directive specifies the name and email address of the image maintainer. However, it is now considered deprecated, and the preferred alternative is to use the `LABEL `directive.<br>
Example: `MAINTAINER John Doe <johndoe@example.com>`

3. LABEL:
The LABEL directive adds metadata to the Docker image, typically in the form of key-value pairs. It is used to provide information about the image, such as version, description, or author.<br>
Example: `LABEL version="1.0" description="My Docker image"`

4. RUN:
The RUN directive executes a command inside the Docker image during the image build process. It is commonly used for installing dependencies, configuring the image, or running any necessary setup commands.<br>
Example: `RUN apt-get update && apt-get install -y python3`

5. COPY:
The COPY directive copies files and directories from the host machine to the Docker image. It is used to add files, dependencies, or configuration files to the image.<br>
Example: `COPY app.py /app/`

6. ADD:
The ADD directive is similar to COPY but has additional functionality. It copies files and directories from the host machine to the image, and it can also extract compressed files or download files from URLs and add them to the image.<br>
Example: `ADD https://example.com/file.tar.gz /app/`

7. WORKDIR:
The WORKDIR directive sets the working directory for subsequent instructions in the Dockerfile. It is used to specify the directory where commands will be executed inside the image.<br>
Example: `WORKDIR /app`

8. EXPOSE:
The EXPOSE directive informs Docker that the container listens on specific network ports at runtime. It does not actually publish the ports; it is used as documentation for users of the image. <br>
Example: `EXPOSE 8080`

9. CMD:
The CMD directive specifies the default command to run when a container is started from the image. It can be overridden by providing a command when running the container. <br>
Example: `CMD ["python", "app.py"]`

10. ENTRYPOINT:
The ENTRYPOINT directive specifies the executable that will be run when a container is started from the image. It provides the primary command for the container and cannot be overridden. <br>
Example: `ENTRYPOINT ["python", "app.py"]`

These are just a few of the commonly used Dockerfile directives. By using these directives and others available, you can define the steps, configurations, and commands required to build a Docker image tailored to your application or service.

###  Registry and Repositories

In Docker, a registry is a centralized storage and distribution system for Docker images. It serves as a repository for Docker images, allowing users to upload, download, and share container images. Docker Hub is the default public registry provided by Docker, but you can also set up private registries or use third-party registries.

Repositories, on the other hand, are collections of related Docker images that share the same name but can have different tags. Each image in a repository represents a different version or variant of the software or application it contains. Repositories can be hosted on a registry and are used to organize and manage Docker images.

Here are some key points to understand about Docker registries and repositories:

1. Docker Hub: Docker Hub is the default public registry provided by Docker. It hosts a vast collection of Docker images that are freely available for use. Users can search for images, pull them to their local machines, and push their own images to Docker Hub.

2. Private Registries: In addition to Docker Hub, you can set up your private registries to store and manage your Docker images. Private registries provide more control and security, as you can restrict access to authorized users or organizations. Docker provides Docker Registry as an open-source tool for hosting private registries.

3. Repository Names: Docker images are organized into repositories. A repository name consists of two parts: the registry URL and the image name. The registry URL is optional and is used to specify a non-default registry. If the registry URL is not provided, Docker assumes it to be Docker Hub. The image name is the name of the repository and can have multiple tags to represent different versions or variants of the image.

4. Image Tags: Image tags are used to differentiate different versions or variants of an image within a repository. A tag can be a version number, a branch name, or any other identifier. By default, if no tag is specified, Docker pulls the image with the "latest" tag.

5. Pulling and Pushing Images: To pull an image from a registry, you can use the docker pull command followed by the repository name. For example, docker pull ubuntu:latest. To push an image to a registry, you need to tag it with the appropriate repository name and then use the docker push command. For example, `docker tag myimage:latest myregistry/myimage:latest` followed by `docker push myregistry/myimage:latest`.

6. Docker Compose and Repositories: Docker Compose allows you to define multi-container applications using a YAML file. In Docker Compose, you can specify the repository and image names for each service, along with any required tags or version numbers.

Understanding Docker registries and repositories is essential for managing Docker images effectively. Whether you use the default Docker Hub, set up private registries, or leverage third-party registries, repositories help organize and version your Docker images, making it easier to distribute and deploy your containerized applications.


### Image repository 

Setting up and accessing an image repository in a cluster typically involves the following steps:

1. Choose an Image Repository: Select an image repository that fits your needs. Docker Hub is a popular public registry, but you may also consider private registries like Amazon Elastic Container Registry (ECR), Google Container Registry (GCR), or self-hosted solutions like Docker Registry.

2. Set up the Image Repository: If you're using a self-hosted registry, follow the documentation provided by the registry provider to set up and configure the repository. This usually involves installing and configuring the registry software on your cluster or infrastructure.

3. Build and Push Images: Build your Docker images locally or in your CI/CD pipeline and tag them appropriately with the repository information. Push the images to the configured image repository using the docker push command or the appropriate commands provided by the registry.

4. Authentication and Access Control: Ensure that you have the necessary authentication and access control mechanisms in place for your image repository. This includes managing user accounts, access tokens, and appropriate permissions to push and pull images.

5. Pull Images in the Cluster: In your cluster configuration or deployment files (such as Kubernetes YAML files), reference the images from your image repository. Use the repository URL and image name with the appropriate tag or version to specify the image to be pulled.

6. Cluster Access to the Repository: Ensure that your cluster nodes or worker nodes have network access to the image repository. This may involve configuring firewall rules, network settings, or network address translation (NAT) rules, depending on your infrastructure setup.

7. Image Pull Policies: Consider configuring image pull policies in your cluster configuration to control how frequently the cluster pulls images from the repository. This can help manage network bandwidth and improve deployment performance.

8. Image Security and Scanning: Depending on your requirements, consider implementing image security scanning in your pipeline or cluster to detect vulnerabilities in the images before deployment. Many image repositories and security tools provide scanning features.



### C groups 

Cgroups (control groups) and namespaces are two important features in Linux that are used in containerization technologies like Docker. They provide isolation and resource control mechanisms to ensure efficient and secure containerization. Let's explain cgroups and namespaces with examples:

* Control Groups (cgroups): Control Groups, or cgroups, allow fine-grained resource allocation and limitation for processes running on a Linux system. They enable controlling and monitoring of resource usage such as CPU, memory, disk I/O, and network bandwidth.

Example:
Let's say you have a Docker container running a resource-intensive application. You can use cgroups to limit the container's CPU usage to a certain percentage, restrict its memory allocation, or prioritize its disk I/O. This ensures that the container doesn't monopolize system resources and affects the performance of other containers or the host system.

The cgroup command-line tool in Linux allows you to create, modify, and monitor control groups. For example, you can create a control group called mygroup and limit its CPU usage to 50% with the following command: <br>

`cgroup -n mygroup cpuset.cpus="0" cpu.cfs_quota_us="50000" cpu.cfs_period_us="100000"`


### Namespaces

Namespaces provide process isolation by creating separate instances of global system resources for each container or process. Each namespace provides an isolated view of resources such as process IDs, network interfaces, mount points, and more. This allows containers to have their own independent environments without interfering with each other or the host system.

Example:
Let's say you have multiple containers running on the same host, and each container has its own network stack. Namespaces ensure that each container has its own network interfaces, IP addresses, routing tables, and firewall rules. This isolation prevents containers from accessing or interfering with network configurations of other containers or the host.

Linux provides different types of namespaces, such as PID (process ID) namespace, network namespace, mount namespace, and more. The unshare command in Linux allows you to create a new namespace or join an existing one. For example, you can create a new network namespace and run a command within that namespace using the following command: <br>

`unshare --net command`

### Docker cheat sheet for Run, registry and services 

1. Run Commands - Docker uses the run command to create containers from provided images. The default syntax for this command looks like this: `docker run [options] image`. After the default syntax, use one of the following flags: <br>

|Flag	|Explanation|
|--------|----------|
|--detach , -d	| Runs a container in the background and prints the container ID|
|--env , -e	| Sets environment variables|
|--hostname , -h	| Sets a hostname to a container|
|--label , -l	| Creates a meta data label for a container|
|--name	|Assigns a name to a container|
|--network	|Connects a container to a network|
|--rm	|Removes container when it stops|
|--read-only	|Sets the container filesystem as read-only|
|--workdir , -w	|Sets a working directory in a container|

2. Registry Commands - If you need to interact with Docker Hub, use the following commands:

|Command |	Explanation|
|---------|-----------|
|docker login	|Logs in to a registry|
|docker logout	|Logs out from a registry|
|docker pull mysql	|Pulls an image from a registry|
|docker push repo/ rhel-httpd:latest|	Pushes an image to a registry|
|docker search term|	Searches Docker Hub for images with the specified term|

3. Service Commands - Manage all Docker services with these basic commands:

|Command	| Explanation|
|-----------|------------|
|docker service ls	|Lists all services running in a swarm|
|docker stack services stackname	|Lists all running services|
|docker service ps servicename	|Lists the tasks of a service|
|docker service update servicename	|Updates a service|
|docker service create image	|Creates a new service|
|docker service scale servicename=10	|Scales one or more replicated services|
|docker service logs stackname servicename|	Lists all service logs|


