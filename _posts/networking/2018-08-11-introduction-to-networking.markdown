---
layout: post-wrapper
title:  "Introduction to Networking"
date:   2018-08-11 21:07:00 +0530
categories: jekyll update
---
<style type="text/css">
.o {
        font-weight: bold;
}

.comment {
        color: #586e75;
}

.nv {
        color: #268bd2;
}

.data {
    white-space: pre;
}

</style>
<h3>Circuit & Packet Switching:</h3>
<p>
Circuit switching is a method of implementing a telecommunications network in which two network nodes establish a dedicated communications channel (circuit) through the network before the nodes may communicate. The circuit functions as if the nodes were physically connected as with an electrical circuit.</p>
<p>
Packet switching is a method of grouping data which is transmitted over a digital network into packets which are made of a header and a payload. Data in the header is used by networking hardware to direct the packet to its destination where the payload is extracted and used by application software. Packet switching is the primary basis for data communications in computer networks worldwide. 
</p>
<h3>Switches & Routers:</h3>
<p>
Switches create network and Routers connects networks.<br><br>
A network switch is a computer networking device that connects devices together on a computer network by using packet switching to receive, process, and forward data to the destination device.  In networks the switch is the device that filters and forwards packets between LAN segments. Traditional Switches operate at the data link layer (layer 2) of the OSI Reference Model. 
</p>
<p>
Routers is connected to at least two networks, commonly two LANs or WANs or a LAN and its ISP's network. Routers are located at gateways, the places where two or more networks connect. Routers use headers and forwarding tables to determine the best path for forwarding the packets, and they use protocols to communicate with each other and configure the best route between any two hosts. Routing operates at layer 3 of the OSI Reference Model.<br><br>
Switches and Routers use packet switching to transfer data in a network.
</p>
<h3>Firewall & Proxy:</h3>
<p>
A firewall is a network security device that grants or rejects network access to traffic flows between an untrusted zone (e.g., the Internet) and a trusted zone (e.g., a private or corporate network).
</p>
<p>In computer networks, a proxy server is a server that acts as an intermediary for requests from clients seeking resources from other servers. A client connects to the proxy server, requesting some service, connection, web page, or other resource available from a different server and the proxy server evaluates the request as a way to simplify and control its complexity.
</p>
<h3>IP Address:</h3>
<p>
An IP address is a unique numberic ID given to a host on a network. It identifies the host, or more specifically its network interface, and it provides the location of the host in the network, and thus the capability of establishing a path to that host. Today, the internet mostly uses IPv4 (fourth version of Internet Protocol) for routing traffic.
</p>
<p>
An IPv4 address has a size of 32 bits, which limits the address space to 4294967296 (232) addresses. Of this number, some addresses are reserved for special purposes such as private networks (~18 million addresses) and multicast addressing (~270 million addresses).
IPv4 addresses are usually represented in dot-decimal notation, consisting of four decimal numbers, each ranging from 0 to 255, separated by dots, e.g., 172.16.254.1. Each part represents a group of 8 bits (an octet) of the address.
</p>
<h3>Subnet Mask:</h3>
<p>
Subnetting is the strategy used to partition a single physical network into more than one smaller logical sub-networks (subnets). An IP address includes a network segment and a host segment. Subnets are designed by accepting bits from the IP address's host part. Subnetting allows an organization to add sub-networks without the need to acquire a new network number via the Internet service provider (ISP). Subnetting helps to reduce the network traffic and conceals network complexity.
</p>
<p>
A Subnet mask is a 32 bits value, in the form of IP Address, that differentiates host portion and Network portion of an IP address. Networking devices perform Logical AND opertation in between Binary form of IP Address and Subnet Mask to determine whether a broadcast packet can be sent from one host to another.
</p>
<h3>Ipv4 Classifications:</h3>
<table>
<colgroup>
<col width="25%" />
<col width="40%" />
<col width="35%" />
</colgroup>
<thead>
<tr class="header">
<th>Class</th>
<th>IP range</th>
<th>Subnet mask</th>
</tr>
</thead>
<tbody>
<tr>
<td>Class A</td>
<td>0.0.0.0 - 127.255.255.255</td>
<td>255.0.0.0</td>
</tr>
<tr>
<td>Class B</td>
<td>128.0.0.0 - 191.255.255.255</td>
<td>255.255.0.0</td>
</tr>
<tr>
<td>Class C</td>
<td>192.0.0.0 - 223.255.255.255</td>
<td>255.255.255.0</td>
</tr>
<tr>
<td>Class D</td>
<td>224.0.0.0 - 239.255.255.255</td>
<td></td>
</tr>
<tr>
<td>Class E</td>
<td>240.0.0.0 - 255.255.255.255</td>
<td></td>
</tr>
</tbody>
</table>
