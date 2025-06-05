---
title: Nocturnal
tags:
  - htb
  - easy
---

#### Author: AMD

![nocturnal](images/nocturnal.png)

This is a writeup for <b>Nocturnal</b> machine.

My target IP: 10.10.11.64

-----------------------------------------------------------------------------------
<b>NMAP</b>

Lets scan out target.

![nmap](images/nmap.png)

-----------------------------------------------------------------------------------
<b>CURL</b>

![curl](images/curl.png)

Add "10.10.11.64   nocturnal.htb" to /etc/hosts

-----------------------------------------------------------------------------------
<b>GOBUSTER</b>

![gobuster](images/gobuster.png)

we can not access these urls

-----------------------------------------------------------------------------------
<b>WEBSITE INSPECTION</b>

![website](images/website.png)

First register then login

-----------------------------------------------------------------------------------
<b>TEST FILE UPLOAD</b>

Test file upload with .php file

See the valid file types

Upload valid file and see the link

![file_link](images/file_link.png)

-----------------------------------------------------------------------------------
<b>FIND OTHER USERS</b>

Lets try to find the other users and use the file link to access other users files.

use https://github.com/C0euss/Nocturnal-Username-Enumeration/tree/main to find other usernames

![enumeration](images/enumeration.png)

-----------------------------------------------------------------------------------
<b>GUESS FILE NAMES</b>

![amandas_file](images/amandas_file.png)

http://nocturnal.htb/view.php?username=amanda&file=privacy.odt

-----------------------------------------------------------------------------------
<b>VIEW FILE</b>

![amanda_password](images/amanda_password.png)

login with amanda and password

-----------------------------------------------------------------------------------
<b>VIEW PAGES</b>

![amandas_page](images/amandas_page.png)

go to admin panel

![admin_panel](images/admin_panel.png)

-----------------------------------------------------------------------------------
<b>VIEW LOGIN.PHP</b>

![login_php](images/login_php.png)

Get db information

-----------------------------------------------------------------------------------
<b>RCE</b>

Enter "dummy" to create backup password field, catch the request with burpsuite and send it to repeater.

url CyberChef to url encode

![cyber_chef](images/cyber_chef.png)

send with repeater

![burp](images/burp.png)

-----------------------------------------------------------------------------------
<b>SSH</b>

Crack tobias's password using https://crackstation.net/

![crackstation](images/crackstation.png)

connect with ssh

![ssh](images/ssh.png)

-----------------------------------------------------------------------------------
<b>GET USER FLAG</b>

![user_txt](images/user_txt.png)

-----------------------------------------------------------------------------------
<b>NETSTAT</b>

Use netstat to use what is working on the machine

![netstat](images/netstat.png)

-----------------------------------------------------------------------------------
<b>PF</b>

We cannot visit http://127.0.0.1:8080 so lets try port forwarding

![pf1](images/pf1.png)

![pf2](images/pf2.png)

Lets visit http://127.0.0.1:8080

![isp_login](images/isp_login.png)

-----------------------------------------------------------------------------------
<b>LOGIN</b>

Lets try to login with tobias's credentials --> doesnt work

Lets try other usernames we found (amanda, admin)  --> admin works

![isp_home](images/isp_home.png)

-----------------------------------------------------------------------------------
<b>EXEMINE SITE</b>

![isp](images/isp.png)

Lets find vulnerability for this version ISPConfig 

![isp_volnurability_search](images/isp_volnurability_search.png)

![code](images/code.png)

-----------------------------------------------------------------------------------
<b>EXPLOIT VULNURABILITY</b>

run "nano CVE-2023-46818.py"  --> paste the code  --> save and exit

![run_exploit](images/run_exploit.png)

-----------------------------------------------------------------------------------
<b>GET ROOT FLAG</b>

![flag](images/flag.png)
