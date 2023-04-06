---           
layout: post
title: '"Just" migrate to Jekyll ...'
date: 2023-04-05 10:49:41 UTC
comments: false
categories: Jekyll blogger blogspot
---
# Intro
[As I already ranted about it](/2022/09/16/Google.html), I am not very satisfied with multiple Google services recently (blogger, YouTube), so I started looking for alternative solutions for my blog. Based on a quick Twitter poll, all the cool kids use Jekyll and Github pages nowadays, and I did not want to miss out ...  
Had I known how much time I had to spend on this blog migration, I probably would not start it in the first place ...  
# Jekyll
Why Jekyll and Github pages?
- Because it is free
- I don't have to operate my own webserver
- I don't have to maintain proper TLS config
- It is a static website
- Github is more secure than most services  

My first task was installing Jekyll locally to test my web pages locally. This was my first mistake. I created this Jekyll environment on the macOS like an animal - without using Docker.   
![Jekyll](/_img/jekyll.jpg)  
![maneuver](/_img/maneuver.jpg)  
 Or ~1 hour at least. Pro tip: use [Docker](https://hub.docker.com/r/jekyll/jekyll/). Cool, now we have it. I spent significant time trying to figure out the proper _config.yml config and Gemfile parameters, but I think I figured out what I needed, kinda. Not perfect, but it works.   
  
The next challenge was saving the blog posts from Blogger/Blogspot in a Jekyll-readable format. I tried multiple solutions, but only [this](https://gist.github.com/rupeshtiwari/80f2203fee697a94e4b11b75b856aa56) was close enough to do anything. After that, I realized the formatting was effed up on numerous posts. It took me a while to figure out the problem was image styles and another \<br\>\ related issue I cannot reproduce anymore. It did not help that I tried to debug this problem on a machine where I did not have the Jekyll installation, and I waited 5 minutes for Github to rebuild the site to test that I still did not figure out what the issue was. Clearly, I did set up the local dev env to avoid this exact situation ...   
  
The next step was to download all the images from the blog posts, store them in the website repo, and rewrite all the URLs on the pages. You would think this is a trivial exercise, but I ended up with ~100 lines of Python code, running and rerunning it at least seven times to get there. 
    
Next, I realized that the "content creation" date was somehow messed up with the last update, but the code looks OK. So I fixed this on all the blog posts manually. While doing this, I realized that the blog post exporter script only saved 25 posts, probably a limitation on the blogger feed system itself. So I had to download the rest of the blog posts, download the images again, download the incorrect HTML tags again, fix the image URLs, and fix the creation dates.  
  
Meanwhile, I always had this issue: whenever I pushed the latest version, my custom domain name did not work anymore - I had to configure this in the CNAME file. And after I had already built and rebuilt, and rebuilt the site with Github pages, it turned out there was a [caching possibility](https://github.com/Z6543/Z6543.github.io/blob/8bff56adf2ba60ab65cc77597e073e790b050fec/.github/workflows/jekyll.yml#L15), which in fact, can change the build time from five minutes to one minute. Probably, there is an even better solution, but this is good enough for now.     

# Conclusion
Am I satisfied with the overall result? Yes.   
Am I satisfied with how I got here? No.  
Do I recommend this route? If you have free time, yes.  
Should this be so complicated? I hope not.   
Do I like that my website uses Ruby? Hell no.  



