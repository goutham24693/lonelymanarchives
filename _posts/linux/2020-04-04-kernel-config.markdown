---
layout: post-wrapper
title:  "Configuring the linux Kernel"
date:   2020-04-04 13:24:40 +0530
categories: jekyll update
---

<p>
If you are using a linux distribution, then there could be a number of reasons why you would want to compile and install a new kernel. Some of the reasons could be to update security features in the OS or use a newer kernel with latest improvements. Before configuring the kernel, it is necessary to understand the underlying hardware that you are going to run your kernel on.
</p>

<h3>Downloading the kernel source:</h3>
<p>
To configure a kernel, it first starts with compiling the source. The desired kernel can be either a vanilla kernel or a distribution kernel. Shown below is the commands to checkout a kernel source from kernel.org. This step can be skipped if you already have a target kernel to configure and install. 
</p>


{% highlight bash %}
$ wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.6.3.tar.xz
--2020-04-09 01:36:44--  https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.6.3.tar.xz
Resolving cdn.kernel.org (cdn.kernel.org)... 199.232.57.176, 2a04:4e42:4b::432
Connecting to cdn.kernel.org (cdn.kernel.org)|199.232.57.176|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 111775548 (107M) [application/x-xz]
Saving to: 'linux-5.6.3.tar.xz'

linux-5.6.3.tar.xz   100%[==========================>] 106.60M  1.43MB/s    in 62s

2020-04-09 01:37:46 (1.72 MB/s) - 'linux-5.6.3.tar.xz' saved [111775548/111775548]
{% endhighlight %}

<p>
Depending on the speed of your network connection, downloading of source may take some time. At the time of posting this webpage, the latest kernel release version is 5.6.3. The recent stable releases can be checked from https://www.kernel.org/ website. wget command line utility can be used to download tarball of your release version. Once the tarball is downloaded, extract the sources like shown below.
</p>

{% highlight bash %}
$ ls
linux-5.6.3.tar.xz
$ unxz linux-5.6.3.tar.xz
$ ls
linux-5.6.3.tar
$ tar -xf linux-5.6.3.tar
$ ls
linux-5.6.3  linux-5.6.3.tar
{% endhighlight %}


<h3>Linux Kernel source directory structures:</h3>
<p>
Before compiling the source, it is worth going through the directory structure and source file categorisation of the linux kernel. Even though the kernel source code changes rapidly, the details below will most likely be relevant. 
</p>
<h4>Makefile</h4>
<p>
The file is the top level Makefile for the whole Linux source. It contains recipes to generate the linux kernel for target machine. 
</p>

<h4>arch/</h4>
<p>
This directory contains architecure specific code. For a target machine with intel x86 architecure, source code under directory arch/x86 will be compiled to linked to final executable. This section also contains interupt handling, assembly routines and other initialization methods. 
</p>


<h4>block/</h4>
<p>
Contains block device driver support for the kernel to partition and use a harddisk. This enable to store a file and access the file from a disk. 
</p>

<h4>certs/</h4>
<p>
Has Public Key Infrastructure Certificate Authentication related code.
</p>

<h4>crypto/</h4>
<p>
This directory contains cryptography related source files.
</p>

<h4>drivers/</h4>
<p>
Has code for network driver, SCSI driver and video drivers mostly responsible for IO interfaces.
</p>

<h4>fs/</h4>
<p>
Has file-system & virtual file system (/proc & /sys) related code are present here. For eg sources specific to ext2 and ntfs are present in fs/ext2 and fs/ntfs directories. 
</p>

<h4>include/</h4>
<p>
Various .h include files and shared definitions are stored here. The variables & constants stored here are usually referenced from other directories. 
</p>

<h4>init/</h4>
<p>
Contains the initialisation code of the kernel. It has the main.c file and is responsible to create the "early userspace".
</p>

<h4>ipc/</h4>
<p>
Contains code for all the ipc mechanisms between process. 
</p>

<h4>kernel/</h4>
<p>
The main parts of the kernel like scheduler, system handler and other system call APIs are here.
</p>

<h4>lib/</h4>
<p>
Generic library routines are present here.
</p>

<h4>mm/</h4>
<p>
Memory Management related functionalities are present here.
</p>

<h4>net/</h4>
<p>
Kernel's networking source files are here. Network drivers send packet buffers from the network cable and the code in this section deliver it to the user-space application.
</p>

<h4>script/</h4>
<p>
Contains script files that helps to building of kernel.
</p>

<h4>security/</h4>
<p>
Code for different Linux security models can be found here, such as NSA Security-Enhanced Linux and socket and network security hooks.
</p>

<h4>sound/</h4>
<p>
Drivers for sound cards and other sound related code is placed here.
</p>

<h4>usr/</h4>
<p>
This directory contains code that builds a cpio-format archive containing a root filesystem image, which will be used for early userspace.
</p>
