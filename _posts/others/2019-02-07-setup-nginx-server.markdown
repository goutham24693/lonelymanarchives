---
layout: post-wrapper
title:  "Set up a local nginx Web Server"
date:   2019-02-12 00:00:40 +0530
categories: jekyll update
---

<h3>What is nginx?</h3>
<p>
NGINX is an opensource webserver with features like load-balancing, reverse proxy and other cache mechanisms. Originally, Nginx was developed to solve the C10k problem (network socket optimization problems for larger connections).
</p>

<h3>NGINX vs Apache:</h3>
<p>
Nginx has lesser memory footprint comapred to Apache web server, and can handle larger number of incoming requests than Apache. Apache server are stable in Windows machines where nginx still suffers from stability issues.
</p>
<h3>nginx Installation - Ubuntu:</h3>
{% highlight bash %}
$ apt-get update

$ apt install nginx
{% endhighlight %}

<h3>Run your webpages on a nginx:</h3>
<p>
    nginx configuration file can be found in <i>/etc/nginx/sites-enabled/default</i>. Configuration of the web server and the document root can be done there. All the html files can be added in the document root - <i>/etc/nginx/sites-enabled/default</i> and nginx server can be restarted.
</p>
{% highlight bash %}
$ touch index.html

$ echo "Hello World" > index.html

$ sudo service nginx start

$ curl localhost:80/index.html
Hello World
{% endhighlight %}


