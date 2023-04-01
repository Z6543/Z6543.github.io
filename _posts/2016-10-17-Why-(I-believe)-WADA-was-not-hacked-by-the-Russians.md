---           
layout: post
title: Why (I believe) WADA was not hacked by the Russians
date: 2016-10-17 13:35:24 UTC
updated: 2019-10-08 13:35:24 UTC
comments: false
categories: attribution Hack Russia WADA
---
Disclaimer: This is my personal opinion. I am not an expert in attribution. But as it turns out, not many people in the world are good at attribution. I know this post lacks real evidence and is mostly based on speculation.

<div class="separator" style=""><a href="https://z6543.github.io/_img/wada.png" imageanchor="1" src="https://z6543.github.io/_img/wada.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/wada.png" width="320"/></a></div>

Let's start with the main facts we know about the WADA hack, in chronological order:


1. Some point in time (August - September 2016), the WADA database has been hacked and exfiltrated
<div>2. August 15th, "WADA has alerted their stakeholders that email phishing scams are being reported in connection with WADA and therefore asks its recipients to be careful"  <a href="https://m.paralympic.org/news/wada-warns-stakeholders-phishing-scams">https://m.paralympic.org/news/wada-warns-stakeholders-phishing-scams</a>
3. September 1st, the fancybear.net domain has been registered
<div><pre style='font-family: "Courier New", monospace; font-size: 13px;'>   Domain Name: FANCYBEAR.NET
   ...
   Updated Date: 18-sep-2016
   Creation Date: 01-sep-2016</pre><pre style='font-family: "Courier New", monospace; font-size: 13px;'></pre></div>4. The content of the WADA hack has been published on the website
5. The @FancyBears and @FancyBearsHT Twitter accounts have been created and started to tweet on 12th September, reaching out to journalists
6. 12th September, Western media started headlines "Russia hacked WADA"</div><div>7. The leaked documents have been altered, states WADA <a href="https://www.wada-ama.org/en/media/news/2016-10/cyber-security-update-wadas-incident-response">https://www.wada-ama.org/en/media/news/2016-10/cyber-security-update-wadas-incident-response</a>


<h3>The Threatconnect analysis</h3>The only technical analysis on why Russia was behind the hack, can be read here: <a href="https://www.threatconnect.com/blog/fancy-bear-anti-doping-agency-phishing/">https://www.threatconnect.com/blog/fancy-bear-anti-doping-agency-phishing/</a>

After reading this, I was able to collect the following main points:

<ol><li>It is Russia because Russian APT groups are capable of phishing</li><li>It is Russia because the phishing site "wada-awa[.]org was registered and uses a name server from ITitch[.]com, a domain registrar that FANCY BEAR actors recently used"</li><li>It is Russia because "Wada-arna[.]org and tas-cass[.]org were registered through and use name servers from Domains4bitcoins[.]com, a registrar that has also been associated with FANCY BEAR activity."</li><li>It is Russia, because "The registration of these domains on August 3rd and 8th, 2016 are consistent with the timeline in which the WADA recommended banning all Russian athletes from the Olympic and Paralympic games."</li><li>It is Russia, because "The use of 1&amp;1 mail.com webmail addresses to register domains matches a TTP we previously identified for FANCY BEAR actors."</li></ol>
There is an interesting side-track in the article, the case of the @anpoland account. Let me deal with this at the end of this post.

My problem with the above points is that all five flag was publicly accessible to anyone as TTP's for Fancy Bear. And meanwhile, all five is weak evidence. Any script kittie in the world is capable of both hacking WADA and planting these false-flags.

A stronger than these weak pieces of evidence would be:

<ul><li>Malware sharing same code attributed to Fancy Bear (where the code is not publicly available or circulating on hackforums)</li><li>Private servers sharing the IP address with previous attacks attributed to Fancy Bear (where the server is not a hacked server or a proxy used by multiple parties)</li><li>E-mail addresses used to register the domain attributed to Fancy Bear</li><li>Many other things</li></ul><div>For me, it is quite strange that after such <a href="https://www.threatconnect.com/blog/guccifer-2-0-dnc-breach/" target="_blank">great analysis on Guccifer 2.0</a>, the Threatconnect guys came up with this low-value post. </div><div>
</div>
<h3>The fancybear website</h3>It is quite unfortunate that the analysis was not updated after the documents have been leaked. But let's just have a look at the fancybear . net website, shall we?
<div class="separator" style=""><a href="https://z6543.github.io/_img/screencapture-fancybear-net-1476519267721.png" imageanchor="1" src="https://z6543.github.io/_img/screencapture-fancybear-net-1476519267721.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/screencapture-fancybear-net-1476519267721.png" width="130"/></a></div>
Now the question is, if you are a Russian state-sponsored hacker group, and you are already accused of the hack itself, do you create a website with tons of bears on the website, and do you choose the same name (Fancy Bear) for your "Hack team" that is already used by Crowdstrike to refer to a Russian state-sponsored hacker group? Well, for me, it makes no sense. Now I can hear people screaming: "The Russians changed tactics to confuse us". Again, it makes no sense to change tactics on this, while keeping tactics on the "evidence" found by Threatconnect.

