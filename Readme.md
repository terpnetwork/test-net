# Terp Network - athena-2 Testnet

This testnet will start at the patched version of Terp-Core (`v0.1.2`). You will need to use the distributed binary from the Terp Network packages repository.

**Genesis File**

[Genesis File](https://raw.githubusercontent.com/terpnetwork/test-net/master/athena-2/genesis.json):

```bash
curl -s  https://raw.githubusercontent.com/terpnetwork/test-net/master/athena-2/genesis.json > ~/.terp/config/genesis.json
```

**Genesis sha256**

```bash
sha256sum ~/.terp/config/genesis.json
# TBD
```

**terpd version**

```bash
$ terpd version --long
name: Terp Core
server_name: terpd
version: v0.1.2
commit: TBD
build_tags: netgo muslc, # THIS BIT IS KEY
```

**Seed nodes**

```
TBD
```

**Persistent Peers**

```
TBD
```

## Setup

**Prerequisites:** Make sure to have [Golang >=1.19](https://golang.org/).

#### Go setup

You need to ensure your gopath configuration is correct. If the following **'make'** step does not work then you might have to add these lines to your .profile or .zshrc in the users home folder:

```sh
nano ~/.profile
```

```
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

Source update .profile

```sh
source .profile
```

#### Download and verify:

```sh
# find out where terpd is - will likely be /home/<your-user>/go/bin/terpd
which terpd

# put new binary there i.e. in path/to/terpd
wget https://github.com/terpnetwork/terp-core/releases/download/v0.1.2/terp-core -O /home/<your-user>/go/bin/terpd



#### Running in production

**Note, we'll be going through some upgrades for this testnet. Consider using [Cosmovisor](https://github.com/cosmos/cosmos-sdk/tree/master/cosmovisor) to make your life easier.** Setting up Cosmovisor is covered in the [Terp Network Documentation]().

Download Genesis file when the time is right. Put it in your `/home/<user>/.terp` folder.

If you have not installed cosmovisor, create a systemd file for your TerpNet service:

```sh
sudo nano /etc/systemd/system/terpd.service
```

Copy and paste the following and update `<YOUR_USERNAME>` and `<CHAIN_ID>`:

```sh
Description=Terp-Core daemon
After=network-online.target

[Service]
User=<YOUR_USER>
ExecStart=/home/<YOUR_USERNAME>/go/bin/terpd start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
```

Enable and start the new service:

```sh
sudo systemctl enable terpd
sudo systemctl start terpd
```

Check status:

```sh
terpd status
```

Check logs:

```sh
journalctl -u terpd -f
```

### Learn more

- [Cosmos Network](https://cosmos.network)
- [Terp Network Documentation](https://docs.terp.network/)

#### Running in production

