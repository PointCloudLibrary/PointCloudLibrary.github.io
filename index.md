---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: page
---

![pcl-logo](/assets/images/logo.png)

The Point Cloud Library (PCL) is a standalone, large scale, open project for 2D/3D image and point cloud processing. PCL is released under the terms of the **BSD license**, and thus free for commercial and research use.

Whether you've just discovered PCL or you're a long time veteran, this page contains links to a set of resources that will help consolidate your knowledge on PCL and 3D processing. An additional Wiki resource for developers is available at [https://github.com/PointCloudLibrary/pcl/wiki](https://github.com/PointCloudLibrary/pcl/wiki).

Getting Started
----------------

To simplify both usage and development, we split PCL into a series of modular libraries. The most important set of released modules in PCL is shown below.
<table align="center">
<tr align="center"><td><b>filters</b></td><td><b>features</b></td><td><b>keypoints</b></td></tr>
<tr align="center">
<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__filters.html"><img height="100" src="{{ '/assets/images/overview/filters_small.png' | relative_url }}" /></a></td>
<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__features.html"><img height="100" src="{{ '/assets/images/overview/features_small.png' | relative_url }}" /></a></td>
<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__keypoints.html"><img height="100" src="{{ '/assets/images/overview/keypoints_small.png' | relative_url }}" /></a></td>
</tr>
<tr align="center"><td><b>registration</b></td><td><b>kdtree</b></td><td><b>octree</b></td></tr>
<tr align="center">
<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__registration.html"><img height="100" src="{{ '/assets/images/overview/registration_small.png' | relative_url }}" /></a></td>
<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__kdtree.html"><img height="100" src="{{ '/assets/images/overview/kdtree_small.png' | relative_url }}" /></a></td>
<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__octree.html"><img height="100" src="{{ '/assets/images/overview/octree_small.png' | relative_url }}" /></a></td>
</tr>
<tr align="center"><td><b>segmentation</b></td><td><b>sample_consensus</b></td><td><b>surface</b></td></tr>
<tr align="center">
	<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__segmentation.html"><img height="100" src="{{ '/assets/images/overview/segmentation_small.png' | relative_url }}" /></a></td>
	<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__sample__consensus.html"><img height="100" src="{{ '/assets/images/overview/sample_consensus_small.png' | relative_url }}" /></a></td>
	<td style="border-bottom: 2px solid rgb(128,128,128);"><a href="{{ site.api_base_url }}/group__surface.html"><img height="100" src="{{ '/assets/images/overview/surface_small.png' | relative_url }}" /></a></td>
</tr>
<tr align="center"><td><b>recognition</b></td><td><b>io</b></td><td><b>visualization</b></td></tr>
<tr align="center">
<td><a href="{{ site.api_base_url }}/group__recognition.html"><img height="100" src="{{ '/assets/images/overview/recognition_small.png' | relative_url }}" /></a></td>
<td><a href="{{ site.api_base_url }}/group__io.html"><img height="100" src="{{ '/assets/images/overview/io_small.jpg' | relative_url }}" /></a></td>
<td><a href="{{ site.api_base_url }}/group__visualization.html"><img height="100" src="{{ '/assets/images/overview/visualization_small.png' | relative_url }}" /></a></td>
</tr>
</table>


[Tutorials][tutorials]
----------------
Our [comprehensive list of tutorials for PCL][tutorials], covers many topics, ranging from simple Point Cloud Input/Output operations to more complicated applications that include visualization, feature estimation, segmentation, etc. 

[![tutorials](/assets/images/tutorials.png)][tutorials]


[Advanced Topics][advanced]
----------------

If you're interested in how PCL works internally, or are looking at optimizing your workflow, we have assembled a [set of topics][tutorials] that cover interesting subjects from reducing your compile time to code profiling.

[![advanced](/assets/images/advanced.png)][advanced]


[API Reference][api]
----------------

The [Application Programming Interface (API) documentation][api] for PCL includes explanations about how each public method and class works, shows simple code snippets that can be used quickly to prototype and test a particular algorithm, and links to scientific publications for those interested in finding out more about the theoretical aspects of a particular method. 

[api]: {{ site.api_base_url }}
[advanced]: {{ site.advanced_base_url }}
[tutorials]: {{ site.tutorials_base_url }}