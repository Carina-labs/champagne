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
