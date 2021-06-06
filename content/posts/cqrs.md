---
title: "CQRS"
series: "architect"
categories: "notes"
summary: "Command Query Responsibility Segregation"
date: 2021-06-06T09:00:00+01:00
draft: false
tags:
- architect
- webscale
---

**Summary**

 - Seperate model of query and command
 - Query model includes actions such as reading, filtering, searching data. Generally, it returns result without change anything from data.
 - Command model includes actions such as save, delete, login, other actions. Generally, it change the state of the data.
 - Data and business of query model and command model can be different depends on usage's requirement

**CQRS single service**

![CQRS single service](/images/architect/cqrs/cqrs-Single.png)

**CQRS multi-services**

![CQRS multi-services](/images/architect/cqrs/cqrs-Multi.png)
