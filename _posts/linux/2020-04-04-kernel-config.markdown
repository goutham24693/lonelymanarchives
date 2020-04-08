---
layout: post-wrapper
title:  "Configuring the linux Kernel"
date:   2020-04-04 13:24:40 +0530
categories: jekyll update
---

<p>
If you are using a linux distribution, then there could be a number of reasons why you would want to compile and install a new kernel. Some of the reasons coould be to update security features in the kernel, use a newer kernel with improvements etc. Before configuring the kernel, it is necessary to understand the underlying hardware that you are going to run your kernel on.
</p>

<h3>Downloading the kernel source:</h3>
<p>
To configure a kernel, it first starts with compiling the source. The desired target kernel can be either a vanilla kernel or a distribution kernel. Shown below is the commands to checkout a kernel source from github. This step can be skipped if you already have a target kernel to configure and install. 
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
Depending on the speed of your network connection, downloading of soure may take some time. At the time of posting this webpage, the latest kernel release version is 5.6.3. The recent stable releases can be checked from https://www.kernel.org/ website. wget command line utility can be used to download tarball of your release version. Once the tarball is downloaded, extract the sources like shown below.
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
Before compiling the source, it is worth going through the directory structure and source file categorisation of the linux kernel. 
</p>
