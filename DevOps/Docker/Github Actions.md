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