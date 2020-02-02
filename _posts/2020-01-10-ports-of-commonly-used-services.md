---
layout: post
title:  "Commonly Used Service Ports"
date:   2020-01-10 18:23:00 -0600
permalink: /commonly-used-service-ports/
---
Ports of some commonly used services. Unless otherwise specified the ports are tcp.

- **FTP** uses 20(for data),21(for commands)
- **SSH** uses 22
- **SMTP** uses 25
- **HTTP** uses 80
- **HTTPS** uses 443
- **POP3** uses 110 <sup>1</sup>
- **IMAP** uses 143 <sup>1</sup>
- **DNS** uses 53 tcp/udp
- **NFS** uses 2049
- **Samba** uses 137, 138, 139
- **Squid** uses 3128
- **Puppet Server** uses 8140
- **TFTP** uses 69 udp
- **Rsync** uses 873
- **DHCP** uses 67 udp(DHCP server), 68 UDP(DHCP client)
- **MySQL** uses 3306

<sup>1</sup> POP3 and IMAP are used by email clients to retrieve emails from server. The difference is that POP3 deletes the emails on the server after they have been retrieved by the client and IMAP keeps the emails on the server even after they have been retrieved.
