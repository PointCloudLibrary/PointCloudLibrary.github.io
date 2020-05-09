---
layout: page
title: Downloads
permalink: /downloads/
---

## Prebuilt binaries


Point Cloud Library (PCL) runs on many operating systems, and prebuilt binaries are available for Linux, Windows, and macOS. In addition to installing PCL, you will need to download and compile a set of 3rd party libraries that PCL requires in order to function. Select the operating system of your choice below to continue.

<div class="row">
	<div class="column column-3 icon-with-text">
		<a href="#linux" class="clear"><div class="fab fa-linux icon-large"></div></a>
		<p><a href="#linux">Linux</a></p>
	</div>
	<div class="column column-3 icon-with-text">
		<a href="#macos" class="clear"><div class="fab fa-apple icon-large"></div></a>
		<p><a href="#macos">macOS</a></p>
	</div>
	<div class="column column-3 icon-with-text">
		<a href="#windows" class="clear"><div class="fab fa-windows icon-large"></div></a>
		<p><a href="#windows">Windows</a></p>
	</div>
</div>

We also provide packaged binaries and installers for a couple of platforms in our [Releases](https://github.com/PointCloudLibrary/pcl/releases) page on [GitHub](https://github.com/). Be sure to check that out, in case you're looking for something different than what is provided in the options below.

### Linux

PCL is available in a number of Linux distributions namely Ubuntu, Debian, Fedora, Gentoo and Arch Linux systems to name a few. The full list of available packages is available at [repology](https://repology.org/project/pcl-pointclouds/packages). Below we provide installation instructions for some of the most popular distributions. If the distribution you use is not here feel free to add it.

#### Ubuntu and Debian

<span class="fab fa-ubuntu icon-large"></span>

You can install pcl using
```
$ sudo apt install libpcl-dev
```

### macOS

The easiest way to install PCL on macOS is through [Homebrew](https://brew.sh/). You can install pcl using
```
$ brew install pcl
```

### Windows

The easiest and most reliable way to install PCL on Windows is through [vcpkg](https://github.com/Microsoft/vcpkg).

```
PS> .\vcpkg install pcl
```
