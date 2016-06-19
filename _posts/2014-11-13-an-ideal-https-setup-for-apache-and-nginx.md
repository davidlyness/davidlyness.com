---
layout: post
title: An Ideal HTTPS setup for Apache and Nginx
excerpt: Up-to-date Apache and Nginx SSL/TLS configurations to secure your website and protect your visitors using HTTPS.
redirect_from: /ideal-https-configuration-nginx-web-servers
---

Now more than ever, it’s imperative that we know when we’re using the Internet securely. With an ever-increasing amount of business being conducted online, even a small issue with the way a web server is configured could result in untold financial and reputational damage for a company, or a breach of someone’s most private information.

It’s no surprise, then, that usage of SSL/TLS (via HTTPS connections) has exploded in recent years. In fact, Google Chrome users have more than doubled their use of HTTPS  in the last 2 years, with 27% of all page loads being conducted over HTTPS in 2013 up to 58% in 2014.

![Chrome HTTPS usage](assets/images/Chrome-HTTPS-navigations.png)

However, HTTPS connections are only as good as the technology they’re based on. IT professionals are doing what they can to keep up with the ever-changing security landscape, but with vulnerabilities such as [CRIME](https://en.wikipedia.org/wiki/CRIME), [BREACH](https://en.wikipedia.org/wiki/BREACH_(security_exploit)), [Lucky 13](http://www.isg.rhul.ac.uk/tls/Lucky13.html), [BEAST](https://blog.qualys.com/ssllabs/2013/09/10/is-beast-still-a-threat), [Heartbleed](http://heartbleed.com/) and [POODLE](https://en.wikipedia.org/wiki/POODLE) coming to light with alarming regularity, administrators can just about finish patching their fleets of servers before the next issue is identified.

In fact, although many people running servers *want* to protect themselves, they don’t necessarily have the in-depth knowledge of the underlying security of their systems to make putting in the effort to investigate worthwhile. And since packages like Apache and Nginx let administrators do just about anything (and by default don’t expose the options which would make their users’ servers more secure), many websites out there are putting both their security and their visitors’ security at risk.

Here are two things that server administrators can do *today* to secure themselves.

1. Add SSL/TLS HTTPS functionality.
2. Secure the setup.

## Add SSL/TPS HTTPS functionality

This part is easier than you might think. While SSL/TLS certificates tend to be in the region of $100 to $200 per year (and EV certificates can be up to $1,000), [StartSSL](https://www.startssl.com/) offer a free certificate for basic use. It’s what this site uses, and I’ve never had any trouble with browser or server compatibility.

## Secure the setup

It is *extremely* easy to misconfigure an SSL/TLS setup. Thankfully, most of the hard work has been done for you! Others have delved deep into the bowels of web server security configuration, and have shared templates for the world to benefit from. Sample Apache configurations can be found all over the internet. For example, this server used something similar to the following before switching to Nginx:

{% highlight plaintext %}
<VirtualHost *:443>
 SSLEngine on
 SSLCertificateFile signed-cert-bundle.crt
 SSLCertificateChainFile intermediate-cert.crt
 SSLCertificateKeyFile davidlyness.com.pem
 SSLCACertificateFile /path/to/all/ca-certs

 SSLProtocol all -SSLv2 -SSLv3
 SSLCipherSuite ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS
 SSLHonorCipherOrder on
 SSLCompression off

 SSLUseStapling on
 SSLStaplingResponderTimeout 5
 SSLStaplingReturnResponderErrors off
 SSLStaplingCache shmcb:/var/run/ocsp(128000)

 Header add Strict-Transport-Security "max-age=31536000; includeSubdomains"
 
 ...
 {% endhighlight %}

 Unfortunately, securing Nginx has been seen as more of an academic exercise – designed to get the highest score on online tests such as [SSL Labs](https://www.ssllabs.com/ssltest/) rather than to cater to the widest range of browsers and operating systems while still remaining secure. A template for Nginx is as follows:

 {% highlight plaintext %}
add_header Strict-Transport-Security "max-age=31536000; includeSubdomains";
ssl_certificate signed-cert-bundle.crt;
ssl_certificate_key davidlyness.com.pem;
ssl_dhparam dhparam.pem;
ssl_session_timeout 5m;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS;
ssl_session_cache shared:SSL:50m;
ssl_stapling on;
ssl_stapling_verify on;
ssl_trusted_certificate unified-cert-bundle.crt;
resolver 8.8.8.8 8.8.4.4;
if ($scheme = http) {
 return 301 https://$host$request_uri;
}
 {% endhighlight %}

Check out the [configuration on GitHub](https://gist.github.com/davidlyness/b528684fcb79d954efae0d04558341c0), where you can find an always-up-to-date version to make sure you stay current as the security landscape evolves. You can also find a thorough explanation for what is included in the configuration and why.

This website is run on an Nginx server using the above configuration, which manages to achieve an [A+ rating on SSL Labs](https://www.ssllabs.com/ssltest/analyze.html?d=davidlyness.com) while still maintaining support for the widest possible range of browsers. The only ones that are incompatible with this configuration are Java 6 (which was EOL’ed in February 2003) and IE6/IE8 on Windows XP (which was EOL’ed in April 2014).
