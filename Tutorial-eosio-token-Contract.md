## Eosio.token, Exchange, and Eosio.msig Contracts
**This tutorial assumes that you have completed the tutorial [Getting Started With Contracts](Tutorial-Getting-Started-With-Contracts).**

At this stage the blockchain doesn't do much, so let's deploy the `eosio.token` contract.  This contract enables the creation of many different tokens all running on the same contract but potentially managed by different users. 

Before we can deploy the token contract we must create an account to deploy it to.

```
$ cleos create account eosio eosio.token EOS7ijWCBmoXBi3CgtK7DJxentZZeTkeUnaSDvyro9dq7Sd1C3dC4 EOS7ijWCBmoXBi3CgtK7DJxentZZeTkeUnaSDvyro9dq7Sd1C3dC4
...
```

Then we can deploy the contract which can be found in `${EOSIO_SOURCE}/build/contracts/eosio.token`

```
$ cleos set contract eosio.token build/contracts/eosio.token -p eosio.token
Reading WAST...
Assembling WASM...
Publishing contract...
executed transaction: 528bdbce1181dc5fd72a24e4181e6587dace8ab43b2d7ac9b22b2017992a07ad  8708 bytes  10000 cycles
#         eosio <= eosio::setcode               {"account":"eosio.token","vmtype":0,"vmversion":0,"code":"0061736d0100000001ce011d60067f7e7f7f7f7f00...
#         eosio <= eosio::setabi                {"account":"eosio.token","abi":{"types":[],"structs":[{"name":"transfer","":"","fields":[{"name"...
```

### Create the EOS Token

You can view the interface to `eosio.token` as defined by `contracts/eosio.token/eosio.token.hpp`:
```
   void create( account_name issuer,
                asset        maximum_supply,
                uint8_t      can_freeze,
                uint8_t      can_recall,
                uint8_t      can_whitelist );


   void issue( account_name to, asset quantity, string memo );

   void transfer( account_name from,
                  account_name to,
                  asset        quantity,
                  string       memo );
```

To create a new token we must call the `create(...)` action with the proper arguments. This command will use the symbol of the maximum supply to uniquely identify this token from other tokens. The issuer will be the one with authority to call issue and or perform other actions such as freezing, recalling, and whitelisting of owners.

The concise way to call this method, using positional arguments:
```
$ cleos push action eosio.token create '[ "eosio", "1000000000.0000 EOS", 0, 0, 0]' -p eosio.token
executed transaction: 0e49a421f6e75f4c5e09dd738a02d3f51bd18a0cf31894f68d335cd70d9c0e12  260 bytes  1000 cycles
#   eosio.token <= eosio.token::create          {"issuer":"eosio","maximum_supply":"1000000000.0000 EOS","can_freeze":0,"can_recall":0,"can_whitelis...
```

Alternatively, a more verbose way to call this method, using named arguments:

```
$ cleos push action eosio.token create '{"issuer":"eosio", "maximum_supply":"1000000000.0000 EOS", "can_freeze":0, "can_recall":0, "can_whitelist":0}' -p eosio.token
executed transaction: 0e49a421f6e75f4c5e09dd738a02d3f51bd18a0cf31894f68d335cd70d9c0e12  260 bytes  1000 cycles
#   eosio.token <= eosio.token::create          {"issuer":"eosio","maximum_supply":"1000000000.0000 EOS","can_freeze":0,"can_recall":0,"can_whitelis...
```


This command created a new token `EOS` with a pecision of 4 decimials and a maximum supply of 1000000000.0000 EOS. 

In order to create this token we required the permission of the `eosio.token` contract because it "owns" the symbol namespace (e.g. "EOS"). Future versions of this contract may allow other parties to buy symbol names automatically.  For this reason we must pass `-p eosio.token` to authorize this call.

### Issue Tokens to Account "User"

Now that we have created the token, the issuer can issue new tokens to the account `user` we created earlier. 

We will use the positional calling convention (vs named args).

```
$ cleos push action eosio.token issue '[ "user", "100.0000 EOS", "memo" ]' -p eosio
executed transaction: 822a607a9196112831ecc2dc14ffb1722634f1749f3ac18b73ffacd41160b019  268 bytes  1000 cycles
#   eosio.token <= eosio.token::issue           {"to":"user","quantity":"100.0000 EOS","memo":"memo"}
>> issue
#   eosio.token <= eosio.token::transfer        {"from":"eosio","to":"user","quantity":"100.0000 EOS","memo":"memo"}
>> transfer
#         eosio <= eosio.token::transfer        {"from":"eosio","to":"user","quantity":"100.0000 EOS","memo":"memo"}
#          user <= eosio.token::transfer        {"from":"eosio","to":"user","quantity":"100.0000 EOS","memo":"memo"}
```

This time the output contains several different actions:  one issue and three transfers.  While the only action we signed was `issue`, the `issue` action performed an "inline transfer" and the "inline transfer" notified the sender and receiver accounts.  The output indicates all of the action handlers that were called, the order they were called in, and whether or not any output was generated by the action.

Technically, the `eosio.token` contract could have skipped the `inline transfer` and opted to just modify the balances directly.  However, in this case, the `eosio.token` contract is following our token convention that requires that all account balances be derivable by the sum of the transfer actions that reference them.  It also requires that the sender and receiver of funds be notified so they can automate handling deposits and withdrawals. 

