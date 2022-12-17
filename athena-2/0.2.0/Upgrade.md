---
title: Terp-Core Upgrade 0.2.0
---
<!-- markdown-link-check-disable -->
# Terp-Core Upgrade Instructions

The following document describes the necessary steps involved that full-node operators
must take in order to upgrade from `0.1.2` to `0.2.0`. 

## Risks

As a validator performing the upgrade procedure on your consensus nodes carries a heightened risk of
double-signing and being slashed. The most important piece of this procedure is verifying your
software version and genesis file hash before starting your validator and signing.

The riskiest thing a validator can do is discover that they made a mistake and repeat the upgrade
procedure again during the network startup. If you discover a mistake in the process, the best thing
to do is wait for the network to start before correcting it. If the network is halted and you have
started with a different genesis file than the expected one, seek advice from a Terp developer
before resetting your validator.

## Recovery

Validators are encouraged to take a full data snapshot at the. Snapshotting depends heavily on infrastructure, but generally this
can be done by backing up the `.terp` directories.

It is critically important to back-up the `.terp/data/priv_validator_state.json` file after stopping your terpd process. This file is updated every block as your validator participates in a consensus rounds. It is a critical file needed to prevent double-signing, in case the upgrade fails and the previous chain needs to be restarted.

In the event that the upgrade does not succeed, validators and operators must downgrade back to
v0.1.2 of the Terp-Core and restore to their latest snapshot before restarting their nodes.

## Upgrade Procedure

__Note__: It is assumed you are currently operating a full-node running v0.1.2 of the Terp-Core.

- The upgrade height as agreed upon by governance: __1,497,396__ [go playground link](https://go.dev/play/p/DOtb4m6OqCk)

1. Verify you are currently running the correct version (v0.1.2) of the Terp-Core:

   ```bash
   $ terpd version
   0.1.2
   ```

2. Stop your service when reached __1,497,396__:

   ```bash
   terpd stop
   or
   systemctl stop terpd.service
   ```

3. Upgrade binary to 0.2.0
   
   ```bash
   cd terp-core && git checkout v0.2.0
   make install
   ```
 __NOTE__: If checkout not found v0.2.0 remove terp-core, download it again and repeat step 3(Olny terp-core not .terp).
 
   ```bash
   rm -r terp-core
   git clone https://github.com/terpnetwork/terp-core
   ```
   
4. Verify you are currently running the correct version (v0.2.0) of the Terp-Core:

   ```bash
   terpd version
   0.2.0
   ```

5. Download Genesis file for 0.2.0
  ```bash
  curl -s  https://raw.githubusercontent.com/terpnetwork/test-net/master/athena-2/0.2.0/genesis.json > ~/.terp/config/genesis.json
  ```

6. Start terpd and confirm stops on blockheght udpdate, if you removed .terp you gona see a lot of errors tring to match headers of blocks with version 0.2.0. 
   Because of this is extremly important backup your validator files included .terp to restore at 0.1.2. 

  ```bash
  terpd start
  ```

6. When you confirmed you are on 0.2.0 at blockehigt update we can continue with last step of upgrade, skip-upgrade.

  ```bash
  terpd start --unsafe-skip-upgrades 1497396
  ```
<!-- markdown-link-check-enable -->
