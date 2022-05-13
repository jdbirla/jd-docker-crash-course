# JD Java Spring Boot Getting started with CCS and CES Microservices on Docker

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



