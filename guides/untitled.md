# Starting the Node

Follow these steps to get a relay node running and/or a block producer node 
# Relay 
``` 
cardano-node run \
--topology mainnet-topology.json \
--database-path /db \
--socket-path /db/node.socket \
--host-addr <PUBLIC IPv4 ADDRESS> \
--port <PORT> \
--config mainnet-config.json
``` 
# Block Producer 
``` 
cardano-node run \
--topology mainnet-topology.json \
--database-path /db \
--socket-path /db/node.socket \
--host-addr <PUBLIC IP> \
--port <PORT> \
--config mainnet-config.json \
--shelley-kes-key kes.skey \
--shelley-vrf-key vrf.skey \
--shelley-operational-certificate node.cert
``` 
