# General OS Hardening \(Ubuntu 20.04 LTS\)

```text
ssh root@yourIPaddress
```

## Patching Software

```text
sudo apt update
sudo apt upgrade
```

### Making New User

You will disable the root user becasue you have sudo

```text
adduser "you"
```

Add that user to sudo group

```text
usermod -aG sudo "you"
```

Change the logged root to create an account

```text
su - username
```

Check to see if your user is part of sudo group

```text
sudo whoami
```

If it returns root you now have sudo privlages

```text
exit
exit
```

Now move the root account and log out of ssh. Log back in with the created user account.

```text
ssh user@yourIPaddress
```

#### Locking root

Remove the root account from logging to ssh

```text
sudo nano /etc/ssh/sshd_config
```

Find "PermitRootLogin" and change to "PermitRootLogin no"

Save and exit

Now restart ssh

```text
sudo service ssh restart
```

Make sure if you try to log in with root again you get "permission denied"

**Change ssh port and lockout policy**

Check if firewall is enabled

```text
sudo ufw status
```

Type in this if it is active

```text
sudo ufw allow ssh
```

If you changed the port add this one too

```text
sudo ufw allow ---
```

Now change the port

```text
sudo nano /etc/ssh/sshd_config
```

Uncomment these lines and type in

```text
Port ---
MaxAuthTries 5
```

You can change the max tries if need be. If the password is wrong more than 5 times it will lock out you IP address

Save and Exit

Restart ssh

```text
sudo service ssh restart
```

To log back in you need to

```text
ssh user@yourIPaddress -p "yourport"
```

**SSH Settings**

```text
sudo nano /etc/ssh/sshd_config
```

First thing you are going to enable is Protocol 2

![image](https://user-images.githubusercontent.com/55625996/134785667-4b2dfdee-db1b-4a8c-b13d-778ca4f526cb.png)

```text
sudo system restart ssh
```

Timeout Idle value

If you're AFK while connected to ssh there could be an issue. You can decrease or increase the time you can be idle.

```text
ClientAliveInterval 180
```

