---
title: Nocturnal
tags:
  - htb
  - easy
---

#### Author: AMD

![nocturnal](../../images/htb/nocturnal/nocturnal.png)

This is a writeup for <b>Nocturnal</b> machine.

My target IP: 10.10.11.64

-----------------------------------------------------------------------------------
<b>NMAP</b>

Lets scan out target.

![nmap](../../images/htb/nocturnal/nmap.png)

-----------------------------------------------------------------------------------
<b>CURL</b>

![curl](../../images/htb/nocturnal/curl.png)

Add "10.10.11.64   nocturnal.htb" to /etc/hosts

-----------------------------------------------------------------------------------
<b>GOBUSTER</b>

![gobuster](../../images/htb/nocturnal/gobuster.png)

we can not access these urls

-----------------------------------------------------------------------------------
<b>WEBSITE INSPECTION</b>

![website](../../images/htb/nocturnal/website.png)

First register then login

-----------------------------------------------------------------------------------
<b>TEST FILE UPLOAD</b>

Test file upload with .php file

See the valid file types

Upload valid file and see the link

![file_link](../../images/htb/nocturnal/file_link.png)

-----------------------------------------------------------------------------------
<b>FIND OTHER USERS</b>

Lets try to find the other users and use the file link to access other users files.

use https://github.com/C0euss/Nocturnal-Username-Enumeration/tree/main to find other usernames

![enumeration](../../images/htb/nocturnal/enumeration.png)

-----------------------------------------------------------------------------------
<b>GUESS FILE NAMES</b>

![amandas_file](../../images/htb/nocturnal/amandas_file.png)

http://nocturnal.htb/view.php?username=amanda&file=privacy.odt

-----------------------------------------------------------------------------------
<b>VIEW FILE</b>

![amanda_password](../../images/htb/nocturnal/amanda_password.png)

login with amanda and password

-----------------------------------------------------------------------------------
<b>VIEW PAGES</b>

![amandas_page](../../images/htb/nocturnal/amandas_page.png)

go to admin panel

![admin_panel](../../images/htb/nocturnal/admin_panel.png)

-----------------------------------------------------------------------------------
<b>VIEW LOGIN.PHP</b>

![login_php](../../images/htb/nocturnal/login_php.png)

Get db information

-----------------------------------------------------------------------------------
<b>RCE</b>

Enter "dummy" to create backup password field, catch the request with burpsuite and send it to repeater.

url CyberChef to url encode

![cyber_chef](../../images/htb/nocturnal/cyber_chef.png)

send with repeater

![burp](../../images/htb/nocturnal/burp.png)

-----------------------------------------------------------------------------------
<b>SSH</b>

Crack tobias's password using https://crackstation.net/

![crackstation](../../images/htb/nocturnal/crackstation.png)

connect with ssh

![ssh](../../images/htb/nocturnal/ssh.png)

-----------------------------------------------------------------------------------
<b>GET USER FLAG</b>

![user_txt](../../images/htb/nocturnal/user_txt.png)

-----------------------------------------------------------------------------------
<b>NETSTAT</b>

Use netstat to use what is working on the machine

![netstat](../../images/htb/nocturnal/netstat.png)

-----------------------------------------------------------------------------------
<b>PF</b>

We cannot visit http://127.0.0.1:8080 so lets try port forwarding

![pf1](../../images/htb/nocturnal/pf1.png)

![pf2](../../images/htb/nocturnal/pf2.png)

Lets visit http://127.0.0.1:8080

![isp_login](../../images/htb/nocturnal/isp_login.png)

-----------------------------------------------------------------------------------
<b>LOGIN</b>

Lets try to login with tobias's credentials --> doesnt work

Lets try other usernames we found (amanda, admin)  --> admin works

![isp_home](../../images/htb/nocturnal/isp_home.png)

-----------------------------------------------------------------------------------
<b>EXEMINE SITE</b>

![isp](../../images/htb/nocturnal/isp.png)

Lets find vulnerability for this version ISPConfig 

![isp_volnurability_search](../../images/htb/nocturnal/isp_volnurability_search.png)

![code](../../images/htb/nocturnal/code.png)

-----------------------------------------------------------------------------------
<b>EXPLOIT VULNURABILITY</b>

run "nano CVE-2023-46818.py"  --> paste the code  --> save and exit

![run_exploit](../../images/htb/nocturnal/run_exploit.png)

-----------------------------------------------------------------------------------
<b>GET ROOT FLAG</b>

![flag](../../images/htb/nocturnal/flag.png)
