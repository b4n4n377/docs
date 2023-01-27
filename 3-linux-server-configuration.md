---
title: Linux Server Configuration
parent: Software-Stuff
has_children: false
nav_order: 1
---

# Linux Server Configuration

## Fail2Ban & OpenSSH Debian 11

```bash
# update system and install fail2ban
sudo apt update && sudo apt upgrade
sudo apt install fail2ban

# change port in sshd config and restart service
sudo vi /etc/ssh/sshd_config 
  # Port 1234
sudo systemctl restart sshd

# adapt fail2ban sshd config, enable and restart the service
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

# watch the log
sudo tail -f /var/log/fail2ban.log
```
