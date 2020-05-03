---
layout: page
title: About
permalink: /about/
---

What is PCL?
============

The Point Cloud Library (or PCL) is a large scale, open project [[1]](#open) for 2D/3D
image and point cloud processing. The PCL framework contains numerous
state-of-the art algorithms including filtering, feature estimation, surface
reconstruction, registration, model fitting and segmentation. These algorithms
can be used, for example, to filter outliers from noisy data, stitch 3D point
clouds together, segment relevant parts of a scene, extract keypoints and
compute descriptors to recognize objects in the world based on their geometric
appearance, and create surfaces from point clouds and visualize them -- to name
a few.
 
PCL is released under the terms of the [3-clause BSD license](https://en.wikipedia.org/wiki/BSD_licenses#3-clause_license_.28.22Revised_BSD_License.22.2C_.22New_BSD_License.22.2C_or_.22Modified_BSD_License.22.29) and is open source software. **It is free for commercial and research use.** PCL is **cross-platform**,
and has been successfully compiled and deployed on Linux, MacOS, Windows and
Android. To simplify development, PCL is split into a series of smaller
code libraries, that can be compiled separately. This modularity is important
for distributing PCL on platforms with reduced computational or size
constraints (for more information about each module see the [documentation]({{ site.api_base_url }})
page). Another way to think about PCL is as a graph of code libraries,
similar to the [Boost](http://www.boost.org/) set of C++ libraries. Here's an example:

![dependency-graph](/assets/images/about/pcl_dependency_graph2.png)


What is a Point Cloud?
======================

A point cloud is a data structure used to represent a collection of
multi-dimensional points and is commonly used to represent three-dimensional
data. In a 3D point cloud, the points usually represent the X, Y, and Z
geometric coordinates of an underlying sampled surface. When color information
is present (see the figures below), the point cloud becomes 4D.

![point_cloud_example](/assets/images/about/point_cloud_example.png)


Point clouds can be acquired from hardware sensors such as stereo cameras, 3D scanners, or time-of-flight cameras, or generated from a computer program synthetically. PCL supports natively the OpenNI 3D interfaces, and can thus acquire and process data from devices such as the PrimeSensor 3D cameras, the Microsoft Kinect or the [Asus
XTionPro](https://www.asus.com/3D-Sensor/Xtion_PRO/).

For more information about point clouds and 3D processing please visit our [documentation]({{ site.api_base_url }}) page.

References
==========

<a name="open"><abbr title="PCL">[1]</abbr></a>
For more information, including a scientific citation (more to be added soon), please see:

{% raw %}
	@InProceedings{Rusu_ICRA2011_PCL,
		author    = {Radu Bogdan Rusu and Steve Cousins},
		title     = {{3D is here: Point Cloud Library (PCL)}},
		booktitle = {{IEEE International Conference on Robotics and Automation (ICRA)}},
		month     = {May 9-13},
		year      = {2011},
		address   = {Shanghai, China}
	}
{% endraw %}

[Download PDF here](/assets/pdf/pcl_icra2011.pdf)