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





