---
layout: post-wrapper
title:  "Attach NIC to docker container"
date:   2019-02-12 21:50:40 +0530
categories: jekyll update
---
<h3> Attaching NIC to Container:</h3>
<p>
At times there might be a requirement to control the traffic from a specific network interface or to perform control plane operation inside a dockers. This post covers how you can directly play with the physical network interface from inside a container. When a container is deployed, it usually created a seperate network namespace from the host operating system. Docker creates a interface "docker0" in the host and this interface acts as a bridge to all containers running in that host. In the container, there is a "eth0" and "lo" interfaces created. This separation or distinguishing of interfaces between container and the host machines is done by a Linux kernel features such as namespaces and cgroup.
Below is the ifconfig on Host machine. Here the interfaces ens6f0 & ens6f1 are to be mapped to container namespace.
</p>
<pre>
$ ifconfig			# inside host machine
eno1      Link encap:Ethernet  HWaddr ac:1f:6b:46:b6:fa
          inet addr:172.30.7.59  Bcast:172.30.7.255  Mask:255.255.255.0
          inet6 addr: fe80::ae1f:6bff:fe46:b6fa/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:19263704 errors:0 dropped:22 overruns:0 frame:0
          TX packets:15597832 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:16639279019 (16.6 GB)  TX bytes:2442425585 (2.4 GB)
          Memory:fb400000-fb47ffff

<b>ens6f0</b>    Link encap:Ethernet  HWaddr ac:1f:6b:2d:66:48
          inet addr:12.0.0.100  Bcast:12.255.255.255  Mask:255.0.0.0
          inet6 addr: fe80::ae1f:6bff:fe2d:6648/64 Scope:Link
          UP BROADCAST PROMISC MULTICAST  MTU:1500  Metric:1
          RX packets:56370514 errors:0 dropped:193409 overruns:0 frame:0
          TX packets:37286594 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:22631502022 (22.6 GB)  TX bytes:4966231180 (4.9 GB)

<b>ens6f1</b>    Link encap:Ethernet  HWaddr ac:1f:6b:2d:66:49
          inet6 addr: fe80::ae1f:6bff:fe2d:6649/64 Scope:Link
          UP BROADCAST RUNNING PROMISC MULTICAST  MTU:1500  Metric:1
          RX packets:59279476 errors:0 dropped:13 overruns:0 frame:0
          TX packets:2744 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:30113789877 (30.1 GB)  TX bytes:349832 (349.8 KB)
</pre>
		  

<p>When a container is deployed, ifconfig output is as shown below.</p>
<pre>
$ ifconfig 			# inside container
eth0: flags=4163(UP,BROADCAST,RUNNING,MULTICAST)  mtu 1500
        inet 172.17.0.3  netmask 255.255.0.0  broadcast 0.0.0.0
        inet6 fe80::42:acff:fe11:3  prefixlen 64  scopeid 0x20(link)
        ether 02:42:ac:11:00:03  txqueuelen 0  (Ethernet)
        RX packets 8  bytes 1124 (1.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 6  bytes 516 (516.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
lo: flags=73(UP,LOOPBACK,RUNNING)  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10(host)
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
</pre>

<p>
To map a physical interfaces ens6f0 ens6f1 to the container from the host OS, the interfaces needs to be set into the container network namespace. It can be done as shown below in the Host machine.
</p>
{% highlight bash %}
$  docker container ps | grep hello | head -n 1 | awk '{print $1}'  # inside host
7bd4425033fd

$ docker inspect 7bd4425033fd | grep  Pid
    "Pid": 4670
    "PidMode":"",
    "PidsLimit": 0,

$ ln -s /proc/4670/ns/net /var/run/netns/container1
{% endhighlight %}
<p>
After creating the link file, executing ip netns command in the host will display the new network namespace docker created for your container.
</p>
{% highlight bash %}
$ ip netns
container1 (id: 3)

$ ip link set ens6f0 netns container1

$ ip link set ens6f1 netns container1
{% endhighlight %}

Now the interfaces ens6f0 & ens6f1 can be linked to the container using ip link set <eth> netns <netns-name>.
In the container, execute the following commands to receive and send packets.

<pre>
$ ifconfig ens6f0 up		# inside container

$ ifconfig ens6f1 up

$ ifconfig 
<b>ens6f0</b>: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 12.0.0.10  netmask 255.0.0.0  broadcast 12.255.255.255
        ether ac:1f:6b:8a:0a:f4  txqueuelen 1000  (Ethernet)
        RX packets 103  bytes 9448 (9.4 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 67  bytes 5782 (5.7 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

<b>ens6f1</b>: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 13.0.0.10  netmask 255.0.0.0  broadcast 13.255.255.255
        ether ac:1f:6b:8a:0a:f5  txqueuelen 1000  (Ethernet)
        RX packets 65  bytes 5724 (5.7 KB)
        RX errors 0  dropped 5  overruns 0  frame 0
        TX packets 59  bytes 5166 (5.1 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.3  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:03  txqueuelen 0  (Ethernet)
        RX packets 10386  bytes 18246485 (18.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5876  bytes 403437 (403.4 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1  (Local Loopback)
        RX packets 240  bytes 12000 (12.0 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 240  bytes 12000 (12.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
</pre>

<p>
After mapping the interfaces to the container namespace, the incoming packets will be visible in container and not in host. This way we can set a interface to a different namespace.                 
</p>
