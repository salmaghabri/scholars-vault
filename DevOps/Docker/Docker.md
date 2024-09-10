# Running a SpringBoot Application in a Docker Container
#### SpringBoot Application Overview

We start by creating a basic SpringBoot application with a single controller. The controller responds to a GET request by returning a simple "Hello" message.
```java
@RestController  
public class HelloController {  
@GetMapping("/hello")  
public String hello(@RequestParam(value = "name", defaultValue = "World") String name) {  
return String.format("Hello %s!", name);  
}  
}
```
#### Dockerfile
To containerize the SpringBoot application, we define a Dockerfile. This file specifies the environment and commands required to package and run the application within a Docker container.
```Dockerfile
# Use OpenJDK 17 as base image  
FROM openjdk:17-alpine  
  
# Set the working directory in the container  
WORKDIR /app  
  
# Copy the packaged jar file into the container at /app  
COPY target/spring-0.0.1-SNAPSHOT.jar /app/spring.jar  
  
# Expose port 8080  
EXPOSE 8080  
  
# Command to run the spring boot application  
CMD ["java", "-jar", "spring.jar"]
```

### Building and Running the Docker Image
```
docker build -t .
```
 ![[spring-app-1.png]]

Then, we launch the Docker container while exposing port 8080 to allow access to the SpringBoot application.

 ```shell
 docker run -p 8080:8080 spring-app
 
```


![[1seul-contenaire-1.png]]
Then we check if it works

![[1seul-contenaire-2.png]]
![[1seul-contenaire-3.png]]

### Pushing to DockerHub
##### login 
```shell
docker login
```

![[Running a SpringBoot Application in a Docker Container-1.png]]

##### create a new repo in dockerhub
![[Running a SpringBoot Application in a Docker Container-2.png]]
##### change the local image's tag
```
docker tag local_image_name:tag_name dockerhub_username/repository_name:tag_name

```

![[Running a SpringBoot Application in a Docker Container-3.png]]
##### push to dockerhub

```shell
docker push dockerhub_username/repository_name:tag_name
```

![[Running a SpringBoot Application in a Docker Container-4.png]]

![[Running a SpringBoot Application in a Docker Container-5.png]]
## Understanding Docker Image Layers and Reusability

### Docker Image Layers

Docker images are composed of multiple layers, each representing a discrete instruction in the Dockerfile. These layers are stacked on top of each other, with each layer caching the results of the corresponding build step. This approach enables efficient image building and sharing.

### Reusability of Images

One of the key advantages of Docker is the reusability of images. By utilizing existing images as base layers in the Dockerfile, developers can build upon well-tested and optimized environments. This reduces the need to recreate the entire environment from scratch, saving time and resources.



### Docker Image Layers

Docker images are composed of multiple layers, each representing a discrete instruction in the Dockerfile. These layers are stacked on top of each other, with each layer caching the results of the corresponding build step. This approach enables efficient image building and sharing.

### Reusability of Images

One of the key advantages of Docker is the reusability of images. By utilizing existing images as base layers in the Dockerfile, developers can build upon well-tested and optimized environments. This reduces the need to recreate the entire environment from scratch, saving time and resources.
# Running a Multi-Container Application
## Without Docker Compose
### Build Docker image for a spring boot app

Launch the container for a  Spring Boot application and verify communication:
``` Dockerfile
FROM openjdk:17-alpine  

WORKDIR /app  

COPY target/CAT.jar /app  

EXPOSE 8080  

# entrypoint khir  
ENTRYPOINT ["java", "-jar", "CAT.jar"]

```


![[Multi-contenaire-1.png]]
	
### postgres-container
```bash
docker pull postgres
```
	
### run  containers spereately 
``` bash
docker run --name postgres-container -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres
	```

``` shell
docker run --name cat-backend -e DB_HOST=host.docker.internal -p 8080:8080  cat-back
```
	
![[Multi-contenaire-4.png]]
checking if it works
![[Multi-contenaire-2.png]]

``` bash
Docker ps
```


![[Multi-contenaire-5.png]]	
### same application different ports

![[Multi-contenaire-6.png]]


![[Multi-contenaire-7.png]]
if we check with the new port

![[Multi-contenaire-8.png]]
## docker compose
Compose a Docker Compose file to set the interconnected services:

``` yaml
	version: '3.8'  
	  
	services:  
	postgres:  
		image: postgres:latest  
			container_name: postgres  
		environment:  
			POSTGRES_DB: gps2  
			POSTGRES_USER: ${POSTGRES_USER}  
			POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}  
			ports:  
			- "5432:5432"  
			volumes:  
			- postgres:/var/lib/postgresql/data  
	  
	back-cat:  
		build: .  
		container_name: back-cat  
		ports:  
		- "8080:8080"  
		environment:  
			SPRING_DATASOURCE_URL: jdbc:postgresql://postgres:5432/gps2  
			SPRING_DATASOURCE_USERNAME: ${POSTGRES_USER}  
			SPRING_DATASOURCE_PASSWORD: ${POSTGRES_PASSWORD}  
	  
	depends_on:  
	- postgres  
	  
	volumes:  
		postgres:  
			driver: local
	```
