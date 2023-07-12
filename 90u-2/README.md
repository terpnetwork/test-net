# 90u-2 relaunch

The following document describes the necessary steps involved that full-node operators
must take in order to restart their nodes for 90u-2.

The genesis state of `90u-2` has been derived from the export of the previous test network, and will include a fresh batch of gentxs, as the previous test network has become stale. 

## Overview

### I. Validation SubDAO Consensus 

First, a [DAO proposal](https://daodao.zone/dao/juno1wpq03vzv4f9fczss0sqt4xxfmxel6zmhdxal68lg8qpa7cgzj25sy3k3dt/) including the PR containing the specifics regarding the 90u-2 test-net relaunch will be proposed. This includes:

- Steps taken to generate the new genesis file (Details are found in [Export Migration](./docs/export-migration.md))
- The relaunch timeline, including the gentx submission.

The proposal can be found [here.](./docs/proposal.md)

### II. Relaunch modification, or Gentx Submissions

If the consensus is to **reject** the proposed details, modifications in the details may be taken, & a reproposal can be made. 

If the consensus is to **accept** the proposed details, they can be used as a guideline for participation in the next steps. 

### III. Network Launch

Node setup instructions, & Peer endpoints can be available for anyone to participate in the relaunch of the test network in  [Terp Network's Chain-Registry Fork](https://github.com/terpnetwork/chain-registry).