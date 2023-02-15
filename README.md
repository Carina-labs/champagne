# Champagne
supernova testnet

# Supernova Testnet Champagne

First public nova testnet.
  
| Attribute | Value     |
|-----------|-----------|
| Chain ID  | `champagne` |

## Requirements

### Hardware

* 4 Cores
* 32 GB RAM
* 100GB+ Storage

### Software Versions

| Name               | Version  |
|--------------------|----------|
| Champagne          | v0.0.1   |
| Go                 | > 1.18   |

## Tools

* [Supernova Champagne Blockchain Explorer]
* [Supernova Champagne Faucet]

## Documentation
* [supernova docs](https://docs.supernovaprotocol.xyz/)

## GenTx generation

### Init
```bash:
novad init "<moniker-name>" --chain-id nova
```

### Generate keys

```bash:
# To create new keypair - make sure you save the mnemonics!
novad keys add <key-name> 
```

or
```
# Restore existing odin wallet with mnemonic seed phrase. 
# You will be prompted to enter mnemonic seed. 
novad keys add <key-name> --recover
```
or
```
# Add keys using ledger
novad keys show <key-name> --ledger
```

Check your key:
```
# Query the keystore for your public address 
novad keys show <key-name> -a
```

### Get Faucet from Discord

![img.png](resources/discord_faucet.png)

In champagne network, our dev team runs default validator for testing. So you can join as a validator with `CreateValidator` transaction.

Before that, you should have some `NOVA` for gas fee and self deposit. For using faucet, please join our [discord](https://discord.gg/B9YU5Mjvvb).

### Check balance

```
novad query bank balances <your address>
```

Check if you have enough tokens.

### Submit CreateValidator Transaction

```shell
novad tx staking create-validator \
  --moniker=<moniker-name>  \
  --chain-id=champagne  \
  --website=<your-website>  \
  --details=<your-node-details>  \
  --commission-rate="0.10"  \
  --commission-max-rate="0.20"  \
  --commission-max-change-rate="0.01"  \
  --min-self-delegation="1"   \
  --keyring-backend=<os | file | test>
```

If you try to submit `CreateValidator` tx, prompt will ask to you want to submit it. Please enter `y`.


### Running in production
Nova clone
```
git clone https://github.com/Carina-labs/nova.git@champagne-v0.0.2
make install
```

If you install novad successfully, you can check version
```
$GOPATH/bin/novad version
// It will be like "champagne-v0.0.3"
```
Note, as usual we'll be going through some upgrades for this testnet.

And set chain id to 'nova'.
```
novad config chain-id champagne
```

You can add testnet peer list:
```
cd $HOME/.novad/config/config.toml

...
persistent_peers = "11843270ecaa7a495ced55bf6e74cdaa4b410b8a@champagne.dev-supernova.xyz:26656,32e3e09aca23812786b821865fdb972830326503@champagne.dev-supernova.xyz:26656,c43687e29985e43986e78653b4967bc9d385562c@champagne.dev-supernova.xyz:26656"
```
Note, these peer list are being used for test-network.

If you have not installed cosmovisor, create a systemd file for your Nova service:
```
sudo nano /etc/systemd/system/novad.service
```
Copy and paste the following and update <YOUR_USERNAME>:
```
Description=Nova daemon
After=network-online.target

[Service]
User=nova
ExecStart=/home/<YOUR_USERNAME>/go/bin/novad start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```
Enable and start the new service:
```
sudo systemctl enable novad
sudo systemctl start novad
```
Check status:
```
novad status
```
Check logs:
```
journalctl -u novad -f
```

## IBC Information
### IBC Transfer channel info
**nova <-> gaia**

connection-0, channel-0 <-> connection-0, channel-0

|            | nova         | stub-cosmos  |
|------------|--------------|--------------|
| connection | connection-0 | connection-0 |
| channel    | channel--    | channel-0    |
| port       | transfer     | transfer     |

**nova <-> osmosis**

|            | nova         | stub-osmosis |
|------------|--------------|--------------|
| connection | connection-1 | connection-0 |
| channel    | channel-1    | channel-0    |
| port       | transfer     | transfer     |

**nova <-> juno**

|            | nova         | stub-juno    |
|------------|--------------|--------------|
| connection | connection-2 | connection-0 |
| channel    | channel-2    | channel-0    |
| port       | transfer     | transfer     |

### ICA channel info


|            | nova                                                           | stub-cosmos  |
|------------|----------------------------------------------------------------|--------------|
| connection | connection-0                                                   | connection-0 |
| channel    | channel-3                                                      | channel-4    |
| port       | icacontroller-gaia.nova1f0ncyee30hlf4m3ym7f87gcr4fu2qffmyk5508 | icahost      |


**nova <-> osmosis**

|            | nova                                                              | stub-osmosis |
|------------|-------------------------------------------------------------------|--------------|
| connection | connection-1                                                      | connection-0 |
| channel    | channel-4                                                         | channel-5    |
| port       | icacontroller-osmosis.nova1kxdf08v5th7g983ggxqak4w6vsyu88tnnkwlxt | icahost      |


**nova <-> juno**

|            | nova                                                             | stub-juno    |
|------------|------------------------------------------------------------------|--------------|
| connection | connection-2                                                     | connection-0 |
| channel    | channel-5                                                        | channel-5    |
| port       | icacontroller-juno.nova16y6jyjg8k7utjtf9zd0zaqef4p8qqs89mwk3f4   | icahost      |


## Useful Endpoint
If you want use public endpoint(tendermint, rest) using test network, please refer it.

> **Caution** Following rpc hosts are not main network of each blockchain. It is just a "fake" blockchain for testing.


### Supernova endpoint

| name           | address                                        |
|----------------|------------------------------------------------|
| tendermint rpc | http://champagne.dev-supernova.xyz:26657       |
|                | https://champagne-tendermint.dev-supernova.xyz |
| rest api       | http://champagne.dev-supernova.xyz:1317        |
|                | https://champagne-rest.dev-supernova.xyz       |


### Stub-gaia endpoint

| name           | address                                         |
|----------------|-------------------------------------------------|
| tendermint rpc | http://champagne-cosmos.dev-supernova.xyz:26657 |
| rest api       | http://champagne-cosmos.dev-supernova.xyz:1317  |


### Stub-osmosis endpoint

| name           | address                                          |
|----------------|--------------------------------------------------|
| tendermint rpc | http://champagne-osmosis.dev-supernova.xyz:26657 |
| rest api       | http://champagne-osmosis.dev-supernova.xyz:1317  |

### Stub-juno endpoint

| name           | address                                       |
|----------------|-----------------------------------------------|
| tendermint rpc | http://champagne-juno.dev-supernova.xyz:26657 |
| rest api       | http://champagne-juno.dev-supernova.xyz:1317  |
