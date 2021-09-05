## Overview

### Description

Student: [Hang Yan][ueqri]

Mentors: [Aaryaman Vasishta][jammm], [Lars Glud][larshg], [Haritha Jayasinghe][haritha-j]

### Background

The increasing parallelism brought by CUDA and GPU libraries benefits PCL as well as other open-source communities greatly. However, the test for these GPU-accelerated code is a bit awkward on those common CI without free GPU. Therefore, building a generic tool to allow CPU to run CUDA without a GPU is inspiring and valuable. And integrating this tool into the CI of PCL will bring more convenience and confidence to CI build.

### Deliverables

- A pipeline based on AMD HIP Toolkits, which allows CPU to run CUDA codes
- Improved CI build which enables multiple pipeline with better extendibility
- Investigation on various possibilities and test those methods

### Benefits to Community

Although there is still no complete solution of enabling CUDA build on CI to run GPU unit tests given for PCL community, we implemented a simple pipeline framework based on the proposal, and also found many problems with the upstream toolkits, i.e. HIP-CPU. Throughout the project, the bugs were reported and we made an effort to fix them in PR.

In addition, we did comprehensive and relatively exhaustive [investigations](gsoc-2021/gpu-report.md#investigations), tried many other approaches such as GPGPU-Sim and cloud GPU for nonprofit, and attached those possible solutions in this report.

When HIP toolkits like HIP-CPU fix those API missing problems and enhance the robustness, or any possibilities provided in Section Investigations have relevant and helpful updates, the pipeline could be reused and integrated with PCL.

## Milestones

- Build a pipeline combined HIP toolkits in a container: [GitHub Repository](https://github.com/ueqri/cuda2hipcpu) [Azure Pipeline](https://dev.azure.com/ueqri-ci/cuda2hipcpu/_build)
- [Build a script to check HIPIFY compatibility check](https://github.com/ueqri/hip-compatibility-check)
- [Enhance PCL CI for more extendibility](https://github.com/PointCloudLibrary/pcl/pull/4737)
- [Fuse PCL docker file with HIP toolkits](https://gist.github.com/ueqri/5fa9b0e5321bc32475f173ddcebccdc0)
- [Apply transformation tool to gpu folder, and get checked codebase](https://github.com/ueqri/pcl/tree/hip-perl-test)
- Raise issues in HIP-CPU repository and merged PR on pitched memory: [#23](https://github.com/ROCm-Developer-Tools/HIP-CPU/issues/23) [#25](https://github.com/ROCm-Developer-Tools/HIP-CPU/issues/25) [PR#24](https://github.com/ROCm-Developer-Tools/HIP-CPU/pull/24)
- Use gpgpu-sim for CUDA PTX assembly simulation, raised issues for bugs: [Issue in GitHub](https://github.com/gpgpu-sim/gpgpu-sim_distribution/issues/230) [Discussion in Google Group](https://groups.google.com/g/accel-sim/c/SxtFMYrshXg/m/pTYTsZesAQAJ)

## Conclusion

In the journey of this project, although we have not achieved all the targets of the proposal, we built a pipeline framework, dug many problems from the upstream toolkits when combining them with a large-scale application, reported bugs and fixed some of them.

Furthermore, we did comprehensive [investigations](gsoc-2021/gpu-report.md#investigations) on the possible approaches for our project. After many experiments, several potential choices and solutions are provided, some are able to be done only after the upstream toolkits fix the problems, some are still working in progress.

- HIP-CPU solution, derived from the proposal, is able to work well only after the HIP community tackles the problems and enhances the support for warp magics.
- GPGPU-Sim solution, described in Section [Investigations](gsoc-2021/gpu-report.md#investigations), is able to work only after correctness bugs are fixed.
- SYCL solution requires deep expertise and much effort to rewrite the GPU modules, which could be put on the agenda of PCL after discussion.
- Cloud GPU for nonprofit, working in progress, is a most promising way to solve all the problems as a completely different approach compared to CUDA transformation or simulation.

I have to say that, this journey is definitely beyond expectation with two many blocks, and without the discussion, help, and encouragement of my fantastic mentors, I couldn’t imagine how to get through these difficulties.

This is a completely roller coaster--find an approach with excitement, test it, fail with disappointment, investigate the causes with hope, no solutions just so far made slight loss, try another approach--but a really great learning experience and really worth it. Immersing myself in the experiments and investigations really broadens the horizons and ideas, incidentally polishing the skills of using tools and programming.

And I would sincerely appreciate the support of my kind mentors, they spend much time discussing my experiments and approaches and give me much guidance. And I soon realized “contribute to make it perfect and work together to go far” in the open source contribution. Not only the further development of this project, but I am also willing to contribute more on PCL and other open source communities, to be an excellent geek like my mentors and other contributors in the open source family!

**For details of the final reports, please see here: [Final Report for "Enable CUDA builds on CI"](gsoc-2021/gpu-report.md#investigations)**

**Here is a mirror report in Google Docs: [Final Report for "Enable CUDA builds on CI"](https://docs.google.com/document/d/1M6oeheNOyYMi45y8mSZUQJdYz7xR5pKYNYrPrgR4RsQ/edit?usp=sharing)**
