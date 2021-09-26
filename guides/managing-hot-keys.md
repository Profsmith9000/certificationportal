# Managing Hot Keys



![image](https://user-images.githubusercontent.com/90267622/134778474-aa9ff649-cfde-4f7e-8d4f-5944249c7f8a.png)

To create an operational certificate for a block-producing node, you need a KES key pair.

KES stands for Key Evolving Signature in this situation. This means that after a certain period the key evolves into a new key and discards it's old version. This is a useful situation since if an attacker compromises the key and gains access to the signing key, they would only be able to use that key to sign blocks but blocks from earlier points would be safe so an attack couldn't rewrite history.

A KES key will only evolve after a certain number of periods and becomes useless afterwards. This means that until the number of periods has passed, the node operator will need to generate a new KES key pair, issue a new operational node certificate with that new key pair and then restart the node with the new certificate.

To find out how long one period is and for how long a key can evolve, you'll need to look into the genesis file. If that file is called mainnet-shelley-genesis.json, you can type

```text
cat mainnet-shelley-genesis.json | grep KES
"slotsPerKESPeriod": 129600,
"maxKESEvolutions": 62,
```

This example has the key evolve after a period of 129600 slots and it can evolve 62 times before it needs to be renewd.

Before making an operational certificate for the node, you need to figure out the start of thr KES validity period which is just the KES evolution period you're in.

To check the current tip of the blockchain you'll use the following command.

```text
cardano-cli query tip --mainnet

{
    "epoch": 259,
    "hash": "dbf5104ab91a7a0b405353ad31760b52b2703098ec17185bdd7ff1800bb61aca",
    "slot": 26633911,
    "block": 5580350
}
```

This example sets you in slot 26633911. You know from the genesis file that a single period lasts for 129600 slots. This lets you calculate the current period with this command

```text
expr 26633911 / 129600
> 205
```

With that you should be able to generate an operational certificate for the stake pool:

```text
cardano-cli node issue-op-cert \
--kes-verification-key-file kes.vkey \
--cold-signing-key-file cold.skey \
--operational-certificate-issue-counter cold.counter \
--kes-period 205 \
--out-file node.cert
```

Above guide based on \([https://github.com/input-output-hk/cardano-node/blob/master/doc/stake-pool-operations/KES\_period.md](https://github.com/input-output-hk/cardano-node/blob/master/doc/stake-pool-operations/KES_period.md)\)

