# Champagne
supernova testnet

# Supernova Testnet Champagne

First public nova testnet.
  
| Attribute | Value     |
|-----------|-----------|
| Chain ID  | `nova` |

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

### Create account to genesis

```
novad add-genesis-account <key-name> 10000000000unova --keyring-backend os
```

### Create GenTX

```
# Create the gentx.
# Note, staking amount should be equal to 10000000000unova.
novad gentx <key-name> 10000000000unova --output-document=gentx.json \
  --chain-id=nova \
  --moniker="<moniker-name>" \
  --website=<your-node-website> \
  --details=<your-node-details> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --keyring-backend <os | file>
```

### Running in production
Nova clone
```
git clone https://github.com/Carina-labs/nova.git@champagne-v0.0.2
make install
```

If you install novad successfully, you can check version
```
$GOPATH/bin/novad version
// It will be like "champagne-v0.0.2"
```
Note, as usual we'll be going through some upgrades for this testnet.

And set chain id to 'nova'.
```
novad config chain-id nova
```

You can add testnet peer list:
```
cd $HOME/.novad/config/config.toml

...
persistent_peers = "11843270ecaa7a495ced55bf6e74cdaa4b410b8a@nova-validator1-l4.dev-supernova.xyz:26656,32e3e09aca23812786b821865fdb972830326503@nova-validator2-l4.dev-supernova.xyz:26656,c43687e29985e43986e78653b4967bc9d385562c@nova-validator3-l4.dev-supernova.xyz:26656,19ee1eed493063a46b5c6eca184ee9819a751020@211.219.19.71:26656,46a078b53653e712b7a3cddd747aa68398ba1418@211.219.19.71:26656,69ebba635540c1019ebdd15057ac5d1cd4bdc89f@211.219.19.71:26656"
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

connection-2, channel-2 <-> connection-0, channel-0

**nova <-> osmosis**

connection-0, channel-1 <-> connection-0, channel-0

**nova <-> juno**

connection-1, channel-0 <-> connection-0, channel-0

### ICA channel info
**nova <-> gaia**
connection-2, channel-3, icacontroller-gaia.nova1f0ncyee30hlf4m3ym7f87gcr4fu2qffmyk5508 <-> connection-0, channel-4, icahost

**nova <-> osmosis**
connection-0, channel-4, icacontroller-osmosis.nova1kxdf08v5th7g983ggxqak4w6vsyu88tnnkwlxt <-> connection-0, channel-2, icahost

**nova <-> juno**
connection-1, channel-5, icacontroller-juno.nova16y6jyjg8k7utjtf9zd0zaqef4p8qqs89mwk3f4 <-> connection-0, channel-2, icahost

## Useful Endpoint
If you want use public endpoint(tendermint, rest) using test network, please refer it.
### Supernova endpoint
* RPC node's tendermint rpc : https://supernova-tendermint.dev-supernova.xyz
* RPC node's rest : https://supernova-rest.dev-supernova.xyz

* Validator 1's tendermint rpc : http://nova-validator1-l4.dev-supernova.xyz:26657
* Validator 2's tendermint rpc : http://nova-validator2-l4.dev-supernova.xyz:26657
* Validator 3's tendermint rpc : http://nova-validator3-l4.dev-supernova.xyz:26657

### Stub-gaia endpoint
* tendermint rpc : https://gaia-tendermint.dev-supernova.xyz

* Validator 1's tendermint rpc : http://gaia-validator1-l4.dev-supernova.xyz:26657
* Validator 2's tendermint rpc : http://gaia-validator2-l4.dev-supernova.xyz:26657
* Validator 3's tendermint rpc : http://gaia-validator3-l4.dev-supernova.xyz:26657

### Stub-osmosis endpoint
* tendermint rpc : https://osmosis-tendermint.dev-supernova.xyz

* Validator 1's tendermint rpc : http://osmosis-validator1-l4.dev-supernova.xyz:26657
* Validator 2's tendermint rpc : http://osmosis-validator2-l4.dev-supernova.xyz:26657
* Validator 3's tendermint rpc : http://osmosis-validator3-l4.dev-supernova.xyz:26657

### Stub-juno endpoint
* tendermint rpc : https://juno-tendermint.dev-supernova.xyz

* Validator 1's tendermint rpc : http://juno-validator1-l4.dev-supernova.xyz:26657
* Validator 2's tendermint rpc : http://juno-validator2-l4.dev-supernova.xyz:26657
* Validator 3's tendermint rpc : http://juno-validator3-l4.dev-supernova.xyz:26657
