# JD Java Spring Boot React Full Stack Application with Docker

## What You Will Learn during this Step 01:
- Exploring 04 Java Full Stack Spring Boot React App

![Browser](Images/react_00_architecture.png)

To understand the application
- https://www.springboottutorial.com/spring-boot-react-full-stack-crud-maven-application

To understand JWT and Spring Security Configuration
- https://www.springboottutorial.com/spring-boot-react-full-stack-with-spring-security-basic-and-jwt-authentication


## Running the Application

- REST API - Import `restful-web-services` into Eclipse as Maven Project. Run `com.in28minutes.rest.webservices.restfulwebservices.RestfulWebServicesApplication` as a Java Application. Check Authentication and REST API Sections for executing REST APIs.
- React Application - Import `frontend/todo-app` into Visual Studio Code. Run `npm install` followed by `npm start`
- http://localhost:3000/ with credentials in28minutes/dummy

> Look at  `Creating New Users` section for creating new users.

## Authentication

All REST API are protected by JWT Authentication with Spring Security. 

POST to http://localhost:8080/authenticate

```
{
  "username":"in28minutes",
  "password":"dummy"
}
```

Response
```
{
"token": "Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJpbjI4bWludXRlcyIsImV4cCI6MTY1MjU3MjI1OCwiaWF0IjoxNjUxOTY3NDU4fQ.6jvEhUajiiO5w6SFI2roAFkWYqzw_7V0bpAlOHlgAB_3At4yLpPq4RIIY-1tU6qxJXBMpsiZ6V2t2Yt1zer5Mw"
}
```

Use the token in the headers for all subsequent requests.

`Authorization` : `Bearer ${token}`

Example 

`Authorization` : `Bearer Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJpbjI4bWludXRlcyIsImV4cCI6MTY1MjU3MjI1OCwiaWF0IjoxNjUxOTY3NDU4fQ.6jvEhUajiiO5w6SFI2roAFkWYqzw_7V0bpAlOHlgAB_3At4yLpPq4RIIY-1tU6qxJXBMpsiZ6V2t2Yt1zer5Mw`



![Browser](Images/Screenshot_25.png)

![Browser](Images/Screenshot_26.png)

![Browser](Images/Screenshot_27.png)

![Browser](Images/Screenshot_28.png)

---
## What You Will Learn during this Step 02:
- Running React Frontend in Local
* package.json update the below 
* OLD
```json
"scripts": {
    "start": "PORT=4200 react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```
* New in windows

```json
"scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```

- Running React Frontend in Local

```
user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/frontend/todo-app (master)
$ pwd
/c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/frontend/todo-app

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/frontend/todo-app (master)
$ npm install

Run `npm audit` for details.
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\04-spring-boot-react-full-stack-h2\frontend\todo-app> npm start

```

![Browser](Images/Screenshot_29.png)

---
## What You Will Learn during this Step 03:
- Containerizing Java REST API Backend


```docker

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/restful-web-services (master)
$ mvn clean package

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/restful-web-services (master)
$ docker run -p 8080:8080 jbirla/rest-api-full-stack:0.0.1-SNAPSHOT


```

## What You Will Learn during this Step 04:
- Creating Multi Stage Docker Build for React Frontend Code

```
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\04-spring-boot-react-full-stack-h2\frontend\todo-app>  npm run build

> todo-app@0.1.0 build
> react-scripts build

Creating an optimized production build...
Compiled successfully.

File sizes after gzip:

  76.31 KB (+1.76 KB)  build\static\js\2.b9156a8c.chunk.js
  3.26 KB (+1 B)       build\static\js\main.9f2df73b.chunk.js
  764 B                build\static\js\runtime~main.c5541365.js
  608 B (-17 B)        build\static\css\main.6e67fd60.chunk.css

The project was built assuming it is hosted at the server root.
You can control this with the homepage field in your package.json.
For example, add this to build it for GitHub Pages:

  "homepage" : "http://myname.github.io/myapp",

The build folder is ready to be deployed.
You may serve it with a static server:

  npm install -g serve
  serve -s build

Find out more about deployment here:

  https://bit.ly/CRA-deploy

PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\04-spring-boot-react-full-stack-h2\frontend\todo-app>


```

### using docker file

* todo-app\Dockerfile
```
## Stage 1 - Lets build the "deployable package"
FROM node:7.10 as frontend-build
WORKDIR /fullstack/frontend

# Step 1 - Download all package dependencies first.
# We will redownload dependencies only when packages change.
COPY package.json package-lock.json ./
RUN npm install

# Step 2 - Copy all source and run build
COPY . ./
RUN npm run build

## Stage 2 - Let's build a minimal image with the "deployable package"
FROM nginx:1.12-alpine
COPY --from=frontend-build /fullstack/frontend/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

```
![Browser](Images/Screenshot_30.png)


```docker
C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\04-spring-boot-react-full-stack-h2\frontend\todo-app> docker build .
```

---
## What You Will Learn during this Step 05:
- Improve Front End Docker Build - dockerignore

