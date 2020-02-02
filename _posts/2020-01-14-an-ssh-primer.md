---
layout: post
title:  "An SSH primer"
date:   2020-01-14 17:07:00 -0600
permalink: /ssh-primer/
---
SSH stands for Secure Shell. It is a protocol used to securely communicate with a server remotely. Using SSH you can login to a remote server and run terminal commands much like telnet.  The difference is that while telnet connection is unencrypted, SSH connections are encrypted and thus secure. SSH uses port 22 of TCP.

There are several clients which allow you to SSH to remote hosts on different OSes. On Mac or Linux you can use the terminal program and ssh command to use SSH.  In Windows you can use a third party program like PuTTY. 

For the purposes of this tutorial, I will be using Linux.  All of this can be applied equally to Mac as well as Windows(with some modifications). The machine on which we are working will be client and the machine that will be connected to remotely using SSH will be the host. 

There are 2 ways in which SSH is implemented on the remote machine

Password login - less secure than key pair based authentication but still better than telnet. 

Key based login - extremely strong encryption which is hard to break. 

### SSH in Linux

SSH in Linux is implemented using OpenSSH.

To log in to a server using ssh and start running commands

```
ssh <user>@<ip>
```

`ip` can be replaced with the FQDN of the server. 

This will ask you for a password. You will enter the password even though the characters do not get displayed on the terminal. 

You can connect to the remote host using SSH because of the SSH service, called `sshd` running on the host.

Once you are logged in, you can check the status of the ssh service on the remote host using 

`systemctl status sshd `

Which shows whether the service is running. 

### SSH configuration

The ssh configuration for connecting to the host is stored in `/etc/ssh/sshd_config`. Open this file. Some interesting directives are

- `Port 22` - by default SSH listens on port 22. To change this, edit this line to include a different port. Be sure that the port is not being used by some other service and is allowed on the firewall. Once the port has been changed, include `-p <port_num>` option in the `ssh` command to login.
- `AuthKeysFile .ssh/authorized_keys` - which keys the host should trust. More on this in the section on key based logins.
- `PasswordAuthentication yes` - allows for SSH password based login. To disable password based and allow only for the stricter key based, change this to no.

If any change has been made, restart the `sshd` service using `systemctl restart sshd`

You can learn more about this config file using `man sshd_config`

### Key based login

The problem with password based login is that a bot or script can try all combinations of password to try to login using SSH. You may have a strong password, but it is still possible that the password can be cracked.

To overcome this, we use key based SSH. You generate a key pair - one private and one public. The private key is kept secret and should be stored on the client. The public key can be shared to a host and is copied there. The only way that the client can login to the host is if the private key on client matches the public key on the remote host. A hacker can only login if they have the exact private key. Theoretically it is possible for a hacker to crack the private key, although practically it is impossible to do so.

In fact it is common once the key pair is generated to keep the private key on the client and share the public to any number of hosts you want to login using SSH.

### Generating the key pair

To generate a key pair use the following command

```
ssh-keygen
```

This asks for a number of options, including where the key pair should be stored and a passphrase. The passphrase is for added security on top of the key pair. Remember this as you will be asked for this everytime you login using the keys.

By default once you have completed running this command, the keys are stored in `~/.ssh`. The private key is `~/.ssh/id_rsa` and the public key is `~/.ssh/id_rsa.pub`. 

The private key can be kept on the client. However the public key needs to be copied.

To copy, you can use `ssh-copy-id` command. This will copy the public key to hosts `~/.ssh/known_hosts` file. In fact this file will contain an entry for every public key, each entry for a different client. However this command assumes that you still have password based authentication on the host. If this is disabled, then you can copy-paste `~/.ssh/id_rsa.pub` into the host's `~/.ssh/known_hosts` file.

Once you have done the above steps, you can use the key to logging so

```
ssh <user>@<ip> -i <path_to_private_key>
```

### An easier way to login from the client

Notice that if the the key based login is used, the command to login becomes quite cumbersome. If you include the port number with -p(if the port is other than the default 22), it is even longer. Also it may be possible that you have multiple private keys, different users for testing etc. It would be nice to have shortcuts that include this info and quickly allow you to login. Luckily, you can include this info in your clients `~/.ssh/config` file and it will make the login process easier.

To begin with, you can make an entry in `~/.ssh/config` like so

```
Host myserver
    User <user>
    HostName <ip>
    IdentityFile <path_to_private_key>
    Port <port_num>
```

Now you can just login using `ssh myserver`. You can make as many entries as you like in this file.


kjnjh
