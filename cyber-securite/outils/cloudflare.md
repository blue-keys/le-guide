# 🇬🇧 Bypassing Cloudflare WAF with the origin server IP address | Detectify Blog

**This is a guest blog post from** [**Detectify Crowdsource**](https://cs.detectify.com) **hacker, Gwendal Le Coguic. This is a tutorial on how to bypass Cloudflare WAF with the origin server IP address.**

[_Detectify_](https://detectify.com) _collaborates with trusted ethical hackers to crowdsource vulnerability research that powers our cutting-edge web application security scanner. The Crowdsource community of hackers help us keep our ears to the ground in the security community to bring us details on active exploits in the wild._

[![](https://blog.detectify.com/wp-content/uploads/2019/07/Guest-Blog-Gwendal2-1024x683.png)](http://blog.detectify.com/wp-content/uploads/2019/07/Guest-Blog-Gwendal2.png)

Cloudflare is a widely used web app firewall (WAF) provider. But what if you could bypass all these protections in a second making the defense useless? This article is a tutorial on bypassing Cloudflare WAF with the origin server IP address.

Note that what is following is probably relevant for any kind of Web Application Firewall.

## Intro <a href="#intro" id="intro"></a>

With more than [16M Internet properties](https://www.cloudflare.com/case-studies/), Cloudflare is now one of the most popular web application firewalls (WAF). A year ago Cloudflare released a fast DNS resolver, which became the proverbial cherry on top of their service offering. Working as a reverse proxy, the WAF does not only offer a protection against DDOS but can also trigger an alert when it detects an attack. For paid subscriptions, users have the option to turn on protection against common vulnerabilities such as SQLi, XSS and CSRF, yet this must be manually enabled. This option is not available for free accounts.

While the WAF is pretty good at blocking basic payloads, many bypasses around Cloudflare WAF already exist and new ones pop up everyday so it’s important to keep poking at testing the security of Cloudflare. At the exact moment I am writing this article:

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-lastday.png)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-lastday.png)

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bypass-tweet.png)](https://twitter.com/le4rner/status/1146453980400082945?ref\_src=twsrc%5Etfw)

As a hacker bug bounty hunter, it’s obvious that it could be very interesting to get rid of the firewall. For that, you basically have 3 options:

1. Customize your payloads in order to bypass the rules in place. It can be interesting to improve your skills about firewall bypass but it can be a tedious and time-consuming task, which is not something you can afford when you’re a bug hunter – _time is prime!_ If you’re up for this option, you better try crazy payloads listed in [PayloadsAllThe Things](https://github.com/swisskyrepo/PayloadsAllTheThings) or search on Twitter.
2. Alter the requests in a proper way to disrupt the server. And as the same as first option, it can be time-consuming, requires patience and good fuzzing skills. [Soroush Dalili](https://twitter.com/irsdl) wrote a nice presentation which could help to create such requests by [Using HTTP Standard and Web Servers’ Behaviour](https://www.slideshare.net/SoroushDalili/waf-bypass-techniques-using-http-standard-and-web-servers-behaviour).
3. **Get around Cloudflare by finding the origin IP of the web server.** Probably the easiest option, no technical skills required, it’s also part of the recon process so no time wasted. As soon as you get it, you don’t have to worry anymore about the WAF or the DDOS protection (rate limit).

In this in this article, I’m going to focus on the last option and how to achieve it based on tips grabbed here and there.

**Reminder:** Cloudflare is a tool that has to be set by humans, usually developers or system administrators. Cloudflare is not responsible of the misconfiguration that could lead to successful attacks performed using the methods described below.

## But first, Recon! <a href="#recon-recon-recon" id="recon-recon-recon"></a>

The idea is to start your normal recon process and grab as many IP addresses as you can (host, nslookup, whois, [ranges](https://bgp.he.net)…), then check which of those servers have a web server enabled (netcat, nmap, masscan). Once you have a list of web server IP, the next step is to check if the protected domain is configured on one of them as a [virtual host](https://httpd.apache.org/docs/2.4/en/vhosts/examples.html). If not, you’ll get the default server page or the default website configured. If yes then you found the entry point! Using Burp:

**This show the subdomain I’m looking for but with the wrong IP address:**\
&#x20;[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-wrong-ip.jpg)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-wrong-ip.jpg)

**This shows the wrong subdomain, but with a good IP address:**\
&#x20;**** [![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-wrong-vh.jpg)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-wrong-vh.jpg)****\
****

**This shows the subdomain I’m looking for, but with a good IP address – perfect!**\
&#x20;[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-good.jpg)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-good.jpg)

**Some tools available to automate this process:**\
&#x20;[https://pentest-tools.com/information-gathering/find-virtual-hosts](https://pentest-tools.com/information-gathering/find-virtual-hosts)\
&#x20;[https://github.com/jobertabma/virtual-host-discovery](https://github.com/jobertabma/virtual-host-discovery)\
&#x20;[https://github.com/gwen001/vhost-brute](https://github.com/gwen001/vhost-brute)

## Censys <a href="#censys" id="censys"></a>

If your target has a SSL certificate (and it should!), then it’s registered in the [Censys](https://censys.io/certificates) database (I strongly recommend to subscribe). Choose “Certificates” in the select input, provide the domain of your target, then hit.

**You should see a list of certificates that fit to your target:**

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-censys-results-1024x576.png)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-censys-results.png)

**Click on every result to display the details and, in the “Explore” menu at the very right, choose “IPv4 Hosts”:**

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-censys-certificate-details-1024x576.png)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-censys-certificate-details.png)

**You should be able to see the IP addresses of the servers that use the certificate:**\
&#x20;**** [![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-censys-certificate-ipv4-1024x576.png)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-censys-certificate-ipv4.png)

From here, grab all IP you can and, back to the previous chapter, try to access your target through all of them.

The next step is to retrieve the headers in the mails issued by your target: Subscribe the newsletter, create an account, use the function “forgotten password”, order something… in a nutshell do whatever you can to get an email from the website you’re testing (note that Burp Collaborator can be used).

Once you get an email, check the source, and especially the headers. Record all IPs you can find there, as well as subdomains, that could possibly belong to a hosting service. And again, try to access your target through all of them.

The value of header `Return-Path` worked pretty well for me:

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-mail-headers.png)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-mail-headers.png)

Test using Curl:

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-curl-test.png)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-curl-test.png)