* .dockerignore 
```
node_modules
```

```

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/frontend/todo-app (master)
$ docker build . -t jbirla/todo-front-end:0.0.1-SNAPSHOT
#1 [internal] load build definition from Dockerfile
#1 sha256:fde313106aa24f6f57ea3b49127d4483203e347393d7a6fd3f6e54a6a1a5bf0e
#1 transferring dockerfile: 32B 0.1s done
#1 DONE 0.1s

#2 [internal] load .dockerignore
#2 sha256:000877efaedfa2e013d016b310d240bd2cf1afcb7b5cb6df949241ae3aea494c
#2 transferring context: 34B 0.0s done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/nginx:1.12-alpine
#3 sha256:d1880b279652348e634e097b37eb0d02409d64dca43e99a33372d18b2a29c44c
#3 DONE 2.3s

#4 [internal] load metadata for docker.io/library/node:7.10
#4 sha256:c64d3c0f25d9340226c8ccf379d731623a105bf6ecd89e12c005e51d8ccd18eb
#4 DONE 2.5s

#5 [stage-1 1/2] FROM docker.io/library/nginx:1.12-alpine@sha256:db5acc22920799fe387a903437eb89387607e5b3f63cf0f4472ac182d7bad644
#5 sha256:49e41f0d92d54a5775a0755409eab218bc38ae8c95972fcb311e3d6cdf6ea5dd
#5 DONE 0.0s

#6 [frontend-build 1/6] FROM docker.io/library/node:7.10@sha256:af5c2c6ac8bc3fa372ac031ef60c45a285eeba7bce9ee9ed66dad3a01e29ab8d
#6 sha256:4dba5db6d27a69c180c5bf0230783e152ee15070028ab569bc1f57d2da382ce8
#6 DONE 0.0s

#8 [internal] load build context
#8 sha256:6e5f40dbad2eef0cc0dd5f886bd9b908ff3c299d76087b91062bcd85877ea538
#8 transferring context: 2.80kB 0.0s done
#8 DONE 0.1s

#7 [frontend-build 2/6] WORKDIR /fullstack/frontend
#7 sha256:0aa4d8906ffb186972c7e8c083a72a3f0f4565f9e2907c446eff02f1220df8a9
#7 CACHED

#10 [frontend-build 4/6] RUN npm install
#10 sha256:f2e23746330eb12a7e978237cb783a771f5ae3539ad19e16a0b212bd626bec03
#10 CACHED

#11 [frontend-build 5/6] COPY . ./
#11 sha256:bac3f77a9d67cea1771951cd2fe9dffb315c2229d3d758b2aca737d71eace1ee
#11 CACHED

#12 [frontend-build 6/6] RUN npm run build
#12 sha256:5bee14f86e9f522ef6f13f2b1f4d26c69a8b52c9f37eb08123fb6a1a1036cff9
#12 CACHED

#9 [frontend-build 3/6] COPY package.json package-lock.json ./
#9 sha256:944d1cf7ac152cc5b17ebd6c05148cefa936ee84d29f98c4a4b86e1a6e6a6c64
#9 CACHED

#13 [stage-1 2/2] COPY --from=frontend-build /fullstack/frontend/build /usr/share/nginx/html
#13 sha256:26fbe4f018f775ac4d196d091d201bbfdbf88856ec2e9c5b72398c277abd95ad
#13 CACHED

#14 exporting to image
#14 sha256:e8c613e07b0b7ff33893b694f7759a10d42e180f2b4dc349fb57dc6b71dcab00
#14 exporting layers done
#14 writing image sha256:7ad8754ba69d7f544959d198f9a37e0b09afa617c735aa674150b78266c0f56e done
#14 naming to docker.io/jbirla/todo-front-end:0.0.1-SNAPSHOT done
#14 DONE 0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/frontend/todo-app (master)
$ docker images
REPOSITORY                          TAG                  IMAGE ID       CREATED          SIZE
jbirla/todo-front-end               0.0.1-SNAPSHOT       7ad8754ba69d   2 minutes ago    16.9MB
<none>                              <none>               7387fcbc5d73   12 minutes ago   16.8MB
jbirla/rest-api-full-stack          0.0.1-SNAPSHOT       0f5a2a91bee7   40 minutes ago   145MB
jbirla/todo-web-application-mysql   0.0.1-SNAPSHOT       9bbe274b7332   18 hours ago     150MB
jbirla/todo-web-application-h2      0.0.1-SNAPSHOT       fc41b76c2bd0   19 hours ago     149MB
<none>                              <none>               4eed235fbd0f   19 hours ago     149MB
jbirla/hello-world-rest-api         0.0.1-SNAPSHOT       efc0e6c37489   20 hours ago     122MB
<none>                              <none>               04b0b95f0b09   20 hours ago     105MB
01-hello-world-rest-api             0.0.1-SNAPSHOT       73dba6f8fc53   2 days ago       147MB
jitubirla/hello-world-rest-api      dockerfile1          a9140d3fbe0b   2 days ago       122MB
<none>                              <none>               d0eb2f0f70c4   2 days ago       122MB
<none>                              <none>               058939030e6f   2 days ago       122MB
jitubirla/hello-world-rest-api      0.0.1-SNAPSHOT       abe382b1793d   2 days ago       122MB
<none>                              <none>               a02550b50868   2 days ago       122MB
<none>                              <none>               4aba1c6d85b1   2 days ago       122MB
<none>                              <none>               1ea7f95a9506   2 days ago       122MB
<none>                              <none>               cc3fbea13df6   2 days ago       122MB
<none>                              <none>               56b977aa7529   2 days ago       122MB
jitubirla/hello-world-rest-api      manual3              d75e467d5e50   2 days ago       122MB
hello-docker                        latest               5cf1242e3281   3 days ago       171MB
mysql                               5.7                  8aa4b5ffb001   10 days ago      462MB
docker/getting-started              latest               cb90f98fd791   3 weeks ago      28.8MB
hello-world                         latest               feb5d9fea6a5   7 months ago     13.3kB
in28min/todo-rest-api-h2            1.0.0.RELEASE        f8049a029560   2 years ago      143MB
in28min/todo-rest-api-h2            latest               f8049a029560   2 years ago      143MB
openjdk                             8-jdk-alpine         a3562aa0b991   2 years ago      105MB
tomcat                              8.0.51-jre8-alpine   fcc5ace83900   4 years ago      106MB

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/frontend/todo-app (master)

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2/frontend/todo-app (master)
$ docker run -p 4200:80 jbirla/todo-front-end:0.0.1-SNAPSHOT


```

