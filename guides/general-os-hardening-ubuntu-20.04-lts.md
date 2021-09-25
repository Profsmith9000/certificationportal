# General OS Hardening \(Ubuntu 20.04 LTS\)

```
ssh root@yourIPaddress
```

## Patching Software

```
sudo apt update
sudo apt upgrade
```

### Making New User 

You will disable the root user becasue you have sudo
```
adduser "you"
```
Add that user to sudo group

```
usermod -aG sudo "you"
```
Change the logged root to create an account 

```
su - username 
```
Check to see if your user is part of sudo group

``` 
sudo whoami
```

If it returns root you now have sudo privlages 

```
exit
exit
```

Now move the root account and log out of ssh. Log back in with the created user account.

```
ssh user@yourIPaddress
```
#### Locking root

Remove the root account from logging to ssh

```
sudo nano /etc/ssh/sshd_config
```

Find "PermitRootLogin" and change to "PermitRootLogin no"

Save and exit

Now restart ssh 

``` 
sudo service ssh restart
```

Make sure if you try to log in with root again you get "permission denied"

##### Change ssh port and lockout policy

Check if firewall is enabled

```
sudo ufw status
```

Type in this if it is active

```
sudo ufw allow ssh 
```

If you changed the port add this one too

```
sudo ufw allow ---
```

Now change the port

```
sudo nano /etc/ssh/sshd_config
```

Uncomment these lines and type in

```
Port ---
MaxAuthTries 5
```
You can change the max tries if need be. 
If the password is wrong more than 5 times it will lock out you IP address

Save and Exit

Restart ssh

``` 
sudo service ssh restart
```

To log back in you need to

```
ssh user@yourIPaddress -p "yourport"
```
###### SSH Settings

```
sudo nano /etc/ssh/sshd_config
```

First thing you are going to enable is Protocol 2

![image](https://user-images.githubusercontent.com/55625996/134785667-4b2dfdee-db1b-4a8c-b13d-778ca4f526cb.png)

```
sudo system restart ssh
```

Timeout Idle value

If you're AFK while connected to ssh there could be an issue. You can decrease or increase the time you can be idle. 

```
ClientAliveInterval 180
```
