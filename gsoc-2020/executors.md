---
layout: page
title: Google Summer of Code 2020 - Unified API for Algorithms
short: GSoC 2020 Executors
permalink: /gsoc-2020/executors/
---

Executors have been a long-standing proposal under discussion and review by the C++ Standard Committee. The earliest proposal dates back to 2012 and was initially proposed by Google based on some concepts that were being used internally by them. Since then, the design has gone through multiple iterations and has evolved into something completely different, but the fundamental has remained the same: provide a mechanism to have control over the execution of a function.
A fascinating analogy was provided in one of the early design documents by Christopher Kohlhoff: "An executor is to function execution as an allocator is to allocation."

This post is in no way a technical document describing executors or their implementation in PCL.  For that, there are numerous proposals (over 30!) that can be found [here](http://wg21.link/P0443R13). For details regarding PCL's implementation, you can read the [design document](https://pcl.readthedocs.io/projects/tutorials/en/master/executor_design.html) or go through the Code API. 

To summarize, executors aim to allow programmers to control where and how functions are executed. In PCL, there was a need for a standardized way to run algorithms on facilities like OpenMP, SIMD & CUDA while also supporting any future facilities like thread pools. All of this pointed to executors. In PCL, only a limited and specific set of features were needed as there were no plans to add asynchronous operations and task parallelism support to the library in the near future. Another factor that influenced a simpler design is that most PCL users would not care about having fine-grained control over the execution of an algorithm. At most, they would need to control on which facility the algorithm runs. These factors influenced the design to be simple yet offer all the customizations a user could need.

The design chosen was a template-based design which offered advantages such as no runtime overhead and offers static checks related to usage and properties of executors. Finally, it provided more flexibility in terms of being able to evolve the design rapidly based on feedback, without breaking code existing executor compatible code. Extra effort was put into trying to follow the proposal as closely as possible while minimizing refactoring within PCL, so that when it is finally accepted into the standard library, it can be used in PCL as is.

Going deep into template metaprogramming posed a challenge, especially to a person like me who had to search what metaprogramming is. I was surprised to discover that not only metaprogramming "is a thing", but it is also Turing complete! Since then, I have come a long way and I am more comfortable with template metaprogramming. I find it a playful challenge to be able to apply this technique in places people generally might not think of using it. (Looking forward to the metaclasses proposal in C++ now). The second challenge faced was reading through the relevant proposals and trying to interpret their meaning. The proposals use very particular style of technical writing, which is not easy to understand if you are not familiar with it. Since each proposal builds on the previous ones, with new ideas being introduced and features being removed, going through them consumed considerable time. It took multiple iterations of reading while trying to make sense of the concepts mentioned, using the documents as the primary information source and complementing them with discussions and videos. I was not well versed with a lot of the concepts used, so I spent a considerable amount of time looking them up.

This heavily altered my original proposal's goals, where I had envisioned spending a notable amount of time adding support for other facilities and integrating executors into various algorithms. I did not expect spending so much time in the design phase for executors. There were many discussions, ideas and design iterations with my mentors, such as how executors could be fundamental to the way PCL handled execution internally. So it was essential to get the foundations right and not come up with something that would have been scrapped or redesigned from the ground up after a short amount of time. Therefore, the timeline had to be modified, and more time was spent on the design of executors themselves in PCL.

You can see the development cycle in the [unified-executors](https://github.com/shrijitsingh99/unified-executors/) repo.

The PR's related to executors in PCL are:
[#4369](https://github.com/PointCloudLibrary/pcl/pull/4369),
[#4370](https://github.com/PointCloudLibrary/pcl/pull/4370),
[#4371](https://github.com/PointCloudLibrary/pcl/pull/4371)

As part of my GSoC, I also worked on improving CI infrastructure
and refactoring parts of the codebase for easier integration with
executors.  
CI: [#4131](https://github.com/PointCloudLibrary/pcl/pull/4131),
[#4130](https://github.com/PointCloudLibrary/pcl/pull/4130),
[#4101](https://github.com/PointCloudLibrary/pcl/pull/4101)
| Pre-GSOC [#3886](https://github.com/PointCloudLibrary/pcl/pull/3886),
[#3795](https://github.com/PointCloudLibrary/pcl/pull/3795),
[#3789](https://github.com/PointCloudLibrary/pcl/pull/3789),
[#3783](https://github.com/PointCloudLibrary/pcl/pull/3783),
[#3745](https://github.com/PointCloudLibrary/pcl/pull/3745),
[#4131](https://github.com/PointCloudLibrary/pcl/pull/4131)

Refactoring (In Progress): [#4287](https://github.com/PointCloudLibrary/pcl/pull/4287)
[#4281](https://github.com/PointCloudLibrary/pcl/pull/4281),
[#4279](https://github.com/PointCloudLibrary/pcl/pull/4279),
[#4628](https://github.com/PointCloudLibrary/pcl/pull/4268)


This project has been a great learning experience. There was a learning curve, but I was able to go deep into the C++ language itself and become familiar with some handy tools and libraries that I would not have come across on my own under normal circumstances. All the hours I put into GSoC have definitely been worth it. This is, in large part, to the fantastic mentors I had. The amount of hours they themselves have put into this project in reviewing my code, helping build prototypes of features, teaching various language tricks and concepts, and in general, just guiding me throughout the project has helped make this experience worthwhile in itself.

Kudos to my mentors @kunaltyagi and @SergioRAgostinho.
Shoutouts to @aPonza  and @larshg for doing a fabulous job mentoring other projects as well.
