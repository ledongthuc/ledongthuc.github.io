---
title: "Five rules to handle errors in Go?"
categories: "tldr"
summary: "Handle error in Golang with five rules to make it efficiently."
date: 2020-04-23T08:00:00+01:00
source: https://web3.coach/golang-how-to-handle-errors-five-rules
author: web3.coach
draft: false
---

Handle error in Golang's always annoying. Author of the post give five rules to make it efficiently.

 - #1 - Don't ignore the error.
 - #2 - Return early.
 - #3 - Return value or Error (but not both).
 - #4 - Log or Return.
 - #5 - Configure an `if err != nil` as a macro.

Short post but they are practicle and effective.