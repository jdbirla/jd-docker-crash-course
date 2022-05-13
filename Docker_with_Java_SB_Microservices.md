# JD Java Spring Boot Getting started with CCS and CES Microservices on Docker

## section 8: Docker container run for microservices
 
## What You Will Learn during this Step 01:
-  Introduction to Microservices

---

## What You Will Learn during this Step 02:
-  Advantages of Microservices

---

## What You Will Learn during this Step 03:
-  Understanding Docker and Microservices - An Amazing Combo

---
## What You Will Learn during this Step 04:
-  Overview of CCS and CES Spring Boot Microservices

---
## What You Will Learn during this Step 05:
-  Create Docker Images and Containers for CCS and CES Microservices

```
docker network create currency-network
docker run -p 8000:8000 --network=currency-network --name=currency-exchange-service -d jbirla/currency-exchange-service:0.0.1-SNAPSHOT
docker run -p 8100:8100 --network=currency-network --name=currency-conversion-service --env CURRENCY_EXCHANGE_URI=http://currency-exchange-service:8000 -d jbirla/currency-conversion-service:0.0.1-SNAPSHOT

```
![Browser](Images/Screenshot_33.png)

![Browser](Images/Screenshot_34.png)


---
## What You Will Learn during this Step 06:

- Run CCS and CES Microservices using Docker Compose

### C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\05-microservices\docker-compose.yml
```
version: '3.7'

services:
  currency-exchange-service:
    image: jbirla/currency-exchange-service:0.0.1-SNAPSHOT
    ports:
      - "8000:8000"
    restart: always
    networks:
      - currency-compose-network

  currency-conversion-service:
    image: jbirla/currency-conversion-service:0.0.1-SNAPSHOT
    ports:
      - "8100:8100"
    restart: always
    environment:
      CURRENCY_EXCHANGE_URI: http://currency-exchange-service:8000
    depends_on:
      - currency-exchange-service
    networks:
      - currency-compose-network
  
# Networks to be created to facilitate communication between containers
networks:
  currency-compose-network:

```
```
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\05-microservices> docker-compose up
```
---
## section 9: Docker to integrate microservices with Eureka namming server

## What You Will Learn during this Step 01:
-  Understanding the need for Service Registry
---

## What You Will Learn during this Step 02:
-  Create Docker Images for Eureka Naming Server

* /07-netflix-eureka-naming-server/pom.xml adding below plugin
```
	<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>2.19.1</version>
				<configuration>
					<testFailureIgnore>true</testFailureIgnore>
				</configuration>
			</plugin>
```

### C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\05-microservices\docker-compose.yml
```
version: '3.7'

services:

  naming-server:
    image: jbirla/netflix-eureka-naming-server:0.0.1-SNAPSHOT
    ports:
      - "8761:8761"
    restart: always
    networks:
      - currency-compose-network
      
  currency-exchange-service:
    image: jbirla/currency-exchange-service:0.0.1-SNAPSHOT
    ports:
      - "8000:8000"
    restart: always
    depends_on:
      - naming-server
    networks:
      - currency-compose-network

  currency-conversion-service:
    image: jbirla/currency-conversion-service:0.0.1-SNAPSHOT
    ports:
      - "8100:8100"
    restart: always
    environment:
      CURRENCY_EXCHANGE_URI: http://currency-exchange-service:8000
    depends_on:
      - currency-exchange-service
      - naming-server
    networks:
      - currency-compose-network
  
# Networks to be created to facilitate communication between containers
networks:
  currency-compose-network:
```
---

## What You Will Learn during this Step 03:
-  Configure and Run CES and CCS Microservices with Eureka Service

![Browser](Images/Screenshot_35.png)

```
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\05-microservices> docker-compose up -d
Creating 05-microservices_naming-server_1 ... done
Recreating 05-microservices_currency-exchange-service_1 ... done
Recreating 05-microservices_currency-conversion-service_1 ... done
PS C:\D_Drive\DXC\Learning\Projects\jd-docker-crash-course\docker-crash-course-master\05-microservices>
```

---
## section 10: Docker to integrate microservices with Zuul API Gateway

## What You Will Learn during this Step 01:
- Configure CES and CCS Microservices with Zuul API Gateway

---

## What You Will Learn during this Step 01:
- Use Docker Compose to Run Microservices with Zuul

```
version: '3.7'

services:

  naming-server:
    image: jbirla/netflix-eureka-naming-server:0.0.1-SNAPSHOT
    ports:
      - "8761:8761"
    restart: always
    networks:
      - currency-compose-network
      
  zuul-api-gateway:
    image: jbirla/netflix-zuul-api-gateway-server:0.0.1-SNAPSHOT
    build:
      context: netflix-zuul-api-gateway-server
      dockerfile: Dockerfile
    environment:
      RABBIT_URI: amqp://guest:guest@rabbitmq:5672
    ports:
      - "8765:8765"
    restart: always
    depends_on:
      - naming-server
    networks:
      - currency-compose-network

      
  currency-exchange-service:
    image: jbirla/currency-exchange-service:0.0.1-SNAPSHOT
    ports:
      - "8000:8000"
    restart: always
    depends_on:
      - naming-server
    networks:
      - currency-compose-network

  currency-conversion-service:
    image: jbirla/currency-conversion-service:0.0.1-SNAPSHOT
    ports:
      - "8100:8100"
    restart: always
    environment:
      CURRENCY_EXCHANGE_URI: http://currency-exchange-service:8000
    depends_on:
      - currency-exchange-service
      - naming-server
    networks:
      - currency-compose-network
  
# Networks to be created to facilitate communication between containers
networks:
  currency-compose-network:

```

---

## section 11: Distributed Tracing with Zipkin and RabbitMq

## What You Will Learn during this Step 01:
- Introduction to Zipkin and Update Microservices to Connect to Zipkin



