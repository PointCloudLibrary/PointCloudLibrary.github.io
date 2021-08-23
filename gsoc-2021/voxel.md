**Student:** [Tin Chon Chan][tin1254]

**Mentors:** [Kunal Tyagi][kunaltyagi], [Markus Vieth][mvieth], [Haritha Jayasinghe][haritha-j]

## Motivation

In PCL, there are a variety of filters, many of which contain similar filtering logic in at least part of their code.
In the early years, the filters were created by different developers, a unified code design did not emerge organically. As a result, there are many boilerplate snippets throughout the filter module. Further enhancements to the filters, such as parallelizing loops, are challenging due to the necessary boilerplate code requiring changes to all the equivalent code across multiple classes without breaking them.

We found some instances of the boilerplate codes in the voxel filters: `ApproximateVoxelGrid`, `VoxelGrid`, `GridMinimum`, `VoxelGridLabel`, `UniformSampling` (and also in the binary removal filters)

Our goal was to investigate a unified code design that eliminates redundancy by refactoring similar code pieces across multiple filters without breaking the present API, while simultaneously attempting to simplify implementation and increase runtime speed.

## Developments

We started with the most often used voxel filters with the many redundancies. After a close inspection, we noticed a similar pattern and boilerplate codes in the existing code. Following that, I proposed different code designs with highly abstracted classes that allow PCL users to simply construct their own grid filter, by enabling some level of customization and maximizing the reusability of our implements. After some discussions with my mentors, we selected some plausible designs and developed prototypes for further discussions. In the end, I extracted the voxel filters logic and implemented them into new building blocks with template metaprogramming. I refactored two voxel filters and applied the new design. It was seen via benchmarks that the new design can eliminate duplicate codes and enhance performance by up to 40%. This design allows PCL users to reuse our existing implementations while focusing on the parts that require separate logic for their custom filter.

In addition to voxel filters, we also explored new code designs for other filters. There is a new proposed filter design that takes a lambda function or functor as the filtering logic. I tried to apply the newly introduced design to some current filters with different approaches. In the end, it proved to have a huge potential that can remove lots of boilerplate code as well as significantly improve the runtime performance of existing filters.

## Challenges

During the course of the project, I realized I didn't have a lot of expertise in code design. Code quality is rarely a concern in academic projects as long as they work, and when I had to develop classes with high abstractions in mind, this deficiency became even more evident. There were different tradeoffs to be made between code design complexity, level of customizability, and upgradeability. Many thanks to my mentors [@kunaltyagi][kunaltyagi] [@mvieth][mvieth] [@haritha-j][haritha-j] who provided me with a wealth of useful and timely input on the code designs and quality.

Template metaprogramming is another obstacle for me. This is a skill I never needed in my academic assignments, so it was no surprise that I was totally unaware of it. I spent a lot of time learning the skills and was finally able to apply them during this project. 

Finally, because it is not uncommon to filter point clouds with millions of points, performance can easily be a problem hidden elsewhere in the code, especially since everything that can go wrong with deep abstraction of C++. I spent a lot of time testing multiple implementations for the filters to ensure that every line of code is efficient enough. Fortunately, all of the refactored filters perform noticeably better than the original implementations. 

## Summary of design

- `VoxelGrid` and `VoxelGridLabel` were refactored and implemented in the experimental namespace as a proof of the new code design.
- `VoxelGrid` is now comprised of several building blocks: `Voxel` (define the voxel grid cell), `VoxelStruct` (implement `VoxelGrid` filtering logic), `VoxelFilter` (implement API for voxel based grid filters), `CartesianFilter` (implement common API for cartesian based grid filter), `TransformFilter` (implement the abstracted logic of grid filters, accept `VoxelStruct` as the filtering logic definition).
- `VoxelGridLabel` reused all building blocks other than a custom voxel grid cell. 
- The code design concept is also documented as a tutorial.
- `PassThrough` and `Cropbox` were refactored using `FunctorFilter`. A `FunctorFilter` object will be created for each filtering operation inside applyFilter. Only the core logic for filtering a point is needed to implement since the boilerplate iteration is handled by `FunctorFilter`.

### PRs

- [Base of grid filters](https://github.com/PointCloudLibrary/pcl/pull/4828)
- [VoxelGrid](https://github.com/PointCloudLibrary/pcl/pull/4829)
- [VoxelGridLabel](https://github.com/PointCloudLibrary/pcl/pull/4870)
- [CropBox & PassThrough](https://github.com/PointCloudLibrary/pcl/pull/4892)
- [Add deprecations in current VoxelGrid](https://github.com/PointCloudLibrary/pcl/pull/4861)

### TODO

- Fix `VoxelGrid` error with point types with label
- Finalize API changes for `VoxelGrid` leaf layout ([discussion](https://github.com/PointCloudLibrary/pcl/issues/4897#issuecomment-903039969))

[tin1254]: https://github.com/tin1254
[kunaltyagi]: https://github.com/kunaltyagi
[mvieth]: https://github.com/mvieth
[haritha-j]: https://github.com/haritha-j

## TL;DR
- A unified code structure design has been implemented. The new grid filter code design allows users to customize their grid filters while also maximizing the reusability of our implementations.
- `VoxelGrid` and `VoxelGridLabel` were refactored, visible boilerplate codes were eliminated and performance was improved.
- Several approaches for integrating `FunctionFilter` were investigated and the new code design was applied to `PassThrough` and `Cropbox`.
- It has been shown that `FunctionFilter` is capable of removing many boilerplate codes in the filter module while also noticeably improving runtime performance.
- Runtime improvements:
    - Up to 40% with `VoxelGrid` and `VoxelGridLabel`
    - Up to 17% with `PassThrough`
    - Up to 37% with `Cropbox`
