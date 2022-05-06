---
title: "Microservice, good service with loose coupling and high cohesion"
series: "microservices"
categories: "notes"
summary: "How to design a good service in a microservices with loose coupling and high cohesion"
date: 2022-05-06T21:00:00+01:00
draft: false
socialImage: "https://thuc.space/images/microservices/microservices-good-service.png"
tags:
- microservices
---

# Good service in microservices

We talk about microservices, we talk about how services communicate, and we talk about scaling, and fault isolation. But how good service, the atomic element, should be in the microservices system?

Let's take a look at 2 main attributes that help to build a good service in a microservices system:

## Loose Coupling

In microservices, a service should be independent. It doesn't require other services to start up or survives. Building and deployment process should be independent of other services.

From the code level, if a service changes the design, implementation, or behaviour; other services shouldn't change them.

Most of the mistakes to make in service coupling are using a shared database, code sharing, and synchronous communication without a circuit breaker. 

## High Cohesion

Ideally, related behaviours should be together in a service. A service or module with high cohesion contains elements that are tightly related to each other and united in their purpose.

If we want to change a behaviour in the system, we should change it in one place. It will take time to deploy, and is riskier and more affected if it requires in many places.

The ideally is inherited from the single responsibility principle, that a service should be lived to do one job and change for one reason.
