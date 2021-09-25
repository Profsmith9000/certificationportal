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

Documentation for building the node can be found here.

Make sure that you download the latest version of cardano-node and cardano-cli:
