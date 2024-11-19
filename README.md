# Nitro Testnode

Nitro-testnode brings up a full environment for local nitro testing (with Stylus support) including a dev-mode geth L1, and multiple instances with different roles.

### Requirements

* bash shell
* docker and docker-compose

All must be installed in PATH.

## Using latest nitro release (recommended)

Check out the release branch of the repository.

> Notice: release branch may be force-pushed at any time.

```bash
git clone -b release --recurse-submodules https://github.com/OffchainLabs/nitro-testnode.git
cd nitro-testnode
```

Initialize the node

```bash
./test-node.bash --init
```
To see more options, use `--help`.

## Using current nitro code (local compilation)

Check out the nitro repository. Use the test-node submodule of nitro repository.

> Notice: testnode may not always be up-to-date with config options of current nitro node, and is not considered stable when operated in that way.

```bash
git clone --recurse-submodules https://github.com/OffchainLabs/nitro.git
cd nitro/nitro-testnode
```

Initialize the node in dev-mode (this will build the docker images from source)
```bash
./test-node.bash --init --dev
```
To see more options, use `--help`.

## Further information

### Working with docker containers

**sequencer** is the main docker to be used to access the nitro testchain. It's http and websocket interfaces are exposed at localhost ports 8547 and 8548 ports, respectively.

Stopping, restarting nodes can be done with docker-compose.

### Helper scripts

Some helper scripts are provided for simple testing of basic actions.

To fund the address 0x1111222233334444555566667777888899990000 on l2, use:

```bash
./test-node.bash script send-l2 --to address_0x1111222233334444555566667777888899990000
```

For help and further scripts, see:

```bash
./test-node.bash script --help
```

## Named accounts

```bash
./test-node.bash script print-address --account sequencer
```
```
sequencer:                  0xe2148eE53c0755215Df69b2616E552154EdC584f
validator:                  0x6A568afe0f82d34759347bb36F14A6bB171d2CBe
l2owner:                    0x5E1497dD1f08C87b2d8FE23e9AAB6c1De833D927
l3owner:                    0x863c904166E801527125D8672442D736194A3362
l3sequencer:                0x3E6134aAD4C4d422FF2A4391Dc315c4DDf98D1a5
user_l1user:                0x058E6C774025ade66153C65672219191c72c7095
user_token_bridge_deployer: 0x3EaCb30f025630857aDffac9B2366F953eFE4F98
user_fee_token_deployer:    0x2AC5278D230f88B481bBE4A94751d7188ef48Ca2
```

While not a named account, 0x3f1eae7d46d88f08fc2f8ed27fcb2ab183eb2d0e is funded on all test chains.

## Contact

