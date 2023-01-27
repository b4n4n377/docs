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

## Trigger ssh commands on Debian 11 via GitHub Actions

```bash
# generate ed25519 ssh key without password, allow the user itself to access the server, copy the key to the  clipboard
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -C "user@server"
cat ~/.ssh/id_ed25519.pub > ~/.ssh/authorized_keys
cat ~/.ssh/id_ed25519.pub # <copy>

# add the public key to GitHub Actions repository deploy keys
  # Settings -> Deploy Keys -> Add Deploy Key -> <paste>
  
# add GitHub Actions Repository secrets
  # Settings -> Secrets and variables -> Actions -> New Repository Secret ->
    # SSH_PRIV_KEY -> the belonging ssh private key
    # SSH_HOST     -> the ip address of the server
    # SSH_USER     -> the ssh user
    # SSH_PORT     -> the port sshd listens on
```

Example flow in repository/.github/workflows/:

```yaml
name: remote ssh command
on: [push]
jobs:

  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
    - name: executing remote ssh commands using password
      uses: appleboy/ssh-action@v0.1.7
      with:
        host: ${{ secrets.SSH_HOST }}
        username: ${{ secrets.SSH_USER }}
        key: ${{ secrets.SSH_PRIV_KEY }}
        port: ${{ secrets.SSH_PORT }}
        script: |
          whoami
          ls -al ~ 
```
## Wireguard Client on Debian 11
```bash
# install wireguard
sudo apt install wireguard

# generate keypair
mkdir ~/.wireguard/
cd ~/.wireguard/
(umask 077 && wg genkey > wg-private.key)
wg pubkey < wg-private.key > wg-public.key

# create client config
sudo nano /etc/wireguard/VPN.conf

    [Interface]
    Address = <client_ip>
    PrivateKey = <client_private_key>

    [Peer]
    PublicKey = <server_public_key>
    AllowedIPs = <server_internal_ip>,<server_internal_network>
    Endpoint = <server_public_ip:port>

# enable and restart service 
systemctl enable wg-quick@VPN.service
systemctl restart wg-quick@VPN.service    
systemctl status wg-quick@VPN.service

# show connection status
sudo wg show VPN
```

