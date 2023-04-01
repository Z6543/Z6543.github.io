---           
layout: post
title: DNSSEC, from an end-user perspective, part 1
date: 2013-12-10 10:49:33 UTC
updated: 2023-04-01 10:49:33 UTC
comments: false
categories: DNS hijack DNS spoofing DNSSEC xx
---
<div>
<div style="text-align: justify;">
We all know since at least 1990 that the DNS protocol is insecure. Yet DNS is still the basis of almost all Internet communication. The biggest problem with DNS is that a malicious attacker can redirect victims, where victims try to connect to, e.g. safesite.com, but instead of this, they relate to the attacker's website. There are a lot of different ways to achieve this attack.</div>
<h2 style="text-align: justify;">
DNS cache poisoning the DNS server, "Da Old way."</h2>
<div>
<div style="text-align: justify;">
The attacker forces the victim's DNS server to resolve a domain where the attacker is the authoritative name server, and the attacker sends additional information to the DNS server that he is also the authoritative name server for safesite.com. This information is cached and used later when the victim resolves safesite.com.</div>
</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
</div>
<div>
<div style="text-align: justify;">
It is easy to protect against this attack, any decent DNS server should be able to protect against this attack by dropping this additional information from DNS responses.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
</div>
<div>
<h2 style="text-align: justify;">
DNS cache poisoning, "<a href="http://www.iana.org/about/presentations/davies-viareggio-entropyvuln-081002.pdf">Da Kaminsky way</a>"</h2>
<div>
<div style="text-align: justify;">
An attacker can initiate queries to the victim's DNS server, and in the meantime, "flood" the DNS server with fake responses. The server will accept the fake response if the following conditions are right:&nbsp;</div>
</div>
<ul>
<li style="text-align: justify;">the DNS server receives the answers on the same IP as it expects (this is not an attack limitation),&nbsp;</li>
<li style="text-align: justify;">the DNS server receives the answer to his question (again not a limitation, an attacker knows what he wants to attack),</li>
<li style="text-align: justify;">the random(?) port used to send the query matches the answer,&nbsp;</li>
<li style="text-align: justify;">the random(?) 16-bit(!) unique transaction number in the answer matches with the query.</li>
</ul>
<div style="text-align: justify;">
If the last two are not random, an attacker can easily poison the server in a matter of seconds. Or if your DNS server is behind NAT, the NAT can remove the randomness :-O</div>
<ul><ul>
</ul>
</ul>
<div style="text-align: justify;">
To protect yourself against this, you should randomize the source port and unique identifier better, and disable your DNS server as being an open recursive resolver.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
<h2 style="text-align: justify;">
Rogue DNS server via malware</h2>
<div style="text-align: justify;">
This is pretty common in both banking trojans, adware, etc. The malware reconfigures the users configured DNS server to use a malicious one, either in the local OS settings or in the home router.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
</div>
<div>
<div style="text-align: justify;">
To protect against this: don't let malware in your environment ;)</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
<h2 style="text-align: justify;">
ISP hijack, for advertisement or spying purposes</h2>
<div style="text-align: justify;">
This is the worst of all. ISP's hijack non-existing domains, and display advertisements. And because they broke the RFC, it will break a lot of your applications. I bet ISPs still think about this as a good idea :(</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
</div>
<div>
<div style="text-align: justify;">
Sometimes the ISP hijacks your connection because the government wants to spy on your communication or divert it. Use <a href="https://www.torproject.org/download/download.html.en#Warning">TOR</a> <a href="http://security.stackexchange.com/a/43485">wisely</a> if you don't want to be a victim.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
<h2 style="text-align: justify;">
Captive portals</h2>
<div style="text-align: justify;">
Sometimes captive portals use DNS hijacking to redirect the users to the default login page. Although it breaks things, it is still better than the ISP solution, because it is only a problem at the beginning of the connection, and not permanently.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
<h2 style="text-align: justify;">
Pentester hijacks DNS to test application via active man-in-the-middle</h2>
<div style="text-align: justify;">
Although this is not malicious at all (developers might think otherwise), but a lot of application security tests (thick clients, mobile applications) can only be carried out via DNS spoofing. Application security testers can easily make this attack because they can place the host machine in a network segment where they can make an active man-in-the-middle attack and respond to DNS queries sooner as the official DNS server. Or simply set the DNS server to their own DNS server.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
<h2 style="text-align: justify;">
Malicious attacker hijacks DNS via active MITM</h2>
<div style="text-align: justify;">
This is similar to the previous one, but the goal is different. Although an attacker with active man-in-the-middle position can attack the victim in 1000 different ways (like SMB relaying, capturing LM/NTLM network hashes, modifying clear-text traffics, etc...), DNS spoofing is one of them.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
<h2 style="text-align: justify;">
Having access to the DNS admin panel and rewriting the IP</h2>
</div>
<div>
<div style="text-align: justify;">
I believe this is going to be a <a href="http://www.zdnet.com/how-the-syrian-electronic-army-took-out-the-new-york-times-and-twitter-sites-7000019989/">new trend</a>. This attack is the most effective one, and the hardest to prevent. Usually, it is not the admin panel that is being hacked, instead an admin's password gets stolen, or the password reset is hacked.</div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
You, as a client, can do three things to protect against this attack:</div>
</div>
<div>
<ol>
<li style="text-align: justify;">Choose a decent provider and prey :)</li>
<li><div style="text-align: justify;">
Put a registry lock on your DNS settings (serverDeleteProhibited, serverTransferProhibited, and serverUpdateProhibited). Although this additional layer is good enough today, I bet some motivated attackers will be able to successfully attack this. The attacker only has to:</div>
<ul>
<li style="text-align: justify;">know the phone number of the client, which will be called by the registrar</li>
<li style="text-align: justify;">hijack the call (social engineering the phone company)</li>
<li style="text-align: justify;">provide an individual security phrase (social engineering the client)</li>
</ul>
</li>
<li style="text-align: justify;">Or run your own DNS server (this is not a solution for everybody)</li>
</ol>
<div style="text-align: justify;">
<h2>
Conclusion&nbsp;</h2>
Although almost all attacks can be mitigated one way or another, there was a desperate need to solve this DNS spoofing issue once and for all. And so it begins, DNSSEC has been developed.</div>
<div style="text-align: justify;">
<br /></div>
<div style="text-align: justify;">
In <a href="http://jumpespjump.blogspot.hu/2014/01/dnssec-from-end-user-perspective-part-2.html">part 2, </a>we are going to investigate DNSSEC, what you can expect from it, and what you can't.</div>
</div>
<div>
<div style="text-align: justify;">
<br /></div>
</div>
