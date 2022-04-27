---
title: "Microservices - Advantages and Disadvantages"
series: "microservices"
categories: "notes"
summary: "Listing why we choose or do not choose microservices based on advantages and disadvantages"
date: 2022-04-27T18:00:00+00:00
draft: false
socialImage: "https://thuc.space/images/microservices/microservices-advantages-and-disadvantages.png"
tags:
- microservices
---

# Advantages

## Technology Heterogeneity

In a monolithic system, all source code is in one place. It's hard to change the library, tech stack or database. So, technology picking becomes a common denominator of problems that need to solve in the system.

In microservices, we can choose different technology with their strongest to solve the exact problem that the service needs to develop. It's the idea about the right tool for the right job. Another small benefit, a team easier to apply new tech while services are small in size and easier to rewrite if something goes the wrong way.

## Resilience

In a monolithic system, when the service fails, all functions and flows crash. Everything stops working.

In microservices, if one service fails, it only affects one or few flows of the system. Other flows still work.

## Scaling

In a monolithic system, if one small part of the system got performance issues, we need to scale the whole system. It takes upgrading resources for the part that doesn't get performance issues.

In Microservices, we only scale what needs to scale. It keeps systems smaller, with less powerful hardware but the same performance.

## Deployment

In a monolithic system, one line code change trigger to build and deploy whole system code to release it. That leads to higher impact, higher risk and higher effort for verifying the whole system. And finally, it ends up we can't release features frequently because of time-consuming and issue fearing.

In microservices, we only build and deploy a service that actually changes. Deployment process is also faster, and more isolated. If an issue occurs, we also easier to scope the problem and roll back.

## Organizational Alignment

In microservices, it's easier to organize teams and resources for service. It's easier to shift the service's ownership to a team.

## Composability

The microservices open up opportunities for reusable service functionality. A service is reused in multiple solutions that are themselves made up of composing services.

It also opens a room for open source or outsource services that run in the systems to solve different problems.

## Optimizing for Replaceability

In a monolithic system, we easily discover legacy part of system that no-one undestands, or no-one really want to touch for update. It's risky when change the code that whole system can be affected.

In microservices, small size service give opportunity to re-write a service in short time with lower risk and effective cost for testing.

# Disadvantages

## Communication failure

The communication in microservices is mainly based on the network. So, we have more chance of network problems between services compare to a monolithic system.

## Number of services

A number of services give microservices a lot of benefits, but it also brings up a few new problems. Managing, healing, and monitoring the number of services takes more time and effort than a monolithic system.

## Issue tracing

Debugging problems becomes more challenging with microservices. More logs, more communication, and more error point to check than a monolithic system.

## Complex testing

The integration and e2e testing take more effort to cover a large number of services and their communication.
