---           
layout: post
title: Recovering data from an old encrypted Time Machine backup
date: 2019-10-08 13:32:30 UTC
updated: 2019-10-08 13:32:30 UTC
comments: false
categories: forensics macos nas time capsule time machine
---
Recovering data from a backup should be an easy thing to do. At least this is what you expect. Yesterday I had a problem which should have been easy to solve, but it was not. I hope this blog post can help others who face the same problem.

<div class="separator" style=""><a href="https://z6543.github.io/_img/macos-high-sierra-system-preferences-time-machine.jpg" imageanchor="1" src="https://z6543.github.io/_img/macos-high-sierra-system-preferences-time-machine.jpg" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="881" data-original-width="1336" height="" src="https://z6543.github.io/_img/macos-high-sierra-system-preferences-time-machine.jpg" width="400"/></a></div>
<h2>The problem</h2>1. I had an encrypted Time Machine backup which was not used for months
2. This backup was not on an official Apple Time Capsule or on a USB HDD, but on a WD MyCloud NAS
3. I needed files from this backup
4. After running out of time I only had SSH access to the macOS, no GUI

<h2>The struggle</h2>By default, Time Machine is one of the best and easiest backup solution I have seen. As long as you stick to the default use case, where you have one active backup disk, life is pink and happy. But this was not my case.

As always, I started to Google what shall I do. One of the first options recommended that I add the backup disk to Time Machine, and it will automagically show the backup snapshots from the old backup. Instead of this, it did not show the old snapshots but started to create a new backup. Panic button has been pressed, backup canceled, back to Google.
<div class="separator" style=""><a href="https://1.bp.blogspot.com/-9Die_5TbKPc/W1BFBZBDAcI/AAAAAAAACrE/jla8CDuxZh8S83G-piox8g9FqzH9IBPvwCLcBGAs/s1600/use-additional-backup-drive-time-machine.jpeg" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="321" data-original-width="516" height="199" src="https://1.bp.blogspot.com/-9Die_5TbKPc/W1BFBZBDAcI/AAAAAAAACrE/jla8CDuxZh8S83G-piox8g9FqzH9IBPvwCLcBGAs/s320/use-additional-backup-drive-time-machine.jpeg" width="320"/></a></div>

Other tutorials recommend to click on the Time Machine icon and pressing alt (Option) key, where I can choose "Browse other backup disks". But this did not list the old Time Machine backup. It did list the backup when selecting disks in Time Machine preferences, but I already tried and failed that way.
<div class="separator" style=""><a href="https://3.bp.blogspot.com/-qIZjV8XjqtU/W1BE3bvCtwI/AAAAAAAACrA/tO8szi90eP8EntDpnv42WHzv7If__keogCLcBGAs/s1600/browse-additional-backup-disks.jpeg" imageanchor="1" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="191" data-original-width="437" height="139" src="https://3.bp.blogspot.com/-qIZjV8XjqtU/W1BE3bvCtwI/AAAAAAAACrA/tO8szi90eP8EntDpnv42WHzv7If__keogCLcBGAs/s320/browse-additional-backup-disks.jpeg" width="320"/></a></div>

YAT (yet another tutorial) recommended to SSH into the NAS, and browse the backup disk, as it is just a simple directory where I can see all the files. But all the files inside where just a bunch of nonsense, no real directory structure.

YAT (yet another tutorial) recommended that I can just easily browse the content of the backup from the Finder by double-clicking on the sparse bundle file. After clicking on it, I can see the disk image on the left part of the Finder, attached as a new disk.
Well, this is true, but because of some bug, when you connect to the Time Capsule, you don't see the sparse bundle file. And I got inconsistent results, for the WD NAS, double-clicking on the sparse bundle did nothing. For the Time Capsule, it did work.
At this point, I had to leave the location where the backup was present, and I only had remote SSH access. You know, if you can't solve a problem, let's complicate things by restrict yourself in solutions.
<span style="color: red;">
</span>Finally, I tried to check out some data forensics blogs, and besides some expensive tools, I could find the solution.
<h2>The solution</h2>Finally, a <a href="https://d4rkw1ll0w4n6.wordpress.com/2015/02/12/timemachine-4n6/" target="_blank">blog post</a> provided the real solution - hdiutil.
The best part of hdiutil is that you can provide the read-only flag to it. This can be very awesome when it comes to forensics acquisition.

<div class="separator" style=""><a href="https://z6543.github.io/_img/Screen2BShot2B2018-07-192Bat2B09.54.50.png" imageanchor="1" src="https://z6543.github.io/_img/Screen2BShot2B2018-07-192Bat2B09.54.50.png" style="margin-left: 1em; margin-right: 1em;"><img border="0" data-original-height="389" data-original-width="1596" height="" src="https://z6543.github.io/_img/Screen2BShot2B2018-07-192Bat2B09.54.50.png" width="400"/></a></div>
To mount any NAS via SMB:
<pre class="prettyprint lang-bsh">mount_smbfs afp://&lt;username&gt;@&lt;NAS_IP&gt;/&lt;Share_for_backup&gt; /&lt;mountpoint&gt;</pre>
To mount a Time Capsule share via AFP:
<pre class="prettyprint lang-bsh">mount_afp afp://any_username:password@&lt;Time_Capsule_IP&gt;/&lt;Share_for_backup&gt; /&lt;mountpoint&gt;</pre>
And finally this command should do the job:
<pre class="prettyprint lang-bsh">hdiutil attach test.sparsebundle -readonly</pre>
It is nice that you can provide read-only parameter.

If the backup was encrypted and you don't want to provide the password in a password prompt, use the following:
<pre class="prettyprint lang-bsh">printf '%s' 'CorrectHorseBatteryStaple' | hdiutil attach test.sparsebundle -stdinpass -readonly</pre>
Note: if you receive the error "resource temporarily unavailable", probably another machine is backing up to the device

And now, you can find your backup disk under /Volumes. Happy restoring!

Probably it would have been quicker to either enable the remote GUI, or to physically travel to the system and login locally, but that would spoil the fun.