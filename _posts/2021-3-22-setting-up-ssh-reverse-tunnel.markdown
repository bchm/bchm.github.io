---
layout: post
title:  "Setting up a reverse SSH tunnel"
date:   2020-12-27 15:19:23
categories: ssh reverse tunnel
---

I'm writing down the process of how I made a reverse SSH tunnel so when a machine boots it will automatically connect with a VPS, but that VPS only has a way of connecting with it via two factor authentication. I will share what I learned and how I made a user with that only returns /bin/false when connected to from the machine that needs to be reverse SSH tunneled.

In the future I will automate this process in Ansible and write a blog post about that.