It makes sense that a Russian state-sponsored group creates a fake persona, names it Guccifer 2.0, pretends Guccifer 2.0 is from Romania, but in the end it turns out Guccifer 2.0 isn't a native Romanian speaker. That really makes sense.

What happens when someone creates this fancybear website for leaking the docs, and from the Twitter account reaches out to the media? Journalists check the website, they see it was done by Fancy Bear, they <strike>Bing</strike> Google this name, and clearly see it is a Russian state-sponsored hacker group. Some journalists also found the Threatconnect report, which seems very convincing for the first read. I mean, it is a work of experts, right? So you can write in the headlines that the hack was done by the Russians.

Just imagine an expert in the USA or Canada writing in report for WADA:
"the hack was done by non-Russian, but state-sponsored actors, who planted a lot of false-flags to accuse the Russians and to destroy confidence in past and future leaks". Well, I am sure this is not a popular opinion, and whoever tries this, risks his career. Experts are human, subject to all kinds of bias.

<h3>The Guardian</h3>The only other source I was able to find is from The Guardian, where not just one side (it was Russia) was represented in the article. It is quite unfortunate that both experts are from Russia - so people from USA will call them being not objective on the matter. But the fact that they are Russian experts does not mean they are not true ...

<a href="https://www.theguardian.com/sport/2016/sep/15/fancy-bears-hackers--russia-wada-tues-leaks">https://www.theguardian.com/sport/2016/sep/15/fancy-bears-hackers--russia-wada-tues-leaks</a>

Sergei Nikitin:
“We don’t have this in the case of the DNC and Wada hacks, so it’s not clear on what basis conclusions are being drawn that Russian hackers or special services were involved. It’s done on the basis of the website design, which is absurd,” he said, referring to the depiction of symbolically Russian animals, brown and white bears, on the “Fancy Bears’ Hack Team” website.

I don't agree with the DNC part, but this is not the topic of conversation here.

Alexander Baranov:
"the hackers were most likely amateurs who published a “semi-finished product” rather than truly compromising information. “They could have done this more harshly and suddenly,” he said. “If it was [state-sponsored] hackers, they would have dug deeper. Since it’s enthusiasts, amateurs, they got what they got and went public with it.”"

<h3>The @anpoland side-track</h3>First please check the tas-cas.org hack <a href="https://www.youtube.com/watch?v=day5Aq0bHsA%C2%A0" target="_blank">https://www.youtube.com/watch?v=day5Aq0bHsA </a> , I will be here when you finished it. This is a website for "Court of Arbitration for Sport’s", and referring to the Threatconnect post, "CAS is the highest international tribunal that was established to settle disputes related to sport through arbitration. Starting in 2016, an anti-doping division of CAS began judging doping cases at the Olympic Games, replacing the IOC disciplinary commission." Now you can see why this attack is also discussed here.
<div class="separator" style=""><a href="https://z6543.github.io/_img/anpoland.png" imageanchor="1" src="https://z6543.github.io/_img/anpoland.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/anpoland.png" width="320"/></a></div>

