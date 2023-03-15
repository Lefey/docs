---
sidebar_position: 4
---

# Join the network

After your node is configured it need to sync the latest state and connect to the network.
We support two ways of syncing your chain.

## Get genesis file

Download the `genesis.json` file and copy it over to the config directory: `~/.kyve/config/genesis.json`. This is a genesis file with the chain-id and genesis account balances.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs groupId="network">
  <TabItem value="kyve" label="Mainnet">

Download the mainnet genesis file (5.2 MB)
```shell
wget https://files.kyve.network/mainnet/genesis.json
```

verify the correctness of the genesis configuration via

```shell
echo "1dc3ec916f49ef8c221851566aca12a3f914b23afb3ab35067fc8a8d5f59c2ee  genesis.json" | sha256sum -c
```

and move the genesis file to the config directory
```shell
mv genesis.json ~/.kyve/config/
```

  </TabItem>
  <TabItem value="kaon" label="Kaon">

Download the Kaon genesis file (55 KB)

```bash
wget https://files.kyve.network/kaon/genesis.json
```

verify the correctness of the genesis configuration via

```shell
echo "3532166eb1605057f633ff577b4fc3e57a6dddc46498c5bc6f2f4e8ab0c756b8  genesis.json" | sha256sum -c
```

and move the genesis file to the config directory
```shell
mv genesis.json ~/.kyve/config/
```

  </TabItem>
  <TabItem value="korellia" label="Korellia">
    Will be supported soon. In the meantime, reach out to the Team for instructions.
  </TabItem>
</Tabs>

## Syncing the network

:::tip
We strongly recommend setting up Cosmovisor and Systemd as a supervisor.
:::

### Use Statesync

import Admonition from '@theme/Admonition';

State sync fetches a snapshot of a recent height and then only syncs the missing blocks
between the snapshot and the current height.
This is the preferred method as it's much faster. Usually minutes versus weeks.

However, one needs to trust the provided state-sync nodes. If you need full
trust you should sync from genesis.

<Tabs groupId="network">
  <TabItem value="kyve" label="Mainnet">

Once the number of blocks grows beyond a certain height we will provide 
statesync instructions. In the meantime syncing from genesis should only
take a few minutes.

  </TabItem>
  <TabItem value="kaon" label="Kaon">

<Admonition type="info">
        <p>Make sure to use the current version of the chain for statesync. All releases can be found here: <a href="https://github.com/KYVENetwork/chain/releases">https://github.com/KYVENetwork/chain/releases</a></p>
</Admonition>

First, one needs to obtain a trusted height and hash of that height.
One can go to a blockexplorer and use the values provided there. For example block 325017 can be viewed at: <a href="https://testnet.mintscan.io/kyve-testnet/blocks/328549">
https://testnet.mintscan.io/kyve-testnet/blocks/325017 </a>

To enable state-sync edit <code>config.yml</code> under the section <code>[statesync]</code>

```toml
[statesync]
enable = true

# ...

rpc_servers = "https://rpc-eu-1.kaon.kyve.network:443,https:rpc-us-1.kaon.kyve.network:443"
trust_height = 325017 # Replace with newer block
trust_hash = "6548AC5525F24CD09BADDBE3A143CC3D44D057F669EB72BDED9C4C7D62E85FDC" # Replace with newer hash
```

After that start the node with

```bash
./kyved start
```

  </TabItem>
  <TabItem value="korellia" label="Korellia">
    Will be supported soon. In the meantime, reach out to the Team for instructions.
  </TabItem>
</Tabs>

### Sync from genesis

<Tabs groupId="network">
  <TabItem value="kyve" label="Mainnet">

<Admonition type="info">
        <p>For syncing from genesis you need chain binary version <b>v1.0.0</b></p>
</Admonition>

After the genisis file is download the chain binary can be started with

```bash
./kyved start
```

  </TabItem>
  <TabItem value="kaon" label="Kaon">

<Admonition type="info">
        <p>For syncing from genesis you need chain binary version <b>v1.0.0-rc0</b></p>
</Admonition>

After the genisis file is download the chain binary can be started with

```bash
./kyved start
```

  </TabItem>
  <TabItem value="korellia" label="Korellia">
    There is no official support for syncing Korellia from genesis.
    If you have a special requirement reach out to the team for more instructions. 
  </TabItem>
</Tabs>