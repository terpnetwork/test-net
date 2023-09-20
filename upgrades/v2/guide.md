# Terp-Core v2 - Humulene Upgrade

|                 |                                                              |
|-----------------|--------------------------------------------------------------|
| Chain-id        | `90u-2`                                                |
| Upgrade Version | `v2.0.0`                                        |
| Upgrade Height  | 807514                                                      |



The target block for this upgrade is 807514, which is expected to arrive at 11:00UTC Friday, September 22nd.. [Go Playground](https://go.dev/play/p/QBqPjGz2xpO)

### Building Manually:
```
cd terp-core
git fetch --tags && git checkout v2.0.0 
make build && make install 

terpd version --long | grep "cosmos_sdk_veresion/|commit\|version:"
# commit: 4fbf792d554594fb4ed7a9927424fb6f379fc293
# cosmos_sdk_version: v0.47.4
# version: 2.0.0

mkdir -P $DAEMON_HOME/cosmovisor/upgrades/v2/bin && cp $HOME/go/bin/terpd $DAEMON_HOME/cosmovisor/upgrades/v2/bin 

$DAEMON_HOME/cosmovisor/upgrades/v2/bin/terpd version
```
### Downloading Verified Build:
```
rm -rf terpd_linux_amd64.tar.gz # delete if exists
wget https://github.com/terpnetwork/terp-core/releases/download/v2.0.0/terpd-v2.0.0-linux-amd64.tar.gz
sha256sum terpd_linux_amd64.tar.gz 
# Output d99c709d6f27cbc670cded20cb55a4a26d82034e531a743c121d6f221d0ea9fb
```
