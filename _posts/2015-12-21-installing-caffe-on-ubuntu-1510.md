---
layout: post
title: "Installing caffe on Ubuntu 15.10"
description: ""
category: 
tags: []
---
{% include JB/setup %}

I had some trouble installing caffe on Ubuntu 15.10 (mostly because I tried to use a gcc 4.x compiler, which ended up giving me a lot of headache) so I decided to write some notes on how I finally got it working. This is on a completely clean Ubuntu 15.10 install with gtx 660 ti card.

- (download Ubuntu updates and restart the computer. Use the proprietary driver by Nvidia by going to Additional Drivers and choosing it from the list) <- this might be optional, but I had to download the updates because otherwise my computer would freeze in around 10 minutes.

- Download the prerequisites of caffe. You can see them for Ubuntu [here](http://caffe.berkeleyvision.org/install_apt.html), and general dependencies [here](http://caffe.berkeleyvision.org/installation.html). Don't compile it yet.

- Install cuda by [downloading it from Nvidia](https://developer.nvidia.com/cuda-downloads) and following the installation instructions. I downloaded the local deb.

- Follow the instructions [here](https://github.com/BVLC/caffe/issues/2347). It fixes some issues with different paths between Ubuntu versions.

- Create symbolic links between the files as seen [here in the third post](https://github.com/NVIDIA/DIGITS/issues/156). This fixes some naming/path issues:

      	cd /usr/lib/x86_64-linux-gnu
		sudo ln -s libhdf5_serial.so.8.0.2 libhdf5.so
		sudo ln -s libhdf5_serial_hl.so.8.0.2 libhdf5_hl.so

- Change a line in the cuda host_config.h (located in /usr/local/cuda-7.5/targets/x86_64-linux/include/host_config.h. Taking a backup first is a good idea) file:
		
		//from this line
		#if __GNUC__ > 4 || (__GNUC__ == 4 && __GNUC_MINOR__ > 6)
		//to this
		#if __GNUC__ > 5 || (__GNUC__ == 5 && __GNUC_MINOR__ > 2)

This changes it so it doesn't complain when you're using the default compiler in Ubuntu 15.10. gcc 5.x isn't officially supported by nvidia yet, but I've had zero problems and all the caffe tests ran fine.

- Follow the instructions on caffe's site: http://caffe.berkeleyvision.org/installation.html. The compilation should run with no hitches. Run the tests and do the MNIST tutorial to test it works.