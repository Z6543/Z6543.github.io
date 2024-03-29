---           
layout: post
title: DNSSEC, from an end-user perspective, part 2
date: 2014-01-06 15:54:34 UTC
updated: 2019-10-08 15:54:34 UTC
comments: false
categories: dig DNSSEC security
---
<div style=""><a href="http://jumpespjump.blogspot.hu/2013/12/dnssec-from-end-user-perspective-part-1.html">In our previous blog post</a>, we have discussed some of the threats against current DNS systems, where the result was that the victim landed on a different resource/website as he/she originally supposed to visit.</div><div style="">
</div><div style="">Since this is not a guide for DNS server operators about DNSSEC implementation, let's jump to the user side and see what you should know if you visit a website where DNSSEC validation fails (like the test site <a href="http://www.dnssec-failed.org/" target="_blank">http://www.dnssec-failed.org/</a>) and you use a DNS server which validates the DNSSEC responses (e.g., 8.8.8.8 - Google public DNS server):</div>
<div class="separator" style=""><a href="https://z6543.github.io/_img/dnssec_notreachable.png" src="https://z6543.github.io/_img/dnssec_notreachable.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/dnssec_notreachable.png" width="400"/></a></div>
<div style="">So far, so good :-) The malicious attacker's evil plan has failed. But what should you see when the server is protected with DNSSEC, and the DNSSEC response is valid/trusted?</div>
<div class="separator" style=""><a href="https://z6543.github.io/_img/DNSSEC_valid.png" src="https://z6543.github.io/_img/DNSSEC_valid.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/DNSSEC_valid.png" width="400"/></a></div>
<div style="">Please note that this <a href="https://addons.mozilla.org/en-us/firefox/addon/dnssec-validator/" target="_blank">"green key" stuff</a> is for experimental purposes only. I hope the users should not have to look for another SSL-like padlock in their browser in the future. The critical thing here is that the site has been rendered by the browser, and the response is protected by DNSSEC, thus eliminating lots of threats.</div><div style="">
</div><div style="">When you visit a site that is protected by DNSSEC, but your DNS server does not validate the DNSSEC, the site will be served and rendered in your browser. It does not matter if it has untrusted or invalid DNSSEC records. This is the same experience what you have today with no DNSSEC at all:</div>
<div class="separator" style=""><a href="https://z6543.github.io/_img/dnssec_untr.png" src="https://z6543.github.io/_img/dnssec_untr.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/dnssec_untr.png" width="400"/></a></div>
<div style="">OK, so we, as end-users, need sites/domains which are protected with DNSSEC, and a DNS server which validates the DNSSEC records. We have the DNS server (thx Google!), so let's find some popular sites which are using DNSSEC!</div>
<div class="separator" style=""><a href="https://z6543.github.io/_img/44540147.jpg" src="https://z6543.github.io/_img/44540147.jpg" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/44540147.jpg" width="320"/></a></div>
<div style="">To demonstrate my 133t h4cking skills, here is a small bash script. You give a file with the list of domains as a first parameter to the bash script, and it will give you back the areas protected with DNSSEC.</div><div style="">
</div><div style=""></div><pre class="prettyprint linenums lang-bash">#!/bin/bash
dig . DNSKEY | grep -Ev '^($|;)' &gt; root.keys
while read line
do
    if dig +sigchase +trusted-key=root.keys $line. A @8.8.8.8 | grep --quiet "DNSSEC validation is ok: SUCC"; then
        echo $line
    fi
done &lt; $1
</pre><div style="">
</div><div style="">Alexa was not kind enough to give me a recent list of top sites in text format, so I have used <a href="http://moz.com/top500">http://moz.com/top500</a> for this test.</div><div style="">
</div><div style="">The results were quite shocking; the first result was at TOP40: <a href="http://gov.uk/">gov.uk</a> - but when you visit gov.uk via browser, it is redirected to a subdomain that is not protected with DNSSEC, so this kind of behavior is considered as FAIL.</div><div style="">
</div><div style="">TOP69: <a href="http://nih.gov/">nih.gov</a>  - so ironically, the winner is the National Institute of Health :P</div><div style="">
Other interesting one where:

</div><div style=""><a href="http://mozilla.org/">mozilla.org</a> - FAIL </div><div style=""><a href="http://cdc.gov/">cdc.gov</a> - FAIL</div><div style=""><a href="http://nasa.gov/">nasa.gov</a> - FAIL :(</div><div style=""><a href="http://ca.gov/">ca.gov</a>  - California homepage, don't mess with Arnie!</div><div style=""><a href="http://noaa.gov/">noaa.gov</a> - National Oceanic and Atmospheric Administration :)</div><div style=""><a href="http://berkeley.edu/">berkeley.edu</a> - thank god the world of universities is saved</div><div style=""><a href="http://loc.gov/">loc.gov</a> - FAIL</div><div style=""><a href="http://whitehouse.gov/">whitehouse.gov</a> - FAIL …</div><div style="">
</div><div style="">I think I have never ever visited these websites before, except the Berkeley one. If you can find a financial institution which website's domain is protected by DNSSEC, let me know. I was not able to find one… For example, paypal.com is a bad example. The main domain is protected with DNSSEC, but not the www.paypal.com.</div><div style="">
</div><div style="">In the next part of our DNSSEC series, I will write about the threats eliminated by DNSSEC and some other exciting things.</div>