Another trick is to send a mail from your own mailbox to a non-existing email address @yourtarget.com. If the delivery fails, you should receive back a notification. Thanks to [@\_3P1C](https://twitter.com/\_3P1C/status/1113896360757977088).

## XML-RPC Pingback <a href="#xml-rpc-pingback" id="xml-rpc-pingback"></a>

This well known tool in WordPress, the XML-RPC (Remote Procedure Call), allows an administrator to manage his/her blog remotely using XML requests. A pingback is the response of a ping. A ping is performed when a site A links to a site B, then the site B notifies the site A that it is aware of the mention. This is the pingback.

You can easily check if it’s enable by calling `https://www.target.com/xmlrpc.php`. You should get the following:`XML-RPC server accepts POST requests only.`

According to [WordPress XML-RPC Pingback API](https://codex.wordpress.org/XML-RPC\_Pingback\_API), the functions takes 2 parameters `sourceUri` and `targetUri`. Here is how it looks like in Burp Suite:

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-xmlrpc-1024x576.jpg)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-bs-xmlrpc.jpg)

Credit to [@Rivitheadz](https://twitter.com/Rivitheadz/status/1114098397051539456).

## Previous findings <a href="#previous-findings" id="previous-findings"></a>

If you’re not able to find the origin IP using the previous methods or if the website was not protected when you first started your hunt but finally became protected, remember that sometimes your best friend is your target itself and it can give you the information you are looking for.

Basically what you need is that the web server of your target performs a request to your server/collaborator. Using another type of issue could also be a good idea: SSRF, XXE, XSS or whatever you already found, to inject a payload that contains your server/collaborator address and check the logs. If you got any hit then check the virtual host again.

Even the simplest vulnerabilities like Open Redirect or HTML/CSS injection can be useful if it’s interpreted by the application web server.

For now we have seen how to find and check IP addresses manually, fortunately we have great developers in our community. Below are some tools that are supposed to do the job for you, and these could save your precious time. You can include them in your recon process as soon as you detect a Cloudflare protection.

Note, that none of these methods are 100% reliable as all targets are different and what will work for one, may not work for another. My advice: try them all.

[HatCloud](https://github.com/HatBashBR/HatCloud): crimeflare, ipinfo.io\
&#x20;[CrimeFlare](http://www.crimeflare.org:82/cfs.html): crimeflare, ipinfo.io\
&#x20;[bypass-firewalls-by-DNS-history](https://github.com/vincentcox/bypass-firewalls-by-DNS-history): securitytrails, crimeflare

[CloudFail](https://github.com/m0rtem/CloudFail): dnsdumpster, crimeflare, subdomain brute force\
&#x20;[CloudFlair](https://github.com/christophetd/CloudFlair): censys key required\
&#x20;[CloudIP](https://github.com/Top-Hat-Sec/thsosrtl/blob/master/CloudIP/cloudip.sh): nslookup some subdomains (_ftp, cpanel, mail, direct, direct-connect, webmail, portal_)

[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-cloudip.jpg)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-cloudip.jpg)[![](https://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-cloudfail.jpg)](http://blog.detectify.com/wp-content/uploads/2019/07/cloudflare-cloudfail.jpg)

**Further reading:**

* [Introducing CFire – Evading CloudFlare Security Protections](https://rhinosecuritylabs.com/cloud-security/cloudflare-bypassing-cloud-security/)
* [Cloudflare Bypass Security](http://www.securityidiots.com/Web-Pentest/Information-Gathering/Cloudflare-Bypass/Part-2-Cloudflare-Security-Bypass.html)
* [CloudFlair – Bypassing Cloudflare using Internet-wide scan data](https://blog.christophetd.fr/bypassing-cloudflare-using-internet-wide-scan-data/)
* [Cloudflare IP ranges](https://www.cloudflare.com/ips/)
* [One tweet about it](https://twitter.com/gwendallecoguic/status/1043484797799223297)
* [And another tweet about it](https://twitter.com/gwendallecoguic/status/1113777240876228609)

**Written by:**

**Gwendal Le Coguic**\
**** Bug Bounty Hunter

**Twitter:** [@gwendallecoguic](https://twitter.com/gwendallecoguic)\
&#x20;**Blog:** [http://10degres.net](http://10degres.net)

**Detectify collaborates with 150 handpicked white hat hackers like Gwendal Le Conguic to** [**crowdsource**](https://cs.detectify.com) **vulnerability research for our automated web application scanner. Check the security status of your websites using our test bed of 1500+ known vulnerabilities.** [**Sign up for Detectify and start your free 14-day trial today!**](https://detectify.com/createaccount)
