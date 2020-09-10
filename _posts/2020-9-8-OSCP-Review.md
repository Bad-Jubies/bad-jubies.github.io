---
layout: post
title: OSCP Review
---

In this post I will outline my experience with [Offensive Security's PWK (Penetration Testing with Kali Linux)](https://www.offensive-security.com/documentation/penetration-testing-with-kali.pdf) course and the accompanying [OSCP (Offensive Security Certified Professional) exam](https://support.offensive-security.com/oscp-exam-guide/).

<img src="/images/Group-175@2x.png" class="center-image" style="width:200px;"/>



## MY BACKGROUND AND PREPARATION

Before enrolling in the PWK course I was working IT helpdesk (answering phones, making tickets, etc ...). I had earned my CompTIA A+ and Network+ Certifications in 2019 and was looking to earn my Security+ to complete the trifecta. While researching Security+ in January 2020, I discovered the fabled OSCP exam on some reddit threads. OSCP sounded way more interesting than Security+, so I decided to give it a shot. I downloaded the official Kali Image, signed up for HackTheBox, and started binging Ippsec videos on youtube. I went through about 20 Ippsec videos prior to signing up for PWK. 

Offensive Security lists the following as course [prerequisites](https://www.offensive-security.com/pwk-oscp/): Solid understanding of TCP/IP networking, Reasonable Windows and Linux administration experience, Familiarity of Bash, and scripting with basic Python or Perl. I did not have any linux experience prior to my first Kali install. I also did not have any bash/scripting experience. This definitely put me at a disadvantage during the course because I was continuously researching bash/python syntax while running exploits. If you are not on a time crunch, I would recommend taking a short python course and reading the linux man pages.

## PWK LABS

I signed up for PWK in February and started the course mid-march with 90 days of lab access. It took me about 60 days to get through the monstrous 850+ page PDF. At the end of the 90 days I had root/admin on 25 machines with a user shell on an additional 6.

The PWK course material was phenomenal. I found a lot of value in watching the videos first while reading the corresponding section in the PDF at the end of each video. The content in the videos and PDF is not always the same for each module. My biggest piece of advice is to take your time. Some people recommend blazing through the material in a week or two and spending the rest of your time in the labs. This may be a good approach if you have some penetration testing experience. If you're new like me, I suggest going slowly through all of the videos and PDF to make sure that you fully understand the material before dedicating a lot of time to the labs.   

## THE EXAM

The OSCP exam is a scary, exciting, and tiresome marathon. You are given a 24 hour VPN connection to 5 machines with varying point values. The objective is to obtain user and root flags on each of the machines. [You need 70 points to pass the exam.](https://www.offensive-security.com/offsec/pwk-oscp-faq/) I attempted the exam on June 12th at 9:00 AM. I scored 35 points from 2 machines within the first couple of hours, but struggled to find the correct exploitation paths on the remaining servers over the next 10+ hours. I ultimately ended my exam with about 60 points - not enough to pass. The hardest part about the exam is the rabbit holes. The exam machines are designed to deceive. There are some exploitation paths that look very promising, but only lead to dead ends. The reason I failed was because I became hyper-focused on the dead ends rather than taking a step back to reevaluate my options.



With one exam attempt behind me, I immediately scheduled my next attempt for July 17th at 8:00 PM. I focused entirely on improving my enumeration in the month leading up to the second attempt. I completed an additional 35 retired HackTheBox machines and intensely studied [Ippsec](https://ippsec.rocks/?#) and [0xdf's](https://0xdf.gitlab.io/) enumeration methodologies. The night of exam I made a big ole pot of coffee, connected to the vpn, greeted the proctors, and started hacking away. This time I was able to quickly identify the truly vulnerable services and applications most of the machines. After 2 hours I once again had 35 points. Another 5 hours later and I was back to roughly 60 points. At this point I was sitting on two low privileged shells. All I had to do was escalate my privileges and claim my certification. I decided to sleep for a few hours and approach privesc with a fresh perspective. After arriving back to the terminal, I elevated both shells in under 2 hours. Knowing that I had enough points to pass, I relaxed and focused on writing clear and concise documentation paired with screenshots for each machine. When the VPN connection closed I had rooted 4 boxes. I took another short nap and completed my report.


After 3 days (which felt like an eternity) I received this glorious email:
<img src="/images/PassedOSCP.jpg" class="center-image" />



## THE FUTURE

Earning my OSCP designation was a bitter sweet moment. I feel validated knowing that I have the technical know-how to pass the exam, but I miss the grind of chasing the certification. I still feel like a complete noob and intend on continuing my infosec education.

I have recently started a Desktop support role, but intend to continue my infosec education. My goal for the remainder of 2020 is to learn more about web exploit development and earn my OSWE certification. I signed up to start Offensive Security's AWAE course in October and hope to be ready for the OSWE exam by December. 

## RESOURCES

Feel free to reach out to me [on Twitter](https://twitter.com/Bad_Jubies) if you have any questions :) 

These are some of the resources that I referenced during my OSCP studies:



| Name | Type | 
| :---: | :---: |
| [Vulnhub Privesc Guide](https://github.com/Ignitetechnologies/Privilege-Escalation) | Guide |
| [Pentest Monkey Reverse Shells](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) | Cheat Sheet | 
| [Total OSCP Guide](https://sushant747.gitbooks.io/total-oscp-guide/content/) | Cheat Sheet |
| [winPEAS/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite) | Script |  
| [Do Stack Buffer Overflow Good](https://github.com/justinsteven/dostackbufferoverflowgood/blob/master/dostackbufferoverflowgood_tutorial.pdf) | Guide |
| [Enumeration Guide](https://github.com/theonlykernel/enumeration/wiki) | Cheat Sheet | 
| [Full MSSQL Injection PWNage](https://www.exploit-db.com/papers/12975) | Guide | 
| [Linux Privilege Escalation w/ SUID Binaries](https://www.hackingarticles.in/linux-privilege-escalation-using-suid-binaries/) | Guide | 
| [Autorecon](https://github.com/Tib3rius/AutoRecon) | Script | 
| [Pivoting](https://github.com/21y4d/Notes/blob/master/Pivoting.txt) | Cheat Sheet | 
| [GTFO Bins (Linux Binaries)](https://gtfobins.github.io/) | Cheat Sheet | 
| [LOLBAS (Windows Binaries)](https://lolbas-project.github.io/#) | Cheat Sheet | 
| [WFUZZ - Webapp Fuzzer](https://github.com/xmendez/wfuzz) | Script | 
| [OSCP Command Filtering Tool](https://github.com/peanuthacker92/oscp-command-filtering-tool) | Cheat Sheet | 
| [Nishang Reverse Shells](https://github.com/samratashok/nishang) | Script | 
| [Unix Wildcard Exploitation](https://www.defensecode.com/public/DefenseCode_Unix_WildCards_Gone_Wild.txt) | Guide |
| [Rana Khalil HTB (metasploit free) Writeups](https://medium.com/@ranakhalil101) | Guide | 
| [0xdf Writeups](https://0xdf.gitlab.io/) | Guide |
| [Ippsec Videos](https://ippsec.rocks/?#) | Guide | 
| [NetSecFocus/TJ_NULL OSCP-like boxes](https://docs.google.com/spreadsheets/u/1/d/1dwSMIAPIam0PuRBkCiDI88pU3yzrqqHkDtBngUHNCw8/htmlview#) | Cheat Sheet|
| [SQLi Auth Bypass](https://pentestlab.blog/2012/12/24/sql-injection-authentication-bypass-cheat-sheet/) | Cheat Sheet | 
| [OSCP Practice Exam](https://h4cklife.org/posts/a-pre-exam-for-future-oscp-students/) | Guide | 
| [Payload All The Things](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master) | Cheat Sheet | 
| [Pentest.ws](https://pentest.ws/) | Cheat Sheet/Notetaking | 
| [TJ_NULL OSCP Joplin Reporting Template](https://github.com/tjnull/TJ-JPT) | Notetaking/Reporting |
| [Penetration Testing: Hands on Introduction](https://nostarch.com/pentesting) | Book | 
| [John Hammond](https://www.youtube.com/channel/UCVeW9qkBjo3zosnqUbG7CFw) | CTF Guides | 
| [Linux Privilege Escalation](https://www.udemy.com/course/linux-privilege-escalation/) | Udemy Course | 
| [Windows Privilege Escalation](https://www.udemy.com/course/windows-privilege-escalation/) | Udemy Course | 
| [Fuzzy Security - Windows Privesc](https://www.fuzzysecurity.com/tutorials/16.html) | Guide/Cheat Sheet | 
| [g0tmi1k - Linux Privesc](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) | Guide/Cheat Sheet | 
| [Python Bootcamp](https://www.udemy.com/course/complete-python-bootcamp/) | Udemy Course | 
| [Kali Linux Revealed](https://www.kali.org/download-kali-linux-revealed-book/) | Book (Free PDF) |
| [The Linux Command Line](https://nostarch.com/tlcl2) | Book | 
| [Infosec Prep Discord](https://discord.com/invite/mEtEFhp) | Discord Server | 
| [PortSwigger Web Academy](https://portswigger.net/web-security) | Guide |
| [Linux Man Pages](https://www.kernel.org/doc/man-pages/) | Cheat Sheet | 
{:.mbtablestyle}
