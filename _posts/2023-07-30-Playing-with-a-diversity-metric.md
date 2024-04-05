---
layout: post
title: "Playing with a diversity metric"
date: 2023-07-30 11-51-59 +0100
category: 
tags: 
---

I have been thinking a bit lately about active learning methods and in particular about metrics used to compare such methods. 

If we have a set $B$, a reference set $S$, and a metric $d$, $S\cap B=\emptyset$, we can define a diversity metric as 

$$
D(S;B)=\frac{1}{|B|}\sum_{x\in B} \min_{y\in S} d(x,y).
$$


TO BE CONTINUED

