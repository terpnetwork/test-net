# Validation subDAO Proposal: Temperature Check for 90u-2 Testnet Launch Procedures

The PR containing the details on the 90u-2 test-net upgrade steps can be found here: https://github.com/terpnetwork/test-net/pull/6


# Upgrade Timeline

## I. Genesis File Modification [Timeline - 4 Days]

There are two ways to participate as a node operator during the launch of 90u-2. To participate as a validator node requires the participant to have an inital `terpx` balance, but to participate as a full node, seed node, or light client does not. 

This first step allows any new contributors to add a genesis account balance of their own so that they may be able to participate as a genesis validator for 90u-2. 

### **Adding Genesis Accounts**

### Step 1: Fork the test-net Repo on Github

*If you are new to github workflows, feel free to checkout this guideline for managing forked repo's & Pull Request's (PR's) [link.](https://opensource.com/article/19/7/create-pull-request-github)*

### Step 2: Build the Terpd Daemon locally

Make sure you have setup your computers local enviroment with the prerequisites needed, dependent on your operating platform (Windows, Linux, MacOS).

*For most linux users, you can copy and paste this script to setup the prerequisites* \
 *(Big-ups to the [Nodejumper Team](https://app.nodejumper.io/)):*
```
# install dependencies, if needed
sudo apt update
sudo apt install -y curl git jq lz4 build-essential unzip

bash <(curl -s "https://raw.githubusercontent.com/nodejumper-org/cosmos-scripts/master/utils/go_install.sh")
source .bash_profile

```
To Download the source file for `terpd`:
```bash
git clone https://github.com/terpnetwork/terp-core.git
cd terp-core || return
git checkout v1.0.0-stable
make install
```

### Step 3: Import or generate new keyring
To import a mnemonic that already exist:
```bash
terpd keys add <your-key-name> --recover
```
To generate a new keyring:
```bash
terpd keys add <your-key-name>
```
Make sure if you are generating a new keyring, you write down the mnemonic & public wallet addresses generated, so that you may reference it for the next steps. 

### Step 4: Download Genesis File

With terpd installed, lets initialize a new node structure:
```bash
terpd init <your-node-name>
```
This will generate a folder by default located at `~/.terp`. We can now download the genesis file configured for 90u-2 to our nodes, replacing the default one generated when we ran the command `terpd init`. 

To replace the genesis file, we recommend pulling from the forked repo you gereated on step-1:

NOTE: Replace <YOUR_GITHUB_ACCOUNT_HERE> with the username of the github account used to fork the repo.
```bash
curl -s https://raw.githubusercontent.com/<YOUR_GITHUB_ACCOUNT_HERE>/test-net/master/90u-2/prelaunch-genesis.json > $HOME/.terp/config/genesis.json
```

### Step 5: Add Genesis Account 

Now with the `90u-2` pre-genesis file configured to your node, we can now add genesis accounts to the genesis file:
```bash
terpd genesis add-genesis-account <your-public-terp-wallet-address> 1000000uterpx,1000000uthiolx

## Ex: terpd genesis add-genesis-account terp1574689mrmzv9mtsrw4l2wtln4edkzw68x96h6j 1000000uterpx,1000000uthiolx
```
Notice that each account will allocate only 1000000uterpx & 1000000uthiolx (1 TERPX & THIOLX each)

### Step 6: Open a New PR to the [Test-Net Repository](https://github.com/terpnetwork/test-net)

Once your have added your genesis account added, open up a new PR to include the addition to the genesis file you have made. You will need to copy the modified genesis file, located at `~/.terp/config/genesis.json` to your forked repo. 

To learn more about opening PR's you can learn more [here.](https://opensource.com/article/19/7/create-pull-request-github)

## II. Genesis Transaction Creation [Timeline - 4 Days]

The next step in the timline is to generate gentx's, which are essentially a transaction made by each validator to be a part of the active set when the first block of the network is generated. This can only be done where each gentx signer is signing an identical genesis file. 


### Copy the generated gentx json file to <repo_path>/90u-2/gentx/
```
cd test-net
cp ~/.terp/config/gentx/gentx*.json ./90u-2/gentx/
```
### Commit and push to your repo

### Create a PR onto https://github.com/terpnetwork/test-net

### III. Genesis Launch of 90u-2 [Timeline - 48 hours after final Gentx submission]

The final step is to distribute the finalized genesis file, and have peers connect with each other in preparation of blocks producing on 90u-2

