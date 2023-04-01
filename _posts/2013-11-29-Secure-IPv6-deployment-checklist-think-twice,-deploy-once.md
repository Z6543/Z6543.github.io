---           
layout: post
title: Secure IPv6 deployment checklist think twice, deploy once
date: 2013-12-10 10:47:39 UTC
updated: 2023-04-01 10:47:39 UTC
comments: false
categories: checklist IPv6 security xx
---
<div style="text-align: justify;">
Before deep-diving into IPv6, I'm going to summarize my views on IPv6:</div>
<ol>
<li style="text-align: justify;">Start now(!) to plan your IPv6 deployment - in case you have not done so.</li>
<li style="text-align: justify;">IPv6 is the future! There is no other choice.</li>
<li style="text-align: justify;">NAT is the root of all evil.</li>
</ol>
<div>
<div style="text-align: justify;">
IPv6 is coming. We've heard this 10 years ago as well, but it is really coming now. If you live in Asia, you can see this in real life. If you live in Europe or the USA, you can feel the pain of triple NATed devices.</div>
</div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
Deploying IPv6 is not a binary task, you can do it in small steps. Get a public IPv6 address space, assign IPv6 addresses to your servers (e-mail, web, DNS).</div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
Great, you are now ahead of the world!</div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
Now, what about clients who want to access IPv6 web or email? You are lucky if you use an IPv6 compatible web/email proxy because the clients can still stay on IPv4.</div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
Now, what about your clients/servers accessing IPv6 services on the Internet directly (without a proxy)?</div>
<ul>
<li style="text-align: justify;">If your network supports IPv6, your clients can directly access IPv6 services.&nbsp;</li>
<li style="text-align: justify;">If your network does not support IPv6, clients can still use tunneling protocols, e.g. ISATAP for Windows clients.</li>
</ul>
<div style="text-align: justify;">
This might sound easier than it is. But do you have the budget/people/management support to do this? </div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
Anyways, before you deploy IPv6 in your corporate environment, you have to think about the security issues. The next list is far from comprehensive or complete. Beware, here be dragons!</div>
<div>
<div style="text-align: justify;">
<br /></div>
</div>
<div style="text-align: justify;">
It is a myth that IPv6 is more secure than IPv4. The only practical security addition in IPv6 is called IPSEC, and has been backported to IPv4 many years ago.</div>
<div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
Most networking people are not familiar with the new terms, best practices introduced with IPv6. It is important to educate the networking and security personnel about IPv6, and let them play with the technology.</div>
<ol>
</ol>
<div style="text-align: justify;">
IPv6 can reintroduce basically all security vulnerabilities which has been mitigated in IPv4:</div>
<ol>
</ol>
<ul>
<li style="text-align: justify;">If the vulnerability is not in the IP layer, the vulnerability will stay there. Although it sounds obvious, most people tend to forget this. Your MS08-067 vulnerability won't magically disappear if you use IPv6. OK, but you closed port 445 on the firewall. Oh crap, it is open on IPv6...</li>
<li style="text-align: justify;"><b>Man-in-the-middle attacks:</b> Organizations have already guarded themselves against ARP spoofing and DHCP server spoofing with mitigation techniques like DAI (Dynamic ARP Inspection) and DHCP snooping in the world of IPv4. The only thing which has been changed is the name of the attacks and defenses. Search for ICMPv6 attacks, and Neighbour Discovery attacks.</li>
<li style="text-align: justify;"><b>Source routing:</b> Source routed IPv4 packets have not been seen for a while bypassing firewalls, but "luckily" the IPv6 standard reintroduced source routing in IPv6, in the name of Type 0 Router Headers. You should check whether your OS/network equipment support Type 0 Router Headers. If you have time, play with Scapy to test it.</li>
<li style="text-align: justify;"><b>Blocking ICMP:</b> Let's face the truth: In order to effectively use IPv6, some ICMPv6 messages has to traverse through firewalls. Like it or not, you can't hide your clients anymore.</li>
<li style="text-align: justify;"><b>Router advertisement:</b> Let's play a game. Go into any network, and start broadcasting IPv6 router advertisement packets in the network. Guess what, the clients are going to connect to you, and you will be the default IPv6 gateway for the clients. If there is no web proxy configured on the client, this means all Google, Youtube and Facebook traffic will be routed through your box, because IPv6 is preferred over IPv4. This can be the next generation WPAD attack :) If you don't want to be vulnerable, better search for RAGuard or similar solutions.</li>
<li style="text-align: justify;"><b>Translation/tunneling:</b> If all your network equipment support&nbsp;IPv6&nbsp;and configured to use it, you are lucky, because your&nbsp;clients can use native IPv6. Otherwise, your clients have to use 6to4/ISATAP/Teredo/NAT64 or whatever IPv6 translation/tunneling. Do you know these protocols? Do your security products know these protocols?</li>
<li style="text-align: justify;"><b>Dual stack:</b> Your clients have to use both IPv4 and IPv6 together for a while. Be aware of the additional operating costs of maintaining 2 network protocol parallel.</li>
<li style="text-align: justify;"><b>Fragmentation:</b> In the first era of IDS/IPS devices, one of the main bypassing technique was IPv4 fragmentation. Have you checked whether your current solutions (IPS/IDS) parse fragmented packets the same way as your protected assets?</li>
<li style="text-align: justify;">The IPv4&nbsp;implementations have been tested thoroughly, and not much implementation bug is left there. But IPv6 implementation bugs, there will be a lot from them. Most of the programmers still don't know the basics of secure coding practices.</li>
<li style="text-align: justify;"><b>IPv6 evercookie:</b> Now that your clients don't have the same NAT IP, they static IP can be used to track them online. Luckily Microsoft solved the issue with IPv6 privacy extension, so clients will use temporarily IPv6 addresses to communicate with the Internet. Great, now how do you know in your security logs, which IPv6 address is which client???</li>
</ul>
<div style="text-align: justify;">
Do your current security devices fully support IPv6? Have you tested this?</div>
<div>
<ul><ul>
</ul>
<li style="text-align: justify;">Hardware Firewall (with all dual-stack/translation/tunneling nightmare)</li>
<ul>
<li style="text-align: justify;">Do you block all outgoing UDP from the clients, especially 3544?</li>
<li style="text-align: justify;">Can you maintain rules for both IPv4 and IPv6?</li>
<li><span style="text-align: justify;">If you remove an IPv4 rule, do you remove the same rule for IPv6?</span></li>
<ul>
</ul>
</ul>
<li style="text-align: justify;">Log collection, log analysis, correlation:</li>
<ul>
<li style="text-align: justify;">Do the logs contain IPv6 addresses?&nbsp;</li>
<li style="text-align: justify;">Is it parsed by the SIEM application?&nbsp;</li>
<li style="text-align: justify;">Do you know that the DHCP IPv4 address is the same asset (workstation) as the IPv6 address with a privacy extension?&nbsp;</li>
</ul>
<li><span style="text-align: justify;">IPS, IDS:</span></li>
<ul>
<li><span style="text-align: justify;">Can these applications monitor/block all kinds of IPv6 translation/tunneling protocols?</span></li>
</ul>
<li style="text-align: justify;">WAF:</li>
<ul>
<li style="text-align: justify;">Is your WAF IPv6&nbsp;ready/aware?</li>
<li style="text-align: justify;">What about fragmentation?</li>
</ul>
<li style="text-align: justify;">VPN:</li>
<ul>
<li style="text-align: justify;">Can your clients connect to IPv6 services through VPN?</li>
<li style="text-align: justify;">Is your IPv6 VPN firewall rule as restrictive as the IPv4 one?</li>
<li style="text-align: justify;">If you remove a rule on IPv4, do you remove the same in IPv6?</li>
</ul>
<li style="text-align: justify;">Software firewall:</li>
<ul>
<li style="text-align: justify;">Is your software firewall IPv6 ready/aware?&nbsp;</li>
<li style="text-align: justify;">Does it understand all translation/tunneling protocols?&nbsp;</li>
<li style="text-align: justify;">Does it filter incoming traffic to the Teredo interface?</li>
</ul>
<ul>
</ul>
<li style="text-align: justify;">AV/endpoint protection:</li>
<ul>
<li style="text-align: justify;">Is your AV/endpoint protection IPv6&nbsp;ready/aware?</li>
<li style="text-align: justify;">What about files downloaded via IPv6, is it scanned the same way as IPv4?</li>
</ul>
<li><span style="text-align: justify;">DLP (if you are in the 1% who actually bought one and enrolled it in production):</span></li>
<ul>
<li><span style="text-align: justify;">Is your DLP IPv6&nbsp;ready/aware?&nbsp;</span></li>
<li><span style="text-align: justify;">Can it monitor tunneled traffics?</span></li>
</ul>
</ul>
</div>
<div>
<div style="text-align: justify;">
And last but not least, the bad news: you are already using IPv6 if you have Windows7 notebooks leaving your company, not having access to the domain controller. Or have you checked your servers link-local IPv6 addresses recently? Do you have a Windows2008 cluster? It is using IPv6 already!</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
</div>
<div>
<div style="text-align: justify;">
The good news? In the not so far future (I estimate 2050) you can turn off your last device with IPv4 address, and forget all the nightmare around NAT and dual-stack. Because remember: NAT is not a security feature. It never was.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
Useful links:</div>
<div style="text-align: justify;">
<a href="https://www.thc.org/thc-ipv6/" target="_blank">https://www.thc.org/thc-ipv6/</a></div>
<div style="text-align: justify;">
<a href="https://www.usenix.org/legacy/events/lisa11/tech/slides/babiker.pdf" target="_blank">https://www.usenix.org/legacy/events/lisa11/tech/slides/babiker.pdf</a></div>
</div>
<div style="text-align: justify;">
<a href="http://www.cisco.com/en/US/prod/collateral/iosswrel/ps6537/ps6554/ps6599/ps10800/white_paper_c11-629044.pdf" target="_blank">http://www.cisco.com/en/US/prod/collateral/iosswrel/ps6537/ps6554/ps6599/ps10800/white_paper_c11-629044.pdf</a></div>
<div style="text-align: justify;">
<a href="http://www.cisco.com/web/about/security/security_services/ciag/documents/v6-v4-threats.pdf" target="_blank">http://www.cisco.com/web/about/security/security_services/ciag/documents/v6-v4-threats.pdf</a></div>
<div style="text-align: justify;">
<a href="http://www.secdev.org/conf/IPv6_RH_security-csw07.pdf" target="_blank">http://www.secdev.org/conf/IPv6_RH_security-csw07.pdf</a></div>
<div style="text-align: justify;">
<br /></div>
</div>
