# JD Java Spring Boot Todo Web Application with Dockerld

## What You Will Learn during this Step 01:
- Setting up 02 Spring Boot Todo Web Application in Local 


Run com.in28minutes.springboot.web.SpringBootFirstWebApplication as a Java Application.

Runs on default port of Spring Boot - 8080 

## Can be run as a Jar or a WAR

`mvn clean install` generate a war which can deployed to your favorite web server.

We will deploy to Docker as a WAR

## Web Application

- http://localhost:8080/login with jbirla/dummy as credentials
- You can add, delete and update your todos
- Spring Security is used to secure the application
- `com.in28minutes.springboot.web.security.SecurityConfiguration` contains the in memory security credential configuration.

## H2 Console

- http://localhost:8080/h2-console
- Use `jdbc:h2:mem:testdb` as JDBC URL 


![Browser](Images/Screenshot_17.png)

```
[INFO] Image will be built as jbirla/todo-web-application-h2:0.0.1-SNAPSHOT
[INFO] 
[INFO] Step 1/5 : FROM tomcat:8.0.51-jre8-alpine
[INFO] 
[INFO] Pulling from library/tomcat
[INFO] Digest: sha256:8c19caad3ac527eb88a8d75448d40216819da15ebf433fbc97b0d8513a9a0767
[INFO] Status: Image is up to date for tomcat:8.0.51-jre8-alpine
[INFO]  ---> fcc5ace83900
[INFO] Step 2/5 : EXPOSE 8080
[INFO] 
[INFO]  ---> Using cache
[INFO]  ---> adbba33a826f
[INFO] Step 3/5 : RUN rm -rf /usr/local/tomcat/webapps/*
[INFO] 
[INFO]  ---> Using cache
[INFO]  ---> c18453da204a
[INFO] Step 4/5 : COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
[INFO] 
[INFO]  ---> a85aa7192ab1
[INFO] Step 5/5 : CMD ["catalina.sh","run"]
[INFO] 
[INFO]  ---> Running in 9f1673b48050
[INFO] Removing intermediate container 9f1673b48050
[INFO]  ---> 4eed235fbd0f
[INFO] Successfully built 4eed235fbd0f
[INFO] Successfully tagged jbirla/todo-web-application-h2:0.0.1-SNAPSHOT
[INFO] 
[INFO] Detected build of image with id 4eed235fbd0f
[INFO] Successfully built jbirla/todo-web-application-h2:0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  35.988 s
[INFO] Finished at: 2022-05-07T12:50:15+05:30
[INFO] ------------------------------------------------------------------------
```

![Browser](Images/Screenshot_18.png)

---
## What You Will Learn during this Step 02:

- Create Docker Image for Spring Boot Todo Web Application

```docker
FROM tomcat:8.0.51-jre8-alpine
EXPOSE 8080
RUN rm -rf /usr/local/tomcat/webapps/*
COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
CMD ["catalina.sh","run"]
```

### Dockerfile Maven

- https://github.com/spotify/dockerfile-maven

```
	<plugin>
		<groupId>com.spotify</groupId>
		<artifactId>dockerfile-maven-plugin</artifactId>
		<version>1.4.10</version>
		<executions>
			<execution>
				<id>default</id>
				<goals>
					<goal>build</goal>
				</goals>
			</execution>
		</executions>
		<configuration>
			<repository>jbirla/${project.name}</repository>
			<tag>${project.version}</tag>
			<skipDockerInfo>true</skipDockerInfo>
		</configuration>
	</plugin>
```

```docker

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/02-todo-web-application-h2 (master)
$ mvn clean package

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/02-todo-web-application-h2 (master)
$ docker container run -p 8080:8080 jbirla/todo-web-application-h2:0.0.1-SNAPSHOT

```

---
## What You Will Learn during this Step 03:
