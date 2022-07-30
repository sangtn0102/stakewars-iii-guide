# Mount your Staking Pool.

Deploy a new staking pool for your validator. Do operations on your staking pool to delegate and stake NEAR.

NEAR uses a staking pool factory with a whitelisted staking contract to ensure delegators’ funds are safe. In order to run a validator on NEAR, a staking pool must be deployed to a NEAR account and integrated into a NEAR validator node. Delegators must use a UI or the command line to stake to the pool. A staking pool is a smart contract that is deployed to a NEAR account.

#### Deploy a Staking Pool Contract
##### Deploy a Staking Pool
Calls the staking pool factory, creates a new staking pool with the specified name, and deploys it to the indicated accountId.

```
near call factory.shardnet.near create_staking_pool '{"staking_pool_id": "<pool id>", "owner_id": "<accountId>", "stake_public_key": "<public key>", "reward_fee_fraction": {"numerator": 5, "denominator": 100}, "code_hash":"DD428g9eqLL8fWUxv8QSpVFzyHi1Qd16P8ephYCTmMSZ"}' --accountId="<accountId>" --amount=30 --gas=300000000000000
```

From the example above, you need to replace:

* **Pool ID**: Staking pool name, the factory automatically adds its name to this parameter, creating {pool_id}.{staking_pool_factory}
Examples:   

- If pool id is stakewars will create : `stakewars.factory.shardnet.near`

* **Owner ID**: The SHARDNET account (i.e. stakewares.shardnet.near) that will manage the staking pool.
* **Public Key**: The public key in your **validator_key.json** file.
* **5**: The fee the pool will charge (e.g. in this case 5 over 100 is 5% of fees).
* **Account Id**: The SHARDNET account deploying and signing the mount tx.  Usually the same as the Owner ID.

> Be sure to have at least 30 NEAR available, it is the minimum required for storage.
Example : near call stake_wars_validator.factory.shardnet.near --amount 30 --accountId stakewars.shardnet.near --gas=300000000000000


To change the pool parameters, such as changing the amount of commission charged to 1% in the example below, use this command:
```
near call <pool_name> update_reward_fee_fraction '{"reward_fee_fraction": {"numerator": 1, "denominator": 100}}' --accountId <account_id> --gas=300000000000000
```


You will see something like this:

![image](https://user-images.githubusercontent.com/107299476/181914502-2c2f5867-9353-40fc-9f88-41f8da22c85f.png)

If there is a “True” at the End. Your pool is created.

**You have now configure your Staking pool.**

Check your pool is now visible on https://explorer.shardnet.near.org/nodes/validators


#### Transactions Guide
##### Deposit and Stake NEAR

Command:
```
near call <staking_pool_id> deposit_and_stake --amount <amount> --accountId <accountId> --gas=300000000000000
```
##### Unstake NEAR
Amount in yoctoNEAR.
1 NEAR = 10^24 yoctoNEAR = 1.000.000.000.000.000.000.000.000 yoctoNEAR

Run the following command to unstake:
```
near call <staking_pool_id> unstake '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```

![image](https://user-images.githubusercontent.com/107299476/181916404-351e9d56-a435-4de0-8785-c6518be0d4e3.png)

To unstake all you can run this one:
```
near call <staking_pool_id> unstake_all --accountId <accountId> --gas=300000000000000
```
##### Withdraw

Unstaking takes 2-3 epochs to complete, after that period you can withdraw in YoctoNEAR from pool.

Command:
```
near call <staking_pool_id> withdraw '{"amount": "<amount yoctoNEAR>"}' --accountId <accountId> --gas=300000000000000
```
Command to withdraw all:
```
near call <staking_pool_id> withdraw_all --accountId <accountId> --gas=300000000000000
```

##### Ping
A ping issues a new proposal and updates the staking balances for your delegators. A ping should be issued each epoch to keep reported rewards current.

Command:
```
near call <staking_pool_id> ping '{}' --accountId <accountId> --gas=300000000000000
```

![image](https://user-images.githubusercontent.com/107299476/181916554-514b3bfb-e354-460f-9081-e562d8f56474.png)

Balances
Total Balance
Command:
```
near view <staking_pool_id> get_account_total_balance '{"account_id": "<accountId>"}'
```

![image](https://user-images.githubusercontent.com/107299476/181916611-f3bdf183-55a5-4878-9ecd-d773ebcbd3aa.png)


##### Staked Balance
Command:
```
near view <staking_pool_id> get_account_staked_balance '{"account_id": "<accountId>"}'
```

![image](https://user-images.githubusercontent.com/107299476/181915761-e3fb5edf-5f86-4a71-9224-18b81fe8213c.png)

##### Unstaked Balance
Command:
```
near view <staking_pool_id> get_account_unstaked_balance '{"account_id": "<accountId>"}'
```
##### Available for Withdrawal
You can only withdraw funds from a contract if they are unlocked.

Command:
```
near view <staking_pool_id> is_account_unstaked_balance_available '{"account_id": "<accountId>"}'
```
##### Pause / Resume Staking
###### Pause
Command:
```
near call <staking_pool_id> pause_staking '{}' --accountId <accountId>
```
###### Resume
Command:
```
near call <staking_pool_id> resume_staking '{}' --accountId <accountId>
```

## Next step🚀

[Check your Node](./4-check-your-node.md).