Discord - [Arbitrum](https://discord.com/invite/5KE54JwyTs)

Twitter: [Arbitrum](https://twitter.com/arbitrum)


How to run a local full chain simulation
Overview
A local full-chain simulation allows you to deploy and test smart contracts in a fully controlled environment. This how-to walks you through the process of setting up and running a complete development environment on your local machine, including a Nitro node, a dev-mode Geth L1, and multiple instances with different roles.

Note that the node is now Stylus-enabled by default, and the setup instructions remain the same as for running a Stylus dev node.

Step 1. Install prerequisites
You'll need docker and docker compose to run your node. Follow the instructions in their site to install them.

Step 2. Clone the nitro-testnode repo
You'll need the release branch.

  `git clone -b release --recurse-submodules https://github.com/OffchainLabs/nitro-testnode.git && cd nitro-testnode`

Step 3. Run your node
./test-node.bash --init

Step 4. Successive runs
To relaunch the node after the first installation, run the following command.

./test-node.bash

CLEAR LOCAL DATA
Note that running with the --init flag will clear all chain data and redeploy!

Rollup contract addresses and chain configuration
You can obtain the rollup chain configuration by running the following command. The chain configuration also includes the addresses of the core contracts.

docker exec nitro-testnode-sequencer-1 cat /config/l2_chain_info.json

You can find other available configuration files by running:

docker exec nitro-testnode-sequencer-1 ls /config

Token bridge
An L1-L2 token bridge can be deployed by using the parameter --tokenbridge. The list of contracts can be found by running:

docker compose run --entrypoint sh tokenbridge -c "cat l1l2_network.json"

Running an L3 chain
An L3 chain can be deployed on top of the L2 chain, by using the parameter --l3node. Its chain configuration can be found by running:

docker exec nitro-testnode-sequencer-1 cat /config/l3_chain_info.json

When deploying an L3 chain, the following parameters are also available:

--l3-fee-token: Uses a custom gas token for the L3 (symbol $APP), deployed on L2 at address 0x9b7c0fcc305ca36412f87fd6bd08c194909a7d4e --l3-token-bridge: Deploys an L2-L3 token bridge. The list of contracts can be found by running docker compose run --entrypoint sh tokenbridge -c "cat l2l3_network.json".

Additional arguments
You can find a list of additional arguments to use with test-node.bash by using --help.

./test-node.bash --help

Helper scripts
The repository includes a set of helper scripts for basic actions like funding accounts or bridging funds. You can see a list of the available scripts by running:

./test-node.bash script --help

If you want to see information of a particular script, you can add the name of the script to the help command.

./test-node.bash script send-l1 --help

Here's an example of how to run the script that funds an address on L2. Replace 0x11223344556677889900 with the address you want to fund.

./test-node.bash script send-l2 --to address_0x11223344556677889900 --ethamount 5

Blockscout
Nitro comes with a local Blockscout block explorer. To access it, add the param --blockscout when running your node.

./test-node.bash --blockscout

The block explorer will be available at http://localhost:4000

Default endpoints and addresses
Node RPC endpoints are available at:

Node	Chain id	RPC endpoint
L1 geth devnet	1337	http://localhost:8545
L2 nitro devnet	412346	http://localhost:8547 and ws://localhost:8548
L3 nitro (if enabled)	333333	http://localhost:3347
Some important addresses:

Role	Public address	Private key
Sequencer	0xe2148eE53c0755215Df69b2616E552154EdC584f	0xcb5790da63720727af975f42c79f69918580209889225fa7128c92402a6d3a65
Validator	0x6A568afe0f82d34759347bb36F14A6bB171d2CBe	0x182fecf15bdf909556a0f617a63e05ab22f1493d25a9f1e27c228266c772a890
L2 rollup owner	0x5E1497dD1f08C87b2d8FE23e9AAB6c1De833D927	0xdc04c5399f82306ec4b4d654a342f40e2e0620fe39950d967e1e574b32d4dd36
L3 rollup owner (if enabled)	0x863c904166E801527125D8672442D736194A3362	0xecdf21cb41c65afb51f91df408b7656e2c8739a5877f2814add0afd780cc210e
L3 sequencer (if enabled)	0x3E6134aAD4C4d422FF2A4391Dc315c4DDf98D1a5	0x90f899754eb42949567d3576224bf533a20857bf0a60318507b75fcb3edc6f5f
Dev account (prefunded with ETH in all networks)	0x3f1Eae7D46d88F08fc2F8ed27FCb2AB183EB2d0E	0xb6b15c8cb491557369f3c7d2c287b053eb229daa9c22138887752191c9520659
You can fund other addresses by using the scripts send-l1 and send-l2 as explained here.

PRIVATE KEYS PUBLICLY KNOWN
Do not use any of these addresses in a production environment.

Optional parameters
Here, We show a list of the parameters that might be useful when running a local devnode. You can also use the flag ./test-node.bash --help to get them.

Flag	Description
--init	Removes all the data, rebuilds, and deploys a new rollup
proof-of-stake chain (using Prysm for consensus)	
heavy computation	
up the L3 chain to use a custom fee token. Only valid if --l3node flag is provided	
--l3-fee-token-decimals	Number of decimals to use for a custom fee token. Only valid if
--l3-fee-token flag is provided	
valid if --l3node flag is provided	
--redundantsequencers	Redundant sequencers [0-3]
running them	
configuration: one node as a sequencer/batch-poster/staker (default unless using --dev)	
--tokenbridge	Deploy an L1-L2 token bridge
launching the token bridge	
--no-simple	Runs a full configuration with separate sequencer/batch-poster/validator/relayer