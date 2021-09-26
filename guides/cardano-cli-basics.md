# Cardano-Cli Basics


# Set of Cardano-cli Sub Commands

**address:** Shelley payment address commands

**stake-address:** Shelley stake address commands

**transaction:** Shelley transaction commands

**node:** Shelley node operation commands

**stake-pool:** Shelley stake pool commands

**query:** Shelley node query commands. This queries the local node whose Unix domain socket is obtained from the CARDANO_NODE_SOCKET_PATH environment variable. 

**block:** Shelley block commands

**system:** Shelley system commands

**genesis:** Shelley genesis block commands

**text-view:** commands for dealing with Shelley text view files that are stored on disk such as transactions or addresses

**governance:** Shelley governance commands



# Cardano-cli Address Sub Commands

**key-gen:** creates a single address key pair

**key-hash:** prints the hash of an address to stdout

**build:** builds a Shelley payment address, with optional delegation to a stake address

**build-multisig:** builds a Shelley payment multi-sig address.

**info:** prints details about the address



# Cardano-clistake address Sub Commands 

**key-gen:** creates a single address key pair

**build:** builds a stake address

**register:** registers a stake address 

**delegate:** delegates from a stake address to a stake pool

**de-register:** de-registers a stake address

**registration-certificate:** creates a registration certificate

**delegation-certificate:** creates a stake address delegation certificate

**deregistration-certificate:** creates a de-registration certificate



# Cardano-cli Transaction Sub Commands

**build-raw:** builds a low-level transaction

**sign:** signs the transaction

**witness:** witnesses a transaction

**sign-witness:** signs and witnesses a transaction

**check:** checks the transaction

**submit:** submits the transaction to the local node whose Unix domain socket is obtained from the CARANO_NODE_SOCKET_PATH environment variable.

**calculate-min-fee:** calculates the minimum fee for the transaction

**info:** prints information about the transaction



# Cardano-cli Node Sub Commands

**key-gen:** creates a key pair for a node operator’s offline key and a new certificate issue counter

**key-gen-KES:** creates a key pair for a node KES operational key

**key-gen-VRF:** creates a key pair for a node VRF operational key

**issue-op-cert:** issues a node operational certificate



# Cardano-cli Stakepool Sub Commands 

**register:** registers a stake pool

**re-register:** re-registers a stake pool

**retire:** retires a stake pool

**registration-certificate:** creates a stake pool registration certificate

**de-registration-certificate:** creates a stake pool de-registration certificate

**id:**  builds pool id from the offline key



# Cardano-cli Query Sub Commands

**pool-id:** retrieves the node’s pool ID

**protocol-parameters:** retrieves the node’s current pool parameters

**tip:** gets the node’s current tip (slot number, hash, and block number)

**utxo:** retrieves the node’s current UTxO, filtered by address

**version:** retrieves the node’s version details

**status:** retrieves the current status of the node

**ledger-state:**  dumps the current state of the node

**stake-address-info:** get the current delegations and reward accounts filtered by stake address.

**stake-distribution:** get the node's current aggregated stake distribution



# Cardano-cli Block Sub Commands 

**info:** retrieves the pool ID that produced a particular block.



##### Cardano-cli System Sub Commands 

**start:** starts the system

**stop:** stops the system


# Cardano-cli Governence 

**create-mir-certificate:** creates an MIR (move instantaneous rewards) certificate

**create-update-proposal:** creates an update proposal

**protocol-update:** performs a protocol update

**cold-keys:** retrieves the cold keys



# Cardano-cli Genisis

**key-gen-genesis:** creates a Shelley genesis key pair

**key-gen-delegate:** creates a Shelley genesis delegate key pair

**key-gen-utxo:** creates a Shelley genesis UTxO key pair

**key-hash:** prints the identifier, or hash, of a public key

**get-ver-key:** derives verification key from a signing key

**initial-addr:** gets the address for an initial UTxO based on the verification key

**initial-txin:** gets the transaction ID for an initial UTxO based on the verification key. 

**create:** creates a Shelley genesis file from a genesis template, as well as genesis keys, delegation keys, and spending keys. 
