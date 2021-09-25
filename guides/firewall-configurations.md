# Firewall Configurations

 The UFW firewall can be used to control the network access to your node
 With any new installation, UFW is disabled by default. You will have to enable it with the following steps:
 
 Deny any incoming and outgoing traffit 
 ```
 sudo ufw default deny incoming 
 sudo ufw default allow outgoing 
 ```
 Allow ssh access 
 ```
 sudo ufw allow ssh (port 22 or your ssh port number) /tcp  
 ```
 Allow cardano-node p2p port 
 ``` 
 sudo ufw allow (your port #) /tcp
 ```
 Allow chrony ntp 
 ```
 sudo ufw allow 123/udp 
 ``` 
 Enable Firewall 
 ```
 sudo ufw enable 
 ```
 Verify status 
 ``` 
 sudo ufw status numbered
 ```
 Only open these following ports on nodes behind a network firewall 
 Allow grafana web server port 
 ``` 
 sudo ufw allow 3000/tcp 
 ``` 
 Allow prometheus endpoint port
 ``` 
 sudo ufw allow 9100/tcp
 ``` 
 Allow prometheus cardano-node metric data port 
 ``` 
 sudo ufw allow 12798/tcp 
 ``` 
 This next step is optional but recomended to follow
 Permitting connections from a specific IP can be set up by following these next commands 
 ``` 
 sudo ufw allow (your laptop) 
 ``` 
 Example 
  - sudo ufw allow from (182.382.84.22)
