# Building the Node


![image](https://user-images.githubusercontent.com/90267622/134788487-87f8b9a9-88cb-41ae-aa8e-98647d53e821.png)


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

Logs are output to the logs/ dir.

# Ptofiling & statistics

![image](https://user-images.githubusercontent.com/90267622/134783089-4e694488-7e07-43b8-807d-81e064499317.png)

Make sure you see scripts/README.md for how to obtain profiling information using the scripts.

# Scripts

![image](https://user-images.githubusercontent.com/90267622/134783124-e649b017-66ea-4759-81fe-ea063cf0c6d1.png)

Be sure you've seen scripts/README.md for information on the various scripts.

# Cardano-cli

![image](https://user-images.githubusercontent.com/90267622/134783185-ed1e2f78-a799-46cd-9b3d-32a1839f5807.png)

A CLI utility to support a variety of key material operations (genesis, migration, pretty-printing) for different system generations. Usage documentation can be found at the following link.

cardano-cli/README.md

The general synopsis is on the following line of code.

```
Usage: cardano-cli (Era based commands | Byron specific commands | Miscellaneous commands)
```

Keep in mind that the exact invocation command depends on the enviornment. If you only have cardano-cli bult, without installing it, then you have to prepend cabal run -- `` before`` cardano-cli. From now on we'll just assume that the necessary enviornment-specific adjustment has been made. Now you'll only have to mention cardano-cli.

The subcommands are subdivided in groups and their full name list is visible in the output of cardano-cli --help.

Remember that all the subcommands have help availible. An example is visible below.

```
cabal run -- cardano-cli -- byron key migrate-delegate-key-from --help

cardano-cli -- byron key migrate-delegate-key-from
Usage: cardano-cli byron key migrate-delegate-key-from --from FILEPATH
                                                       --to FILEPATH
  Migrate a delegate key from an older version.


Available options:
  --byron-legacy-formats   Byron/cardano-sl formats and compatibility
  --byron-formats          Byron era formats and compatibility
  --from FILEPATH          Signing key file to migrate.
  --to FILEPATH            Non-existent file to write the signing key to.
  -h,--help                Show this help text
```

# Genesis operations

![image](https://user-images.githubusercontent.com/90267622/134783751-f1769f64-a7b4-49d6-9906-1e72f481dc6e.png)

The Byron genesis operations will create a directory that contains all the following bullet points.

• genesis.json: The genesis JSON file itself.

• avvm-seed.*.seed: Ada Voucher Vending Machine seeds (secret). Affected by --avvm-entry-count and --avvm-entry-balance.

• delegate-keys.*.key: Delegate private keys. Affected by: --n-delegate-addresses.

• delegation-cert.*.json: Delegation certificates. Affected by: --n-delegate-addresses.

• genesis-keys.*.key: Genesis stake private keys. Affected by: --n-delegate-addresses, --total-balance.

• poor-keys.*.key: Non-delegate private keys with genesis UTxO. Affected by: --n-poor-addresses, --total-balance.

More details on the Byron Genesis JSON file can be found in the following link

docs/reference/byron-genesis.md

Byron genesis delegation and related concepts are also described in detail in the link below

https://hydra.iohk.io/job/Cardano/cardano-ledger-specs/byronLedgerSpec/latest/download-by-type/doc-pdf/ledger-spec

# Key operations

![image](https://user-images.githubusercontent.com/90267622/134784005-257dd66b-7e1e-454d-9c32-1a6f8013ce36.png)

Note that key operations don't support password-protected keys.

# Signing key generation & verification key extraction

Signing kets can be generated using the keygen subcommand.

Extracting a verification key out of the signing key is preformed by the to verification subcommand.

# Delegate key migration.

![image](https://user-images.githubusercontent.com/90267622/134785027-c4bbc0b7-ce18-4039-a3b6-46db871733a3.png)

In order to continue using a delegate key from the Byron legacy era in the new implementation, it needs to be migrated over. That is done by the migrate-delegate-key-from the follwing subcommand.

```
$ cabal v2-run -- cardano-cli byron key migrate-delegate-key-from
        --from key0.sk --to key0Converted.sk
```

# Signing key queries

![image](https://user-images.githubusercontent.com/90267622/134785772-6c00af0e-0340-404f-bf04-261933aed549.png)

One can gather information about signing key's properties through the signing-key-public and signing-key-address subcommands. The latter of the two commands requires network magic.

```
$ cabal v2-run -- cardano-cli byron key signing-key-public --byron-formats --secret key0.sk

public key hash: a2b1af0df8ca764876a45608fae36cf04400ed9f413de2e37d92ce04
public key: sc4pa1pAriXO7IzMpByKo4cG90HCFD465Iad284uDYz06dHCqBwMHRukReQ90+TA/vQpj4L1YNaLHI7DS0Z2Vg==

$ cabal v2-run -- cardano-cli signing-key-address --byron-formats --secret key0.pbft --testnet-magic 42

2cWKMJemoBakxhXgZSsMteLP9TUvz7owHyEYbUDwKRLsw2UGDrG93gPqmpv1D9ohWNddx
VerKey address with root e5a3807d99a1807c3f161a1558bcbc45de8392e049682df01809c488, attributes: AddrAttributes { derivation path: {} }
```

## Transactions

![image](https://user-images.githubusercontent.com/90267622/134785869-687b7c3e-37ee-4cbf-a4d6-44f281eead75.png)

# Creation

Transactions can be created via the issue-genesis-utxo-expenditure and issue-utxo-expenditure commands.

The easiest way way to create a transactionis by using the scripts/benchmarking/issue-genesis-utxo-expenditure.sh script as follows.

./scripts/benchmarking/issue-genesis-utxo-expenditure.sh transaction_file

NB: This by efault creates transactions based on 
configuration/defaults/liveview/config-0.yaml

If you don't have a genesis_file you can run scripts/benchmarking/genesis.sh to create an example genesis file for you. The script scripts/benchmarking/issue-genesis-utxo-expenditure.sh has defaults for every requirement of the issue-genesis-utxo-expenditure command.

# Submission

The submit-tx subcommand gives you the option of submitting a pre-signed transaction in its war wire format. (See GenTx for Byron transactions)

The canned scripts/benchmarking/submit-tx.sh script submits the supplied transaction as a testnet launched by scripts/benchmarking/shelly-testnet-liveview.sh script.

# Issuing UTxO expenditure (genesis and regular)



If you plan to make a transaction using UTxO, you can either the following subcommands directly ot you can use one of the canned scripts that make transactions tailored for the testnet cluster. Keep in mind that the first two commands are the directly used subcommands and the next two commands use the canned scripts.

• issue-genesis-utxo-expenditure (genesis UTxO)
• issue-utxo-expenditure (normal UTxO)

• scripts/benchmarking/issue-genesis-utxo-expenditure.sh.
• scripts/benchmarking/issue-utxo-expenditure.sh.

Keep in mind that the script requires the target file name so it can write the transaction to it input Txld (If you're using normal UTxO), as well as optionally allows specifying the source txin output index, source and target signing keys and lovelace value to send.

The target adress defaults to the 1-st richman key (configuration/delegate-keys.001.key) of the testnet, and lovelace amount is almost the entirety of its funds.

# Local node queries

![image](https://user-images.githubusercontent.com/90267622/134788550-28bcd4e2-4afd-445d-ae7c-70a61d12d4a9.png)

You can query the tip of your local node with the get-tip command. The instructions are listed below.

1. Open tmux
2. Run cabal build cardano-nano
3. Run ./scripts/lite/shelly-testnet.sh example
4. Run export CARDANO_NODE_SOCKET_PATH=/cardano-node/example/socket/node-1-socket 4. ``cabal exec cardano-cli -- get-tip --testnet-magic 42``

You should then see the output from the stdout in the format below.

```
Current tip:
Block hash: 4ab21a10e1b25e39
Slot: 6
Block number: 5
```

# Update proposals

![image](https://user-images.githubusercontent.com/90267622/134788564-baf5baf0-365f-4194-a83d-962efd46dd21.png)


A byron update proposal can be created with the command below

```
cardano-cli -- byron governance
               create-update-proposal
                 (--mainnet | --testnet-magic NATURAL)
                 --signing-key FILEPATH
                 --protocol-version-major WORD16
                 --protocol-version-minor WORD16
                 --protocol-version-alt WORD8
                 --application-name STRING
                 --software-version-num WORD32
                 --system-tag STRING
                 --installer-hash HASH
                 --filepath FILEPATH
               ..
```

The mandatory arguments for this to work are --mainnet | --testnet-magic, signing-key, protocol-version-major, protocol-version-minor, protocol-version-alt, application-name, software-version-num, system-tag, installer-hash and filepath.

Any remaining argument is an optional parameter you want to update in your update proposal.

You can also check your proposal's validity using the validate-cbor command. (Follow the link below)
https://github.com/input-output-hk/cardano-node#validate-cbor-files

Look at the the following link for more details on update proposals.

https://hydra.iohk.io/build/5737190/download/1/ledger-spec.pdf

#Update proposal submission

You can submit your proposal using the submit-upadte-proposal command. An example is listed below

```
cardano-cli -- byron governance
            submit-update-proposal
            --config configuration/defaults/mainnet/configuration.yaml
            (--mainnet | --testnet-magic NATURAL)
            --filepath my-update-proposal
```

See the following link for more details on update proposals

https://hydra.iohk.io/build/5737190/download/1/ledger-spec.pdf

# Update proposal voting

![image](https://user-images.githubusercontent.com/90267622/134788569-0f3ac188-7983-4515-9b3c-ba8afbfb53f0.png)


You can create and submit byron update proposal votes woth the create-proposal-vote & submit-proposal-vote commands. Below are two examples of the commands.

Byron vote creation:

```
cabal exec cardano-cli -- byron governance create-proposal-vote
                       (--mainnet | --testnet-magic NATURAL)
                       --signing-key configuration/defaults/liveview/genesis/delegate-keys.000.key
                       --proposal-filepath ProtocolUpdateProposalFile
                       --vote-yes
                       --output-filepath UpdateProposalVoteFile
```

Byron vote submission:

```
cabal exec cardano-cli -- byron governance submit-proposal-vote
                       (--mainnet | --testnet-magic NATURAL)
                       --filepath UpdateProposalVoteFile
```

# Development

![image](https://user-images.githubusercontent.com/90267622/134788583-f8705bc4-de1b-4b4d-8190-6644c72c27a9.png)


GHCID

You can run ghcid with: ghcid -c "cabal repl exe:cardano-node --reorder-goals"

# Haskell Language Server

![image](https://user-images.githubusercontent.com/90267622/134788593-a575ad74-f01b-4780-bfba-94506aab06f1.png)

When using Haskell Language Server with Visual Studio code, you may find HLINT annotations ate ignore<https://github.com/haskell/haskell-language-server/issues/638>.

To work around this, you can run the script ./scripts/reconfigure-hlint.sh which will generate a .hlint.yaml file with HLINT ignore rules derived from the source code.

# Testing

![image](https://user-images.githubusercontent.com/90267622/134788602-9c2bc772-d8c0-48f7-9202-9a20bad27906.png)

Cardano-node is basically a container which implements several components such as networking, consensus, and storage. All of these components have individual test coverage. The node goes through integration and release testing by Devops/QA while automated CLI tests are ongoing alongside development.

Remember that developers on cardano-node can launch their own testnets (https://github.com/input-output-hk/cardano-node/blob/master/doc/getting-started/launching-a-testnet.md) or run the chairman tests (https://github.com/input-output-hk/cardano-node/blob/master/doc/getting-started/running-chairman-tests.md) locally.

## Debugging

# Pretty printing CBOR encoded files

![image](https://user-images.githubusercontent.com/90267622/134788619-fb60f4c4-f220-48d8-9306-a30547e6f574.png)

It may be useful if you print the on chain. representations of blocks, delegation certificates, txs and update proposals. There are two commands that do this (for any cbor encoded file.), though only one is listed directly below this line.

To pretty print as CBOR: cabal exec cardano-cli -- pretty-print-cbor --filepath CBOREncodedFile

# Validate CBOR files

You can validate Byron era blocks, delegation certificates, txs and update proposals with the validate-cbor command.

cabal exec cardano-cli -- validate-cbor --byron-block 21600 --filepath CBOREncodedByronBlockFile

## Native Tokens

![image](https://user-images.githubusercontent.com/90267622/134788626-5c4bda95-b3bb-4d1b-9ef6-ca34fdcc05a8.png)

Native tokens is a new feature that enables the transacting of multi-assets on Cardano. Native tokens are now supported on mainnet and users can transact with ada, and an unlimited number of user-defined (custom) tokens natively.

Below is a compiled list of resources which can help you get started.

https://forum.cardano.org/c/developers/cardano-tokens/150

https://docs.cardano.org/native-tokens/learn

additionally to that, you can read more about native token and how they compare to things like ada and ERC20 with the link below. Browse native tokens made on the Cardano blockchain itself as well as see their transactions in an interactive dashboard which allows filtering and searching:
nativetokens.da.iogservices.io.

(https://github.com/input-output-hk/cardano-ledger-specs/blob/master/doc/explanations/features.rst)

## API Documentation

![image](https://user-images.githubusercontent.com/90267622/134788631-9b675491-f786-4208-a16c-035950095c0c.png)

The API documentation is published at the link below.

https://input-output-hk.github.io/cardano-node/

The documentation is built with each push, but is only published from the master branch. If you want to test if the documentation is working, you can build the documentation loccaly with ./scripts/haddocs.sh and ipen haddocs/index.html in the browser.
