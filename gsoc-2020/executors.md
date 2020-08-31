---
layout: page
title: Google Summer of Code 2020 - Unified API for Algorithms
short: GSoC 2020 Executors
permalink: /gsoc-2020/executors/
---

Executors have been a long-standing proposal under discussion and review by the C++ Standard Committee. The earliest proposal dates back to 2012 and was initially proposed by Google based on some concepts that were being used internally by them. Since then, the design has gone through multiple iterations and has evolved into something completely different, but the fundamental has remained the same: to provide a mechanism to have control over the execution of a function.
A fascinating analogy was provided in one of the early design documents by Christopher Kohlhoff: "An executor is to function execution as an allocator is to allocation."

This post is in no way a technical document describing executors or their implementation in PCL.  For that, there are numerous proposals (over 30!) that can be found [here](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2020/p0443r13.html). For details regarding PCL's implementation, you can read the design document or go through the Code API. 

To summarize, executors aim to allow programmers to control the where and how functions are executed. In PCL, there was a need for a standardized way to run algorithms on facilities like OpenMP, SIMD & CUDA while also supporting any future facilities like thread pools. All of this pointed to executors. Though, only a limited and specific set of features were needed in PCL since asynchronous implementations are not available. Another factor that influenced a simpler design is that most PCL users would not care about having fine-grained control over the execution of an algorithm. At the most, they would need to control on which facility the algorithm runs.
These influenced the design to be a simple yet offer all the customizations a user could need.

The design chosen was a template-based design which offered advantages such as no runtime overhead, errors related to usage and properties of executors are caught during compilation. Finally, it offered more flexibility in terms of being able to evolve the design rapidly based on feedback without breaking code existing executor compatible code. Extra effort was put into trying to follow the proposal as closely as possible to minimize refactoring within PCL so that when it is finally accepted into the standard library, it can be used in PCL as is.
This posed a challenge, going deep into template metaprogramming, especially to a person like me who had to search what metaprogramming is and being surprised that such a concept and was Turing complete nevertheless! Since then, I have come a long way and am more comfortable with template metaprogramming and find it a playful challenge to be able to apply this technique in places people generally might not think of using it. (Looking forward to the metaclasses proposal in C++ now).

The second challenge that I faced was reading through the relevant proposals and trying to interpret their meaning. The proposal uses quite a technical language, which is not easy to understand if you are not familiar with it. Since each proposal builds on the previous ones with new ideas being introduced and features being removed, this involved spending considerable time going through all the relevant proposals. This took multiple iterations of reading and trying to make sense of the concepts mentioned as the documents were the primary source of understanding and some discussions and videos. A lot of concepts I was not well versed with were used, so I spend a considerable amount of time in the browser searching stuff (no wonder my Chrome screen time was off the roof).

This altered my goals quite a bit from my original proposal, where I had envisioned spending a notable amount of time adding support for other facilities and integrating executors into various algorithms. I had not expected so much time in the design phase for executors. There were many discussions, ideas, and design iterations with my mentors as executors could be fundamental to how PCL handled execution internally. So it was essential to get the fundamentals right and not come up with something that would have to be scrapped or redesigned from the ground up after a short amount of time. Therefore, the timeline had to be modified, and more time was spent on the design of executors themselves in PCL.

You can see the development cycle in the [unified-executors](https://github.com/shrijitsingh99/unified-executors/) repo.

The PR's related to executors in PCL are:
[#4369](https://github.com/PointCloudLibrary/pcl/pull/4369)
[#4370](https://github.com/PointCloudLibrary/pcl/pull/4370)
[#4371](https://github.com/PointCloudLibrary/pcl/pull/4371)


