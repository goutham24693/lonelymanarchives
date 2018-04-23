---
layout: post-wrapper
title:  "Create the Root CA"
date:   2018-04-14 23:24:40 +0530
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

<h3>The Root CA:</h3>
<p>
The Certificate Authority is an entity that deals with a pair of public-private keys. In a X.509 system, a Certificate holds a public key, so creating a CA involes creating of a private key and a Certificate that holds the matching public key.
</p>
<p>
A Root CA forms basis of any X.509-based Public Key Infrastructure. Root CA comprises of a Root CA Certificate and a private key, where the Root CA Certificate is self-signed i.e the issuer and the subject of the Certificate are same. Ideally, the Root CA should be created in a secure environment since a breach in the system can compromise the whole PKI network.
</p>
<h4>Creating directory structure:</h4>
Create a directory <em>ca/root-ca</em> to save all the Root CA files and configuration here. Then a few directories can be created as shown below.
{% highlight bash %}
$ mkdir -p ca/root-ca
$ cd ca/root-ca
$ mkdir certs newcerts private crl
$ touch index.txt serial
$ chmod 700 private
$ echo 01 > serial
{% endhighlight %}
<p>
Openssl commands will use a configuration file to do signing and issuing process. A copy of openssl.cnf file can be copied to the <em>ca/root</em> path from your linux distro. (<em>'locate openssl.cnf'</em> will return the file path). A few changes has to be done in order to get the openssl commands working to create keys and issue certificates.
</p>
<p>
A openssl.cnf file is divided into many sections. Each section start with [section-name] and ends when a new section starts. Each section in a configuration file consists of a number of name and value pairs of the form "name=value". For CA configurations [ ca ] section has related preferences.
</p>
<pre>
<span class="o">    [</span> ca <span class="o">]</span>
<span class="nv">    default_ca</span> 	<span class="o">=</span> <span class="sx">CA_default</span>		<span class="comment"># The default ca section</span>
<br><span class="o">    [</span> CA_default <span class="o">]</span>
<span class="comment">    # Directory & file path as per created directories & files above.</span>
<span class="nv">    dir</span>               <span class="o">=</span> <span class="sx">/ca/root-ca</span>
<span class="nv">    certs</span>             <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/certs</span>
<span class="nv">    crl_dir</span>           <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/crl</span>
<span class="nv">    new_certs_dir</span>     <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/newcerts</span>
<span class="nv">    database</span>          <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/index.txt</span>
<span class="nv">    serial</span>            <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/serial</span>
<span class="nv">    RANDFILE</span>          <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/private/.rand</span>

<span class="comment">    # The root key and root certificate.</span>
<span class="nv">    private_key</span>       <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/private/root_ca_key.pem</span>
<span class="nv">    certificate</span>       <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/root_ca_cert.pem</span>

</pre>

<p>
Modify the openssl.cnf file to set the <em>dir</em> and other values as per the created folder structure, this will help the openssl command line tool to find required files without specifying explicitly. After setting up the file and directory path for CA in the configuration file, CA certificate and private keys can be created.
</p>

{% highlight bash %}
$ openssl genrsa -out private/root_ca_key.pem 2048
........+++
.........................+++
e is 65537 (0x10001) 

$ openssl req -key private/root_ca_key.pem \
        -new -x509 -days 7300 -sha256 \
        -out root_ca_cert.pem

You are about to be asked to enter information that will be incorporated
into your certificate request. 
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.

Country Name (2 letter code) [AU]:IN
State or Province Name (full name) [Some-State]:TN
Locality Name (eg, city) []:Chennai
Organization Name (eg, company) [Internet Widgits Pty Ltd]:lm
Organizational Unit Name (eg, section) []:Engineering
Common Name (e.g. server FQDN or YOUR name) []:www.lonely-man.com 
Email Address []:d_lonely_man@lonely-man.com
{% endhighlight %}

<p>
The Root CA Certificate is a self signed Certificate i.e the issuer is same as the subject. The private key of the Root CA is used only to sign only Intermediate CAs. It is always a good practice to have two Intermediate CAs and issue certificates using one of the two Intermediate CAs. The other Intermediate CA is used only in case the first Intermediate CA is compromised. Meanwhile the private key of Root CA is kept offline.
</p>
The created Root CA certificate can be viewed by the below command.
{% highlight bash %}
$ openssl x509 -in root_ca_cert.pem -noout -subject -issuer
subject=/C=IN/ST=TN/L=Chennai/O=lm/OU=Engineering       \
             /CN=www.lonely-man.com/emailAddress=dlonelyman@lonely-man.com
issuer= /C=IN/ST=TN/L=Chennai/O=lm/OU=Engineering      \
             /CN=www.lonely-man.com/emailAddress=dlonelyman@lonely-man.com
{% endhighlight %}


[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
