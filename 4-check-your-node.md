## Check your Node
Setup tools for monitoring node status. Install and use RPC on port 3030 to get useful information for keep your node working.

### Monitor and make alerts 

An email notification can make it more comfortable to maintain a validator up and running. Achieve to be a validator confirming transactions on testnet and get >95% of uptime.

#### Log Files
The log file is stored either in the ~/.nearup/logs directory or in systemd depending on your setup.


Systemd Command:
```
journalctl -n 100 -f -u neard | ccze -A
```

**Log file sample:**

Validator | 1 validator

```

INFO stats: # 1540066 2gE4uhYDPRG2BfpUGpW6d9CARyJrfa7PP9nMKDvTmife 100 validators 31 peers ⬇ 312 kB/s ⬆ 266 kB/s 0.50 bps 0 gas/s CPU: 111%, Mem: 1.43 GB

```

* **Validator**: A “Validator” will indicate you are an active validator
* **100 validators**: Total 100 validators on the network
* **31 peers**: You current have 31 peers. You need at least 3 peers to reach consensus and start validating
* **# 1540066**: block – Look to ensure blocks are moving

#### RPC
Any node within the network offers RPC services on port 3030 as long as the port is open in the nodes firewall. The NEAR-CLI uses RPC calls behind the scenes. Common uses for RPC are to check on validator stats, node version and to see delegator stake, although it can be used to interact with the blockchain, accounts and contracts overall.

Find many commands and how to use them in more detail here:

https://docs.near.org/api/rpc/introduction



Command:
```
sudo apt install curl jq
```
###### Common Commands:
####### Check your node version:
Command:
```
curl -s http://127.0.0.1:3030/status | jq .version
```
####### Check Delegators and Stake
Command:
```
near view <your pool>.factory.shardnet.near get_accounts '{"from_index": 0, "limit": 10}' --accountId <accountId>.shardnet.near
```
####### Check Reason Validator Kicked
Command:
```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.prev_epoch_kickout[] | select(.account_id | contains ("<POOL_ID>"))' | jq .reason
```
####### Check Blocks Produced / Expected
Command:
```
curl -s -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' 127.0.0.1:3030 | jq -c '.result.current_validators[] | select(.account_id | contains ("POOL_ID"))'
```

## Congratulations on setting up your node.


