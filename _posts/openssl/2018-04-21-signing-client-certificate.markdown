---
layout: post-wrapper
title:  "Signing certificates using CA"
date:   2018-04-21 13:24:40 +0530
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
To create a Root CA, click [here]({{ site.baseurl }}{% link _posts/openssl/2018-04-14-create-root-ca.markdown %}).
and to create a Subordinate CA click [here]({{ site.baseurl }}{% link _posts/openssl/2018-04-18-create-sub-ca.markdown %}).
<h3>Signing Certificates:</h3>
<p>
        To issue a certificates, an organisation or a user should create a private key and a CSR. This CSR is then used by the CA to issue a signed certificate. After signing, this certificate can then be used in any application where Authentication, confidentiality and integrity is needed. During verification of the certificate the complete CA chain (Root CA -> Sub CA1 -> Sub CA2 ) that was used to sign the certificate should be present.
    </p>
<p>
        When a Certificate is used on a Web Server, the Browser (Client) will receive the Certificate during the TLS handshake. The browser will then use its Trust Store (collection of Trusted CAs) to verify the Certificate. Only if the Certificate is verified by any of the CAs in the Trust Store, the session will be established. 
</p>

<h4>Creating Private key and CSR:</h4>
{% highlight bash %}
$ openssl genrsa -out dominname.key 2048
Generating RSA private key, 2048 bit long modulus
..............................................................................................................................................+++
....................................+++
e is 65537 (0x10001)

$ openssl req -key dominname.key -new -sha256 -out domainname.csr
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
Organization Name (eg, company) [Internet Widgits Pty Ltd]:test
Organizational Unit Name (eg, section) []:testunit
Common Name (e.g. server FQDN or YOUR name) []:www.domainname.com
Email Address []:user@domainname.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:   
{% endhighlight %}

<p>
After creating the CSR, the intermediate CA or the Sub CA can be used to sign it. 
</p>
<h4>Creating the Certificate:</h4>
<p>
<b>Note:</b><br>
The configurations in openssl.cnf file from the /ca/sub-ca/ directory should be used when the Sub CA needs to signs the Certificate.  
</p>
{% highlight bash %}
$ openssl ca -config /ca/sub-ca/openssl.cnf -days 356 -in ../domainname.csr -md sha256 -out domainname.pem
Using configuration from openssl.cnf
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 1 (0x1)
        Validity
            Not Before: Apr 21 16:04:28 2018 GMT
            Not After : Apr 12 16:04:28 2019 GMT
        Subject:
            countryName               = IN
            stateOrProvinceName       = Tamil Nadu
            localityName              = Chennai
            organizationName          = test
            organizationalUnitName    = testunit
            commonName                = www.domainname.com
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Comment: 
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier: 
                64:D9:73:A1:AD:CF:BF:4B:39:A6:0D:3E:58:F0:7C:1E:81:6B:B5:4B
            X509v3 Authority Key Identifier: 
                keyid:7C:AA:1A:B3:24:FE:70:7F:1C:27:94:7A:24:13:BB:45:24:6B:BE:D8

Certificate is to be certified until Apr 12 16:04:28 2019 GMT (356 days)
Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
{% endhighlight %}

<p>
Now to view and verify the certificate created, the below command can be used.
</p>
{% highlight bash %}
$ openssl x509 -in domainname.pem -noout -issuer -subject
issuer= /C=IN/ST=Tamil Nadu/O=lm/OU=Engineering/ \
                CN=www.lonelymansub.com/emailAddress=d_lonely_man_sub@lonely-man.com
subject= /C=IN/ST=Tamil Nadu/L=Chennai/O=test/  \
                OU=testunit/CN=www.domainname.com

$ openssl verify -CAfile <(cat root-ca/root_ca_cert.pem sub-ca/sub_ca.pem) domainname.pem 
domainname.pem: OK
{% endhighlight %}
<p>
The <em>index.txt</em> file contains the details of Certificates issued by a CA. This file can be used to look at the issued and revoked certificates.
</p>
{% highlight bash %}
$ cat index.txt
V  190412160428Z  01  unknown  /C=IN/ST=Tamil Nadu/L=Chennai/O=test/OU=testunit/CN=www.domainname.com
{% endhighlight %}

<p>
The serial file will contain an integer (eg. 01). Each time a certificate is issued by a CA, this integer is listed with more details in <em>index.txt</em> (as shown above). Later this number is incremented and saved in <em>serial</em> file again.
</p>
