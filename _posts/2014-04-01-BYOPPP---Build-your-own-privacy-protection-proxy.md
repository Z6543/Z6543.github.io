---           
layout: post
title: BYOPPP - Build your own privacy protection proxy
date: 2014-04-01 13:57:28 UTC
updated: 2019-10-08 13:57:28 UTC
comments: false
categories: openvpn privacy privoxy
---
<div class="separator" style=""></div><div class="separator" style=""></div><div style="">I have read a <a href="http://lifehacker.com/5978098/turn-a-raspberry-pi-into-a-personal-vpn-for-secure-browsing-anywhere-you-go/all">blog post</a>, where you can build your own privacy proxy server built on Raspberry PI. The post got me thinking about how I can use this to protect my privacy on my Android phone, and also get rid of those annoying ads. </div><div style=""><br/></div><div style="">Since I own a Samsung Galaxy S3 LTE with Android 4.3 (with a HW based Knox counter), rooting the phone now means you <a href="http://forum.xda-developers.com/galaxy-s3/general/to-root-s3-lte-android-4-3-t2567118">break Knox, and loose warranty</a>. Past the point of no return ...</div><div style=""><br/></div><div style="">This means I have to solve this without root. Luckily newer Androids support VPN without rooting, but setting a mandatory system-wide proxy is still not possible without root. </div><div style="">But thanks to some iptables magic and Privoxy, this is not a problem anymore :) </div><div style=""><br/></div><div style="">The ingredients to build your own privacy protection proxy:</div><ul><li style="text-align: justify;">One (or more) cheap VPS server(s)</li><li style="text-align: justify;">a decent VPN program</li><li style="text-align: justify;">Privoxy</li><li style="text-align: justify;">iptables</li></ul><ul></ul><h2>VPS server</h2><div style="">To get the cheap VPS server, I recommend using Amazon EC2, but choose whatever you like. The micro instance is very cheap (<a href="http://aws.amazon.com/free/">or even free</a>), and has totally enough resources for this task. I'm using the Ubuntu free tier now and it works like a charm. And last but not least Amazon has two-factor authentication! You can set up an Ubuntu server under 10 minutes. Use the AWS region nearest to you, e.g. I choose EU - Ireland.<br/><br/></div><div style=""><div class="separator" style=""><a href="https://z6543.github.io/_img/aws_3.png" imageanchor="1" src="https://z6543.github.io/_img/aws_3.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/aws_3.png" width="400"/></a></div><br/><div class="separator" style=""><a href="https://z6543.github.io/_img/aws_2.png" imageanchor="1" src="https://z6543.github.io/_img/aws_2.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/aws_2.png" width="400"/></a></div><div class="separator" style=""><br/></div><h2>VPN</h2></div><div style="">For the VPN program, I recommend the free version of the OpenVPN AS (EDIT: be sure to use <a href="https://community.openvpn.net/openvpn/wiki/heartbleed">OpenVPN AS 2.0.6</a> or later, both on the server and the client). <a href="https://openvpn.net/index.php/access-server/docs/quick-start-guide.html">Easy to set-up quick start guide is here</a>, GUI based configuration, and one-click client installer for Android, iOS, Windows, Linux, OSX. The Ubuntu installer packages are <a href="https://openvpn.net/index.php/access-server/download-openvpn-as-sw/113.html?osfamily=Ubuntu">here</a>.<br/><br/><div class="separator" style=""><a href="https://z6543.github.io/_img/openvpn1.png" imageanchor="1" src="https://z6543.github.io/_img/openvpn1.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/openvpn1.png" width="400"/></a></div><div class="separator" style=""><br/></div><div class="separator" style=""><a href="https://z6543.github.io/_img/openvpn2.png" imageanchor="1" src="https://z6543.github.io/_img/openvpn2.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/openvpn2.png" width="400"/></a></div><div class="separator" style=""><br/></div><div class="separator" style=""><a href="https://z6543.github.io/_img/openvpn3.png" imageanchor="1" src="https://z6543.github.io/_img/openvpn3.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/openvpn3.png" width="330"/></a></div><div class="separator" style=""><br/></div></div><div style="">The most important settings:<br/><br/><ul><li>I prefer to use the TCP 443 and UDP 53 ports for my OpenVPN setup, and let the user guess why. </li><li>For good performance, UDP is preferred over TCP. </li><li>VPN mode is Layer 3 (routing/NAT).</li><li>Don't forget to allow the configured VPN ports in the AWS firewall (security groups). </li></ul><br/></div><div style=""><div class="separator" style=""><a href="https://z6543.github.io/_img/aws_security_groups.png" imageanchor="1" src="https://z6543.github.io/_img/aws_security_groups.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/aws_security_groups.png" width="400"/></a></div><div class="separator" style=""><br/></div></div><div style="">Other VPN settings:</div><div style=""><div><ul><li>Should VPN clients have access to private subnets (non-public networks on the server side)? - Yes</li><li>Should client Internet traffic be routed through the VPN? - Yes</li></ul></div><div><h2></h2><h2>Privoxy</h2>The next component we have to install and configure is Privoxy. As usual, "apt-get install privoxy" just works. The next step is to configure privoxy via /etc/privoxy/config file, there are two options to change:</div></div><div style=""><ul><li>listen-address your.ip.add.ress:8118</li><li>accept-intercepted-requests 1</li></ul></div><div style="">Beware not to allow everyone accessing your Privoxy server in the AWS EC2 security groups, be sure it is reachable only to VPN users!<br/><br/>After everything is set, start privoxy with "service privoxy start", and add it to the autostart "update-rc.d privoxy defaults".<br/><br/></div><div style=""><h2>Iptables</h2></div><div style="">And the final step is to configure your iptables chain to forward every web traffic from the VPN clients to the Privoxy server:<br/><br/></div><div style=""><pre class="prettyprint">iptables -t nat -A PREROUTING -s 5.5.0.0/16 -p tcp -m multiport --dports 80,8080,81 -j DNAT --to-destination your.ip.add.ress:8118<br/></pre><br/></div><div style="">Optionally you can block access to all other ports as well, and what does not go through your Privoxy won't be reachable.</div><div style="">Based on your Linux distribution and preference, you might make this rule persistent.<br/><br/></div><div style=""><h3>Final test</h3></div><div style="">Now you can connect to the VPN server from your Android device.<br/><div>After logging in from a client, you get the following nice packages to install on your device:<br/><br/></div><div class="separator" style=""><a href="https://z6543.github.io/_img/openvpn.png" imageanchor="1" src="https://z6543.github.io/_img/openvpn.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/openvpn.png" width="297"/></a></div><div class="separator" style=""><a href="https://z6543.github.io/_img/2014-04-01+08.57.36.png" imageanchor="1" src="https://z6543.github.io/_img/2014-04-01+08.57.36.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/2014-04-01+08.57.36.png" width="180"/></a></div><div><br/></div>After connecting, the final results can be seen in the following screenshots. And yes, there is a reason I chose <a href="http://www.fireeye.com/blog/technical/mobile-threats/2014/03/a-little-bird-told-me-personal-information-sharing-in-angry-birds-and-its-ad-libraries.html">Angry Birds</a> as an example.<br/><br/></div><table align="center" cellpadding="0" cellspacing="0" class="tr-caption-container" style="margin-left: auto; margin-right: auto; text-align: center;"><tbody><tr><td style="text-align: center;"><a href="https://z6543.github.io/_img/2014-03-28+22.10.53.png" imageanchor="1" src="https://z6543.github.io/_img/2014-03-28+22.10.53.png" style="margin-left: auto; margin-right: auto;"><img border="0" height="" src="https://z6543.github.io/_img/2014-03-28+22.10.53.png" width="400"/></a></td></tr><tr><td class="tr-caption" style="text-align: center;">Angry Birds without Privoxy</td></tr></tbody></table><table align="center" cellpadding="0" cellspacing="0" class="tr-caption-container" style="margin-left: auto; margin-right: auto; text-align: center;"><tbody><tr><td style="text-align: center;"><a href="https://z6543.github.io/_img/2014-03-28+22.13.21.png" imageanchor="1" src="https://z6543.github.io/_img/2014-03-28+22.13.21.png" style="margin-left: auto; margin-right: auto;"><img border="0" height="" src="https://z6543.github.io/_img/2014-03-28+22.13.21.png" width="400"/></a></td></tr><tr><td class="tr-caption" style="text-align: center;">Angry Birds with Privoxy</td></tr></tbody></table><table align="center" cellpadding="0" cellspacing="0" class="tr-caption-container" style="margin-left: auto; margin-right: auto; text-align: center;"><tbody><tr><td style="text-align: center;"><a href="https://z6543.github.io/_img/2014-03-28+22.15.11.png" imageanchor="1" src="https://z6543.github.io/_img/2014-03-28+22.15.11.png" style="margin-left: auto; margin-right: auto;"><img border="0" height="" src="https://z6543.github.io/_img/2014-03-28+22.15.11.png" width="180"/></a></td></tr><tr><td class="tr-caption" style="text-align: center;">Stupid flashlight app with ad</td></tr></tbody></table><table align="center" cellpadding="0" cellspacing="0" class="tr-caption-container" style="margin-left: auto; margin-right: auto; text-align: center;"><tbody><tr><td style="text-align: center;"><a href="https://z6543.github.io/_img/2014-03-28+14.22.00.png" imageanchor="1" src="https://z6543.github.io/_img/2014-03-28+14.22.00.png" style="margin-left: auto; margin-right: auto;"><img border="0" height="" src="https://z6543.github.io/_img/2014-03-28+14.22.00.png" width="180"/></a></td></tr><tr><td class="tr-caption" style="text-align: center;">Stupid flashlight app with Privoxy</td></tr></tbody></table><div style=""><b>Spoiler alert</b></div><div style="">If you are afraid of NSA tracking you, this post is not for you. If you want to achieve IP layer anonymity, this post is not for you. As long as you are the only one using that service, it should be trivial to see what could possibly go wrong with that.</div><div style=""><br/></div><div style=""><b>Known issues</b></div><div style="">Whenever the Internet connection (Wifi, 3G) drops, the VPN connection drops as well, and your privacy is gone ...</div><div style="">Sites breaking your privacy through SSL can still do that as long as the domain is not in the Privoxy blacklist.</div><div style=""><br/></div><div style=""><b>Additional recommendation</b></div><div style="">If you are using OSX or Windows, I can recommend Aviator to be used as your default browser. It is just great, give it a try!</div><div style=""><br/></div><div style=""><b>PS:</b> There are also some <a href="https://adblockplus.org/blog/adblock-plus-for-android-removed-from-google-play-store">adblock apps removed from the official store</a> which can block some ads, but you have to configure a proxy for every WiFi connection you use, and it is not working over 3G.<br/><br/></div><div class="separator" style=""><a href="https://z6543.github.io/_img/Mikko_Privacy_is_not_negotiable.png" imageanchor="1" src="https://z6543.github.io/_img/Mikko_Privacy_is_not_negotiable.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/Mikko_Privacy_is_not_negotiable.png" width="320"/></a></div><div style=""><br/></div><div style=""><br/></div>