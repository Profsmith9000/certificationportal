# SSH Hardening

If you use Linux you most likely use SSH. SSH allows you to make connections without a password.

## Enable passwordless Authentication

```text
PubkeyAuthentication yes
```

## Disable Password Authentication

```text
PasswordAuthentication no
```

## Disable Empty Passwords

Some user accounts are created without passwords, administrators of linux machines can create standard users without passwords. SSH does not prevent empty passwords from being allowed.

```text
PermitEmptyPasswords no
```

## Disable Root Login

```text
PermitRootLogin no
```

## Defult SSH Port

```text
Port 12345
```

## Allow Users and Groups\*\*

```text
AllowUsers user1 user2
AllowGroups group1 group2
```

## Disable X11 Forwarding

X11 Forwarding allows anyone to tunnel GUI applications with SSH. You probably dont want that.

```text
X11Forwarding no
```

## Disable Gateway Ports

```text
GatewayPorts no
```

## Disable PermitUserEnvironment

```text
PermitUserEnvironment no
```

## Disable Weak Cryptographic Algorithims

```text
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr
KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512,hmac-sha2-256
```

```text
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes256-ctr
KexAlgorithms curve25519-sha256@libssh.org,ecdh-sha2-nistp521
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-512****
```

> You can test support algorithims using nmap
>
> ```text
> nmap -sV --script ssh2-enum-algos -p PORT TARGET
> ```

## Regenerate Host Keys

```text
rm /etc/ssh/ssh_host_*
ssh-keygen -t rsa -b 4096 -f /etc/ssh/ssh_host_rsa_key -N ""
ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key -N ""
```

## Disable Host Keys

```text
#HostKey /etc/ssh/ssh_host_dsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
```

## Disable Small Diffie-Hellman Key Sizes

```text
awk '$5 >= 3071' /etc/ssh/moduli > /etc/ssh/moduli.safe
mv /etc/ssh/moduli.safe /etc/ssh/moduli
```

## Disable SSHv1

```text
Protocol 2
```

