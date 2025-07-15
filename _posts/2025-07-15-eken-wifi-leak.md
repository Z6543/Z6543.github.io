---           
layout: post
title: 'CVE-MISSING - EKEN video doorbells leaking Wi-Fi credentials through HTTP to cloud servers'
date: 2025-07-15 08:15:00 UTC
comments: false
categories:  
---

CVE-ID:&nbsp;
CVE-MISSING

&nbsp;

[Suggested description]

Video doorbells developed by EKEN group periodically sends debug logs to the EKEN cloud servers. These debug logs contain sensitive information like Wi-Fi SSID and credentials.

Example log: 

<div style="text-align: left;">
[0000000654]{I] fua_video_get_config preview_resolution 0wdr_mode 0 wifi_test_mode 0 
<br/>
[0000000656][1970-01-01 00:00:00.925268]{D] ########################################### 
<br/>
[0000000658][1970-01-01 00:00:00.927269]{D] BT60PLUSEKDB_<REDACTED-<REDACTED-<REDACTED-<REDACTED-<REDACTED>
<br/>
[0000000659][1970-01-01 00:00:00.928104]{D] <SSID<PASSWORD>
<br/>  
[0000000659][1970-01-01 00:00:00.928274]{D] http://api.gdxp.com:8100 https://push.gdxp.com 47.243.113.135:139963 [tcp]47.254.95.196:9007 47.243.228.110:102587 47.107.28.145:17051 
  <br/>
[0000000659][1970-01-01 00:00:00.928436]{D] 9 http://oss-eu-central-1.aliyuncs.com de1-aiwit
</div>

<pre>
[VulnerabilityType Other]

Wi-Fi credentials exposed to cloud servers via clear-text HTTP

[Vendor of Product]

Eken
Product name: Video doorbell
Model: T6
Manufacturer: Topvision (Shenzen) Technology Co., Ltd

[Affected Product Code Base]
Eken Aiwit video doorbells - BT60PLUS_MAIN_V1.0_GC1084_20230531

[Affected Component]
HTTP PUT to de1-aiwit.oss-eu-central-1.aliyuncs.com/device_log/<date/<camera_id

[Attack Type Other]
Attacker listening to clear-text HTTP traffic in the upstream

[Impact Information Disclosure]&nbsp;
true


[Attack Vectors]
Attacker listening to clear-text HTTP traffic in the upstream

[Discoverer]
Zoltan Balazs

[Reference]
http://api.gdxp.com:8100
http://eken.com
http://oss-eu-central-1.aliyuncs.com
https://push.gdxp.com


Vulnerability timeline: 
2025-04-04: E-mail sent to support-us@eken.com &gt;support-us@eken.com&lt;, no reply received.
2025-05-18: E-mail sent to support-us@eken.com &gt;support-us@eken.com&lt;, no reply received.
2025-05-25: E-mail sent to support-us@eken.com &gt;support-us@eken.com&lt;, no reply received.
2025-07-15: Vulnerability published
  </pre>

