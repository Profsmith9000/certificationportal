# Operating with Cold Keys

One of the primary things to keep in mind about cold keys is that they are often kept on cold nodes which are not connected to a main blockchain and are offline. 

The primary use of cold keys is to generate a operational certificate along with the verification key and KES period.

This will show the code needed to generate an operational certificate or node.cert:

```
cardano-cli shelley node issue-op-cert \
 --kes-verification-key-file kes.vkey \
 --cold-signing-key-file cold.skey \
 --operational-certificate-issue-counter cold.counter \
 --kes-period 65 \
 --out-file node.cert
 ```
 
 (The code used comes from https://iohk.zendesk.com/hc/en-us/articles/900001209326-Node-s-operational-certificate-and-KES-period-stake-pools-)
 
 ![](https://user-images.githubusercontent.com/73238815/134784136-067cb02b-8117-4eb0-b9a3-422a27d670e7.png)