<ul><li>My bet is that this machine was set-up for these @anpoland videos only. Whether google.ru is a false flag or it is real, hard to decide. It is interesting to see that there is no google search done via google.ru, it is used only once. </li><li>The creator of the video can't double click. Is it because he has a malfunctioning mouse? Is it because he uses a virtualization console, which is near-perfect OPSEC to hide your real identity? My personal experience is that using virtualization consoles remotely (e.g. RDP) has very similar effects to what we can see on the video. </li><li>The timeline of the Twitter account is quite strange, registered in 2010</li><li>I agree with the Threatconnect analysis that this @anpoland account is probably a faketivist, and not an activist. But who is behind it, remains a mystery. </li><li>Either the "activist" is using a whonix-like setup for remaining anonymous, or a TOR router (something like <a href="https://makezine.com/projects/browse-anonymously-with-a-diy-raspberry-pi-vpntor-router/" target="_blank">this</a>), or does not care about privacy at all. Looking at the response times (SQLmap, web browser), I doubt this "activist" is behind anything related to TOR. Which makes no sense for an activist, who publishes his hack on Youtube. People are stupid for sure, but this does not add up. It makes sense that this was a server (paid by bitcoins or stolen credit cards or whatever) rather than a home computer.</li></ul><div>For me, this whole @anpoland thing makes no sense, and I think it is just loosely connected to the WADA hack. </div>
<h3>The mysterious Korean characters in the HTML source</h3><div>There is another interesting flag in the whole story, which actually makes no sense. When the website was published, there were Korean characters in HTML comments. </div><div><a href="https://web.archive.org/web/20160913013727/http://fancybear.net/">https://web.archive.org/web/20160913013727/http://fancybear.net/</a></div><div class="separator" style=""><a href="https://z6543.github.io/_img/korean1.png" imageanchor="1" src="https://z6543.github.io/_img/korean1.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/korean1.png" width="320"/></a></div>
<div class="separator" style=""></div><div>
</div><div>
</div><div>When someone pointed this out on Twitter, these Korean HTML comments disappeared:</div><div><a href="https://web.archive.org/web/20160914231209/http://www.fancybear.net/">https://web.archive.org/web/20160914231209/http://www.fancybear.net/</a></div><div class="separator" style=""><a href="https://z6543.github.io/_img/korean2.png" imageanchor="1" src="https://z6543.github.io/_img/korean2.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" height="" src="https://z6543.github.io/_img/korean2.png" width="320"/></a></div><div>These HTML comments look like generated HTML comments, from a WYSIWYG editor, which is using the Korean language. Let me know if you can identify the editor.</div><div>
</div><h3>The Russians are denying it</h3><div>Well, what choice they have? It does not matter if they did this or not, they will deny it. And they can't deny this differently. Just imagine a spokesperson: "Previously we have falsely denied the DCC and DNC hacks, but this time please believe us, this wasn't Russia." Sounds plausible ...

</div><h3>Attribution</h3>Let me sum up what we know:

It makes sense that the WADA hack was done by Russia, because:

<ol><li>Russia being almost banned from the Olympics due to doping scandal, it made sense to discredit WADA and US Olympians</li><li>There are multiple(weak) pieces of evidence which point to Russia</li></ol><div>It makes sense that the WADA hack was not done by  Russia, because: </div><div><ol><li>By instantly attributing the hack to the Russians, the story was more about to discredit Russia than discrediting WADA or US Olympians.</li><li>In reality, there was no gain for Russia for disclosing the documents. Nothing happened, nothing changed, no discredit for WADA. Not a single case turned out to be illegal or unethical.</li><li><strike>Altering the leaked documents makes no sense if it was Russia</strike> (see update at the end). Altering the leaked documents makes a lot of sense if it was not Russia. Because from now on, people can always state "these leaks cannot be trusted, so it is not true what is written there". It is quite cozy for any US organization, who has been hacked or will be hacked. If you are interested in the "Russians forging leaked documents" debate, I highly recommend to start with this <a href="https://theintercept.com/2016/10/11/in-the-democratic-echo-chamber-inconvenient-truths-are-recast-as-putin-plots/" target="_blank">The Intercept</a> article</li><li>If the Korean characters were false flags planted by the Russians, why would they remove it? If it had been Russian characters, I would understand removing it.</li><li>All evidence against Russia is weak, can be easily forged by even any script kittie.</li></ol><div>
</div>I don't like guessing, but here is my guess. This WADA hack was an operation of a (non-professional) hackers-for-hire service, paid by an enemy of Russia. The goal was to hack WADA, leak the documents, modify some contents in the documents, and blame it all on the Russians ...

<h3>Questions and answers</h3></div><div><ul><li>Was Russia capable of doing this WADA hack? Yes.</li><li>Was Russia hacking WADA? Maybe yes, maybe not.</li><li>Was this leak done by a Russian state-sponsored hacker group? I highly doubt that.</li><li>Is it possible to buy an attribution-dice where all six-side is Russia? No, it is sold-out. </li></ul></div><div>
</div><div>To quote Patrick Gray: "Russia is the new China, and the Russians ate my homework."©</div><div>
</div><div>Let me know what you think about this, and please comment. </div><div>
</div><div>Update: As TheGrugq pointed out, Guccifer has been found to alter documents <a href="https://www.reddit.com/r/EnoughTrumpSpam/comments/4uyih3/russian_hackers_altered_emails_before_release_to/?st=IUJDLSIE&amp;sh=e195e908" style="font-family: 'Helvetica Neue Light', HelveticaNeue-Light, helvetica, arial, sans-serif;">https://www.reddit.com/r/EnoughTrumpSpam/comments/4uyih3/russian_hackers_altered_emails_before_release_to/?st=IUJDLSIE&amp;sh=e195e908</a></div></div>