If you want to see the actual transaction that was broadcast, you can use the `-d -j` options to indicate "don't broadcast" and "return transaction as json".

```
$ cleos push action eosio.token issue '["user", "100.0000 EOS", "memo"]' -p eosio -d -j
{
  "expiration": "2018-04-01T15:20:44",
  "region": 0,
  "ref_block_num": 42580,
  "ref_block_prefix": 3987474256,
  "net_usage_words": 21,
  "kcpu_usage": 1000,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "eosio.token",
      "name": "issue",
      "authorization": [{
          "actor": "eosio",
          "permission": "active"
        }
      ],
      "data": "00000000007015d640420f000000000004454f5300000000046d656d6f"
    }
  ],
  "signatures": [
    "EOSJzPywCKsgBitRh9kxFNeMJc8BeD6QZLagtXzmdS2ib5gKTeELiVxXvcnrdRUiY3ExP9saVkdkzvUNyRZSXj2CLJnj7U42H"
  ],
  "context_free_data": []
}
```

### Transfer Tokens to Account "Tester"

Now that account `user` has tokens, we will transfer some to account `tester`.  We indicate that `user` authorized this action using the permission argument `-p user`.

```
$ cleos push action eosio.token transfer '[ "user", "tester", "25.0000 EOS", "m" ]' -p user
executed transaction: 06d0a99652c11637230d08a207520bf38066b8817ef7cafaab2f0344aafd7018  268 bytes  1000 cycles
#   eosio.token <= eosio.token::transfer        {"from":"user","to":"tester","quantity":"25.0000 EOS","memo":"m"}
>> transfer
#          user <= eosio.token::transfer        {"from":"user","to":"tester","quantity":"25.0000 EOS","memo":"m"}
#        tester <= eosio.token::transfer        {"from":"user","to":"tester","quantity":"25.0000 EOS","memo":"m"}
```

## Deploy Exchange Contract
Similar to the examples shown above, we can deploy the `exchange` contract.  The `exchange` contract provides capabilities to create and trade currency.  It is assumed this is being run from the root of the EOSIO source.

```
$ cleos create account eosio exchange  EOS7ijWCBmoXBi3CgtK7DJxentZZeTkeUnaSDvyro9dq7Sd1C3dC4 EOS7ijWCBmoXBi3CgtK7DJxentZZeTkeUnaSDvyro9dq7Sd1C3dC4
executed transaction: 4d38de16631a2dc698f1d433f7eb30982d855219e7c7314a888efbbba04e571c  364 bytes  1000 cycles
#         eosio <= eosio::newaccount            {"creator":"eosio","name":"exchange","owner":{"threshold":1,"keys":[{"key":"EOS7ijWCBmoXBi3CgtK7DJxe...

$ cleos set contract exchange build/contracts/exchange -p exchange
Reading WAST...
Assembling WASM...
Publishing contract...
executed transaction: 5a63b4de8a1da415590778f163c5ed26dc164c960185b20fd834c297cf7fa8f4  35172 bytes  10000 cycles
#         eosio <= eosio::setcode               {"account":"exchange","vmtype":0,"vmversion":0,"code":"0061736d0100000001f0023460067f7e7f7f7f7f00600...
#         eosio <= eosio::setabi                {"account":"exchange","abi":{"types":[{"new_type_name":"account_name","type":"name"}],"structs":[{"n...
```

## Deploy Eosio.msig Contract
The `eosio.msig` contract allows multiple parties to sign a single transaction asynchronously.  EOSIO has multi-signature (multisig) support at a  level, but it requires a synchronous side channel where data is ferried around and signed.  `Eosio.msig` is a more user friendly way of asynchronously proposing, approving and eventually publishing a transaction with multiple parties' consent.

The following steps can be used to deploy the `eosio.msig` contract. 
```
$ cleos create account eosio eosio.msig  EOS7ijWCBmoXBi3CgtK7DJxentZZeTkeUnaSDvyro9dq7Sd1C3dC4 EOS7ijWCBmoXBi3CgtK7DJxentZZeTkeUnaSDvyro9dq7Sd1C3dC4
#         eosio <= eosio::newaccount            {"creator":"eosio","name":"eosio.msig","owner":{"threshold":1,"keys":[{"key":"EOS7ijWCBmoXBi3CgtK7DJ...
  
$ cleos set contract eosio.msig build/contracts/eosio.msig -p eosio.msig
Reading WAST...
Assembling WASM...
Publishing contract...
executed transaction: a113a7db8c878dfd894671792770b59a04efb3aa8295f5b3d585daf89c314ec9  8964 bytes  10000 cycles
#         eosio <= eosio::setcode               {"account":"eosio.msig","vmtype":0,"vmversion":0,"code":"0061736d0100000001bd011b60047f7e7e7f0060047...
#         eosio <= eosio::setabi                {"account":"eosio.msig","abi":{"types":[{"new_type_name":"account_name","type":"name"},{"new_type_na...
```

## Next Step - Hello World Tutorial
Please proceed to the [Hello World Tutorial](Tutorial-Hello-World-Contract).
