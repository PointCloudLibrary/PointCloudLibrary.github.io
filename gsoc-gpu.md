---
layout: page
title: Google Summer of Code 2020 - Refactoring, Modernisation & Feature Addition with Emphasis on GPU Module
short: GSoC 2020 GPU & Refactoring
permalink: /gsoc-gpu/
---

<img src="{{ '/assets/images/gsoc-2020/gpu_approx.png' | relative_url }}" alt="approx-search" width="200" class="center" />
<img src="{{ '/assets/images/gsoc-2020/gpu_radius.png' | relative_url }}" alt="radius-search" width="200" class="center" />
<img src="{{ '/assets/images/gsoc-2020/gpu_knn.png' | relative_url }}" alt="knn-search" width="200" class="center" />

**Student:** [Haritha Jayasinghe][haritha]

**Mentors:** [Sérgio Agostinho][sergio], [Lars Glud][lars]


## Modernising the GPU Octree module

Octrees are specialized data structures with nodes that split into eight subchildren, and are a widely used structure when working with point clouds, since they can be used to efficiently perform many operations. The PCL GPU module is a crucial yet somewhat overlooked component of PCL, and is implemented using explicit warp-level programming in order to maximize performance.

In many cases the GPU algorithms can execute tasks magnitudes faster than it’s CPU counterpart, which can be crucial when working with large point clouds. Unfortunately the GPU API is quite limited and often lacks much of the functionality offered in the equivalent CPU algorithms. The primary aim of this task was to bridge these inconsistencies.

While the initial plan was not to spend an extensive amount of time on the GPU octree module, upon closer inspection it was discovered that there were many irregularities and errors within the GPU octree module. Specifically, two of the three primary methods offered by the GPU octree module, namely K Nearest Neighbours search and Approximate Nearest Neighbors search were both returning incorrect results while one of the implementations of the other remaining module (Radius Search) was also returning incorrect results.

In addition these functions were utilizing outdated CUDA primitives which could easily cause further bugs in the future. When diving into the code, it was also discovered that the GPU approximate nearest neighbours algorithm used a completely different traversal methodology from it's CPU counterpart.

Due to these discoveries,  the scope of the GPU modernization effort was expanded and prioritized over some of the other goals and thus a majority of the internship period was spent on addressing this task.

### Fixes to Octree search methods and modernizing CUDA functions

Related PRs -
https://github.com/PointCloudLibrary/pcl/pull/4146
https://github.com/PointCloudLibrary/pcl/pull/4313
https://github.com/PointCloudLibrary/pcl/pull/4306

After comprehensively going through the GPU search methods to Investigate their functionality and the causes of the above issues, it was determined that there were two separate bugs which were responsible for the above issues.

In approximate nearest search and K nearest search, an outdated method was being utilized to synchronize data between threads in order to sort distances across warp threads. This was fixed by replacing the functionality with warp level primitives introduced in CUDA 9.0 detailed in https://developer.nvidia.com/blog/using-cuda-warp-level-primitives/ .
In radius search, radius was not shared between warp threads, thus the search was being conducted for incorrect radius values. Synchronizing the radius values across the threads fixed this issue.

Since much of the code inside the above functions utilized an outdated concept of using volatile memory for sharing data between threads, they were also replaced by utilizing warp primitives to synchronize thread data.

### Implementation of new traversal mechanism of approximate nearest search

Related PRs -
https://github.com/PointCloudLibrary/pcl/pull/4294

The existing implementation of approximate nearest search utilized a simple traversal mechanism which traversed down octree nodes until an empty node is found. Once an empty node is discovered, all points within the parent are searched exhaustively for the closest point. However the CPU counterpart of the approximate nearest search algorithm uses a heuristic (distance from query point to voxel center) to determine the most appropriate voxel to traverse, in case an empty node is discovered. Thus this algorithm will always traverse to the lowest level of an octree. The same traversal method was adapted to the morton code based octree traversal mechanism and implemented for the two GPU approximate nearest search methods.

In addition a new test was designed to assess the functionality of the new traversal mechanism and to ensure that it tallies with that of CPU approximate nearest search.

### Modifying search functions to return square distances

Related PRs -
https://github.com/PointCloudLibrary/pcl/pull/4340
https://github.com/PointCloudLibrary/pcl/pull/4338

One noticeable flaw in the current GPU search implementations was the inability to return square distances to the identified result points. In order to counter this, the search methods were modified to keep track of and return the distances to the identified results. For Approximate nearest search and K nearest search this was relatively easy, and did not incur a time penalty.

However this was not the case for Radius search, as any octree node that was located within the search radius from the search point was automatically added to the results without performing a distance calculation. Thus a new kernel was introduced to efficiently perform distance computations for points located within these nodes. Due to the additional penalty of performing these computations (benchmark pending) it was decided to preserve the existing radius search methods as well, without deprecation.

Additional tests were added to ensure the accuracy of all returned distances.

### Addition of a GPU octree tutorial
<TODO link to tutorial>

This tutorial aims to provide users an in depth overview of the functionality offered by the modernized GPU octree module. It consists of;
An introduction to the GPU octree module and search functions
Differences between the CPU and GPU implementations
Instructions on when to use the GPU module
Code samples detailing the use of
	Radius Search
	Approximate nearest search
	K nearest search
Visualizations depicting the functionality of above functions

### Other work

Related PRs -
https://github.com/larshg/pcl/pull/8
https://github.com/larshg/pcl/pull/7

Some of the other work carried out to support the modernization of the GPU octree module include;
Addition of a CI job to ensure compilation of the GPU module
Adding GPU octree tests to the main test module

Furthermore, there were some noticeable instances of code repetition which reduced the manageability of the code and these methods were cleaned and code that implemented similar functionality was refactored.

In addition the GPU module contains “host” methods for approximate nearest search and radius search, which are essentially methods which are synchronous versions of the GPU code which do not use CUDA kernels (i.e. these methods run purely on CPU). Of these, it was decided that the GPU approximate nearest search method could be deprecated as it provides no significant advantage over the traditional CPU approximate nearest search function, however since initial investigations suggest that the host radius search function offers noticeably faster performance over the traditional CPU radius search function, the deprecation of this method has been put off untill comprehensive tests are carried out to investigate this behaviour.

### Future work

One additional drawback with the current GPU K nearest neighbour search algorithm is that it is currently restricted to k = 1 only. A review of the current code base makes it clear that significant modifications and additions are required to both the traversal mechanism as well as the distance computation kernel in order to extend support for any arbitrary K. This provides an interesting challenge to be tackled in the future, and which would bring additional value to the GPU octree module.

## Introducing flexible type for indices

## Summary

The internship period was focused on tackling “modernization of the GPU octree module” and “Introducing flexible types for indices”. The scope of these tasks were initially under-estimated in the original proposal, and additional requirements were discovered, which resulted in skipping some of the other goals in favour of prioritizing these goals.

The work carried out during the period ensure that the GPU octree search functions now produce accurate results, are verified by tests, adheres to modern CUDA standards, tallies in most cases with their CPU counterpart, and provides users an in depth overview of their usage and functionality, and sets up the groundwork needed for a full transition to flexible types for point indices to ensure that PCL meets the growing needs of its community.
