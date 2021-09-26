# Configuration, Topology, Genesis

Starting the node and connecting it to the testnet requires three configuration files:

## config.json

This file defines what a node does, and includes the following four sub sections:

Basic node configuration, this declares the basic node configuration parameters. The basic configuration section might look like this:

```bash
{
  "Protocol": "TPraos",
  "GenesisFile": "ff-genesis.json",
  "RequiresNetworkMagic": "RequiresMagic",
```

Update parameters, Block-producing nodes use a protocol version number as part of the system for the agreement and synchronization of protocol updates. When configuring this section, you need to be aware of the latest version of the protocol supported by the network The update parameters section might look like this:

```bash
  "ApplicationName": "cardano-sl",
  "ApplicationVersion": 0,
  "LastKnownBlockVersion-Alt": 0,
  "LastKnownBlockVersion-Major": 2,
  "LastKnownBlockVersion-Minor": 0,
```

Tracing, Tracers tell the node what information you are interested in when logging. For example, switches that you can turn ON or OFF according to the type and quantity of information that you are interested in. This provides fairly coarse-grained control, but it is relatively efficient at filtering out unwanted trace output.

Logging control, It is possible to have more fine-grained control over filtering of trace output, and to match and route trace output to particular backends. This is less efficient than the coarse trace filters, but offers precise control options.

## topology.json

The information contained in this file tells the node which nodes in the network it should talk to.

A simple topology file might look like this:

```bash
   {
  "Producers": [
    {
      "addr": "x.x.x.x",
      "port": 3001,
      "valency": 1
    }
  ]
}
```

## genesis.json

The genesis file is generated with the cardano-cli by reading a genesis.spec.json file.

The genesis file is very important, as it sets four key parameters:

genDelegs: maps from genesis keys to genesis delegates. initialFunds: maps from the initial addresses to the initial values at those addresses. MaxLovelaceSupply: the total amount of lovelaces in the blockchain. startTime: the time of slot zero.