![Browser](Images/Screenshot_31.png)

---
## What You Will Learn during this Step 06:

- Using Multi Stage Docker Build for Java REST API Backend
- Running Java REST API Backend Docker Image
```
##### Stage 1 - Lets build the "deployable package"

FROM maven:3.6.1-jdk-8-alpine as backend-build
WORKDIR /fullstack/backend

### Step 1 - Copy pom.xml and download project dependencies

# Dividing copy into two steps to ensure that we download dependencies 
# only when pom.xml changes
COPY pom.xml .
# dependency:go-offline - Goal that resolves all project dependencies, 
# including plugins and reports and their dependencies. -B -> Batch mode
RUN mvn dependency:go-offline -B

### Step 2 - Copy source and build "deployable package"
COPY src src
RUN mvn install -DskipTests

# Unzip
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

##### Stage 2 - Let's build a minimal image with the "deployable package"
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ARG DEPENDENCY=/fullstack/backend/target/dependency
COPY --from=backend-build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=backend-build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=backend-build ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","com.in28minutes.rest.webservices.restfulwebservices.RestfulWebServicesApplication"]
```
---
## What You Will Learn during this Step 07 08:
- Exploring Docker Compose

```
o-app> docker compose version
Docker Compose version v2.4.1
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\04-spring-boot-react-full-stack-h2\frontend\todo-app>
```

---

## What You Will Learn during this Step 09:

- Running Full Stack Application with Docker Compose

```yml
version: '3.7'
# ERROR - Removed subprocess.CalledProcessError: 
# Command '['/usr/local/bin/docker-credential-desktop', 'get']' 
# returned non-zero exit status 1

# SOLUTION - Remove "credsStore":"desktop" from ~/.docker/config.json 
# Original Content of ~/.docker/config.json
# {"auths":{},"credsStore":"", "credsStore":"desktop","stackOrchestrator":"swarm"}
# Update it to this
# {"auths":{},"credsStore":"","stackOrchestrator":"swarm"}
# OR
# {"auths":{},"stackOrchestrator":"swarm"}
services:
  todo-frontend:
    image: jbirla/todo-front-end:0.0.1-SNAPSHOT
    #build:
      #context: frontend/todo-app
      #dockerfile: Dockerfile
    ports:
      - "4200:80"
    restart: always
    depends_on: # Start the depends_on first
      - todo-api 
    networks:
      - fullstack-application-network

  todo-api:
    image: jbirla/rest-api-full-stack:0.0.1-SNAPSHOT
    ports:
      - "8080:8080"
    restart: always
    networks:
      - fullstack-application-network
  
# Networks to be created to facilitate communication between containers
networks:
  fullstack-application-network:
```

```
user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2 (master)
$ docker-compose up

```

```

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2 (master)
$ docker-compose up -d
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Created
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Created
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Starting
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Started
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Starting
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Started

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2 (master)
$ docker-compose down
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Stopping
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Stopping
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Stopped
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Removing
Container 04-spring-boot-react-full-stack-h2-todo-frontend-1  Removed
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Stopping
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Stopping
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Stopped
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Removing
Container 04-spring-boot-react-full-stack-h2-todo-api-1  Removed
Network 04-spring-boot-react-full-stack-h2_fullstack-application-network  Removing
Network 04-spring-boot-react-full-stack-h2_fullstack-application-network  Removed

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/04-spring-boot-react-full-stack-h2 (master)
$

```