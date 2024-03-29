---           
layout: post
title: 'Personal VPN services are snake-oil'
date: 2024-03-01 10:19:41 UTC
comments: false
categories:  
---
# Intro
Personal VPN services are a multi-million dollar business. But is it worth your money?  

![NordVPN](/_img/VPN-1.png)  
If they can [afford to sponsor Pewdiepie](https://www.thoughtleaders.io/blog/how-much-money-does-pewdiepie-make), they should do something right, right?  

This is going to be a complex topic, so bear with me.  

The statement I would like to prove:  
There are only a few problems for a few people worldwide where the answer is using a personal VPN service. 
You don't need a personal VPN service if you are an average Internet user. 
If you are a tech-savvy user, you can do much better than using a VPN.  

# Glossary
By personal/commercial VPN service, I mean NordVPN, Tunnelbear, Mullvad VPN, etc. Company VPNs, or a VPN to phone into your home network, are out of the scope of this article.
By public Wi-Fi, I mean Wi-Fi services where it is either open or the WPA key is shared with everyone. From a security point of view, there is not much difference between the two. 

Let's break down the problem of privacy and security. Let's start with ...

# Security
Fifteen years ago, it was common malpractice that even though websites used HTTPS for the login page, the rest of the communication was over HTTP. One could steal the authentication cookie from the unencrypted Wi-Fi stream and use that to log in to that service. Tools like Surfjack and Firesheep were popular tools for wannabe hackers. Back then, using a personal VPN service at public Wi-Fi made sense. Also, back then, many email clients checked their email using unencrypted POP3 or IMAP protocols. 

But nowadays, there [is practically no vital service on the Internet](https://transparencyreport.google.com/https/overview), which does not run on HTTPS. 

OK, but what about tools like [SSLStrip?](https://github.com/moxie0/sslstrip)
SSLStrip works like this:
1. User types www.mybanklogin.com
2. The browser connects to clear-text, unprotected http://www.mybanklogin.com
3. Attacker having active man-in-the-middle ([MiTM](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)) capability sees this traffic
4. The web server notifies the user's browser that it should use the secure https://www.mybanklogin.com site
5. The attacker removes this notification
6. The user continues the login process over the insecure, unencrypted www.mybanklogin.com

So, can hackers steal my username and password? First, for accounts that matter, you should use 2FA (short for multi-factor authentication). It would be best to use unphisable 2FA like FIDO2/webauthn (e.g. Yubikey) for accounts that matter. 
Nowadays, most browsers implement ["HTTPS only mode"](https://therecord.media/eff-to-deprecate-https-everywhere-extension-as-https-is-becoming-ubiquitous). Next time, instead of turning on your VPN, you should check if your browser already uses HTTPS only mode, and if not, turn that on. For example, Safari does that by default. 
Ask your service provider to implement [HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security).
It is excellent about HSTS that the user cannot bypass certificate errors. And the browser will auto-connect to HTTPS by default. 
The good news is that if you use mobile apps, almost all are safe already. Just keep your mobile OS up-to-date. No one will spoil a precious [0-day](https://en.wikipedia.org/wiki/Zero-day_(computing)) to attack you. 
If you are worried about DNS MiTM, use [DNS over HTTP](https://support.mozilla.org/en-US/kb/dns-over-https) in your browser. 
OK, but what about tools like [Metasploit browser autopwn](https://www.rapid7.com/blog/post/2015/07/15/the-new-metasploit-browser-autopwn-strikes-faster-and-smarter-part-1/)? 
Well, the good news is that it is much easier to keep your browser up-to-date nowadays compared to ~ ten years ago. Nowadays, however, it is rare that recent browser exploits are openly available for any hacker to use. If you are a target of a government, spy agency, or hacker-for-hire services, yes, they may have the resources to hack your browser, but chances are you are not that important. Sorry. And if you are that important, VPNs will not decrease your risks.

One massive problem with personal VPN services is that they are working to fail open. If the connection fails, your connection is not "protected" anymore. Some premium VPN providers sell "kill switch" functionality, but I am sure less than 1% of the users use this properly. 

Looking at the practical side, how many legit stories do you know where someone lost money that could have been avoided just by using a VPN? I have never heard such a story. The attacker group called "Darkhotel" could have been such example, except that a VPN would not protect you against their attack, as they used fake Wi-Fi portals to deliver exploit/malware. And before logging into a hotel Wi-Fi portal, you can't start your VPN ...   

And before I forget, let's quickly discuss free VPNs. Never, ever install a free VPN solution. They will abuse your network connectivity, and other malicious or non-malicious users will use it to do whatever they want. It is like inviting one thousand unknown people to your house to have a party and leave them unattended. 

![VPN_ads](/_img/vpn-2.webp)

# Privacy concerns
OK, but what about my DNS and TLS records being exposed to everyone so they can follow what I am doing? In a public place, anyone can look at your display already. Or, if you are worried about your ISP selling your traffic data, there are better options for you. 
Use [DNS over HTTPS](https://developers.cloudflare.com/1.1.1.1/encryption/dns-over-https/encrypted-dns-browsers/), for example. 
You have to use a provider you trust better than your ISP/Wi-Fi provider/nearby Wi-Fi users.
Also, as [Encrypted Client Hello](https://support.mozilla.org/en-US/kb/understand-encrypted-client-hello) is about to start soon, it will be exponentially harder for eavesdroppers to figure out which sites you are trying to visit. 

But if you care about privacy, the answer is always ToR, ToR browser or Tails, and never VPN. Except in cases where you first have to hide your ToR usage using a VPN, which is a rare exception among users. If you don't understand why you would need that, you probably don't need that complexity. 
Tor Browser uses uncountable techniques that prevent tracking your browser. And if your privacy is essential against local Wi-Fi attackers, your ISP, why is the ad industry not in scope? Adblockers are only half the solution against tracking. 

# When to use a personal VPN?
1. geofence bypass for region-locked content
2. piracy? 
3. soft network bloc/censorship

# Conclusion
Claims made by VPN companies:  
1. VPNs reduce the risk of "getting hacked," whatever that means. 
This is not true. Additional services provided by VPNs, like URL filtering, can reduce your risk. However, it is more effective to use URL filtering on the endpoint level, not on the network level, as most VPN companies do. 

2. VPNs protect your privacy  
Even though they increase your privacy, it is still far from what ToR can do for you. 

If you want to increase your privacy and security, instead of watching VPN reviews on YouTube, I recommend that you patch your operating system, patch your browser, configure always HTTPS and DoH in your browser. Use an adblocker, or even better, a [browser built with privacy in mind](https://www.privacytools.io/private-browser). If you frequently install new applications, you should also use an antivirus/endpoint protection on your non-mobile OS. For Windows, the built-in, default Microsoft Defender is a good option. 


# What to read? 
I found these articles also useful. As this is a complex topic, and if you want to read more, this is where I recommend to start:  
https://overengineer.dev/blog/2019/04/08/very-precarious-narrative.html  
https://krebsonsecurity.com/2017/03/post-fcc-privacy-rules-should-you-vpn/  
https://gist.github.com/joepie91/5a9909939e6ce7d09e29  
https://www.spacebar.news/you-dont-need-a-vpn/  
https://www.forbes.com/advisor/business/vpn-statistics/  
https://it.slashdot.org/story/22/01/02/2143256/nbc-you-probably-dont-need-to-rely-on-a-vpn-anymore  


# PS  
Yes, I know that some of these VPN products provide extra services/functions/protections. And some of these extra make sense. But that does not mean that in overall, VPN is the answer for your problems. 
