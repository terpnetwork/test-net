# 90u-1 relaunch

The following document describes the necessary steps involved that full-node operators
must take in order to restart their nodes for 90u-1. 

## Steps Took To Improve Genesis Process
- New Genesis Time now `2023-05-10T12:00:00Z`

## Relaunch Procedure

### 1. Reset Node
```
sudo systemctl stop terpd.service
terpd tendermint unsafe-reset-all --home $HOME/.terp --keep-addr-book
```
### 2. Download Updated Genesis 
```
curl -s  https://raw.githubusercontent.com/terpnetwork/test-net/master/90u-1/genesis.json > $HOME/.terp/config/genesis.json
```
### 3. Set Minimum Gas Price
```
sed -i 's/minimum-gas-prices = "[^"]*"/minimum-gas-prices = "0.0002uthiol"/' $HOME/.terp/config/app.toml
```
### 4. Configure Chain-ID
```
terpd config chain-id 90u-1
```

### 5. Check Node

#### Genesis 
```
sha256sum $HOME/.terp/config/genesis.json
```
`9119703c9b10e5f672b34119d9d209a01fd877d72c5e6d26733a0b6427e8a6fa $HOME/.terp/config/genesis.json`
#### Chain-ID
```
terpd config | grep chain-id
```
`"chain-id": "90u-1"`

#### Binary
```
terpd version --long | grep -e version -e commit
```
`commit: 9bb91afded0ad182d97e642afddf0a45e0b4c438` \
`version: 1.0.0-stable-4-g9bb91af`

### 6. Reset Service File
```
sudo systemctl daemon-reload
sudo systemctl enable terpd
sudo systemctl restart terpd && sudo journalctl -u terpd -f
```
