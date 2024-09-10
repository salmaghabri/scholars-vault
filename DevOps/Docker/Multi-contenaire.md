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
