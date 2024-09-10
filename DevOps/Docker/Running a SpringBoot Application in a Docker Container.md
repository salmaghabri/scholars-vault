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