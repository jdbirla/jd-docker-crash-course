# JD Docker Interview

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
  

