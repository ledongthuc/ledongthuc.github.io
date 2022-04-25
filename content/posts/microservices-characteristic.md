---
title: "Microservices and characteristic"
series: "microservices"
categories: "notes"
summary: "Microservices basic information "
date: 2022-04-24T21:00:00+01:00
draft: false
socialImage: "https://thuc.space/images/microservices/microservices-characteristic.png"
tags:
- microservices
---

# What are microservices?

Microservices are architectural style to design a system. They are small services that are independent and work together to build the system.

# Why small service?

Big service, or monolithic system design easily creates complexity, and abstraction and depends on  different logic.

Microservices recommend service should be small, simple and solve one thing well. It's the same idea as the single responsibility principle.

# How small is small enough?

As we knew, we should keep services are small. But if they were too small, the communication would be more complex.

A lot of ideas about how to measure a service small enough:
 1. Keep a service as tiny as it can and can't break it smaller.
 2. Keep a service small enough can to be re-write in a specific time (e.g. 2 weeks)
 3. Keep a service only containing things that change for the same reason. Or one domain per service.
 4. Keep service can maintain by the smallest team in company org. And the smallest team size in a company is flexible depending on the company organization.

In my personal opinion, (3) and (4) are what I recommend to decide how a service is small enough.

# How are services independent?

Services should be isolated from technology, deployment flow, execution process and operation system and code change side effect. It avoids services side-effects from development to deployment and operation.

Technology in services should be independent too. The service's tech reason should be decided by how good it resolves the problem instead of other service dependencies. It provides services develop with their strongest reason.

# How do services work together?

All communication between services is recommended through network calls that keep services independent from machine, OS, and file system.

The communication shouldn't be OS interface, terminal command, named pipe, or no-network shared memory.
