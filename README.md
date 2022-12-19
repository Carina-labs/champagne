# Champagne
supernova testent

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
