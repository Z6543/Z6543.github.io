---           
layout: post
title: Thousand ways to backdoor a Windows domain (forest)
date: 2019-10-08 13:40:29 UTC
updated: 2019-10-08 13:40:29 UTC
comments: false
categories: backdoor domain Hack Windows
---
<div style="text-align: left;"><div style="text-align: justify;">When the Kerberos elevation of privilege (CVE-2014-6324 / MS14-068) vulnerability has been made public, the remediation paragraph of the following blog post made some waves:<br/><a href="http://blogs.technet.com/b/srd/archive/2014/11/18/additional-information-about-cve-2014-6324.aspx">http://blogs.technet.com/b/srd/archive/2014/11/18/additional-information-about-cve-2014-6324.aspx</a><br/><br/>"The only way a domain compromise can be remediated with a high level of certainty is a complete rebuild of the domain."<br/><br/>Personally, I agree with this, but .... But whether this is the real solution, I'm not sure. And the same applies to compromised computers. When it has been identified that malware was able to run on the computer (e.g. scheduled scan found the malware), there is no easy way to determine with 100% certainty that there is no rootkit on the computer. Thus rebuilding the computer might be a good thing to consider. For paranoids, use new hardware ;)<br/><br/>But rebuilding a single workstation and rebuilding a whole domain is not on the same complexity level. Rebuilding a domain can take weeks or months (or years, which will never happen, as the business will close before that).<br/><br/>There are countless documented methods to backdoor a computer, but I have never seen a post where someone collects all the methods to backdoor a domain. In the following, I will refer to domain admin, but in reality, I mean Domain Admins, Enterprise Admins, and Schema Admins.<br/><br/><br/><h3>Ways to backdoor a domain</h3>So here you go, an incomplete list to backdoor a domain:<br/><br/><ul><li>Create a new domain admin user. Easy to do, easy to detect, easy to remediate</li><li>Dump password hashes. The attacker can either crack those or just pass-the-hash. Since KB2871997, pass-the-hash might be trickier (<a href="https://technet.microsoft.com/library/security/2871997">https://technet.microsoft.com/library/security/2871997</a>), but not impossible. Easy to do, hard to detect, hard to remediate - just think about service user passwords. And during remediation, consider all passwords compromised, even strong ones.</li><li>Logon scripts - modify the logon scripts and add something malicious in it. Almost anything detailed in this post can be added :D</li><li>Use an already available account, and add domain admin privileges to that. Reset its password. Mess with current group memberships - e.g. <a href="http://www.exploit-db.com/papers/17167/">http://www.exploit-db.com/papers/17167/</a></li><li>Backdoor any workstation where domain admins login. While remediating workstations, don't forget to clean the roaming profile. The type of backdoor can use different forms: malware, local admin, password (hidden admin with 500 RID), sticky keys, etc.</li><li>Backdoor any domain controller server. For advanced attacks, see <a href="https://github.com/gentilkiwi/mimikatz/releases/tag/2.0.0-alpha-20150117" target="_blank">Skeleton keys </a></li><li>Backdoor files on network shares which are commonly used by domain admins by adding malware to commonly used executables - <a href="https://github.com/secretsquirrel/the-backdoor-factory" target="_blank">Backdoor factory</a></li><li>Change ownership/permissions on AD partitions - if you have particular details on how to do this specifically, please comment</li><li>Create a new domain user. Hide admin privileges with SID history. Easy to do, hard to detect, easy to remediate - check <a href="https://github.com/gentilkiwi/mimikatz" target="_blank">Mimikatz </a>experimental for addsid</li><li><a href="http://rycon.hu/papers/goldenticket.html" target="_blank">Golden tickets</a> - easy to do, hard to detect, medium remediation</li><li><a href="http://www.beneaththewaves.net/Projects/Mimikatz_20_-_Silver_Ticket_Walkthrough.html" target="_blank">Silver tickets</a> - easy to do, hard to detect, medium/hard remediation</li><li>Backdoor workstations/servers via group policy</li><ul><li>HKEY_LOCAL_MACHINE\ Software\ Microsoft\ Windows\ CurrentVersion\ RunOnce,</li><li>scheduled tasks (run task 2 years later),</li><li><a href="http://www.labofapenetrationtester.com/2012/05/fun-with-sticky-keys-utilman-and.html" target="_blank">sticky-keys with debug</a></li></ul><li><a href="https://www.youtube.com/watch?v=Mz9Bg9KAKBs" target="_blank">Backdoor patch management tool</a>, see <a href="https://www.trustedsec.com/files/Owning_One_Rule_All_v2.pdf" target="_blank">slides here</a></li></ul><div>[Update 2017.01.10]</div><ul><li>Assign <a href="http://www.harmj0y.net/blog/activedirectory/the-most-dangerous-user-right-you-probably-have-never-heard-of/" target="_blank">SeEnableDelegationPrivilege</a> to a user </li><li><a href="https://adsecurity.org/?p=1714" target="_blank">Directory Service Restore Mode (DSRM)</a></li><li><a href="https://adsecurity.org/?p=1760" target="_blank">Malicious Security Support Provider (SSP)</a></li><li><a href="https://adsecurity.org/?p=1785" target="_blank">DSRMv2</a></li><li><a href="https://adsecurity.org/?p=1906" target="_blank">AdminSDHolder</a></li><li><a href="https://adsecurity.org/?p=2716" target="_blank">Edit GPO</a> </li></ul><br/><br/><h3>Other tricks</h3>The following list does not fit in the previous "instant admin" tips, but still, it can make the attackers life easier if their primary foothold has been disabled:<br/><br/><ul><li>Backdoor recent backups - and when the backdoor is needed, destroy the files, so the files will be restored from the backdoored backup</li><li>Backdoor the Exchange server - get a copy of emails</li><li>Backdoor workstation/server golden image</li><li>Change permission of logon scripts to allow modification later</li><li>Place malicious symlinks to file shares, collect hashes via SMB auth tries on specified IP address, grab password hashes later</li><li>Backdoor remote admin management e.g. HP iLO - e.g. create new user or steal current password</li><li>Backdoor files e.g. on shares to use in SMB relay</li><li>Backdoor source code of in-house-developed software</li><li>Use any type of sniffed or reused passwords in new attacks, e.g. network admin, firewall admin, VPN admin, AV admin, etc.</li><li>Change the content of the proxy pac file (change browser configuration if necessary), including special exception(s) for a chosen domain(s)  to use proxy on malicious IP. Redirect the traffic, enforce authentication, grab password hashes, ???, profit.</li><li>Create high privileged users in applications running with high privileges, e.g. MSSQL, Tomcat, and own the machine, impersonate users, grab their credentials, etc. The typical pentest path made easy.</li><li>Remove patches from servers, change patch policy not to install those patches.</li><li>Steal Windows root/intermediate CA keys</li><li>Weaken AD security by changing group policy (e.g. re-enabling LM-hashes)</li></ul>Update [2015-09-27]: I found this <a href="https://www.youtube.com/watch?v=w6761-NWmj4" target="_blank">great presentation</a> from Jakob Heidelberg. It mentions (at least) the following techniques, it is worth to check these:<br/><ul><li>Microsoft Local Administrator Password Solution</li><li>Enroll virtual smart card certificates for domain admins</li></ul><br/><h3>Forensics</h3><div>If you have been chosen to remediate a network where attackers gained domain admin privileges, well, you have a lot of things to look for :)</div><div><br/></div><div>I can recommend two tools which can help you during your investigation:</div><div><br/><ul><li><a href="http://blogs.microsoft.com/cybertrust/2013/06/03/microsoft-releases-new-mitigation-guidance-for-active-directory/" target="_blank">AD explorer tool to diff AD</a></li><li><a href="http://www.ntdsxtract.com/downloads/ntdsxtract/ntds_forensics.pdf" target="_blank">NTDS forensics</a></li><li><a href="https://bitbucket.org/iwseclabs/bta" target="_blank">BTA</a> (thx to Sn0rkY)</li></ul><br/><br/></div><h3>Lessons learned</h3>But guess what, not all of these problems are solved by rebuilding the AD. One has to rebuild all the computers from scratch as well. Which seems quite impossible. When someone is creating a new AD, it is impossible not to migrate some configuration/data/files from the old domain. And whenever this happens, there is a risk that the new AD will be backdoored as well.<br/><br/>Ok, we are doomed, but what can we do? I recommend proper log analysis, analyze trends, and detect strange patterns in your network. Better spend money on these, than on the domain rebuild. And when you find something, do a proper incident response. And good luck!</div><div style="text-align: justify;"><br/></div><div style="text-align: justify;">Ps: Thanks to Andrew, EQ, and Tileo for adding new ideas to this post.<br/><br/>Check out the <a href="https://jumpespjump.blogspot.hu/2015/05/many-ways-of-malware-persistence-that.html" target="_blank">host backdooring</a> post as well! :)</div></div>