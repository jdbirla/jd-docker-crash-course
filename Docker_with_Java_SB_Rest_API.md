# JD Docker with Java Spring Boot Hello World

## What You Will Learn during this Step 01:
- Setting up 01 Spring Boot Hello World Rest API in Local 
---
## What You Will Learn during this Step 02:
- Build Docker Image Manually for 01 Hello World Rest API

### Building an Image

1. Build a Jar - /target/hello-world-rest-api.jar
2. Setup the Prerequisites for Running the JAR - openjdk:8-jdk-alpine
3. Copy the jar
4. Run the jar

1. Build a Jar - /target/hello-world-rest-api.jar
```
Building jar: C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api\target\hello-world-rest-api.jar
```

![Browser](Images/Screenshot_13.png)
![Browser](Images/Screenshot_14.png)
![Browser](Images/Screenshot_15.png)


```
2. Setup the Prerequisites for Running the JAR - openjdk:8-jdk-alpine



C:\Users\user>cd C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api

C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api>docker run -dit openjdk:8-jdk-alpine
Unable to find image 'openjdk:8-jdk-alpine' locally
8-jdk-alpine: Pulling from library/openjdk
e7c96db7181b: Already exists
f910a506b6cb: Already exists
c2274a1a0e27: Already exists
Digest: sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3
Status: Downloaded newer image for openjdk:8-jdk-alpine
72ebb31f74858df58410702d654a63c8e08719e551b82154e7c60624b9380f2d


PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker images
REPOSITORY                 TAG             IMAGE ID       CREATED             SIZE
hello-docker               latest          5cf1242e3281   About an hour ago   171MB
docker/getting-started     latest          cb90f98fd791   3 weeks ago         28.8MB
hello-world                latest          feb5d9fea6a5   7 months ago        13.3kB
in28min/todo-rest-api-h2   1.0.0.RELEASE   f8049a029560   2 years ago         143MB
in28min/todo-rest-api-h2   latest          f8049a029560   2 years ago         143MB
openjdk                    8-jdk-alpine    a3562aa0b991   2 years ago         105MB
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker container ls
CONTAINER ID   IMAGE                  COMMAND     CREATED         STATUS         PORTS     NAMES
65f59657988a   openjdk:8-jdk-alpine   "/bin/sh"   2 minutes ago   Up 2 minutes             keen_shannon
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api>


3. Copy the jar


PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker container exec keen_shannon ls /tmp
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker container cp target/hello-world-rest-api.jar keen_shannon:/tmp
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker container exec keen_shannon ls /tmp
hello-world-rest-api.jar
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api>


4. Run the jar

PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker container commit keen_shannon jitubirla/hello-world-rest-api:manual1
sha256:8d80cb3ca34918ddb3b61bd5cc99dcc9388ea96814c95eb2113d69724bafa48b
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker images
REPOSITORY                       TAG             IMAGE ID       CREATED         SIZE
jitubirla/hello-world-rest-api   manual1         8d80cb3ca349   5 seconds ago   122MB
hello-docker                     latest          5cf1242e3281   2 hours ago     171MB
docker/getting-started           latest          cb90f98fd791   3 weeks ago     28.8MB
hello-world                      latest          feb5d9fea6a5   7 months ago    13.3kB
in28min/todo-rest-api-h2         1.0.0.RELEASE   f8049a029560   2 years ago     143MB
in28min/todo-rest-api-h2         latest          f8049a029560   2 years ago     143MB
openjdk                          8-jdk-alpine    a3562aa0b991   2 years ago     105MB
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api>


PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker run jitubirla/hello-world-rest-api:manual1
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker container ls
CONTAINER ID   IMAGE                  COMMAND     CREATED          STATUS          PORTS     NAMES
65f59657988a   openjdk:8-jdk-alpine   "/bin/sh"   12 minutes ago   Up 12 minutes             keen_shannon
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api>

PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker container commit --change='CMD ["java","-jar","/tmp/hello-world-rest-api.jar"]' keen_shannon jitubirla/hello-world-rest-api:manual2
sha256:f5cf88227da4f2f327ba3f6b06464116a3972eb66080bdf8a3cec7b21acfc133
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker images
REPOSITORY                       TAG             IMAGE ID       CREATED          SIZE
jitubirla/hello-world-rest-api   manual2         f5cf88227da4   35 seconds ago   122MB
jitubirla/hello-world-rest-api   manual1         8d80cb3ca349   6 minutes ago    122MB
hello-docker                     latest          5cf1242e3281   2 hours ago      171MB
docker/getting-started           latest          cb90f98fd791   3 weeks ago      28.8MB
hello-world                      latest          feb5d9fea6a5   7 months ago     13.3kB
in28min/todo-rest-api-h2         1.0.0.RELEASE   f8049a029560   2 years ago      143MB
in28min/todo-rest-api-h2         latest          f8049a029560   2 years ago      143MB
openjdk                          8-jdk-alpine    a3562aa0b991   2 years ago      105MB

PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\01-hello-world-rest-api> docker run -p 8080:8080 jitubirla/hello-world-rest-api:manual2

```
* Output 

![Browser](Images/Screenshot_16.png)

---


