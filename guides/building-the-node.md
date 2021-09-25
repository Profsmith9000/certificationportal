# Building the Node

This guide takes heavy inspiration and much of it's information from the following guide.
(https://github.com/input-output-hk/cardano-node)

Integration of the ledger, consensus, networking and node shell repositories.

Logging is provided as a feature by the node shell to the other packages.

The cardano-node is the top level for the node and aggregates the other components from other packages: consensus, ledger and networking, with configuration, CLI, logging and monitoring.
The node no longer incorporates wallet or explorer functionality. The wallet backend and explorer backend are separate components that run in separate external processes that communicate with the node via local IPC.

# Network Configuration, Genesis and Topology Files
![image](https://user-images.githubusercontent.com/90267622/134778963-2972d4fe-5c21-4b3b-88ec-e8cd111f8b1c.png)

![image](https://user-images.githubusercontent.com/90267622/134778975-ec566ea2-7ca8-4af8-86bd-cb39b4b245cf.png)

Keep in mind that the latest supported networks can be found at https://hydra.iohk.io/job/Cardano/cardano-node/cardano-deployment/latest-finished/download/1/index.html

# How to build
![image](https://user-images.githubusercontent.com/90267622/134779709-56b66784-d191-48a5-93a7-6c7041ea5e08.png)

Documentation for building the node can be found at the following link

(https://docs.cardano.org/getting-started/installing-the-cardano-node)

Make sure that you download the latest version of cardano-node and cardano-cli.

https://hydra.iohk.io/build/7760210 (Linux)
https://hydra.iohk.io/build/7760171 (Win64)
https://hydra.iohk.io/build/7760341 (Macos)

# Windows Executable
![image](https://user-images.githubusercontent.com/90267622/134779721-db3d5cb5-ff7f-488e-8fab-0d2e975ded92.png)

This can be downloaded from the following link.
https://hydra.iohk.io/build/7760171

The download includes cardano-node.exe and a.dll. to run the node with cardano-node run you need to reference a few files and directories as arguments. You can just copy these directly from the cardano-node repo into the executables directory. The command to run the node on mainnet looks like this.

```
cardano-node.exe run --topology ./mainnet-topology.json --database-path ./state --port 3001 --config ./configuration-mainnet.yaml --socket-path \\.\pipe\cardano-node
```

# Docker image
![image](https://user-images.githubusercontent.com/90267622/134780022-86f9429f-f917-425a-b882-a118fbcb5a2f.png)

The docker image with the latest version of cardano-node is availible at the link below
https://hub.docker.com/r/inputoutput/cardano-node

# cardano-node

![image](https://user-images.githubusercontent.com/90267622/134782733-55c138e1-8933-420b-8db5-bbff93a4d97d.png)


This refers to the client being used when running a node.

The general synopsis is as follows

```
Usage: cardano-node run [--topology FILEPATH] [--database-path FILEPATH]
                        [--socket-path FILEPATH]
                        [--byron-delegation-certificate FILEPATH]
                        [--byron-signing-key FILEPATH]
                        [--shelley-kes-key FILEPATH]
                        [--shelley-vrf-key FILEPATH]
                        [--shelley-operational-certificate FILEPATH]
                        [--host-addr IPV4-ADDRESS]
                        [--host-ipv6-addr IPV6-ADDRESS]
                        [--port PORT]
                        [--config NODE-CONFIGURATION] [--validate-db]
  Run the node.
  ```
  
  • --topology - Filepath to a topology file describing which peers the node should connect to.
•  --database-path - path to the blockchain database.

•  --byron-delegation-certificate - An optional path to the Byron delegation certificate. The certificate allows the delegator (The user of the certificate) to give his/her own block signing rights to somebody else (the delegatee). The delegatee can then sign blocks of behalf of the delegator

• --byron-signing-key - Optional path to the Byron signing key.

• --shelly-signing-key -  Optional path to the Shelly signing key.

• --shelly-kes-key - Optional path to the shelly KES signing key.

• --shelly-vrf-key - Optional path to the Shelly VRF signing key.

• --shelly-operational-certificate - Optional path to the Shelly operation certificate.
• --socket-path - Path to the socket file.

• --host-addr - Optionally specify your node's IPv4 address.

• --host-ipv6-addr - Optionally specify your node's IPv4 address.

• --port - Specify the filepath to the config .ymal file. This is the file that's responsible for all the other node's required settings. You can find examples in configuration (e.g. https://github.com/input-output-hk/cardano-node/blob/master/configuration/defaults/simpleview/config-0.yaml).

• --Validate-db - Flag to revalidate all on-disk database files

# Configuration .ymal files

![image](https://user-images.githubusercontent.com/90267622/134782794-ca5943a8-5eea-488d-923a-79517fa8b572.png)

The --config flag points to a .ymal file which is responsible to configurig the logging & other important settings for your node. E.g. see the Byron mainnet configuration in the following link. Some of the more important settings will be below the link.

https://github.com/input-output-hk/cardano-node/blob/master/configuration/defaults/byron-mainnet/configuration.yaml

• Protocol: RealPBFT -- The protocol the node will execute

• RequiresNetworkMagic: RequiresNoMagic -- Used to distinquish between the mainnet (RequiresNoMagic) and the testnets (RequiresMagic)

#Logging

![image](https://user-images.githubusercontent.com/90267622/134783031-5a544a51-6720-4c1a-b399-22ad2140310f.png)

