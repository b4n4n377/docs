---
title: Linux Server Configuration
parent: Hardware-Stuff
has_children: false
nav_order: 1
---

# Linux Server Configuration

## Fail2Ban & OpenSSH Debian 11

```bash
sudo apt update && sudo apt upgrade
sudo apt install fail2ban

sudo vi /etc/ssh/sshd_config 
  # Port 1234

sudo systemctl restart sshd

sudo nano /etc/fail2ban/jail.d/defaults-debian.conf                                                                    
  # [sshd]
  # enabled = true
  # port = 1234
  # maxretry = 3
  # findtime = 10m
  # bantime = 1m

sudo systemctl enable fail2ban
sudo systemctl restart fail2ban
sudo systemctl status fail2ban

sudo tail -f /var/log/fail2ban.log
```
