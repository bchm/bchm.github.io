---
layout: post
title:  "Creating reverse SSH tunnel on server with 2 factor authentication SSH access"
date:   2021-3-22 15:19:23
categories: ssh reverse tunnel
---

I have used different guides which I combined together to make a reverse SSH tunnel which works with 2 factor authentication for extra security.

## Step 0

Automatic wifi on boot:

with netctl: http://www.linuxandubuntu.com/home/how-to-setup-a-wifi-in-arch-linux-using-terminal

**Sources used:**

https://linuxhostsupport.com/blog/how-to-setup-reverse-ssh-tunnel-on-linux/

https://cnly.github.io/2018/08/16/setting-up-autossh-to-maintain-a-reverse-tunnel-ssh-server-having-a-dynamic-ip-address.html

**Client** here means the computer that is behind a firewall and needs to be reached from the VPS.

**Server** here means the VPS that has ports that can be opened.

## Step 1

I have a server with one user where there can be only logged in with 2-factor authentication. This is not practical for an automatic SSH reverse tunnel, so I made a extra user which gives no shell back if you log into it, but just runs /bin/false when you log in. 

Now follow the instructions of the heading "Setting up SSH Environment" from this guide: https://cnly.github.io/2018/08/16/setting-up-autossh-to-maintain-a-reverse-tunnel-ssh-server-having-a-dynamic-ip-address.html

Add this line in /etc/pam.d/sshd:

``` auth [success=done default=ignore] pam_succeed_if.so user ingroup skip2factorauth ```

This line has to be pasted before the @include common-password line and the lines that auth requires for the 2factor authentication!

```bash
groupadd skip2factorauth
```

```bash
usermod -a -G skip2factorauth sshtunuser
```

## Step 2

We are now going to make sure that the new user can make a connection **from the VPS: **

VPS as root: ```sudo -u sshtunuser bash```

We can now manually input the public SSH key from the client that is going to be SSH reverse tunneled:

VPS as sshtunuser:

```bash
ssh-keygen
```

```bash
cd .ssh/
```

```bash
touch authorized_keys
```

```bash
chmod 600 authorized_keys
```

```bash
chmod 700 sshtunuser/.ssh
```

and place the public key in the file.

To log in with the new user. This user has minimal access to the rest of the system. It does not have root and has a custom home directory.

This user will be added to a group that does not need 2 factor authentication to get logged in, but don't worry because this user has almost no rights.

Now that should work. Test if there can be a connection made to sshtunuser, it should return nothing and exit the SSH session.

## Step 4

Now for the *client* computer we set this up:

Log in with the user of the client computer where connections may go to from the server. Then:

```bash
mkdir -p .config/systemd/user/default.target.wants
```

But making the file in this folder did not work, so I created the same file in the parent directory.

And make a file in the default.target.wants folder and the systemd/user folder called autossh.service with the following content:

```bash
[Unit]
Description=Autossh
[Service]
ExecStartPre=/bin/sleep 10
ExecStart=/usr/bin/autossh -M 0 -N -o "ServerAliveInterval 15" -o "ServerAliveCountMax 3" -o "ConnectTimeout 10" -o "ExitOnForwardFailure yes" -i /home/[user]/.ssh/id_rsa sshtunuser@[ipaddress_of_server] -R 24553:localhost:22
Restart=always
RestartSec=10
[Install]
WantedBy=default.target
```

Before the SSH reverse tunnel works, the **server** has to be in the authorized_keys file of the **client as well!** and in the known_hosts.

Make sure that the server and client computers can SSH into each other without using 2FA auth (ultimately with only a normal user, so root can not log in directly)!

The system service can be tested by closing all connections to the server and running this **command**:

``` bash
systemctl --user start --now autossh
```

Now, if the system service work, there can be logged in into the client via another computer with SSH connected to the server with:

```ssh [username of client server]@localhost -p 24553```

If this works, the system service can be activated and the system can be rebooted. The SSH reverse tunnel should work immediately on boot.

```bash
systemctl --user enable --now autossh
```

```bash
systemctl --user status
```

I hope this helps!
