---
layout: page
title: Downloads
permalink: /downloads/
---

## Prebuilt binaries


Point Cloud Library (PCL) runs on many operating systems, and prebuilt binaries are available for Linux, Windows, and macOS. In addition to installing PCL, you will need to download and compile a set of 3rd party libraries that PCL requires in order to function. To sidestep all that trouble, we recommend you to install PCL through one of the many available package managers out there. Package managers are the preferred distribution mechanism to download PCL. We provide a couple of options below dependent on your platform of choice.

<div class="row">
	<div class="column column-2 icon-with-text">
		<a href="#cross-platform" class="clear"><div class="fas fa-desktop icon-large"></div></a>
		<p><a href="#cross-platform">Cross-Platform</a></p>
	</div>
	<div class="column column-2 icon-with-text">
		<a href="#linux" class="clear"><div class="fab fa-linux icon-large"></div></a>
		<p><a href="#linux">Linux</a></p>
	</div>
</div>

An extensive list of package managers which distribute PCL is available at [repology](https://repology.org/project/pcl-pointclouds/packages).
Nevertheless, we also provide packaged binaries and installers for a couple of platforms in our [Releases](https://github.com/PointCloudLibrary/pcl/releases) page on our [GitHub repository](https://github.com/PointCloudLibrary/pcl). Be sure to check that out, in case you're looking for something different than what is provided in the options below.

## Cross-Platform

In case your OS does not have a dedicated package manager, consider one of the following options.

### vcpkg

[![Vcpkg package](https://repology.org/badge/version-for-repo/vcpkg/pcl-pointclouds.svg)](https://github.com/Microsoft/vcpkg/tree/master/ports/pcl)

[vcpkg](https://github.com/microsoft/vcpkg) is a cross-platform open source package manager created by Microsoft, available for Windows, Linux and macOS. To install PCL on vcpkg-enabled desktops type the following on your terminal

```
PS> .\vcpkg install pcl
```

This is our recommended installation method for **Windows** users.

### Homebrew

[![Homebrew package](https://repology.org/badge/version-for-repo/homebrew/pcl-pointclouds.svg)](https://formulae.brew.sh/formula/pcl)

[Homebrew](https://brew.sh/) became known as being missing package manager for macOS, but in recent years it extended its support to Linux. To install PCL on Homebrew-enabled desktops type the following on your terminal

```
$ brew install pcl
```

This is our recommended installation method for **macOS** users.


## Linux

PCL is available in a number of Linux distributions namely Ubuntu, Debian, Fedora, Gentoo and Arch Linux systems to name a few. Below we provide installation instructions Ubuntu and Debian. You can install pcl using

```
$ sudo apt install libpcl-dev
```

This is our recommended installation method for **Linux** users.