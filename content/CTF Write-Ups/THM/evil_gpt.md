---
title: Evil GPT
tags:
  - thm
  - easy
  - llm
---

#### Author: AMD

![[static/evil_gpt/evil_gpt.png]]

This is a writeup for <b>Evil GPT</b> room.

-----------------------------------------------------------------------------------
<b>FIND THE FLAG</b>

![[static/evil_gpt/pwd.png]]

It didn't let us use pwd. Lets check directories.

![[static/evil_gpt/ls.png]]

No flag here, lets try /root

![[static/evil_gpt/ls_root.png]]

We found the flag, lets try to read it.

-----------------------------------------------------------------------------------
<b>READ THE FLAG</b>

![[static/evil_gpt/cat_try.png]]

We can't read or change directory. Lets try sudo

![[static/evil_gpt/sudo_cat.png]]

It worked!!!
