# Terp Network - athena-1 Testnet

This testnet will start at the patched version of Terp-Core (`v0.1.0`). You will need to use the distributed binary from the Terp Network packages repository.

**Genesis File**

[Genesis File](https://raw.githubusercontent.com/terpnetwork/test-net/master/athena-1/genesis.json):

```bash
   curl -s  https://raw.githubusercontent.com/terpnetwork/test-net/master/athena-1/genesis.json > ~/.terp/config/genesis.json
```

**Genesis sha256**

```bash
sha256sum ~/.terp/config/genesis.json
# 322951ef73713c550fcc70e312cb2d67c58b85d2870961f982ede7dd0447162d
```

**terpd version**

```bash
$ terpd version --long
name: Terp Core
server_name: terpd
version: v0.1.0
commit: 9ef3e1eada1f06509aa223e64a2380b55aaa3bca
build_tags: netgo muslc, # THIS BIT IS KEY
```

**Seed nodes**

```
5a6f4f80c7a29055028fc503216a1539594ad33f@89.111.15.146:11513
```

**Persistent Peers**

```
a24cbc18af3f3558719e2f479ff412f60e126683@181.41.142.78:11503
7e5c0b9384a1b9636f1c670d5dc91ba4721ab1ca@23.88.53.28:36656
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
wget https://github.com/terpnetwork/terp-core/releases/download/v0.1.0/terp-core -O /home/<your-user>/go/bin/terpd

# if you run this, you should see build_tags: netgo muslc,
# if there is a permissions problem use chmod/chown to make sure it is executable
terpd version --long

# confirm it is using the static lib - should return "not a dynamic executable"
ldd $(which terpd)

# if you really want to be sure
# this should return:
# ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, 
# Go BuildID=4Ec3fj_EKdvh_u8K3RGJ/JIKOgYFXTJ9LzGROhs8n/uC9gpeaM9MaYurh9DJiN/YcvB8Jc2ivQM2zUSHMhg, stripped
file $(which terpd)
```

Check that you have the right Terp-Core version installed:

```sh
$ terpd version --long
name: Terp-Core
server_name: terpd
version: v0.1.0
commit: e6b8c212b178cf575035065b78309aed547b1335
build_tags: netgo muslc, # THE MUSLC TAG IS KEY
```


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

