# JD Docker Interview

## Docker With Java Spring Boot Hello World Rest APIs
### Building an Image Manually
 1. Intruducing ``` Docker ``` [Solution for Problem: Create docker Image and run]
 ```
 1. Build a Jar - /target/hello-world-rest-api.jar
2. Setup the Prerequisites for Running the JAR - openjdk:8-jdk-alpine
3. Copy the jar
4. Run the jar

### Docker Commands - Creating Image Manually

- docker run -dit openjdk:8-jdk-alpine
- docker container exec naughty_knuth ls /tmp
- docker container cp target/hello-world-rest-api.jar naughty_knuth:/tmp
- docker container exec naughty_knuth ls /tmp
- docker container commit naughty_knuth jbirla/hello-world-rest-api:manual1
- docker run jbirla/hello-world-rest-api:manual1
- docker container ls
- docker container commit --change='CMD ["java","-jar","/tmp/hello-world-rest-api.jar"]' naughty_knuth jbirla/hello-world-rest-api:manual2
- docker run -p 8080:8080 jbirla/hello-world-rest-api:manual2
 
 ```
 ### Building an Image Using docker file 
  2. Intruducing ``` Dockerfile ``` [Solution for Problem: In previoud step we have to do lot maual work]
  ```
  FROM openjdk:8-jdk-alpine
EXPOSE 8080
ADD target/hello-world-rest-api.jar hello-world-rest-api.jar
ENTRYPOINT ["sh", "-c", "java -jar /hello-world-rest-api.jar"]
  ```
 ### Using Docker Spotify Plugin to create docker images
  3. Intruducing ``` Spotify Maven Plugin ``` [Solution for Problem: It integrates with maven and using maven clean build]

 ### Generic Reusable Dockerfile
  4. Intruducing ``` Generic Docker file ``` [Solution for Problem: Resuable docker file]
  ```
  FROM openjdk:8-jdk-alpine
EXPOSE 8080
ADD target/*.jar app.jar
ENTRYPOINT ["sh", "-c", "java -jar /app.jar"]
```
 ### Using JIB plugin to create docker images
  5. Intruducing ``` JIB Plugin ``` [Solution for Problem: Jib builds containers without using a Dockerfile or requiring a Docker installation. You can use Jib in the Jib plugins for Maven ]

 ### Using Fabric8 docker maven plugin to create docker images
  6. Intruducing ``` Fabric8 docker maven plugin ``` [Solution for Problem:  This is a Maven plugin for managing Docker images and containers]

----
## Docker With Java Spring Boot Todo WEB APP
  1. Intruducing ``` Create docker image for Web ``` [Solution for Problem:  Docker image for Spring boot web app]
```
FROM tomcat:8.0.51-jre8-alpine
EXPOSE 8080
RUN rm -rf /usr/local/tomcat/webapps/*
COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
CMD ["catalina.sh","run"]
```
## Docker With Java Spring Boot Todo WEB APP using MySQL

### MySQL + Web App

  1. Intruducing ``` Create docker image for Web using MySQL ``` [Solution for Problem:  Docker image for Spring boot web app using MySQL]

#### Launching MySQL and Web App using Link in bridge network

2. Intruducing ``` Launching MySQL using Docker ``` [Solution for Problem:  Docker image running for MYSQL]
  
```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --     publish 3306:3306 mysql:5.7
```

 3. Intruducing ``` Launching Web App using link with MySQL (deprecated) ``` [Solution for Problem:  Two cotainer can't communicate becasue they use default bridge network which used default setting we need to link both]
 
 ```
 docker container run -p 8080:8080 --link=mysql -e RDS_HOSTNAME=mysql  in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
 ```
 #### Launching MySQL and Web App using Custom Network
 

 4. Intruducing ``` Launching Web App using MYSQL using custom network ``` [Solution for Problem: We can't use host network in windows and mac as docker alway run in these OS as docker desktop while network host can run in unix in below link not required]
 * MySQL
```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 --network=web-application-mysql-network mysql:5.7
```
* Web App 
```
docker container run -p 8080:8080 --network=web-application-mysql-network -e RDS_HOSTNAME=mysql  in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
```

