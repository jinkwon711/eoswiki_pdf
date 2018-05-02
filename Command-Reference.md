## cleos command

| Action                      | Syntax                                                                                               | Example                                    |
| --------------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| Get ALL commands            | $ cleos                                                                                              | [View](#all-command)                       |
| Get ALL subcommands         | $ cleos ${command}                                                                                   | [View](#all-subcommand)                    |
| Connect to node             | $ cleos --url ${node}:${port}                                                                        | [View](#connect-to-node)                   |
| Query blockchain state      | $ cleos get info                                                                                     | [View](#query-blockchain-state)            |
| Get transaction by id       | $ cleos get transaction ${transaction_id}                                                            | [View](#get-transaction-by-transaction_id) |
| Get transactions by account | $ cleos get transaction ${account}                                                                   | [View](#get-transaction-by-account)        |
| Transfer EOS                | $ cleos transfer ${from_account} ${to_account} ${quantity}                                           | [View](#transfer-eos)                      |
| Wallet - Create wallet      | $ cleos wallet create {-n} ${wallet_name}                                                            | [View](#create-wallet)                     |
| Wallet - List wallets       | $ cleos wallet list                                                                                  | [View](#list-wallets)                      |
| Wallet - Import key         | $ cleos wallet import ${key}                                                                         | [View](#import-key-to-wallet)              |
| Wallet - List keys          | $ cleos wallet keys                                                                                  | [View](#list-wallet-keys)                  |
| Wallet - Lock               | $ cleos wallet lock -n ${wallet_name}                                                                | [View](#lock-wallet)                       |
| Wallet - Unlock             | $ cleos wallet unlock -n ${wallet_name} --password ${password}                                       | [View](#unlock-wallet)                     |
| Wallet - Open               | $ cleos wallet open                                                                                  | [View](#open-wallet)                       |
| Account - Create keys       | $ cleos create key                                                                                   | [View](#create-keys)                       |
| Account - Create account    | $ cleos create account ${control_account} ${account_name} ${owner_public_key} ${active_public_key}   | [View](#create-account)                    |
| Account - See servants      | $ cleos get servants ${account_name}                                                                 | [View](#account-servants)                  |
| Account - Check balance     | $ cleos get account ${account_name}                                                                  | [View](#check-account-balance)             |
| Permission - Create/Modify  | $ cleos set account permission ${permission} ${account} ${permission_json} ${account_authority}      | [View](#create-or-modify-permissions)      |
| Contract - Deploy           | $ cleos set contract ../${contract}.wast ../${contract}.abi                                          | [View](#deploy-contract)                   |
| Contract - Query ABI        | $ cleos get code -a ${contract}.abi ${contract}                                                      | [View](#query-contract-abi)                |
| Contract - Push Message     | $ cleos push message ${contract} ${action} ${param} -S ${scope_1} -S ${scope_2} -p ${account}@active | [View](#push-message-to-contract)          |
| Contract - Query table      | $ cleos get table ${field} ${contract} ${table}                                                      | [View](#querying-contract)                 |

## nodeos command

| Action          | Syntax                                 | Example                  |
| --------------- | -------------------------------------- | ------------------------ |
| Skip signatures | $ nodeos --skip-transaction-signatures | [View](#skip-signatures) |

## eos-wallets command

| Action                  | Syntax                                        | Example                            |
| ----------------------- | --------------------------------------------- | ---------------------------------- |
| Use separate wallet app | $ keosd --http-server-endpoint ${node}:{port} | [View](#using-separate-wallet-app) |

## Examples

### All command

cleos contains documentation for all of its commands. For a list of all commands known to cleos, simply run it with no arguments:

```
$ cleos
ERROR: RequiredError: Subcommand required
Command Line Interface to EOSIO Client
Usage: cleos [OPTIONS] SUBCOMMAND

Options:
  -h,--help                   Print this help message and exit
  -u,--url TEXT=http://localhost:8888/
                              the http/https URL where nodeos is running
  --wallet-url TEXT=http://localhost:8888/
                              the http/https URL where keosd is running
  -v,--verbose                output verbose actions on error

Subcommands:
  version                     Retrieve version information
  create                      Create various items, on and off the blockchain
  get                         Retrieve various items and information from the blockchain
  set                         Set or update blockchain state
  transfer                    Transfer EOS from account to account
  net                         Interact with local p2p network connections
  wallet                      Interact with local wallet
  sign                        Sign a transaction
  push                        Push arbitrary transactions to the blockchain
  multisig                    Multisig contract commands
  system                      Send eosio.system contract action to the blockchain.
```

### All subcommand

To get help with any particular subcommand, run it with no arguments as well:

```
$ cleos create
ERROR: RequiredError: Subcommand required
Create various items, on and off the blockchain
Usage: ./cleos create SUBCOMMAND
Subcommands:
  key                         Create a new keypair and print the public and private keys
  account                     Create a new account on the blockchain
  producer                    Create a new producer on the blockchain

$ cleos create account
ERROR: RequiredError: creator
Create a new account on the blockchain
Usage: ./cleos create account creator name OwnerKey ActiveKey
Positionals:
  creator TEXT                The name of the account creating the new account
  name TEXT                   The name of the new account
  OwnerKey TEXT               The owner public key for the account
  ActiveKey TEXT              The active public key for the account

```

### Connect to node

This will connect you to your local node

```
$ cleos -u localhost:8889 <subcommand>
$ cleos --url localhost:8889 <subcommand>
```

You can also adjust the node params to connect to a different node, e.g. the public testnet

```
$ cleos -H test1.eos.io -p 80 <subcommand>
```

**Note** You need to include the `-H` and `-p` arguments with each request to `cleos`

### Query blockchain state

```bash
$ cleos get info
{
  "server_version": "7451e092",
  "head_block_num": 6980,
  "last_irreversible_block_num": 6963,
  "head_block_id": "00001b4490e32b84861230871bb1c25fb8ee777153f4f82c5f3e4ca2b9877712",
  "head_block_time": "2017-12-07T09:18:48",
  "head_block_producer": "initp",
  "recent_slots": "1111111111111111111111111111111111111111111111111111111111111111",
  "participation_rate": "1.00000000000000000"
}
```

### Get Transaction by transaction_id

With account_history_api_plugin loaded in nodeos, we can query for particular transaction using the transaciton_id

```
$ cleos get transaction eb4b94b72718a369af09eb2e7885b3f494dd1d8a20278a6634611d5edd76b703
{
  "transaction_id": "eb4b94b72718a369af09eb2e7885b3f494dd1d8a20278a6634611d5edd76b703",
  "processed": {
    "refBlockNum": 2206,
    "refBlockPrefix": 221394282,
    "expiration": "2017-09-05T08:03:58",
    "scope": [
      "inita",
      "tester"
    ],
    "signatures": [
      "1f22e64240e1e479eee6ccbbd79a29f1a6eb6020384b4cca1a958e7c708d3e562009ae6e60afac96f9a3b89d729a50cd5a7b5a7a647540ba1678831bf970e83312"
    ],
    "messages": [{
        "code": "eos",
        "type": "transfer",
        "authorization": [{
            "account": "inita",
            "permission": "active"
          }
        ],
        "data": {
          "from": "inita",
          "to": "tester",
          "amount": 1000,
          "memo": ""
        },
        "hex_data": "000000008040934b00000000c84267a1e80300000000000000"
      }
    ],
    "output": [{
        "notify": [{
            "name": "tester",
            "output": {
              "notify": [],
              "sync_transactions": [],
              "async_transactions": []
            }
          },{
            "name": "inita",
            "output": {
              "notify": [],
              "sync_transactions": [],
              "async_transactions": []
            }
          }
        ],
        "sync_transactions": [],
        "async_transactions": []
      }
    ]
  }
}
```

### Get Transaction by account

We can also query list of transactions performed by certain account starting from recent one

```
$ cleos get transactions inita
[
  {
    "transaction_id": "eb4b94b72718a369af09eb2e7885b3f494dd1d8a20278a6634611d5edd76b703",
    ...
  },
  {
    "transaction_id": "6acd2ece68c4b86c1fa209c3989235063384020781f2c67bbb80bc8d540ca120",
    ...
  },
  ...
]
```

### Transfer EOS

```
$ cleos transfer inita tester 1000
{
  "transaction_id": "eb4b94b72718a369af09eb2e7885b3f494dd1d8a20278a6634611d5edd76b703",
  "processed": {
    "refBlockNum": 2206,
    "refBlockPrefix": 221394282,
    "expiration": "2017-09-05T08:03:58",
    "scope": [
      "inita",
      "tester"
    ],
    "signatures": [
      "1f22e64240e1e479eee6ccbbd79a29f1a6eb6020384b4cca1a958e7c708d3e562009ae6e60afac96f9a3b89d729a50cd5a7b5a7a647540ba1678831bf970e83312"
    ],
    "messages": [{
        "code": "eos",
        "type": "transfer",
        "authorization": [{
            "account": "inita",
            "permission": "active"
          }
        ],
        "data": {
          "from": "inita",
          "to": "tester",
          "amount": 1000,
          "memo": ""
        },
        "hex_data": "000000008040934b00000000c84267a1e80300000000000000"
      }
    ],
    "output": [{
        "notify": [{
            "name": "tester",
            "output": { ... }
          },{
            "name": "inita",
            "output": { ... }
          }
        ],
        "sync_transactions": [],
        "async_transactions": []
      }
    ]
  }
}
```

### Create wallet

Create wallet without specifying a name, the wallet will be created with the name 'default'

```
$ cleos wallet create
Creating wallet: default
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5JD9cw9YY288AXPvnbwUk5JK4Cy6YyZ83wzHcshu8F2akU9rRWE"
```

You can name the wallet adding the `-n ${wallet_name}` in the command

```
$ cleos wallet create -n second-wallet
Creating wallet: second-wallet
Save password to use in the future to unlock this wallet.
Without password imported keys will not be retrievable.
"PW5Ji6JUrLjhKAVn68nmacLxwhvtqUAV18J7iycZppsPKeoGGgBEw"
```

### List wallets

The list wallet command will list all wallet with status of each wallet, the \* symbol indicates that the wallet is currently unlocked.

```
$ cleos wallet list
Wallets:
[
  "default *",
  "second-wallet *"
]
```

### Import key to wallet

Note: If you do not hold an account key, you will need to use the create account command to first create account keys.

```
$ cleos wallet import 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
imported private key for: EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV
```

### List wallet keys

This will list all the keys stored in the wallet in public private key pair.

```
$ cleos wallet keys
[[
    "EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
    "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"
  ]
]
```

### Lock wallet

```
$ cleos wallet lock -n second-wallet
Locked: 'second-wallet'

Notice that the locked wallet doesn't have * symbol in the list

$ cleos wallet list
Wallets:
[
  "default *",
  "second-wallet"
]
```

### Unlock wallet

To unlock it specify the password you get when creating the wallet

```
$ cleos wallet unlock -n second-wallet --password PW5Ji6JUrLjhKAVn68nmacLxwhvtqUAV18J7iycZppsPKeoGGgBEw
Unlocked: 'second-wallet'
```

### Open wallet

```
$ cleos wallet open
Wallets: [
  "default"
]
$ cleos wallet open -n second-wallet
Wallets: [
  "default",
  "second-wallet"
]
```

### Create keys

In order to create an account you will need two new keys: owner and active. You can ask cleos to create some keys for you:

This will be your owner key,

```
$ cleos create key
public: EOS4toFS3YXEQCkuuw1aqDLrtHim86Gz9u3hBdcBw5KNPZcursVHq
private: 5JKbLfCXgcafDQVwHMm3shHt6iRWgrr9adcmt6vX3FNjAEtJGaT
```

And this will be your active key,

```
$ cleos create key
public: EOS7d9A3uLe6As66jzN8j44TXJUqJSK3bFjjEEqR4oTvNAB3iM9SA
private: 5Hv22aPcjnENBv6X9o9nKGdkfrW44En6z4zJUt2PobAvbQXrT9z
```

Note: cleos does not save the generated private key.

### Create account

You will need your EOS keys in order to create an account, you must either have your EOS keys registered on the Ethereum network or you can use the [create keys](#create-keys) function to create a new sets of keys

```
$ cleos create account inita tester EOS4toFS3YXEQCkuuw1aqDLrtHim86Gz9u3hBdcBw5KNPZcursVHq EOS7d9A3uLe6As66jzN8j44TXJUqJSK3bFjjEEqR4oTvNAB3iM9SA
{
  "transaction_id": "6acd2ece68c4b86c1fa209c3989235063384020781f2c67bbb80bc8d540ca120",
  "processed": {
    "refBlockNum": "25217",
    "refBlockPrefix": "2095475630",
    "expiration": "2017-07-25T17:54:55",
    "scope": [
      "eos",
      "inita"
    ],
    "signatures": [],
    "messages": [{
        "code": "eos",
        "type": "newaccount",
        "authorization": [{
            "account": "inita",
            "permission": "active"
          }
        ],
        "data": "c9251a0000000000b44c5a2400000000010000000102bcca6347d828d4e1868b7dfa91692a16d5b20d0ee3d16a7ca2ddcc7f6dd03344010000010000000102bcca6347d828d4e1868b7dfa91692a16d5b20d0ee3d16a7ca2ddcc7f6dd03344010000010000000001c9251a000000000061d0640b000000000100010000000000000008454f5300000000"
      }
    ],
    "output": [{
        "notify": [],
        "sync_transactions": [],
        "async_transactions": []
      }
    ]
  }
}
```

### Account servants

To check the servant accounts created by an account (control account)

```
$ cleos get servants inita
{
  "controlled_accounts": [
    "tester"
  ]
}
```

### Check account balance

```
$ cleos get account tester
{
  "name": "tester",
  "eos_balance": 0,
  "staked_balance": 1,
  "unstaking_balance": 0,
  "last_unstaking_time": "1969-12-31T23:59:59",
  "permissions": [{
      "name": "active",
      "parent": "owner",
      "required_auth": {
        "threshold": 1,
        "keys": [{
            "key": "EOS7d9A3uLe6As66jzN8j44TXJUqJSK3bFjjEEqR4oTvNAB3iM9SA",
            "weight": 1
          }
        ],
        "accounts": []
      }
    },{
      "name": "owner",
      "parent": "owner",
      "required_auth": {
        "threshold": 1,
        "keys": [{
            "key": "EOS4toFS3YXEQCkuuw1aqDLrtHim86Gz9u3hBdcBw5KNPZcursVHq",
            "weight": 1
          }
        ],
        "accounts": []
      }
    }
  ]
}
```

### Create or Modify Permissions

To modify permissions of an account, you must have the authority over the account and the permission of which you are modifying. The `set account permission` command is subject to change so it's associated Class is not fully documented.

The first example associates a new key to the active permissions of an account

```bash
$ cleos set account permission test active '{"threshold" : 1, "keys" : [{"permission":{"key":"EOS8X7Mp7apQWtL6T2sfSZzBcQNUqZB7tARFEm9gA9Tn9nbMdsvBB","permission":"active"},"weight":1}], "accounts" : [{"permission":{"actor":"acc2","permission":"active"},"weight":50}]}' owner
```

This second example modifies the same account permission, but removes the _key_ set in the last example, and grants active authority of the **@test** account to another _account_.

```bash
$ cleos set account permission test active '{"threshold" : 1, "keys" : [], "accounts" : [{"permission":{"actor":"sandwich","permission":"active"},"weight":1},{"permission":{"actor":"acc1","permission":"active"},"weight":50}]}' owner
```

The third example demonstrates how to setup permissions for multisig

```bash
$ cleos set account permission test active '{"threshold" : 100, "keys" : [{"permission":{"key":"EOS8X7Mp7apQWtL6T2sfSZzBcQNUqZB7tARFEm9gA9Tn9nbMdsvBB","permission":"active"},"weight":25}], "accounts" : [{"permission":{"actor":"@sandwich","permission":"active"},"weight":75}]}' owner
```

The JSON object used in this command is actually composed of two different types of objects

_The authority JSON object ..._

```javascript
{
  "threshold"       : 100,    /*An integer that defines cumulative signature weight required for authorization*/
  "keys"            : [],     /*An array made up of individual permissions defined with an EOS PUBLIC KEY*/
  "accounts"        : []      /*An array made up of individual permissions defined with an EOS ACCOUNT*/
}
```

_...which includes one or more permissions objects_

```javascript
/*Set Permission with Key*/
{
  "permission" : {
    "key"           : "EOS8X7Mp7apQWtL6T2sfSZzBcQNUqZB7tARFEm9gA9Tn9nbMdsvBB",
    "permission"    : "active"
  },
  weight            : 25      /*Set the weight of a signature from this permission*/
}

/*Set Permission with Account*/
{
  "permission" : {
    "actor"       : "sandwich",
    "permission"  : "active"
  },
  weight            : 75      /*Set the weight of a signature from this permission*/
}
```

_You can read more about some practical uses of permissions [here]()_

### Deploy Contract

To upload a contract to the blockchain, you must first hold an account and a wallet that holds the account unlocked.
Secondly, you will need your contract files (.wast) and its abi (.abi).
Then you may proceed with setting the code.

```
$ cleos set contract currency ../../../contracts/currency/currency.wast ../../../contracts/currency/currency.abi
Reading WAST...
Assembling WASM...
Publishing contract...
{
  "transaction_id": "9990306e13f630a9c5436a5a0b6fb8fe2c7f3da2f342b4898a39c4a2c17dcdb3",
  "processed": {
    "refBlockNum": 1208,
    "refBlockPrefix": 3058534156,
    "expiration": "2017-08-24T18:29:52",
    "scope": [
      "currency",
      "eos"
    ],
    "signatures": [],
    "messages": [{
        "code": "eos",
        "type": "setcode",
        "authorization": [{
            "account": "currency",
            "permission": "active"
          }
        ],
        "data": "00000079b822651d0000e8150061736d0100000001390a60017e0060037e7e7f017f60047e7e7f7f017f60017f0060057e7e7e7f7f017f60027f7f0060027f7f017f60027e7f0060000060027e7e00029d010a03656e7606617373657274000503656e76086c6f61645f693634000403656e76067072696e7469000003656e76067072696e746e000003656e76067072696e7473000303656e760b726561644d657373616765000603656e760a72656d6f76655f693634000103656e760b7265717569726541757468000003656e760d726571756972654e6f74696365000003656e760973746f72655f6936340002030706000007030809040401700000050301000107cc0107066d656d6f72790200205f5a4e33656f733133726571756972654e6f74696365454e535f344e616d6545000a1e5f5a4e33656f7331317265717569726541757468454e535f344e616d6545000b345f5a4e3863757272656e6379313273746f72654163636f756e74454e33656f73344e616d6545524b4e535f374163636f756e7445000c355f5a4e3863757272656e637932336170706c795f63757272656e63795f7472616e7366657245524b4e535f385472616e7366657245000d04696e6974000e056170706c79000f0a9d0d060600200010080b0600200010070b3400024020012903084200510d0020004280808080a8d7bee3082001411010091a0f0b20004280808080a8d7bee308200110061a0b8a0604017e027f047e017f4100410028020441206b2208360204200029030821052000290300210720002903102104411010042004100241c000100442808080c887d7c8b21d100341d00010042007100341e000100420051003200029030021052000290308100820051008200029030010072000290300210142002105423b210441f00021034200210603400240024002400240024020054206560d0020032c00002202419f7f6a41ff017141194b0d01200241a0016a21020c020b420021072005420b580d020c030b200241ea016a41002002414f6a41ff01714105491b21020b2002ad42388642388721070b2007421f83200442ffffffff0f838621070b200341016a2103200542017c2105200720068421062004427b7c2204427a520d000b420021052008420037031820082006370310200142808080c887d7c8b21d4280808080a8d7bee308200841106a411010011a200041086a2903002101423b210441f00021034200210603400240024002400240024020054206560d0020032c00002202419f7f6a41ff017141194b0d01200241a0016a21020c020b420021072005420b580d020c030b200241ea016a41002002414f6a41ff01714105491b21020b2002ad42388642388721070b2007421f83200442ffffffff0f838621070b200341016a2103200542017c2105200720068421062004427b7c2204427a520d000b2008200637030020084200370308200142808080c887d7c8b21d4280808080a8d7bee3082008411010011a200841186a2203290300200041106a22022903005a418001100020032003290300200229030022057d370300200520082903087c20055a41b00110002008200829030820022903007c370308200029030021050240024020032903004200510d0020054280808080a8d7bee308200841106a411010091a0c010b20054280808080a8d7bee308200841106a10061a0b200041086a290300210502400240200841086a2903004200510d0020054280808080a8d7bee3082008411010091a0c010b20054280808080a8d7bee308200810061a0b4100200841206a3602040b980303027f057e017f4100410028020441106b220736020442002103423b210241e00121014200210403400240024002400240024020034207560d0020012c00002200419f7f6a41ff017141194b0d01200041a0016a21000c020b420021052003420b580d020c030b200041ea016a41002000414f6a41ff01714105491b21000b2000ad42388642388721050b2005421f83200242ffffffff0f838621050b200141016a2101200342017c2103200520048421042002427b7c2202427a520d000b42002103423b210241f00021014200210603400240024002400240024020034206560d0020012c00002200419f7f6a41ff017141194b0d01200041a0016a21000c020b420021052003420b580d020c030b200041ea016a41002000414f6a41ff01714105491b21000b2000ad42388642388721050b2005421f83200242ffffffff0f838621050b200141016a2101200342017c2103200520068421062002427b7c2202427a520d000b2007428094ebdc033703082007200637030020044280808080a8d7bee3082007411010091a4100200741106a3602040bb10303027f047e017f4100410028020441206b220836020442002105423b210441e00121034200210603400240024002400240024020054207560d0020032c00002202419f7f6a41ff017141194b0d01200241a0016a21020c020b420021072005420b580d020c030b200241ea016a41002002414f6a41ff01714105491b21020b2002ad42388642388721070b2007421f83200442ffffffff0f838621070b200341016a2103200542017c2105200720068421062004427b7c2204427a520d000b024020062000520d0042002105423b210441f00121034200210603400240024002400240024020054207560d0020032c00002202419f7f6a41ff017141194b0d01200241a0016a21020c020b420021072005420b580d020c030b200241ea016a41002002414f6a41ff01714105491b21020b2002ad42388642388721070b2007421f83200442ffffffff0f838621070b200341016a2103200542017c2105200720068421062004427b7c2204427a520d000b20062001520d00200842003703102008420037030820084200370318200841086a4118100541174b4180021000200841086a100d0b4100200841206a3602040b0bff010b0041040b04200500000041100b2254686973206170706561727320746f2062652061207472616e73666572206f6620000041c0000b0220000041d0000b072066726f6d20000041e0000b0520746f20000041f0000b086163636f756e7400004180010b2c696e746567657220756e646572666c6f77207375627472616374696e6720746f6b656e2062616c616e6365000041b0010b26696e7465676572206f766572666c6f7720616464696e6720746f6b656e2062616c616e6365000041e0010b0963757272656e6379000041f0010b097472616e7366657200004180020b1e6d6573736167652073686f72746572207468616e2065787065637465640000fd02046e616d651006617373657274020000086c6f61645f693634050000000000067072696e74690100067072696e746e0100067072696e747301000b726561644d6573736167650200000a72656d6f76655f693634030000000b726571756972654175746801000d726571756972654e6f7469636501000973746f72655f6936340400000000205f5a4e33656f733133726571756972654e6f74696365454e535f344e616d65450101301e5f5a4e33656f7331317265717569726541757468454e535f344e616d6545010130345f5a4e3863757272656e6379313273746f72654163636f756e74454e33656f73344e616d6545524b4e535f374163636f756e74450201300131355f5a4e3863757272656e637932336170706c795f63757272656e63795f7472616e7366657245524b4e535f385472616e73666572450901300131013201330134013501360137013804696e69740801300131013201330134013501360137056170706c7909013001310132013301340135013601370138010b4163636f756e744e616d65044e616d6502087472616e7366657200030466726f6d0b4163636f756e744e616d6502746f0b4163636f756e744e616d65087175616e746974790655496e743634076163636f756e740002036b65790655496e7436340762616c616e63650655496e74363401000000b298e982a4087472616e736665720100000080bafac608076163636f756e74"
      }
    ],
    "output": [{
        "notify": [],
        "sync_transactions": [],
        "async_transactions": []
      }
    ]
  }
}
```

### Query Contract ABI

We can query the blockchain for the .abi of the contract, on which we can check the list of available actions and their respective message structure

```
$ cleos get code -a currency.abi currency
code hash: 9b9db1a7940503a88535517049e64467a6e8f4e9e03af15e9968ec89dd794975
saving abi to currency.abi
$ cat currency.abi
{
  "types": [{
      "newTypeName": "AccountName",
      "type": "Name"
    }
  ],
  "structs": [{
      "name": "transfer",
      "": "",
      "fields": {
        "from": "AccountName",
        "to": "AccountName",
        "amount": "UInt64"
      }
    },{
      "name": "account",
      "": "",
      "fields": {
        "account": "Name",
        "balance": "UInt64"
      }
    }
  ],
  "actions": [{
      "action": "transfer",
      "type": "transfer"
    }
  ],
  "tables": [{
      "table": "account",
      "indextype": "i64",
      "keynames": [
        "account"
      ],
      "keytype": [],
      "type": "account"
    }
  ]
}
```

### Push Message to Contract

A message should be contract according to the contract ABI.

For example, the ABI of the currency contract is contructed as follow.

```
$ cleos get code -a currency.abi currency
code hash: 9b9db1a7940503a88535517049e64467a6e8f4e9e03af15e9968ec89dd794975
saving abi to currency.abi
$ cat currency.abi
{
  "types": [{
      "newTypeName": "AccountName",
      "type": "Name"
    }
  ],
  "structs": [{
      "name": "transfer",
      "": "",
      "fields": {
        "from": "AccountName",
        "to": "AccountName",
        "amount": "UInt64"
      }
    },{
      "name": "account",
      "": "",
      "fields": {
        "account": "Name",
        "balance": "UInt64"
      }
    }
  ],
  "actions": [{
      "action": "transfer",
      "type": "transfer"
    }
  ],
  "tables": [{
      "table": "account",
      "indextype": "i64",
      "keynames": [
        "account"
      ],
      "keytype": [],
      "type": "account"
    }
  ]
}
```

From the above abi, we can see that currency contract accepts an action called transfer that accepts message with fields from, to, and amount.

```
$ ./cleos push message currency transfer '{"from":"currency","to":"tester","amount":50}' -S currency -S tester -p currency@active
1589302ms thread-0   main.cpp:271                  operator()           ] Converting argument to binary...
1589304ms thread-0   main.cpp:290                  operator()           ] Transaction result:
{
  "transaction_id": "1c4911c0b277566dce4217edbbca0f688f7bdef761ed445ff31b31f286720057",
  "processed": {
    "refBlockNum": 1173,
    "refBlockPrefix": 2184027244,
    "expiration": "2017-08-24T18:28:07",
    "scope": [
      "currency",
      "tester"
    ],
    "signatures": [],
    "messages": [{
        "code": "currency",
        "type": "transfer",
        "authorization": [{
            "account": "currency",
            "permission": "active"
          }
        ],
        "data": {
          "from": "currency",
          "to": "tester",
          "quantity": 50
        },
        "hex_data": "00000079b822651d00000000c84267a13200000000000000"
      }
    ],
    "output": [{
        "notify": [{
            "name": "tester",
            "output": {
              "notify": [],
              "sync_transactions": [],
              "async_transactions": []
            }
          }
        ],
        "sync_transactions": [],
        "async_transactions": []
      }
    ]
  }
}
```

### Querying Contract

Depending on the table structure defines in the contract, you can query the data within the contract.

For example, the currency contract ABI contains the account table.

```
$ cleos get code -a currency.abi currency
code hash: 9b9db1a7940503a88535517049e64467a6e8f4e9e03af15e9968ec89dd794975
saving abi to currency.abi
$ cat currency.abi
{
  ...
  "tables": [{
      "table": "account",
      "indextype": "i64",
      "keynames": [
        "account"
      ],
      "keytype": [],
      "type": "account"
    }
  ]
}
```

You can query the table specifying the necessary fields.

```
$ cleos get table inita currency account
{
  "rows": [{
      "account": "account",
      "balance": 50
    }
  ],
  "more": true
}
```

### Skip signatures

If you have nodeos running in your local, as an easy way for developers to test functionality without dealing with keys, nodeos can be run so that Transaction signatures are not required.

```
$ nodeos --skip-transaction-signatures
```

And then for any operation that requires signing, use the -s option

```
$ cleos ${command} ${subcommand} -s ${param}
```

### Using Separate Wallet App

Instead of using the wallet functionality built-in to nodeos, you can also use a separate wallet app which can be found inside programs/keosd. By default, port 8888 is used by nodeos, so choose another port for the wallet app.

```
$ keosd --http-server-endpoint 127.0.0.1:8887
```

Then for any operation that requires signing, use the –wallet-host and –wallet-port option

```
$ cleos —-wallet-url 127.0.0.1:8887 ${command} ${subcommand} ${param}
```

## Error Examples

```
status_code == 200: Error
: 10 assert_exception: Assert Exception
test: assertion failed: integer underflow subtracting token balance
    {"s":"integer underflow subtracting token balance","ptr":176}
    thread-1  wasm_interface.cpp:248 assertnonei32i32
[...snipped...]
```
