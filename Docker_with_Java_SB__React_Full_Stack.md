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