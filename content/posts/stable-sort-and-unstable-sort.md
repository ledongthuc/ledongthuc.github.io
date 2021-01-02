---
title: "Stable sort - Unstable sort"
series: "algorithm"
categories: "notes"
summary: "Quick notes what's stable sort and unstable sort"
date: 2021-01-02T21:00:00+01:00
draft: false
tags:
- algorithm
- sort
---

 - Stable sort is a sorting algorithm with equal keys appear in same order in output as they appear in unsorted input.
 - https://play.golang.org/p/qs2bJ8RDWeR

```
┌─┐ ┌─┐ ┌─┐ ┌─┐ ┌─┐
│2│ │1│ │3│ │3│ │4│
└─┘ └─┘ └┬┘ └┬┘ └─┘
         │   │     
         │   │     
┌─┐ ┌─┐ ┌▼┐ ┌▼┐ ┌─┐
│1│ │2│ │3│ │3│ │4│
└─┘ └─┘ └─┘ └─┘ └─┘
```

 - Unstable sort is a sorting algorithm with equal keys not guaranteed same order in output as they appear in unsorted input. 
 - https://play.golang.org/p/du-hKwguoC3

```
           ┌─┐     
           │ │     
           │ │     
┌─┐ ┌─┐ ┌─┐│┌─┐ ┌─┐
│2│ │1│ │3│││3│ │4│
└─┘ └─┘ └─┘│└─┘ └─┘
         │ │       
         └─│─┐     
┌─┐ ┌─┐ ┌─┐│┌▼┐ ┌─┐
│1│ │2│ │3│││3│ │4│
└─┘ └─┘ └─┘│└─┘ └─┘
         ▲ │       
         │ │       
         └─┘       
```

 - Equal keys: a term determines 2 elements in input as understand as same rank
