# SSH Hardening

If you use Linux you most likely use SSH. SSH allows you to make connections without a password. 
## Enable passwordless Authentication 

```
PubkeyAuthentication yes
```

### Disable Password Authentication 
```
PasswordAuthentication no
```

#### Disable Empty Passwords 

Some user accounts are created without passwords, administrators of linux machines can create standard users without passwords. SSH does not prevent empty passwords from being allowed. 
```
PermitEmptyPasswords no
```

##### Disable Root Login
```
PermitRootLogin no
```

###### Defult SSH Port 
```
Port 12345 
```

####### Allow Users and Groups 
```
AllowUsers user1 user2
AllowGroups group1 group2
```

######## Disable X11 Forwarding 

X11 Forwarding allows anyone to tunnel GUI applications with SSH. You probably dont want that. 
```
X11Forwarding no 
```

######### Disable Gateway Ports 
```
GatewayPorts no 
```
########## Disable PermitUserEnvironment 
```
PermitUserEnvironment no
```

########### Disable Weak Cryptographic Algorithims 
```
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512,hmac-sha2-256
```

```
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes256-ctr
KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-512****
```
> You can test support algorithims using nmap
```
nmap -sV --script ssh2-enum-algos -p PORT TARGET
```

############# Regenerate Host Keys 
```
rm /etc/ssh/ssh_host_*
ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key -N ""
ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
```

############## Disable Host Keys 
```
#HostKey /etc/ssh/ssh_host_dsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
```

############### Disable Small Diffie-Hellman Key Sizes
```
awk '$5 >= 3071' /etc/ssh/moduli > /etc/ssh/moduli.safe
mv /etc/ssh/moduli.safe /etc/ssh/moduli
```

################ Disable SSHv1
```
Protocol 2
```
