---
layout: post-wrapper
title:  "Creating Subordinate CA"
date:   2018-04-18 23:24:40 +0530
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
If you have not created a Root CA, click [here]({{ site.baseurl }}{% link _posts/openssl/2018-04-14-create-root-ca.markdown %}) to see how to create it. 
<h3>Intermediate or Subordinate CA:</h3>
<p>
In a PKI, there can be multiple CAs and all the CAs are structured in a hierarchy or chain. The top level CA in the hierarchy is always called the Root CA whereas the CA below it is called Subordinate CA. The Root CA and the subordinate CAs form the chain of trust in a PKI.
</p>
<p>
The purpose of using an subordinate CA is primarily for security. The root key can be kept offline and used as infrequently as possible. If the intermediate key is compromised, the root CA can revoke the intermediate certificate and create a new intermediate cryptographic pair.
</p>
<h4>Creating directory structure:</h4>
While the Root CA files are kept in <em>ca/root-ca</em>, create a new directory <em>ca/sub-ca/</em> to hold the sub CA files . Then a few directories can be created as shown below.
{% highlight bash %}
$ mkdir -p ca/sub-ca
$ cd ca/sub-ca
$ mkdir certs newcerts private csr
$ touch index.txt serial
$ chmod 700 private
$ echo 01 > serial
{% endhighlight %}
<p>
Place another copy of openssl.cnf file in <em>/ca/sub-ca/</em> directory, this file will hold all the configurations and file locations of the sub CA. Just like the Root CA, the [ ca ] should have the sub-ca related preferences.
</p>
<pre>
<span class="o">    [</span> CA_default <span class="o">]</span>
<span class="comment">    # Directory & file path as per created directories & files above.</span>
<span class="nv">    dir</span>               <span class="o">=</span> <span class="sx">/ca/sub-ca</span>
<span class="nv">    certs</span>             <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/certs</span>
<span class="nv">    crl_dir</span>           <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/crl</span>
<span class="nv">    new_certs_dir</span>     <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/newcerts</span>
<span class="nv">    database</span>          <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/index.txt</span>
<span class="nv">    serial</span>            <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/serial</span>
<span class="nv">    RANDFILE</span>          <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/private/.rand</span>

<span class="comment">    # The root key and root certificate.</span>
<span class="nv">    private_key</span>       <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/private/sub_ca_key.pem</span>
<span class="nv">    certificate</span>       <span class="o">=</span> <span class="nv">$dir</span><span class="sx">/sub_ca.pem</span>
</pre>

Also the signing policy can be changed to <em>policy = policy_anything</em> as shown below. Doing so, the CA during signing will not have strict checking on Subject Name.

<pre>
<span class="nv">    policy</span>               <span class="o">=</span> <span class="sx">policy_anything</span>
</pre>

<h4>Creating Sub CA private key and CSR:</h4>
<p>
Modify the openssl.cnf file to set the <em>dir</em> and other values as per the created folder structure as per the /ca/sub-ca folder structure in you machine. After setting up the file and directory path for sub CA in the configuration file, a sub-ca private keys and a csr (Certificate Signing Request) can be created.
</p>

{% highlight bash %}
$ openssl genrsa -out private/sub_ca_key.pem 2048
........+++
.........................+++
e is 65537 (0x10001) 

$ openssl req -new -sha256 -key private/sub_ca_key.pem -out csr/sub_ca.csr 
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:IN
State or Province Name (full name) [Some-State]:Tamil Nadu
Locality Name (eg, city) []:Chennai
Organization Name (eg, company) [Internet Widgits Pty Ltd]:lm
Organizational Unit Name (eg, section) []:LMCA
Common Name (e.g. server FQDN or YOUR name) []:www.lonelyman-ca.com
Email Address []:d_lonely_man@lonely-man.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
{% endhighlight %}

<p>
To create a Certificate for Sub CA, the Root CA has to sign the Sub-CA CSR using the openssl.cnf file in the root CA directory. The challenge password option can be left empty. This option can be used to restrict access to someone, who can potentially revoke certificate, having a challenge password can be usefull in such scenarios.
</p>

<h4>Signing using Root CA</h4>
<p>
Signing of Sub CA has to be done by the Root CA pair using the openssl.cnf file of the Root CA as shown below.
</p>
<b>Note:</b> 
<p>
The configuration file used to sign the Sub-CA is the Root CA configuration file /ca/root-ca/openssl.cnf. This file will have all the required information that is used in the signing process. Also use of configuration file eliminates the necessity of specifying all the required options in the command line. 
</p>
{% highlight bash %}
$ cd /ca/root-ca
$ openssl ca -config openssl.cnf -in ../sub-ca/csr/sub_ca.csr \
                -out ../sub-ca/sub_ca.pem -md sha256
Using configuration from openssl.cnf
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 1 (0x1)
        Validity
            Not Before: Apr 21 12:33:47 2018 GMT
            Not After : Apr 21 12:33:47 2019 GMT
        Subject:
            countryName               = IN
            stateOrProvinceName       = Tamil Nadu
            organizationName          = lm
            organizationalUnitName    = Sub-Engineering
            commonName                = www.lonelymansub.com
            emailAddress              = d_lonely_man_sub@lonely-man.com
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Comment: 
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier: 
                7C:AA:1A:B3:24:FE:70:7F:1C:27:94:7A:24:13:BB:45:24:6B:BE:D8
            X509v3 Authority Key Identifier: 
                keyid:53:D1:27:13:5F:E8:FB:0C:C1:10:39:ED:AF:26:3D:6F:E7:7E:93:6A

Certificate is to be certified until Apr 21 12:33:47 2019 GMT (365 days)
Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
{% endhighlight %}

Now to view the Sub CA certificate created, the below command can be used.
{% highlight bash %}
$ openssl x509 -in ../sub-ca/sub_ca.pem -noout -issuer -subject
issuer= /C=IN/ST=Tamil Nadu/L=Chennai/O=lm/OU=Engineering/     \
                CN=www.lonely-man.com/emailAddress=d_lonely_man@lonely-man.com
subject= /C=IN/ST=Tamil Nadu/O=lm/OU=Sub-Engineering/           \
                CN=www.lonelymansub.com/emailAddress=d_lonely_man_sub@lonely-man.com
{% endhighlight %}

To verify the Sub CA certificate created, the below command can be used.
{% highlight bash %}
$ openssl verify -CAfile root_ca_cert.pem ../sub-ca/sub_ca.pem 
../sub-ca/sub_ca.pem: OK
{% endhighlight %}

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
