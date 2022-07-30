## Setup your node

This challenge is focused on deploying a node (nearcore), downloading a snapshot, syncing it to the actual state of the network, then activating the node as a validator.


### Setup your node
#### Server Requirements
Please see the hardware requirement below:

| Hardware       | Chunk-Only Producer  Specifications                                   |
| -------------- | ---------------------------------------------------------------       |
| CPU            | 4-Core CPU with AVX support                                           |
| RAM            | 8GB DDR4                                                              |
| Storage        | 500GB SSD                                                             |

The above is the minimum requirement for you to run node, you will need a computer running 24/7, you can use a personal computer, but I recommend you to use a service that also provides VPS.
Below I have used Contabo's VPS, it is quite cheap compared to other VPS providers, the VPS configuration I have chosen is 8vCPU, 30GB RAM, 800GB SSD.
In addition to Contabo, you can refer to other VPS service providers such as Google cloud, Hetzner, Vultr, Linode, Digital Ocean, ...

![image](https://user-images.githubusercontent.com/107299476/181902955-d7aea9bf-c40a-4c20-af2a-510ccbabb595.png)

#### Install required software & set the configuration

##### Prerequisites:
Before you start, you may want to confirm that your machine has the right CPU features. 

```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
```
> Supported

##### Install developer tools:
```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo
```
#####  Install Python pip:

```
sudo apt install python3-pip
```


![image](https://user-images.githubusercontent.com/107299476/181891823-b9fd95c1-0245-440a-8132-4bceec8fd38d.png)

Type in y, then press Enter to agree to the installation.

##### Set the configuration:

```
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```
![image](https://user-images.githubusercontent.com/107299476/181893085-36324310-8923-4805-a17c-2933698e3ecf.png)

##### Install Building env
```
sudo apt install clang build-essential make
```
![image](https://user-images.githubusercontent.com/107299476/181893571-de84514a-2673-4dba-8596-06600a017d53.png)

##### Install Rust & Cargo
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

You will see the following:

![image](https://user-images.githubusercontent.com/107299476/181894326-5d7a4696-c6c1-4ce8-abac-600eabb06075.png)

Press 1 and press enter.

##### Source the environment
```
source $HOME/.cargo/env
```
![image](https://user-images.githubusercontent.com/107299476/181894676-446e5690-58c8-4e77-9de5-7421af859b65.png)

#### Clone `nearcore` project from GitHub
First, clone the [`nearcore` repository](https://github.com/near/nearcore).

```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```

Checkout to the commit needed. Please refer to the commit defined in [this file](https://github.com/near/stakewars-iii/blob/main/commit.md). 
```
git checkout <commit>
```

#### Compile `nearcore` binary
In the `nearcore` folder run the following commands:

```
cargo build -p neard --release --features shardnet
```

![image](https://user-images.githubusercontent.com/107299476/181901583-aa501e09-7d53-4a60-82b2-de90a0236cec.png)


The binary path is `target/release/neard`. If you are seeing issues, it is possible that cargo command is not found. Compilation of `nearcore` binaries may take some time depending on hardware configuration, mine took about 28 minutes to compile so please wait patiently.

![image](https://user-images.githubusercontent.com/107299476/181903250-8229109a-5f12-4361-b20c-1f773581a44c.png)


#### Initialize working directory

In order to work properly, the NEAR node requires a working directory and a couple of configuration files. Generate the initial required working directory by running:

```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```

![init](https://user-images.githubusercontent.com/107299476/181903439-af78243d-09e5-4f20-9519-7a71d93532c9.PNG)


This command will create the directory structure and will generate `config.json`, `node_key.json`, and `genesis.json` on the network you have passed. 

- `config.json` - Configuration parameters which are responsive for how the node will work. The config.json contains needed information for a node to run on the network, how to communicate with peers, and how to reach consensus. Although some options are configurable. In general validators have opted to use the default config.json provided.

- `genesis.json` - A file with all the data the network started with at genesis. This contains initial accounts, contracts, access keys, and other records which represents the initial state of the blockchain. The genesis.json file is a snapshot of the network state at a point in time. In contacts accounts, balances, active validators, and other information about the network. 

- `node_key.json` -  A file which contains a public and private key for the node. Also includes an optional `account_id` parameter which is required to run a validator node (not covered in this doc).

- `data/` -  A folder in which a NEAR node will write it's state.

#### Replace the `config.json`

From the generated `config.json`, there two parameters to modify:
- `boot_nodes`: If you had not specify the boot nodes to use during init in Step 3, the generated `config.json` shows an empty array, so we will need to replace it with a full one specifying the boot nodes.
- `tracked_shards`: In the generated `config.json`, this field is an empty. You will have to replace it to `"tracked_shards": [0]`

```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```

![image](https://user-images.githubusercontent.com/107299476/181904448-0b1ac8a1-1904-47f0-b9bd-5ac927dcd6c5.png)

#### Get latest snapshot

**IMPORTANT: NOT REQUIRED TO GET SNAPSHOT AFTER HARDFORK ON SHARDNET DURING 2022-07-18**

Install AWS Cli
```
sudo apt-get install awscli -y
```

Download snapshot (genesis.json)
```
// IMPORTANT: NOT REQUIRED TO GET SNAPSHOT AFTER HARDFORK ON SHARDNET DURING 2022-07-18
cd ~/.near
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json
```

If the above fails, AWS CLI may be oudated in your distribution repository. Instead, try:
```
pip3 install awscli --upgrade
```

#### Run the node
To start your node simply run the following command:

```
cd ~/nearcore
./target/release/neard --home ~/.near run
```

![image](https://user-images.githubusercontent.com/107299476/181904972-2030f8c4-c443-418f-b473-88e5632bf622.png)

The node is now running you can see log outputs in your console. 

Your node should be find peers, download headers to 100%, and then download blocks.
This takes some time, please wait until it is 100%.

----



### Activating the node as validator
##### Authorize Wallet Locally
A full access key needs to be installed locally to be able to sign transactions via NEAR-CLI.


* You need to run this command:

```
near login
```

> Note: This command launches a web browser allowing for the authorization of a full access key to be copied locally.

1 – Copy the link in your browser

![image](https://user-images.githubusercontent.com/107299476/181905212-b8f14a3d-89d0-4ece-95c9-f609f99d328e.png)

Type in y and then press Enter to continue

![image](https://user-images.githubusercontent.com/107299476/181905813-e2611f78-c6db-4509-bbd7-daa3bacb4a2d.PNG)


2 – Grant Access to Near CLI

![image](https://user-images.githubusercontent.com/107299476/181905848-2ac29dc7-15eb-45f5-834f-354302d66956.PNG)


![image](https://user-images.githubusercontent.com/107299476/181905853-2e2dd841-4f7e-4d77-8947-7833afa802a5.PNG)

3 – After Grant, you will see a page like this, go back to console

![image](https://user-images.githubusercontent.com/107299476/181905880-2e560b10-690d-4f13-a078-2733241da49c.png)

4 – Enter your wallet and press Enter

![image](https://user-images.githubusercontent.com/107299476/181905986-21d87e79-4d47-4222-8b24-70b277d0593a.PNG)


#####  Check the validator_key.json
* Run the following command:
```
cat ~/.near/validator_key.json
```


> Note: If a validator_key.json is not present, follow these steps to create one

Create a `validator_key.json` 

*   Generate the Key file:

```
near generate-key <pool_id>
```
<pool_id> ---> xx.factory.shardnet.near WHERE xx is you pool name

* Copy the file generated to shardnet folder:
Make sure to replace <pool_id> by your accountId
```
cp ~/.near-credentials/shardnet/YOUR_WALLET.json ~/.near/validator_key.json
```
* Edit “account_id” => xx.factory.shardnet.near, where xx is your PoolName
* Change `private_key` to `secret_key`

> Note: The account_id must match the staking pool contract name or you will not be able to sign blocks.\

File content must be in the following pattern:
```
{
  "account_id": "xx.factory.shardnet.near",
  "public_key": "ed25519:****",
  "secret_key": "ed25519:****"
}
```

####  Start the validator node

```
target/release/neard run
```
* Setup Systemd
Command:

```
echo "[Unit]
Description=NEARd Daemon Service

[Service]
Type=simple
User=$USER
#Group=near
WorkingDirectory=$HOME/.near
ExecStart=$HOME/nearcore/target/release/neard run
Restart=on-failure
RestartSec=30
KillSignal=SIGINT
TimeoutStopSec=45
KillMode=mixed

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/neard.service
```

Command:

```
sudo systemctl enable neard
```
Command:

```
sudo systemctl start neard
```
If you need to make a change to service because of an error in the file. It has to be reloaded:

```
sudo systemctl reload neard
```
###### Watch logs
Command:

```
journalctl -n 100 -f -u neard
```
Make log output in pretty print

Command:

```
sudo apt install ccze
```
View Logs with color

Command:

```
journalctl -n 100 -f -u neard | ccze -A
```
#### Becoming a Validator
In order to become a validator and enter the validator set, a minimum set of success criteria must be met.

* The node must be fully synced
* The `validator_key.json` must be in place
* The contract must be initialized with the public_key in `validator_key.json`
* The account_id must be set to the staking pool contract id
* There must be enough delegations to meet the minimum seat price. See the seat price [here](https://explorer.shardnet.near.org/nodes/validators).
* A proposal must be submitted by pinging the contract
* Once a proposal is accepted a validator must wait 2-3 epoch to enter the validator set
* Once in the validator set the validator must produce great than 90% of assigned blocks

Check running status of validator node. If “Validator” is showing up, your pool is selected in the current validators list.


## Next step 🚀

[Mount your Staking Pool](./3-mount-your-staking-pool.md).
