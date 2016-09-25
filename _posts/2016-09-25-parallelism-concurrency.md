---
layout: post
title: Parallelism vs. Concurrency
description: "Differences between parallelism and concurrency"
tags: [parallelism]
image:
    feature: Concurrency_vs_Parallelism.png
    credit: adapted from raywenderlich.com
    creditlink: https://www.raywenderlich.com/60749/grand-central-dispatch-in-depth-part-1
---

Recently I looked into parallelizing my code and came across 2 terms which are often used interchangeably but do not mean the same: concurrency and parallelism. 
Both concurrency and parallelism perform tasks virtually at the ‘same time’ since tasks share the same time space. 
However, this is achieved fundamentally differently. In fact, concurrency can be achieved on a single processor while parallelism requires at least 2. This is because concurrency interleaves 
multiple tasks, while parallelism divides a single task into multiples which are in turn initiated and ended at the same time.

<!-- more -->

A nice analogy which I came across from a [quora user](https://www.quora.com/profile/Vicky-Katara), is when you are assigned the task to build 2
parallel walls starting from a pile of bricks at one end. Option 1 is to build one wall, go back to the pile, and build the second one – this is 
typical sequential processing. Option 2 is to alternately lay a brick for each wall – the two walls are being built within the same time space i.e. 
concurrently. Option 3 is to ask another worker and build the walls at the same time – parallel processing. This explains why multiple processing 
units are required for parallel processing. 

<figure>
	<img src="/images/parallel_sequential_concurrent.jpg" alt="">
</figure>

An additional way of looking at concurrency and parallelism is the number and type of tasks (as explained [here](http://tutorials.jenkov.com/java-concurrency/concurrency-vs-parallelism.html)). 
Concurrency concerns handling of multiple tasks (e.g. IO drivers in the kernel) and is used for the multi-tasking of operating systems, while parallelism concerns handling an individual task by splitting it into multiple subtasks
(e.g. performing simple arithmetic functions on a set of vectors, where they are split into individual elements and processed separately/in parallel). Thus, an application can be concurrent but not parallel in the sense that multiple tasks are run at the same time but not split into 
multiple subtasks. An application can be parallel but not concurrent, meaning that a task is split 
into multiple subtasks and processed at the same time, but multiple tasks are not handled. An application can be both concurrent and parallelized, where multiple tasks are processed and each 
task is subdivided into subtasks. 

Rob Pike (Unix, Golang, UTF-8 and Plan 9 software engineer) explains this well in this video:

{::nomarkdown}
<iframe width="1280" height="720" src="https://www.youtube.com/embed/B9lP-E4J_lc" frameborder="0" allowfullscreen></iframe>
{:/nomarkdown}

