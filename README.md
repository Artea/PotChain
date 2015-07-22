## API README

PotChain provides a REST API at `http://potchain.net/api`

The end-points are:

### Block
```
  /api/block/[:hash]
  /api/block/00000000a967199a2fad0877433c93df785a8d8ce062e5f9b451cd1397bdbf62
```
### Block index
Get block hash by height
```
  /api/block-index/[:height]
  /api/block-index/0
```
This would return:
```
{"blockHash":"000000000019d6689c085ae165831e934ff763ae46a2a6c172b3f1b60a8ce26f"}
```
which is the hash of the Genesis block (0 height)

### Transaction
```
  /api/tx/[:txid]
  /api/tx/525de308971eabd941b139f46c7198b5af9479325c2395db7f2fb5ae8562556c
  /api/raw/[:rawid]
  /api/raw/525de308971eabd941b139f46c7198b5af9479325c2395db7f2fb5ae8562556c
```
### Address
```
  /api/addr/[:addr][?noTxList=1&noCache=1]
  /api/addr/PmvP3mTe53qxHdPqXEvdu8WdC7GfQ2vmx5?noTxList=1
```
### Address Properties
```
  /api/addr/[:addr]/balance
  /api/addr/[:addr]/totalReceived
  /api/addr/[:addr]/totalSent
  /api/addr/[:addr]/unconfirmedBalance
```
The response contains the value in Satoshis.
### Unspent Outputs
```
  /api/addr/[:addr]/utxo[?noCache=1]
```
Sample return:
``` json
[
    {
      "address": "P2PuaAguxZqLddRbTnAoAuwKYgN2w2hZk7",
      "txid": "dbfdc2a0d22a8282c4e7be0452d595695f3a39173bed4f48e590877382b112fc",
      "vout": 0,
      "ts": 1401276201,
      "scriptPubKey": "76a914e50575162795cd77366fb80d728e3216bd52deac88ac",
      "amount": 0.001,
      "confirmations": 3
    },
    {
      "address": "P2PuaAguxZqLddRbTnAoAuwKYgN2w2hZk7",
      "txid": "e2b82af55d64f12fd0dd075d0922ee7d6a300f58fe60a23cbb5831b31d1d58b4",
      "vout": 0,
      "ts": 1401226410,
      "scriptPubKey": "76a914e50575162795cd77366fb80d728e3216bd52deac88ac",
      "amount": 0.001,
      "confirmation": 6,
      "confirmationsFromCache": true
    }
]
```

### Unspent Outputs for multiple addresses
GET method:
```
  /api/addrs/[:addrs]/utxo
  /api/addrs/P2PuaAguxZqLddRbTnAoAuwKYgN2w2hZk7,PV9hdaAgfdhj6dfjHhdjHGgfdhkld75w2hZk7/utxo
```

POST method:
```
  /api/addrs/utxo
```

POST params:
```
addrs: P2PuaAguxZqLddRbTnAoAuwKYgN2w2hZk7,PV9hdaAgfdhj6dfjHhdjHGgfdhkld75w2hZk7
```

### Transactions by Block
```
  /api/txs/?block=HASH
  /api/txs/?block=00000000fa6cf7367e50ad14eb0ca4737131f256fc4c5841fd3c3f140140e6b6
```
### Transactions by Address
```
  /api/txs/?address=ADDR
  /api/txs/?address=P2PuaAguxZqLddRbTnAoAuwKYgN2w2hZk7
```

### Transactions for multiple addresses
GET method:
```
  /api/addrs/[:addrs]/txs[?from=&to=]
  /api/addrs/P2PuaAguxZqLddRbTnAoAuwKYgN2w2hZk7,PV9hdaAgfdhj6dfjHhdjHGgfdhkld75w2hZk7/txs?from=0&to=20
```

POST method:
```
  /api/addrs/txs
```

POST params:
```
addrs: P2PuaAguxZqLddRbTnAoAuwKYgN2w2hZk7,PV9hdaAgfdhj6dfjHhdjHGgfdhkld75w2hZk7
from (optional): 0
to (optional): 20
```

Sample output:
```
{ totalItems: 100,
  from: 0,
  to: 20,
  items:
    [ { txid: '3e81723d069b12983b2ef694c9782d32fca26cc978de744acbc32c3d3496e915',
       version: 1,
       locktime: 0,
       vin: [Object],
       vout: [Object],
       blockhash: '00000000011a135e5277f5493c52c66829792392632b8b65429cf07ad3c47a6c',
       confirmations: 109367,
       time: 1393659685,
       blocktime: 1393659685,
       valueOut: 0.3453,
       size: 225,
       firstSeenTs: undefined,
       valueIn: 0.3454,
       fees: 0.0001 },
      { ... },
      { ... },
      ...
      { ... }
    ]
 }
```

### Transaction broadcasting
POST method:
```
  /api/tx/send
```
POST params:
```
  rawtx: "signed transaction as hex string"

  eg

  rawtx: 01000000017b1eabe0209b1fe794124575ef807057c77ada2138ae4fa8d6c4de0398a14f3f00000000494830450221008949f0cb400094ad2b5eb399d59d01c14d73d8fe6e96df1a7150deb388ab8935022079656090d7f6bac4c9a94e0aad311a4268e082a725f8aeae0573fb12ff866a5f01ffffffff01f0ca052a010000001976a914cbc20a7664f2f69e5355aa427045bc15e7c6c77288ac00000000

```
POST response:
```
  {
      txid: [:txid]
  }

  eg

  {
      txid: "c7736a0a0046d5a8cc61c8c3c2821d4d7517f5de2bc66a966011aaa79965ffba"
  }
```

### Historic blockchain data sync status
```
  /api/sync
```

### Live network p2p data sync status
```
  /api/peer
```

### Status of the potcoin network
```
  /api/status?q=xxx
```

Where "xxx" can be:

 * getInfo
 * getDifficulty
 * getTxOutSetInfo
 * getBestBlockHash
 * getLastBlockHash


### Utility methods
```
  /api/utils/estimatefee[?nbBlocks=2]
```
