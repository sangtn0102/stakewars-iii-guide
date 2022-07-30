# Create your Shardnet wallet & deploy the NEAR CLI.
Create your Shardnet wallet & deploy the NEAR CLI. This is designed to be your very first challenge: use it to understand how staking on NEAR works.
Table of Contents:

[1. Create your Shardnet wallet](https://github.com/sangtn0102/stakewars-iii-guide/blob/main/1-create-wallet-deploy-near-cli.md#1-create-your-shardnet-wallet)

[2. Deploy the NEAR CLI](https://github.com/sangtn0102/stakewars-iii-guide/blob/main/1-create-wallet-deploy-near-cli.md#1-create-your-shardnet-wallet)

## 1. Create your Shardnet wallet
Follow the instructions on the website to create a NEAR wallet on the Shardnet testnet.

Link: https://wallet.shardnet.near.org/

Select Create Account to create a new wallet.

![image](https://user-images.githubusercontent.com/107299476/181875879-356427ec-4ab2-4a00-915e-2e7a52fc8559.png)

Enter your desired wallet name. The naming convention must satisfy as described, then click Reserve My Account ID to create a wallet.

![image](https://user-images.githubusercontent.com/107299476/181876066-2acbe979-07a1-4363-a8d9-8f5622d4a6b1.png)

Here select the recommended Secure Passphrase, then click Continue

![image](https://user-images.githubusercontent.com/107299476/181876162-d519b006-4302-44fd-b77e-072693ff1e82.png)

Back up these 12 characters in a safe place, then click Continue.

![image](https://user-images.githubusercontent.com/107299476/181876251-ab505d96-d900-44dd-a054-9d4e9f75d3a8.PNG)

Please re-enter the word that has been backed up above, the example below is to enter the 3rd word then click Verify & Complete.
Then wait a bit for the wallet creation process to complete.

![image](https://user-images.githubusercontent.com/107299476/181876331-8bfa2db7-1a65-42a8-8c01-c4389ce89019.png)


Successful wallet creation, you will receive ~2000 NEAR to the wallet (this Testnet tokens have no value on Mainnet)
## 2. Deploy the NEAR CLI

NEAR-CLI is a command-line interface that communicates with the NEAR blockchain via remote procedure calls (RPC):

* Setup and Installation NEAR CLI
* View Validator Stats*

> Note: For security reasons, it is recommended that NEAR-CLI be installed on a different computer than your validator node and that no full access keys be kept on your validator node.

First, let's make sure the linux machine is up-to-date.
```
sudo apt update && sudo apt upgrade -y
```
##### Install developer tools, Node.js, and npm
First, we will start with installing `Node.js` and `npm`:
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
sudo apt install build-essential nodejs
PATH="$PATH"
```

Check `Node.js` and `npm` version:
```
node -v
```
> v18.x.x

```
npm -v
```
> 8.x.x

##### Install NEAR-CLI
Here's the Github Repository for NEAR CLI.: https://github.com/near/near-cli. To install NEAR-CLI, unless you are logged in as root, which is not recommended you will need to use `sudo` to install NEAR-CLI so that the near binary is saved to /usr/local/bin

```
sudo npm install -g near-cli
```

![image](https://user-images.githubusercontent.com/107299476/181876876-7cbf8b6b-7e37-4527-b0e5-4a0677e30bcb.png)

### Validator Stats

Now that NEAR-CLI is installed, let's test out the CLI and use the following commands to interact with the blockchain as well as to view validator stats. There are three reports used to monitor validator status:

###### Environment
The environment will need to be set each time a new shell is launched to select the correct network.

Networks:
- GuildNet
- TestNet
- MainNet
- **Shardnet** (this is the network we will use for Stake Wars)

Command:
```
export NEAR_ENV=shardnet
```

You can also run this command to set the Near testnet Environment persistent:
```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
echo 'export NEAR_ENV=shardnet' >> ~/.bash_profile
source $HOME/.bash_profile
```
![image](https://user-images.githubusercontent.com/107299476/181876961-007f7384-6bee-42d6-b155-bbbcfa37d2dc.png)


#### NEAR CLI Commands Guide:

###### Proposals
A proposal by a validator indicates they would like to enter the validator set, in order for a proposal to be accepted it must meet the minimum seat price.

Command:
```
near proposals
```

![image](https://user-images.githubusercontent.com/107299476/181877039-c515eb96-aa68-4bd8-b934-5ea3ef9b156e.png)


###### Validators Current
This shows a list of active validators in the current epoch, the number of blocks produced, number of blocks expected, and online rate. Used to monitor if a validator is having issues.

Command:
```
near validators current
```

![image](https://user-images.githubusercontent.com/107299476/181877083-deb614bb-bcac-4ba1-a5a7-a1d8ad27a970.png)

###### Validators Next
This shows validators whose proposal was accepted one epoch ago, and that will enter the validator set in the next epoch.

Command:
```
near validators next
```
![image](https://user-images.githubusercontent.com/107299476/181877105-7d6f96bc-2e58-4fda-87bb-5fbfc2e6fd9b.png)

---
## Next step?

[Setup and Run your Node](./setup-and-run-your-node.md)
