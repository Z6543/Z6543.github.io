---
layout: post
title:  "My WHCD experience"
date:   2022-09-13 15:53:14 +0200
categories: WHCD
---


# Introduction
My story starts in July in Budapest, summer is hot. Way too hot. I am working in the cozy air-conditioned office room and look at my daily schedule. I have a business lunch with Sándor Fehér, co-founder & CEO at White Hat IT Security at an Italian restaurant. This will be a casual meeting, catching up with each other, no preparation needed. I show up at the place, meet Sándor, order a pizza, and we discuss average lunch topics like how APT attackers operate nowadays. He mentions that in September they will organize the exam for their WHCD - White Hat Certified Defender course. And he is happy to provide me a slot for the exam for free. But as nothing is free in life. If I pass - and why would not I? :) - I create a blog post about my view on the exam. I happily accept the invitation and finish the pizza. 

# Preparation
In 2020, the course consisted of 36 hours of training, touching topics like threat intelligence, IoCs, computer forensics, network forensics, all the blue team-related topics. I felt pretty confident that I don't have to prepare for this exam, even if I did not attend the training and I did zero real forensics investigation in the past eight years.  
Before the exam, I received multiple documents on the rules and practical information for the exam. The exam started at 20:00 Friday and ended on 20:00 Sunday. 48 hours for finding 10 flags and provide documentation. Actually, 7 flags are enough to pass the test. The only preparations I made was to buy some energy drinks and sweets. Time is 19:30, so I still have some time to watch the latest Last week tonight before the exam starts.  

# My past experience
I have the following certifications which helped me during the exam: OSCP, OSCE, PSP (Powershell for Penetration Testers). I also have some experience with CTFs. 8 years ago I did some forensics investigations, but that was a loooong time ago, things changed. A lot. 

# The exam - Friday
I receive all the IP addresses and credentials to log in to my dedicated Windows AD network, consisting of a workstation, a File Server, and a Domain Controller. I am happy that I have to work with a live system - it is really painful when you work with offline images. I set the goal that today I find at least one flag, have a good night's sleep and find the remaining 9 on Saturday, and write the documentation on Sunday. Let's find the easiest task and get the flag quickly ... The challenge descriptions are clear, and even the challenge IDs nicely map to MITRE ATT&CK®.  
Two hours later I realize I underestimated the difficulty of this exam, and I overestimated my skills. Zero flags found and ...

![image-title-here](/_img/dog.jpg){:class="img-responsive"}

Spoiler alert - I was logging into the workstation with the admin user, and not with the victim. I realized this on Sunday afternoon ... This first initial mistake took me at least two hours lost in this initial stage. Anyway, always read through the exam instructions and don't rush into connecting to the first machine on the provided list. I finally find a sneaky backdoor, analyze it, and submit my first flag. Phew, this is not going to be easy. I go to sleep, but at 5:30 a.m. my brain starts to figure out the challenges even though I am still in my bed, eyes closed. Based on past experience, it does not make sense to try to force myself to sleep, it is better to wake up and keep rollin'. 

# Saturday
Around 10:00 a.m., I submit another flag. I already spent like seven hours, and I only have two flags. Boy oh boy, this is going to be tough. Time to crack open another energy drink. Hours later I find myself ALT + TAB-bing between the different challenges and making no progress. Time to have a break. When I am back, I make some good progress. Basic programming skills are really needed to solve multiple challenges. Sometimes I mix up AES keys with IVs but luckily it is OK to fail. Flag after flag. It is evening, and I have 6 flags already \o/. I also found our main suspect, a bird! I was like "let's find the seventh flag now and I can rest in peace". Let's connect the "Rubber duck debugging" and writing the documentation together - maybe I find out what I missed. It is already dark outside, I am tired as hell, but I made good progress with the documentation at least. This tactic turned out to be useful, as I submitted my seventh flag! Finally. I feel like rock and I can find all the remaining flags on Sunday. GOTO sleep(28800). 

![image-title-here](/_img/flag.png){:class="img-responsive"}

# Sunday
My brain wakes up again earlier than I want. Let's finish the documentation and after that focus on the remaining flags. I spend a lot of time on Stackoverflow on how to insert nice syntax-highlighted code into the report (Hello Microsoft, it is 2020, please make it easier for us with builtin tools ...) and I also spend more time than I want to admit on how to format the flags in Wordart to make it look nice. Documentation finished, let's go back to work. While I close all the opened tabs, I finally find the exam instructions, with the credentials to the user's workspace. "What the fluff, how could I overlook this?". MMMKay, let's log in. Run a bunch of tools. Run even more tools. Let's dig up emails from 2009 for a German forensics tool I don't remember the name anymore. But this whole exam is less about tools and more about perseverance and thoroughness. Time flies fast, no progress. Let's have a break, and kill Sigrun, the Valkyrie Queen in God of War. When I got back, no new flags submitted themselves. Let's listen to a CTF playlist, maybe that helps. Well, no, it does not. Let's download malware information collector software to the workstation and run that for clues. Pro tip: never do this on a live forensics investigation ;) 

![image-title-here](/_img/bear.jpg){:class="img-responsive"}

It's at 6 o'clock in the evening and I am getting really tired. And the fact that I already have enough flags to pass the exam makes me lazy. I close my eyes and all I can see is obfuscated code flickering. I literally made zero progress on Sunday on the remaining flags. Maybe if the passing limit was 8 flags, I would have tried harder®? We will never know. 

# Monday

![image-title-here](/_img/cat.gif){:class="img-responsive"}

# Conclusion
The whole exam experience was perfect. Access to the machines was fast and responsive. I did not experience any technical glitch. All the challenges and the whole story is very challenging but possible to solve. I would say that high-end financially motivated attackers could attack any corporation the same way, and basic detection or forensics would miss most clues. The whole scenario is 100% realistic. Most attacks/techniques are sneaky. The only thing I missed is that on the official exam Discord, they should create a #rage or #random or #memes channel where you can send all your frustration during the exam. 
 
Lesson l: Never underestimate an exam just because it is not known. 
Lesson 2: Never overestimate the knowledge and skills you did not practice for a long time. 
Lesson 3: This WHCD exam provided me the same - or even better experience than I had at Offensive exams like OSCP or OSCE. 
If someone passes this challenge, it is really true that the person really knows how to technically deal with computer incidents. I vision great success for this certification because as of today, there are not many hands-on practical defender exams available on the market. 
I would like to thank Sándor Fehér for this opportunity and to János Pallagi for his support during the weekend and to share some thoughts with me after the exam. 

![image-title-here](/_img/whcd.png){:class="img-responsive"}