---
layout: post-wrapper
title:  "WiFi Band Steering"
date:   2020-02-11 15:18:00 +0530
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
<h3>WiFi Acronyms:</h3>
<table>
<colgroup>
<col width="35%" />
<col width="65%" />
</colgroup>
<thead>
<tr class="header">
<th>Acronyms</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>SSID</td>
<td>Service Set ID</td>
</tr>
<tr>
<td>WLAN</td>
<td>Wireless LAN</td>
</tr>
<tr>
<td>WAP</td>
<td>Wireless Access Point</td>
</tr>
<tr>
<td>WPA</td>
<td>WiFi Protect Access</td>
</tr>
</tbody>
</table>
<h3>WiFi Frequency, Band & Channel:</h3>
<p>
WiFi works in certain unlicensed frequency ranges within radio spectrum. Wifi frequency band are globally agreed to be used to reception/transmission.
</p>
<h3>2.4GHz Band Properties:</h3>
<p>
<ul>
<li>Speed – 450 or 600 Mbps </li>
<li>Longer wavelength</li>
<li>Wide Range</li>
<li>Can penetrate through solid objects</li>
<li>Same frequency band is ysed by devices like cordless phone, bluetooth earpiece etc</li>
<li>More Possibility of Congestions</li>
<li>Susceptibility to RF interference makes it less than ideal for industrial applications </li>
</ul>
</p>
<h3>5GHz Band Properties:</h3>
<p>
<ul>
<li>Speed – 1300 Mbps</li>
<li>Smaller wavelength compared to 2.4GHz</li>
<li>Shorter Range</li>
<li>Faster than 2.4 GHz Band</li>
<li>Can not penetrate through solid objects</li>
<li>Greater Bandwidth – can support more devices</li>
<li>Band allocation is less crowded – lesser interference</li>
<li>Ideal for commercial and industrial use</li>
</ul>
</p>
<h3>Single, Dual Band Routers:</h3>
<p>
Single Band Router can transmit a 2.4GHz signal, whereas a Dual Band Routers can transmit 2 signals, a 2.4Ghz signal and a 5GHz signal. The bandwidth and  maximum number of connected devices are higher in Dual Band. Dual band routers comes in simultaneous and selectable dual band variants. Tri Band Routers can broadcast two separate 5 Ghz and a single 2.5GHz signal into 3 different network.
</p>
<h3>Band Steering in WiFi:</h3>
<p>
Band Steering is a solution ensures that clients are connected to the best radio. Dual Band supported Gateway can transmit SSIDs in both 2.4GHz and 5GHz frequency band. Band Steering solution does upgrade steering ( from 2.4 GHz to 5 GHz) and downgrade steering (5 GHz to 2.4 GHz) which allows optimal utilization of both bands. The Steering of the Wireless clients are based on the following parameter,
</p>
<ul>
<li>    Wireless Client Capacity </li>
<li>    Signal Strength in current and target band</li>
<li>   Bandwidth Utilization in current and target band</li>
<li>   Steering History</li>
<li>   Wireless Client Activity (Inactive Client/ Active Client)</li>
</ul>
The purpose of Band Steering feature is to maintain Wireless Clients in band best suited to them. For instance, a 5GHz capable client would be maintained in faster & wider-channeled 5GHz band but if the Received Signal Strength is lesser than the threshold value, the Client is moved to a wider-range 2.4GHz band. 
</p>
