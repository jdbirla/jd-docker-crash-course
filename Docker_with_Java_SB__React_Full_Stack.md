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

