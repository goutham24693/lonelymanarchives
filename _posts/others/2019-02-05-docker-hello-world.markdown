---
layout: post-wrapper
title:  "Run a Hello World program in a Docker"
date:   2019-02-05 22:00:40 +0530
categories: jekyll update
---

<h3>What is Docker?</h3>
<p>
Docker is a tool that helps developers to easily package their application into virtual containers. These containers are very portable and this container environment in the developer-end and user-end are almost similar. When an application and its dependancies are packaged as containers, the memory footprint and overhead are a lot lesser, compared to the same application running on a Virtual Machine.
</p>

<h3>Docker vs VM:</h3>
<p>
Virtual machines have a full OS with its own memory management installed with the associated overhead of virtual device drivers. On the other hand Docker containers are executed with the Docker engine rather than the hypervisor. Containers are therefore smaller than Virtual Machines and enable faster start up with better performance, less isolation and greater compatibility possible due to sharing of the host’s kernel.
</p>
<h3>Docker Installation - Ubuntu:</h3>
{% highlight bash %}
$ apt-get update

$ apt install docker.io

{% endhighlight %}
<p>
To check if docker is installed properly, execute below command and check if you get a similar print message.
</p>
{% highlight bash %}
$ docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
{% endhighlight %}
<h3>Run your application on a Docker:</h3>
<p>
Docker lets developers build their own application and deploy them in containers. The application and its dependant libraries are bundled as a single image and these docker images can be run in a containerized environment. A <i>Dockerfile</i> is a text document that contains all the commands a user could call on the command line to assemble a docker image. So to create a docker image, create a new Dockerfile and copy below content.
</p>

{% highlight bash %}
FROM ubuntu:18.04    #1
ADD hello /          #2
CMD ["/hello"]       #3
{% endhighlight %}

<p>
1. Specifies the parent image as ubuntu version 18.04. It is over this layer your application will run. The image in the FROM section is called as the parent image.<br><br>

2. Instructs to add executable named "hello" to the root of the container file system. You can create a directory and add your local files using ADD command. In this example, "hello" is just an executable that prints hello world.<br><br>

3. CMD label contains the command and its argument for the container. Once the container is started, the CMD section is executed. <br><br>
</p>

<h3>Building your image:</h3>
<p>
Docker images can be built using Dockerfile as shown below.
</p>
{% highlight bash %}
$ docker build --tag hello .
..
..
Successfully tagged hello:latest
{% endhighlight %}

<p>
Note: Make sure docker build command is executed in same working directory as the Dockerfile, else -f option can be used.
<p>
{% highlight bash %}
$ docker images
REPOSITORY            TAG                 IMAGE ID
hello 		        latest              326387sea398
{% endhighlight %}

After building image, the image can be run as container using following command.

<h3>Running your docker image:</h3>
{% highlight bash %}
$ docker run hello
Hello World
{% endhighlight %}

<p>
The <i>docker run</i> command executes the docker images built locally, if the specified image is not present, the Docker daemon will pull an image from local or public registry, if present. Docker images can be shared using public registry like Docker Hub. The parent image specified in the FROM label in the above example Dockerfile, is the official ubuntu image from pulled from public registry. Similarly developers can build their own images and push it to public reistry where other can use it by pulling them.
</p>