#### Postgres Service:

- **Image**: Pulls the latest PostgreSQL image from Docker Hub.
- **Container Name**: Specifies the name of the container as "postgres".
- **Environment Variables**:
    - `POSTGRES_DB`: Sets the name of the database to "gps2".
    - `POSTGRES_USER`: Uses the value of the `POSTGRES_USER` environment variable.
    - `POSTGRES_PASSWORD`: Uses the value of the `POSTGRES_PASSWORD` environment variable.
- **Ports**: Maps port 5432 of the host to port 5432 of the container, allowing external access to the PostgreSQL service.
- **Volumes**: Mounts a volume named "postgres" to persist data at "/var/lib/postgresql/data" in the container.

#### Back-cat Service:

- **Build**: Specifies to build the Dockerfile found in the current directory (`.`).
- **Container Name**: Specifies the name of the container as "back-cat".
- **Ports**: Maps port 8080 of the host to port 8080 of the container, allowing external access to the Spring Boot application.
- **Environment Variables**:
    - `SPRING_DATASOURCE_URL`: Sets the URL for the Spring Boot application to connect to the PostgreSQL database.
    - `SPRING_DATASOURCE_USERNAME`: Uses the value of the `POSTGRES_USER` environment variable.
    - `SPRING_DATASOURCE_PASSWORD`: Uses the value of the `POSTGRES_PASSWORD` environment variable.
- **Depends On**: Specifies that the `back-cat` service depends on the `postgres` service.

#### Volumes:
- **postgres**: Defines a volume named "postgres" with the "local" driver to persist PostgreSQL data.

### Checking communication
![[Multi-contenaire-11.png]]![[Multi-contenaire-10.png]]




# Building and pushing docker image with github actions
## create a GitHub Actions workflow
This GitHub Actions workflow file automates the process of building a Spring Boot application, packaging it with Maven, building a Docker image, and pushing it to Docker Hub.
```yaml
name: Build and Push Docker Image   
 
  on:   
  push:   
  branches:   
  - master   
  env:   
  USERNAME: ${{ secrets.DOCKERHUB_USERNAME }}   
  DOCKER_TOKEN: ${{ secrets.DOCKER_TOKEN }}   
 
  jobs:   
  build:   
  runs-on: ubuntu-latest   
 
  steps:   
  - name: Checkout code   
  uses: actions/checkout@v2   
  - name: Set up JDK 17   
  uses: actions/setup-java@v2   
  with:   
  java-version: 17   
  distribution: 'adopt'   
 
  - name: Build with Maven   
  run: ./mvnw clean package   
 
  # Login to DockerHub   
  - name: Login to DockerHub   
  run: docker login -u $USERNAME --password $DOCKER_TOKEN   
 
  # Build and push Docker image   
  - name: Build and push Docker image   
  run: \  
  docker build -t salmaghabri/with-ga:latest .   
  docker push salmaghabri/with-ga:latest   
```


1. **Workflow Triggers**:
    - The workflow triggers on `push` events to the `master` branch.
2. **Environment Variables**:
    - It sets up environment variables `USERNAME` and `DOCKER_TOKEN` to store Docker Hub credentials. These are sourced from GitHub secrets.
    ![[Github Actions-4.png]]
3. **Jobs**:
    - **build**: This job runs on an `ubuntu-latest` virtual machine.
        - **Steps**:
            - `Checkout code`: Checks out the source code from the repository.
            - `Set up JDK 17`: Sets up JDK 17 using the `actions/setup-java@v2` action.
            - `Build with Maven`: Runs the Maven wrapper script (`mvnw`) to clean the project and package it into an executable JAR file. But first, we need to ensure that git has the execution permission on mnvw  by running `git update-index --chmod=+x ./mvnw`
            - `Login to DockerHub`: Logs in to Docker Hub using the provided credentials.
            - `Build and push Docker image`: Builds a Docker image from the Dockerfile in the repository and pushes it to Docker Hub. The Docker image is tagged as `salmaghabri/with-ga:latest`.
4. **Docker Hub Credentials**:
    - The workflow uses the `DOCKERHUB_USERNAME` and `DOCKER_TOKEN` secrets to authenticate with Docker Hub.
5. **Docker Image Tagging**:
    - The Docker image is tagged with `salmaghabri/with-ga:latest`.

## Build finished
After running run the jobs of the workflow, we can see that the build succeded and the image was pushed.
![[Github Actions-1.png]]
![[Github Actions-2.png]]