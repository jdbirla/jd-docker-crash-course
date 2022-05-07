# JD Java Spring Boot Todo Web Application using MySQL with Docker

## What You Will Learn during this Step 01:
- Code Review of 03 Todo Web Application MySQL

---
## What You Will Learn during this Step 02:

- Running MySQL as Docker Container on Local

## Changes from H2 Application

#### pom.xml

```
<dependency>
	<groupId>com.h2database</groupId>
	<artifactId>h2</artifactId>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
</dependency>
```

#### src/main/resources/application.properties

```
#spring.h2.console.enabled=true
#spring.h2.console.settings.web-allow-others=true

spring.jpa.hibernate.ddl-auto=update
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL55Dialect
spring.datasource.url=jdbc:mysql://localhost:3306/todos
spring.datasource.username=todos-user
spring.datasource.password=dummytodos
```

#### src/test/resources/application.properties

```
spring.jpa.hibernate.ddl-auto=create-drop
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.url=jdbc:h2:mem:testdb;DB_CLOSE_DELAY=-1
spring.datasource.username=sa
spring.datasource.password=sa
```

#### public class Todo

```
@Size(min=10, message="Enter at least 10 Characters...")
@Column(name="description")
private String desc;


- Connect Spring Boot Todo Web App to MySQL on Local

![Browser](Images/Screenshot_20.png)


```docker

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/02-todo-web-application-h2 (master)
$ docker run mysql:5.7
Unable to find image 'mysql:5.7' locally
5.7: Pulling from library/mysql
4be315f6562f: Pulling fs layer
96e2eb237a1b: Pulling fs layer
8aa3ac85066b: Pulling fs layer
ac7e524f6c89: Pulling fs layer
f6a88631064f: Pulling fs layer
15bb3ec3ff50: Pulling fs layer
ae65dc337dcb: Pulling fs layer
a4c4c43adf52: Pulling fs layer
c6cab33e8f91: Pulling fs layer
2e1c4f2c43f6: Pulling fs layer
2e5ee322af48: Pulling fs layer
ae65dc337dcb: Waiting
a4c4c43adf52: Waiting
c6cab33e8f91: Waiting
2e1c4f2c43f6: Waiting
2e5ee322af48: Waiting
ac7e524f6c89: Waiting
f6a88631064f: Waiting
15bb3ec3ff50: Waiting
96e2eb237a1b: Verifying Checksum
96e2eb237a1b: Download complete
ac7e524f6c89: Verifying Checksum
ac7e524f6c89: Download complete
f6a88631064f: Verifying Checksum
f6a88631064f: Download complete
8aa3ac85066b: Download complete
ae65dc337dcb: Verifying Checksum
ae65dc337dcb: Download complete
a4c4c43adf52: Verifying Checksum
a4c4c43adf52: Download complete
15bb3ec3ff50: Verifying Checksum
15bb3ec3ff50: Download complete
2e1c4f2c43f6: Verifying Checksum
2e1c4f2c43f6: Download complete
2e5ee322af48: Verifying Checksum
2e5ee322af48: Download complete
4be315f6562f: Verifying Checksum
4be315f6562f: Download complete
4be315f6562f: Pull complete
96e2eb237a1b: Pull complete
8aa3ac85066b: Pull complete
ac7e524f6c89: Pull complete
f6a88631064f: Pull complete
15bb3ec3ff50: Pull complete
ae65dc337dcb: Pull complete
a4c4c43adf52: Pull complete
c6cab33e8f91: Verifying Checksum
c6cab33e8f91: Download complete
c6cab33e8f91: Pull complete
2e1c4f2c43f6: Pull complete
2e5ee322af48: Pull complete
Digest: sha256:e767595ba3408fbb2dda493be3594b9a148178df58325fafe8b0363662935624
Status: Downloaded newer image for mysql:5.7
2022-05-07 08:11:11+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.38-1debian10 started.
2022-05-07 08:11:11+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2022-05-07 08:11:11+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.38-1debian10 started.
2022-05-07 08:11:11+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
    You need to specify one of the following:
    - MYSQL_ROOT_PASSWORD
    - MYSQL_ALLOW_EMPTY_PASSWORD
    - MYSQL_RANDOM_ROOT_PASSWORD

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/02-todo-web-application-h2 (master)
$ docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 mysql:5.7
018b5938c5b4d4026ef234ea960a53a85746c2a1da00853b0e82710351cdb9a3

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/02-todo-web-application-h2 (master)
$ docker container ls
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                               NAMES
018b5938c5b4   mysql:5.7   "docker-entrypoint.s…"   12 seconds ago   Up 11 seconds   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql

user@DESKTOP-AS2FQOH MINGW64 /c/D_Drive/DXC/Learning/Projects/jd-docker-crash-course/docker-crash-course-master/02-todo-web-application-h2 (master)


```
### Launching MySQL using Docker

```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 mysql:5.7
```

Using Custom Network

```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 --network=web-application-mysql-network mysql:5.7
```

Using a Volume

```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 --network=web-application-mysql-network --volume mysql-database-volume:/var/lib/mysql  mysql:5.7
```

```sql
Name:  todo_DBmySQL 
Host: 127.0.0.1 
Port: 3306 
Login User: todos-user
Password: todos
```

### Install MySQL workbench and connect using above DB details

![Browser](Images/Screenshot_21.png)


### Create Todo Table for Production

```sql
create table hibernate_sequence (next_val bigint) engine=InnoDB
insert into hibernate_sequence values ( 1 )
create table todo (id integer not null, description varchar(255), is_done bit not null, target_date datetime(6), user varchar(255), primary key (id)) engine=InnoDB
```

![Browser](Images/Screenshot_22.png)


---



