JSON RPC 接口命令
================

`JSON <http://json.org/>` ( JavaScript Object Notation) ，是一种轻量级的数据交互格式.

`JSON-RPC <http://www.jsonrpc.org/specification>` 是一个无状态的轻量级远程过程调用（RPC）协议。 
这个协议定义了一套数据结构和围绕它们的规则处理。 它与传输过程无关，可以通过socket，HTTP或其它多种传输方式使用这些数据。
通常使用的数据格式是 JSON (`RFC
4627 <http://www.ietf.org/rfc/rfc4627.txt>`).

MOAC 系统中的基础链节点 VNODE 和应用链节点 SCS 都内置了JSON-RPC 接口来方便用户使用。
MOAC JSON-RPC 部分接口兼容 ETHEREUM JSON-RPC, 如下表所示：

+------------+------------+
| Chain3     | Web3.js    |
+============+============+
| chain3     | web3       |
+------------+------------+
| mc         | eth        |
+------------+------------+
| net        | net        |
+------------+------------+
| admin      | admin      |
+------------+------------+
| txpool     | txpool     |
+------------+------------+
| personal   | personal   |
+------------+------------+
| vnode      | n/a        |
+------------+------------+
| scs        | n/a        |
+------------+------------+

MOAC 和 ETHEREUM 相同的接口对象有net, admin, txpool 和personal. 此外，chain3 对象 mc 中的方法和 web3 中部分方法也可以互用。
MOAC 独有的两个 JSON-RPC 对象是 VNODE 和 SCS，分别对应VNODE的部分属性和SCS接口。

默认启动VNODE的时候，chain3，mc，和net是默认启动的，其它部分需要指定：
.. code:: bash

    moac --rpc --rpcapi "chain3, mc, net, txpool"

可以打开txpool的JSON-RPC命令接口。

JSON-RPC Endpoint
-----------------

默认 JSON-RPC 的接入为:

+----------+-------------------------+
| Client   | URL                     |
+==========+=========================+
| Go       | http://localhost:8545   |
+----------+-------------------------+

在VNODE和SCS监听节点启动时，可以使用 HTTP JSON-RPC 选项``--rpc`` 命令来开启：

--rpc: 启用HTTP的RPC服务，以便非本机访问该MOAC节点服务；

母链节点

.. code:: bash

    moac --rpc

VNODE 默认RPC接口的地址是 localhost，端口号是 8545，可以通过 rpcaddr 和 rpcport 选项进行修改：

.. code:: bash

    moac --rpc --rpcaddr <ip> --rpcport <portnumber>

VNODE和SCS的JSON-RPC接口默认使用同源的服务方式。如果要通过浏览器来访问 JSON-RPC 接口，需要允许CORS选项，否则访问无法成功：

.. code:: bash

    moac --rpc --rpccorsdomain "http://localhost:3000"

--rpcapi value: 指定RPC要开放的API服务，默认为"chain3,mc,net"，常用的一般还会配置比如personal,admin,debug,miner,txpool,db,shh等，但是因为RPC服务是明文传输，所以，如果使用personal的时候，要注意安全问题；


应用链节点

启用HTTP的RPC服务，以便非本机访问该MOAC节点服务

.. code:: bash

    scsserver --rpc

SCS 默认RPC接口的地址是 localhost，端口号是 8548，可以通过 rpcaddr 和 rpcport 选项进行修改：

.. code:: bash

    scsserver --rpc --rpcaddr <ip> --rpcport <portnumber>

如果要通过浏览器来访问 JSON-RPC 接口，需要允许CORS选项，否则访问无法成功：

.. code:: bash

    scsserver --rpc --rpccorsdomain "http://localhost:8548"

HEX value encoding
------------------

At present there are two key datatypes that are passed over JSON:
unformatted byte arrays and quantities. Both are passed with a hex
encoding, however with different requirements to formatting:

When encoding **QUANTITIES** (integers, numbers): encode as hex, prefix
with "0x", the most compact representation (slight exception: zero
should be represented as "0x0"). Examples: - 0x41 (65 in decimal) -
0x400 (1024 in decimal) - WRONG: 0x (should always have at least one
digit - zero is "0x0") - WRONG: 0x0400 (no leading zeroes allowed) -
WRONG: ff (must be prefixed 0x)

When encoding **UNFORMATTED DATA** (byte arrays, account addresses,
hashes, bytecode arrays): encode as hex, prefix with "0x", two hex
digits per byte. Examples: - 0x41 (size 1, "A") - 0x004200 (size 3,
":raw-latex:`\0`B:raw-latex:`\0`") - 0x (size 0, "") - WRONG: 0xf0f0f
(must be even number of digits) - WRONG: 004200 (must be prefixed 0x)

Currently `MOAC <https://github.com/MOACChain/moac-core/releases>`
VNODE server provides JSON-RPC communication over http and IPC (unix
socket Linux and OSX/named pipes on Windows). SCS server provides 
JSON-RPC communication over http only.

默认区块值
---------------------------

以下接口包含默认区块值。如果没有有效区块数值输入，则使用节点的最高区块高度：

-  :ref:`mc\_getBalance <mc_getbalance>`
-  :ref:`mc\_getCode <mc_getcode>`
-  :ref:`mc\_getTransactionCount <mc_gettransactioncount>`
-  :ref:`mc\_getStorageAt <mc_getstorageat>`
-  :ref:`mc\_call <mc_call>`

When requests are made that act on the state of moac, the last default
block parameter determines the height of the block.

The following options are possible for the defaultBlock parameter:

-  ``HEX String`` - an integer block number
-  ``String "earliest"`` for the earliest/genesis block
-  ``String "latest"`` - for the latest mined block
-  ``String "pending"`` - for the pending state/transactions

Curl 命令示例
------------

The curl options below might return a response where the node complains
about the content type, this is because the --data option sets the
content type to application/x-www-form-urlencoded . If your node does
complain, manually set the header by placing -H "Content-Type:
application/json" at the start of the call.

The examples assume a local MOAC node is running and connected to
testnet (network id = 101). The URL/IP & port combination is
localhost:8545 or '127.0.0.1', which must be the last argument given to
curl.

JSON-RPC 命令
----------------

-  chain3

   -  :ref:`chain3\_clientVersion <chain3_clientversion>`
   -  :ref:`chain3\_sha3 <chain3_sha3>`

-  net

   -  :ref:`net\_version <net_version>`
   -  :ref:`net\_peerCount <net_peercount>`
   -  :ref:`net\_listening <net_listening>`

-  mc

   -  :ref:`mc\_protocolVersion <mc_protocolversion>`
   -  :ref:`mc\_syncing <mc_syncing>`
   -  :ref:`mc\_coinbase <mc_coinbase>`
   -  :ref:`mc\_mining <mc_mining>`
   -  :ref:`mc\_hashrate <mc_hashrate>`
   -  :ref:`mc\_gasPrice <mc_gasprice>`
   -  :ref:`mc\_accounts <mc_accounts>`
   -  :ref:`mc\_blockNumber <mc_blocknumber>`
   -  :ref:`mc\_getBalance <mc_getbalance>`
   -  :ref:`mc\_getStorageAt <mc_getstorageat>`
   -  :ref:`mc\_getTransactionCount <mc_gettransactioncount>`
   -  :ref:`mc\_getBlockTransactionCountByHash <mc_getblocktransactioncountbyhash>`
   -  :ref:`mc\_getBlockTransactionCountByNumber <mc_getblocktransactioncountbynumber>`
   -  :ref:`mc\_getUncleCountByBlockHash <mc_getunclecountbyblockhash>`
   -  :ref:`mc\_getUncleCountByBlockNumber <mc_getunclecountbyblocknumber>`
   -  :ref:`mc\_getCode <mc_getcode>`
   -  :ref:`mc\_sign <mc_sign>`
   -  :ref:`mc\_sendTransaction <mc_sendtransaction>`
   -  :ref:`mc\_sendRawTransaction <mc_sendrawtransaction>`
   -  :ref:`mc\_call <mc_call>`
   -  :ref:`mc\_estimateGas <mc_estimategas>`
   -  :ref:`mc\_getBlockByHash <mc_getblockbyhash>`
   -  :ref:`mc\_getBlockByNumber <mc_getblockbynumber>`
   -  :ref:`mc\_getTransactionByHash <mc_gettransactionbyhash>`
   -  :ref:`mc\_getTransactionByBlockHashAndIndex <mc_gettransactionbyblockhashandindex>`
   -  :ref:`mc\_getTransactionByBlockNumberAndIndex <mc_gettransactionbyblocknumberandindex>`
   -  :ref:`mc\_getTransactionReceipt <mc_gettransactionreceipt>`
   -  :ref:`mc\_getUncleByBlockHashAndIndex <mc_getunclebyblockhashandindex>`
   -  :ref:`mc\_getUncleByBlockNumberAndIndex <mc_getunclebyblocknumberandindex>`
   -  :ref:`mc\_newFilter <mc_newfilter>`
   -  :ref:`mc\_newBlockFilter <mc_newblockfilter>`
   -  :ref:`mc\_newPendingTransactionFilter <mc_newpendingtransactionfilter>`
   -  :ref:`mc\_uninstallFilter <mc_uninstallfilter>`
   -  :ref:`mc\_getFilterChanges <mc_getfilterchanges>`
   -  :ref:`mc\_getFilterLogs <mc_getfilterlogs>`
   -  :ref:`mc\_getLogs <mc_getlogs>`
   -  :ref:`mc\_getWork <mc_getwork>`
   -  :ref:`mc\_submitWork <mc_submitwork>`

-  admin

   -  :ref:`admin\_addPeer <admin_addPeer>`
   -  :ref:`admin\_datadir <admin_datadir>`
   -  :ref:`admin\_lnodeInfo <admin_nodeInfo>`
   -  :ref:`admin\_peers <admin_peers>`
   -  :ref:`admin\_stopRPC <admin_stopRPC>`
   -  :ref:`admin\_startRPC <admin_startRPC>`
   -  :ref:`admin\_addSubnetP2P <admin_addSubnetP2P>`
   -  :ref:`admin\_removeSubnetP2P <admin_removeSubnetP2P>`
   -  :ref:`admin\_getSubnetP2PList <admin_getSubnetP2PList>`

-  txpool

   -  :ref:`txpool\_content <txpool_content>`
   -  :ref:`txpool\_status <txpool_status>`
   -  :ref:`txpool\_inspec <txpool_inspect>`

-  vnode

   -  :ref:`vnode\_address <vnode_address>`
   -  :ref:`vnode\_scsService <vnode_scsservice>`
   -  :ref:`vnode\_serviceCfg <vnode_servicecfg>`
   -  :ref:`vnode\_showToPublic <vnode_showtopublic>`
   -  :ref:`vnode\_vnodeIP <vnode_vnodeip>`

-  scs

   -  :ref:`scs_directcall<scs_directcall>`
   -  :ref:`scs_getblock<scs_getblock>`
   -  :ref:`scs_getBlockNumber <scs_getblocknumber>`
   -  :ref:`scs_getDappList <scs_getdapplist>`
   -  :ref:`scs_getDappState <scs_getdappstate>`
   -  :ref:`scs_getMicroChainInfo <scs_getmicrochaininfo>`
   -  :ref:`scs_getMicroChainList <scs_getmicrochainlist>`
   -  :ref:`scs\_getNonce <scs_getnonce>`
   -  :ref:`scs\_getSCSId <scs_getscsid>`
   -  :ref:`scs\_getTransactionByHash <scs_gettransactionbyhash>`
   -  :ref:`scs\_getTransactionByNonce <scs_gettransactionbynonce>`
   -  :ref:`scs\_getReceiptByHash <scs_getreceiptbyhash>`
   -  :ref:`scs\_getReceiptByNonce <scs_getreceiptbynonce>`
   -  :ref:`scs\_getExchangeByAddress <scs_getexchangebyaddress>`
   -  :ref:`scs\_getExchangeInfo <scs_getexchangeinfo>`
   -  :ref:`scs\_getTxpool <scs_gettxpool>`

-----------

CHAIN3
''''''

.. _chain3_clientversion:

**Chain3_clientVersion**

Returns the current client version.

**Parameters**

none

**Returns**

``String`` - The current client version

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"chain3_clientVersion","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "jsonrpc":"2.0",
      "id":101,
      "result":"Moac/v0.8.3-release-ad500ab5/darwin-amd64/go1.10"
    }

--------------

**Chain3_sha3**

.. _chain3_sha3:

返回输入数据的 Keccak-256 (*not* the standardized SHA3-256) 哈希值（HASH）.

*Parameters*

1. ``DATA`` - the data to convert into a SHA3 hash

.. code:: js

    params: [
      "0x68656c6c6f20776f726c64"
    ]

*Returns*


``DATA`` - The SHA3 result of the given string.

*Example*

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"chain3_sha3","params":["0x68656c6c6f20776f726c64"],"id":101}' localhost:8545

    // Result
    {
      "jsonrpc":"2.0",
      "id":101,
      "result":"0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
    }

--------------

NET
'''

.. _net_version:

**net\_version**

返回当前网络 ID，MOAC网络主网为99， 测试网为101，本地调试私网默认为100.

*Parameters*


none

*Returns*


``String`` - The current network id. - ``"99"``: MOAC Mainnet -
``"101"``: MOAC Testnet - ``"100"``: Devnet

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "101"
    }

--------------

.. _net_listening:

**net\_listening**


Returns ``true`` if client is actively listening for network
connections.

*Parameters*

none

*Returns*


``Boolean`` - ``true`` when listening, otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"net_listening","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc":"2.0",
      "result":true
    }

--------------

.. _net_peerCount:

**net\_peerCount**

Returns number of peers currently connected to the client.

*Parameters*

none

*Returns*


``QUANTITY`` - integer of the number of connected peers.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":74,
      "jsonrpc": "2.0",
      "result": "0x4" // 4 net peers are connecting
    }

--------------

MC
''
.. _mc_protocolVersion:

**mc\_protocolVersion**

Returns the current MOAC protocol version.

*Parameters*

none

*Returns*


``String`` - The current MOAC protocol version

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_protocolVersion","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x3f"
    }

--------------

**mc\_syncing**

.. _mc_syncing:

Returns an object with data about the sync status or ``false``.

*Parameters*


none

*Returns*


``Object|Boolean``, An object with sync status data or ``FALSE``, when
not syncing: 

- ``startingBlock``: ``QUANTITY`` - The block at which the
import started (will only be reset, after the sync reached his head) -
``currentBlock``: ``QUANTITY`` - The current block, same as
mc\_blockNumber - ``highestBlock``: ``QUANTITY`` - The estimated highest
block

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_syncing","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": {
        startingBlock: '0x384',
        currentBlock: '0x386',
        highestBlock: '0x454'
      }
    }
    // Or when not syncing
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": false
    }

--------------

.. _mc_coinbase:

**mc\_coinbase**

Returns the client coinbase address.

*Parameters*


none

*Returns*


``DATA``, 20 bytes - the current coinbase address.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_coinbase","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x407d73d8a49eeb85d32cf465507dd71d507100c1"
    }

--------------

**mc\_mining**

.. _mc_mining:

Returns ``true`` if client is actively mining new blocks.

*Parameters*


none

*Returns*


``Boolean`` - returns ``true`` of the client is mining, otherwise
``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_mining","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": true
    }

--------------

.. _mc_hashrate:

**mc\_hashrate**

Returns the number of hashes per second that the node is mining with.

*Parameters*


none

*Returns*


``QUANTITY`` - number of hashes per second.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_hashrate","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x38a"
    }

--------------

.. _mc_gasprice:

**mc\_gasPrice**

Returns the current price per gas in sha.

*Parameters*


none

*Returns*


``QUANTITY`` - integer of the current gas price in sha.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_gasPrice","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x09184e72a000" // 10000000000000
    }

--------------

**mc\_accounts**

.. _mc_accounts:

Returns a list of addresses owned by client.

*Parameters*


none

*Returns*


``Array of DATA``, 20 Bytes - addresses owned by the client.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_accounts","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1","0x87dc9d8014e189b9d32c622a9ad1d02f72383979"]
    }

--------------

**mc\_blockNumber**

.. _mc_blockNumber:

Returns the number of most recent block.

*Parameters*


none

*Returns*


``QUANTITY`` - integer of the current block number the client is on.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_blockNumber","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":83,
      "jsonrpc": "2.0",
      "result": "0x4b7" // 1207
    }

--------------

**mc\_getBalance**

.. _mc_getBalance:

Returns the balance of the account of given address.

*Parameters*


1. ``DATA``, 20 Bytes - address to check for balance.
2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`

.. code:: js

    params: [
       '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
       'latest'
    ]

*Returns*


``QUANTITY`` - integer of the current balance in sha.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getBalance","params":["0x87dc9d8014e189b9d32c622a9ad1d02f72383979", "latest"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x12f8b3a319c000" // 5340000000000000
    }

--------------

**mc\_getStorageAt**

.. _mc_getStorageAt:

Returns the value from a storage position at a given address.

*Parameters*


1. ``DATA``, 20 Bytes - address of the storage.
2. ``QUANTITY`` - integer of the position in the storage.
3. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`

*Returns*


``DATA`` - the value at this storage position.

Example


Calculating the correct position depends on the storage to retrieve.
Consider the following contract deployed at
``0x02701dc451e126316ece6870fd70ea140efa7b15`` by address
``0xa8863fc8ce3816411378685223c03daae9770ebb``.

::

    contract Storage {
        uint pos0;
        mapping(address => uint) pos1;
        
        function Storage() {
            pos0 = 1234;
            pos1[msg.sender] = 5678;
        }
    }

Retrieving the value of pos0 is straight forward:

.. code:: js

    curl -X POST --data '{"jsonrpc":"2.0", "method": "mc_getStorageAt", "params": ["0x02701dc451e126316ece6870fd70ea140efa7b15", "0x0", "latest"], "id": 101}' localhost:8545

    {"jsonrpc":"2.0","id":1,"result":"0x00000000000000000000000000000000000000000000000000000000000004d2"}

Retrieving an element of the map is harder. The position of an element
in the map is calculated with:

.. code:: js

    keccack(LeftPad32(key, 0), LeftPad32(map position, 0))

This means to retrieve the storage on
pos1["0x391694e7e0b0cce554cb130d723a9d27458f9298"] we need to calculate
the position with:

.. code:: js

    keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))

The moac console which comes with the chain3 library can be used to make
the calculation:

.. code:: js

    > var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
    undefined
    > chain3.sha3(key, {"encoding": "hex"})
    "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"

Now to fetch the storage:

.. code:: js

    curl -X POST --data '{"jsonrpc":"2.0", "method": "mc_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"], "id": 101}' localhost:8545

    {"jsonrpc":"2.0","id":101,"result":"0x000000000000000000000000000000000000000000000000000000000000162e"}

--------------

**mc\_getTransactionCount**

.. _mc_getTransactionCount:

Returns the number of transactions *sent* from an address.

*Parameters*


1. ``DATA``, 20 Bytes - address.
2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`

.. code:: js

    params: [
       '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
       'latest' // state at the latest block
    ]

*Returns*


``QUANTITY`` - integer of the number of transactions send from this
address.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getTransactionCount","params":["0x57d83802a772adf506a89f5021c93a05749e3c6e","latest"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x29" // 41
    }

--------------

**mc\_getBlockTransactionCountByHash**

.. _mc_getBlockTransactionCountByHash:

Returns the number of transactions in a block from a block matching the
given block hash.

*Parameters*


1. ``DATA``, 32 Bytes - hash of a block

.. code:: js

    params: [
       '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
    ]

*Returns*


``QUANTITY`` - integer of the number of transactions in this block.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getBlockTransactionCountByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":101}' localhost:8545

    // Result
    {
      "id":1,
      "jsonrpc": "2.0",
      "result": "0xb" // 11
    }

--------------

**mc\_getBlockTransactionCountByNumber**

.. _mc_getBlockTransactionCountByNumber:

Returns the number of transactions in a block matching the given
block number.

*Parameters*


1. ``QUANTITY|TAG`` - integer of a block number, or the string
   ``"earliest"``, ``"latest"`` or ``"pending"``, as in the `default
   block parameter <#the-default-block-parameter>`.

.. code:: js

    params: [
       '0xe8', // 232
    ]

*Returns*


``QUANTITY`` - integer of the number of transactions in this block.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getBlockTransactionCountByNumber","params":["0xe8"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0xa" // 10
    }

--------------

**mc\_getUncleCountByBlockHash**

.. _mc_getUncleCountByBlockHash:

Returns the number of uncles in a block from a block matching the given
block hash.

*Parameters*


1. ``DATA``, 32 Bytes - hash of a block

.. code:: js

    params: [
       '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
    ]

*Returns*


``QUANTITY`` - integer of the number of uncles in this block.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getUncleCountByBlockHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x1" // 1
    }

--------------

**mc\_getUncleCountByBlockNumber**

.. _mc_getUncleCountByBlockNumber:

Returns the number of uncles in a block from a block matching the given
block number.

*Parameters*


1. ``QUANTITY|TAG`` - integer of a block number, or the string "latest",
   "earliest" or "pending", see the `default block
   parameter <#the-default-block-parameter>`

.. code:: js

    params: [
       '0xe8', // 232
    ]

*Returns*


``QUANTITY`` - integer of the number of uncles in this block.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getUncleCountByBlockNumber","params":["0xe8"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x1" // 1
    }

--------------

**mc\_getCode**

.. _mc_getCode:

*Returns* code at a given address.

*Parameters*


1. ``DATA``, 20 Bytes - address
2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`

.. code:: js

    params: [
       '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
       '0x2'  // 2
    ]

*Returns*


``DATA`` - the code from the given address.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
    }

--------------

**mc\_sign**

.. _mc_sign:

The sign method calculates an MOAC specific signature with:
``sign(keccak256("\x19MOAC Signed Message:\n" + len(message) + message)))``.

By adding a prefix to the message makes the calculated signature
recognisable as an MOAC specific signature. This prevents misuse where a
malicious DApp can sign arbitrary data (e.g. transaction) and use the
signature to impersonate the victim.

**Note** the address to sign with must be unlocked.

*Parameters*


account, message

1. ``DATA``, 20 Bytes - address
2. ``DATA``, N Bytes - message to sign

*Returns*


``DATA``: Signature

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_sign","params":["0x57d83802a772adf506a89f5021c93a05749e3c6e", "0xdeadbeaf"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0xefa354f816ab378a7da6ec25afe9e6393b72a3c06b2d08550815cc43d1993f6c58afbf228559df6a39ac419bef847ca2dc3047886df6a0f06a4c5e6a4e2dc9a61c"
    }

An example how to use solidity ecrecover to verify the signature
calculated with `:ref:`mc_sign`` can be found
`here <https://gist.github.com/bas-vk/d46d83da2b2b4721efb0907aecdb7ebd>`.
The contract is deployed on the testnet Ropsten and Rinkeby.

--------------

**mc\_sendTransaction**

.. _mc_sendTransaction:

Creates new message call transaction or a contract creation, if the data
field contains code.

**Note** the address to sign with must be unlocked.

*Parameters*


1. ``Object`` - The transaction object

-  ``from``: ``DATA``, 20 Bytes - The address the transaction is send
   from.
-  ``to``: ``DATA``, 20 Bytes - (optional when creating new contract)
   The address the transaction is directed to.
-  ``gas``: ``QUANTITY`` - (optional, default: 90000) Integer of the gas
   provided for the transaction execution. It will return unused gas.
-  ``gasPrice``: ``QUANTITY`` - (optional, default: To-Be-Determined)
   Integer of the gasPrice used for each paid gas
-  ``value``: ``QUANTITY`` - (optional) Integer of the value sent with
   this transaction
-  ``data``: ``DATA`` - The compiled code of a contract OR the hash of
   the invoked method signature and encoded parameters. MOAC used the 
   solidity ABI. For details see `Ethereum Contract
   ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`
-  ``nonce``: ``QUANTITY`` - (optional) Integer of a nonce. This allows
   to overwrite your own pending transactions that use the same nonce.

.. code:: js

    params: [{
      "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
      "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
      "gas": "0x76c0", // 30400
      "gasPrice": "0x9184e72a000", // 10000000000000
      "value": "0x9184e72a", // 2441406250
      "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
    }]

*Returns*


``DATA``, 32 Bytes - the transaction hash, or the zero hash if the
transaction is not yet available.

Use :ref:`mc\_getTransactionReceipt <mc_gettransactionreceipt>` to get the
contract address, after the transaction was mined, when you created a
contract.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_sendTransaction","params":[{see above}],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
    }

--------------

**mc\_sendRawTransaction**

.. _mc_sendRawTransaction:


Creates new message call transaction or a contract creation for signed
transactions.

*Parameters*


1. ``DATA``, The signed transaction data.

.. code:: js

    params: ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"]

*Returns*


``DATA``, 32 Bytes - the transaction hash, or the zero hash if the
transaction is not yet available.

Use :ref:`mc\_getTransactionReceipt <mc_gettransactionreceipt>` to get the
contract address, after the transaction was mined, when you created a
contract.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_sendRawTransaction","params":[{see above}],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
    }

--------------

**mc\_call**

.. _mc_call:

Executes a new message call immediately without creating a transaction
on the block chain.

*Parameters*


1. ``Object`` - The transaction call object

-  ``from``: ``DATA``, 20 Bytes - (optional) The address the transaction
   is sent from.
-  ``to``: ``DATA``, 20 Bytes - The address the transaction is directed
   to.
-  ``gas``: ``QUANTITY`` - (optional) Integer of the gas provided for
   the transaction execution. mc\_call consumes zero gas, but this
   parameter may be needed by some executions.
-  ``gasPrice``: ``QUANTITY`` - (optional) Integer of the gasPrice used
   for each paid gas
-  ``value``: ``QUANTITY`` - (optional) Integer of the value sent with
   this transaction
-  ``data``: ``DATA`` - (optional) Hash of the method signature and
   encoded parameters. For details see `Ethereum Contract
   ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`

2. ``QUANTITY|TAG`` - integer block number, or the string ``"latest"``,
   ``"earliest"`` or ``"pending"``, see the `default block
   parameter <#the-default-block-parameter>`

*Returns*


``DATA`` - the return value of executed contract.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_call","params":[{see above}],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x"
    }

--------------

**mc\_estimateGas**

.. _mc_estimateGas:

Generates and returns an estimate of how much gas is necessary to allow
the transaction to complete. The transaction will not be added to the
blockchain. Note that the estimate may be significantly more than the
amount of gas actually used by the transaction, for a variety of reasons
including EVM mechanics and node performance.

*Parameters*


See :ref:`mc\_call <mc_call>` parameters, expect that all properties are
optional. If no gas limit is specified moac uses the block gas limit
from the pending block as an upper bound. As a result the returned
estimate might not be enough to executed the call/transaction when the
amount of gas is higher than the pending block gas limit.

*Returns*


``QUANTITY`` - the amount of gas used.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_estimateGas","params":[{see above}],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x5208" // 21000
    }

--------------

**mc\_getBlockByHash**

.. _mc_getBlockByHash:

Returns information about a block by hash.

*Parameters*


1. ``DATA``, 32 Bytes - Hash of a block.
2. ``Boolean`` - If ``true`` it returns the full transaction objects, if
   ``false`` only the hashes of the transactions.

.. code:: js

    params: [
       '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
       true
    ]

*Returns*


``Object`` - A block object, or ``null`` when no block was found:

-  ``number``: ``QUANTITY`` - the block number. ``null`` when its
   pending block.
-  ``hash``: ``DATA``, 32 Bytes - hash of the block. ``null`` when its
   pending block.
-  ``parentHash``: ``DATA``, 32 Bytes - hash of the parent block.
-  ``nonce``: ``DATA``, 8 Bytes - hash of the generated proof-of-work.
   ``null`` when its pending block.
-  ``sha3Uncles``: ``DATA``, 32 Bytes - SHA3 of the uncles data in the
   block.
-  ``logsBloom``: ``DATA``, 256 Bytes - the bloom filter for the logs of
   the block. ``null`` when its pending block.
-  ``transactionsRoot``: ``DATA``, 32 Bytes - the root of the
   transaction trie of the block.
-  ``stateRoot``: ``DATA``, 32 Bytes - the root of the final state trie
   of the block.
-  ``receiptsRoot``: ``DATA``, 32 Bytes - the root of the receipts trie
   of the block.
-  ``miner``: ``DATA``, 20 Bytes - the address of the beneficiary to
   whom the mining rewards were given.
-  ``difficulty``: ``QUANTITY`` - integer of the difficulty for this
   block.
-  ``totalDifficulty``: ``QUANTITY`` - integer of the total difficulty
   of the chain until this block.
-  ``extraData``: ``DATA`` - the "extra data" field of this block.
-  ``size``: ``QUANTITY`` - integer the size of this block in bytes.
-  ``gasLimit``: ``QUANTITY`` - the maximum gas allowed in this block.
-  ``gasUsed``: ``QUANTITY`` - the total used gas by all transactions in
   this block.
-  ``timestamp``: ``QUANTITY`` - the unix timestamp for when the block
   was collated.
-  ``transactions``: ``Array`` - Array of transaction objects, or 32
   Bytes transaction hashes depending on the last given parameter.
-  ``uncles``: ``Array`` - Array of uncle hashes.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getBlockByHash","params":["0x9d88f2a2a348eec743b149f461fddcf0843d3920ddf998896672cee476b99127", true],"id":101}' localhost:8545

    // Result
    {
    "jsonrpc":"2.0",
    "id":101,
    "result":{
      "difficulty":"0x9ad5136",
      "extraData":"0xdd854d4f41432d85302e382e332d87676f312e382e338777696e646f7773",
      "gasLimit":"0x895440",
      "gasUsed":"0x0",
      "hash":"0x9d88f2a2a348eec743b149f461fddcf0843d3920ddf998896672cee476b99127",
      "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      "miner":"0x0a2168d2f08161c01745fec4e6e8fe06f314ab41",
      "mixHash":"0xd6e9f8d36f8f0f57abff5ccf6390ece4d5a14eb624779628fd6e6119451589e2",
      "nonce":"0x423e7acb4d05a23c",
      "number":"0x4803d",
      "parentHash":"0xc494dbda1ef4f673fd14fc487a1346a7747a41910d1a1f0e72d6e0e660c0e792",
      "receiptsRoot":"0x9287370eb27f11b0c2188431cbc58a23b685f02dbd851ed4d974f932bd780839",
      "sha3Uncles":"0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
      "size":"0x24e",
      "stateRoot":"0xff791a26fd0cb6addfe4bec0287b3905bf44898f0fd71e446cb1385f94333e18",
      "timestamp":"0x5b1f3a14",
      "totalDifficulty":"0x325dc638b38e",
      "transactions":[{"blockHash":"0x9d88f2a2a348eec743b149f461fddcf0843d3920ddf998896672cee476b99127","blockNumber":"0x4803d","from":"0x0000000000000000000000000000000000000064","gas":"0x0","gasPrice":"0x4a817c800","hash":"0x52ccd05bdfb137fcbb6154f1cb7c006ba53cd8ce80511841d2e7b3c6324d49df","input":"0xc1c0e9c4","nonce":"0x4803c","syscnt":"0x65","to":"0x0000000000000000000000000000000000000065","transactionIndex":"0x0","value":"0x0","v":"0x0","r":"0x0","s":"0x0","shardingFlag":"0x0"}],
      "transactionsRoot":"0xa29e25d8e3db000e44122a594cc52a7bf050850b3a7e028fd54cb83784fc1bc3",
      "uncles":[]
      }
    }

--------------

**mc\_getBlockByNumber**

.. _mc_getBlockByNumber:

Returns information about a block by block number.

*Parameters*


1. ``QUANTITY|TAG`` - integer of a block number, or the string
   ``"earliest"``, ``"latest"`` or ``"pending"``, as in the `default
   block parameter <#the-default-block-parameter>`.
2. ``Boolean`` - If ``true`` it returns the full transaction objects, if
   ``false`` only the hashes of the transactions.

.. code:: js

    params: [
       '0x1b4', // 436
       true
    ]

*Returns*


See :ref:`mc\_getBlockByHash <mc_getblockbyhash>`

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getBlockByNumber","params":["0x1b4", true],"id":101}' localhost:8545

Result see :ref:`mc\_getBlockByHash <mc_getblockbyhash>`

--------------

**mc\_getTransactionByHash**

.. _mc_getTransactionByHash:

Returns the information about a transaction requested by transaction
hash.

*Parameters*


1. ``DATA``, 32 Bytes - hash of a transaction

.. code:: js

    params: [
       "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
    ]

*Returns*


``Object`` - A transaction object, or ``null`` when no transaction was
found:

-  ``hash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``nonce``: ``QUANTITY`` - the number of transactions made by the
   sender prior to this one.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in. ``null`` when its pending.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in. ``null`` when its pending.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block. ``null`` when its pending.
-  ``from``: ``DATA``, 20 Bytes - address of the sender.
-  ``to``: ``DATA``, 20 Bytes - address of the receiver. ``null`` when
   its a contract creation transaction.
-  ``value``: ``QUANTITY`` - value transferred in Wei.
-  ``gasPrice``: ``QUANTITY`` - gas price provided by the sender in Wei.
-  ``gas``: ``QUANTITY`` - gas provided by the sender.
-  ``input``: ``DATA`` - the data send along with the transaction.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getTransactionByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":101}' localhost:8545

    // Result
    {
    "id":101,
    "jsonrpc":"2.0",
    "result": {
        "hash":"0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b",
        "nonce":"0x",
        "blockHash": "0xbeab0aa2411b7ab17f30a99d3cb9c6ef2fc5426d6ad6fd9e2a26a6aed1d1055b",
        "blockNumber": "0x15df", // 5599
        "transactionIndex":  "0x1", // 1
        "from":"0x407d73d8a49eeb85d32cf465507dd71d507100c1",
        "to":"0x85h43d8a49eeb85d32cf465507dd71d507100c1",
        "value":"0x7f110", // 520464
        "gas": "0x7f110", // 520464
        "gasPrice":"0x09184e72a000",
        "input":"0x603880600c6000396000f300603880600c6000396000f3603880600c6000396000f360",
      }
    }

--------------

**mc\_getTransactionByBlockHashAndIndex**

.. _mc_getTransactionByBlockHashAndIndex:

Returns information about a transaction by block hash and transaction
index position.

*Parameters*


1. ``DATA``, 32 Bytes - hash of a block.
2. ``QUANTITY`` - integer of the transaction index position.

.. code:: js

    params: [
       '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
       '0x0' // 0
    ]

*Returns*


See :ref:`mc\_getTransactionByHash <mc_gettransactionbyhash>`

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getTransactionByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":101}' localhost:8545

Result see :ref:`mc\_getTransactionByHash <mc_gettransactionbyhash>`

--------------

**mc\_getTransactionByBlockNumberAndIndex**

.. _mc_getTransactionByBlockNumberAndIndex:

Returns information about a transaction by block number and transaction
index position.

*Parameters*


1. ``QUANTITY|TAG`` - a block number, or the string ``"earliest"``,
   ``"latest"`` or ``"pending"``, as in the `default block
   parameter <#the-default-block-parameter>`.
2. ``QUANTITY`` - the transaction index position.

.. code:: js

    params: [
       '0x29c', // 668
       '0x0' // 0
    ]

*Returns*


See :ref:`mc\_getTransactionByHash <mc_gettransactionbyhash>`

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getTransactionByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":101}' localhost:8545

Result see :ref:`mc\_getTransactionByHash <mc_gettransactionbyhash>`

--------------

**mc\_getTransactionReceipt**

.. _mc_getTransactionReceipt:

Returns the receipt of a transaction by transaction hash.

**Note** That the receipt is not available for pending transactions.

*Parameters*


1. ``DATA``, 32 Bytes - hash of a transaction

.. code:: js

    params: [
       '0x7bb694c3462764cb113e9b742faaf06adc728e70b607f8b7aa95207ee32b1c5e'
    ]

*Returns*


``Object`` - A transaction receipt object, or ``null`` when no receipt
was found:

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.
-  ``cumulativeGasUsed``: ``QUANTITY`` - The total amount of gas used
   when this transaction was executed in the block.
-  ``gasUsed``: ``QUANTITY`` - The amount of gas used by this specific
   transaction alone.
-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.

It also returns *either* :

-  ``root`` : ``DATA`` 32 bytes of post-transaction stateroot (pre
   Byzantium)
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getTransactionReceipt","params":["0x7bb694c3462764cb113e9b742faaf06adc728e70b607f8b7aa95207ee32b1c5e"],"id":101}' localhost:8545

    // Result
    {
    {"jsonrpc":"2.0",
    "id":101,
    "result":{
    "blockHash":"0xad7d2eec610b3e04ea4676c11a3184cb07df43ed7ab717f331d3c952fc1b55cf",
    "blockNumber":"0x1b943",
    "contractAddress":"0xf2f4eec6c2adfcf780aae828de0b25f86506ffae",
    "cumulativeGasUsed":"0x1b337c",
    "from":"0x7312f4b8a4457a36827f185325fd6b66a3f8bb8b",
    "gasUsed":"0x1b337c",
    "logs":[],
    "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
    "status":"0x1",
    "to":null,
    "transactionHash":"0x7bb694c3462764cb113e9b742faaf06adc728e70b607f8b7aa95207ee32b1c5e",
    "transactionIndex":"0x1"}
    }

--------------

**mc\_getUncleByBlockHashAndIndex**

.. _mc_getUncleByBlockHashAndIndex:

Returns information about a uncle of a block by hash and uncle index
position.

*Parameters*


1. ``DATA``, 32 Bytes - hash a block.
2. ``QUANTITY`` - the uncle's index position.

.. code:: js

    params: [
       '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
       '0x0' // 0
    ]

*Returns*


See :ref:`mc\_getBlockByHash <mc_getblockbyhash>`

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getUncleByBlockHashAndIndex","params":["0x696d389aa9b6b44e08af9f3528c51587aac435b75a54ece42f4b2d1289043497", "0x0"],"id":101}' localhost:8545

Result see :ref:`mc\_getBlockByHash <mc_getblockbyhash>`

**Note**: An uncle doesn't contain individual transactions.

--------------

**mc\_getUncleByBlockNumberAndIndex**

.. _mc_getUncleByBlockNumberAndIndex:

Returns information about a uncle of a block by number and uncle index
position.

*Parameters*


1. ``QUANTITY|TAG`` - a block number, or the string ``"earliest"``,
   ``"latest"`` or ``"pending"``, as in the `default block
   parameter <#the-default-block-parameter>`.
2. ``QUANTITY`` - the uncle's index position.

.. code:: js

    params: [
       '0x29c', // 668
       '0x0' // 0
    ]

*Returns*


See :ref:`mc\_getBlockByHash <mc_getblockbyhash>`

**Note**: An uncle doesn't contain individual transactions.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getUncleByBlockNumberAndIndex","params":["0x29c", "0x0"],"id":101}' localhost:8545

Result see :ref:`mc\_getBlockByHash <mc_getblockbyhash>`

--------------

**mc\_newFilter**

.. _mc_newFilter:

Creates a filter object, based on filter options, to notify when the
state changes (logs). To check if the state has changed, call
:ref:`mc\_getFilterChanges <mc_getfilterchanges>`.

\*A note on specifying topic filters:*

Topics are order-dependent. A transaction with a log with topics [A, B]
will be matched by the following topic filters: \* ``[]`` "anything" \*
``[A]`` "A in first position (and anything after)" \* ``[null, B]``
"anything in first position AND B in second position (and anything
after)" \* ``[A, B]`` "A in first position AND B in second position (and
anything after)" \* ``[[A, B], [A, B]]`` "(A OR B) in first position AND
(A OR B) in second position (and anything after)"

*Parameters*


1. ``Object`` - The filter options:

-  ``fromBlock``: ``QUANTITY|TAG`` - (optional, default: ``"latest"``)
   Integer block number, or ``"latest"`` for the last mined block or
   ``"pending"``, ``"earliest"`` for not yet mined transactions.
-  ``toBlock``: ``QUANTITY|TAG`` - (optional, default: ``"latest"``)
   Integer block number, or ``"latest"`` for the last mined block or
   ``"pending"``, ``"earliest"`` for not yet mined transactions.
-  ``address``: ``DATA|Array``, 20 Bytes - (optional) Contract address
   or a list of addresses from which logs should originate.
-  ``topics``: ``Array of DATA``, - (optional) Array of 32 Bytes
   ``DATA`` topics. Topics are order-dependent. Each topic can also be
   an array of DATA with "or" options.

.. code:: js

    params: [{
      "fromBlock": "0x1",
      "toBlock": "0x2",
      "address": "0x8888f1f195afa192cfee860698584c030f4c9db1",
      "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", null, ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x0000000000000000000000000aff3454fce5edbc8cca8697c15331677e6ebccc"]]
    }]

*Returns*


``QUANTITY`` - A filter id.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_newFilter","params":[{"topics":["0x12341234"]}],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x1" // 1
    }

--------------

**mc\_newBlockFilter**

.. _mc_newBlockFilter:

Creates a filter in the node, to notify when a new block arrives. To
check if the state has changed, call
:ref:`mc\_getFilterChanges <mc_getfilterchanges>`.

*Parameters*


None

*Returns*


``QUANTITY`` - A filter id.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_newBlockFilter","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc":  "2.0",
      "result": "0x244afc5c2cb35b24c5b91bba7f7ab19d" // 1
    }

--------------

**mc\_newPendingTransactionFilter**

.. _mc_newPendingTransactionFilter:

Creates a filter in the node, to notify when new pending transactions
arrive. To check if the state has changed, call
:ref:`mc\_getFilterChanges <mc_getfilterchanges>`.

*Parameters*


None

*Returns*


``QUANTITY`` - A filter id.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_newPendingTransactionFilter","params":[],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc":  "2.0",
      "result": "0x1" // 1
    }

--------------

**mc\_uninstallFilter**

.. _mc_uninstallFilter:

Uninstalls a filter with given id. Should always be called when watch is
no longer needed. Additonally Filters timeout when they aren't requested
with :ref:`mc\_getFilterChanges <mc_getfilterchanges>` for a period of
time.

*Parameters*


1. ``QUANTITY`` - The filter id.

.. code:: js

    params: [
      "0xb" // 11
    ]

*Returns*


``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_uninstallFilter","params":["0xb"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": true
    }

--------------

**mc\_getFilterChanges**

.. _mc_getfilterchanges:

Polling method for a filter, which returns an array of logs which
occurred since last poll.

*Parameters*


1. ``QUANTITY`` - the filter id.

.. code:: js

    params: [
      "0x16" // 22
    ]

*Returns*


``Array`` - Array of log objects, or an empty array if nothing has
changed since last poll.

-  For filters created with `:ref:`mc_newBlockFilter`` the return are block
   hashes (``DATA``, 32 Bytes), e.g. ``["0x3454645634534..."]``.
-  For filters created with `:ref:`mc_newPendingTransactionFilter`` the
   return are transaction hashes (``DATA``, 32 Bytes), e.g.
   ``["0x6345343454645..."]``.
-  For filters created with `:ref:`mc_newFilter`` logs are objects with
   following params:

-  ``removed``: ``TAG`` - ``true`` when the log was removed, due to a
   chain reorganization. ``false`` if its a valid log.
-  ``logIndex``: ``QUANTITY`` - integer of the log index position in the
   block. ``null`` when its pending log.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position log was created from. ``null`` when its pending log.
-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transactions
   this log was created from. ``null`` when its pending log.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this log
   was in. ``null`` when its pending. ``null`` when its pending log.
-  ``blockNumber``: ``QUANTITY`` - the block number where this log was
   in. ``null`` when its pending. ``null`` when its pending log.
-  ``address``: ``DATA``, 20 Bytes - address from which this log
   originated.
-  ``data``: ``DATA`` - contains one or more 32 Bytes non-indexed
   arguments of the log.
-  ``topics``: ``Array of DATA`` - Array of 0 to 4 32 Bytes ``DATA`` of
   indexed log arguments. (In *solidity*: The first topic is the *hash*
   of the signature of the event (e.g.
   ``Deposit(address,bytes32,uint256)``), except you declared the event
   with the ``anonymous`` specifier.)

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getFilterChanges","params":["0x16"],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc":"2.0",
      "result": [{
        "logIndex": "0x1", // 1
        "blockNumber":"0x1b4", // 436
        "blockHash": "0x8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcfdf829c5a142f1fccd7d",
        "transactionHash":  "0xdf829c5a142f1fccd7d8216c5785ac562ff41e2dcfdf5785ac562ff41e2dcf",
        "transactionIndex": "0x0", // 0
        "address": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d",
        "data":"0x0000000000000000000000000000000000000000000000000000000000000000",
        "topics": ["0x59ebeb90bc63057b6515673c3ecf9438e5058bca0f92585014eced636878c9a5"]
        },{
          ...
        }]
    }

--------------

**mc\_getFilterLogs**

.. _mc_getFilterLogs:

Returns an array of all logs matching filter with given id.

*Parameters*


1. ``QUANTITY`` - The filter id.

.. code:: js

    params: [
      "0x16" // 22
    ]

*Returns*


See :ref:`mc\_getFilterChanges <mc_getfilterchanges>`

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getFilterLogs","params":["0x16"],"id":101}' localhost:8545

Result see :ref:`mc\_getFilterChanges <mc_getfilterchanges>`

--------------

**mc\_getLogs**

.. _mc_getLogs:

Returns an array of all logs matching a given filter object.

*Parameters*


1. ``Object`` - the filter object, see :ref:`mc\_newFilter
   parameters <mc_newfilter>`.

.. code:: js

    params: [{
      "topics": ["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]
    }]

*Returns*


See :ref:`mc\_getFilterChanges <mc_getfilterchanges>`

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getLogs","params":[{"topics":["0x000000000000000000000000a94f5374fce5edbc8e2a8697c15331677e6ebf0b"]}],"id":101}'

Result see :ref:`mc\_getFilterChanges <mc_getfilterchanges>`

--------------

**mc\_getWork**

.. _mc_getWork:

Returns the hash of the current block, the seedHash, and the boundary
condition to be met ("target").

*Parameters*

none

*Returns*


``Array`` - Array with the following properties: 

1. ``DATA``, 32 Bytes - current block header pow-hash 
2. ``DATA``, 32 Bytes - the seed hash used for the DAG. 
3. ``DATA``, 32 Bytes - the boundary condition ("target"), 2^256 / difficulty.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"mc_getWork","params":[],"id":101}' localhost:8545

    // Result
    {
    "jsonrpc":"2.0",
    "id":101,
    "result":["0x367ebe7ed77dde0a4cc134adaa769f93a833e08a86a0d38ab18e32cc3740b5f8","0x9b2baad7528ecec612c5751a6bd525905892d7892e155c3b05e61363154a940b","0x0000001dd67ebb66a2c0fc63b9907077f70b5315e1780a8b74f33d40e2a9a7e4"]
    }

--------------

**mc\_submitWork**

.. _mc_submitWork:

Used for submitting a proof-of-work solution.

*Parameters*


1. ``DATA``, 8 Bytes - The nonce found (64 bits)
2. ``DATA``, 32 Bytes - The header's pow-hash (256 bits)
3. ``DATA``, 32 Bytes - The mix digest (256 bits)

.. code:: js

    params: [
      "0x0000000000000001",
      "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
      "0xD1FE5700000000000000000000000000D1FE5700000000000000000000000000"
    ]

*Returns*

``Boolean`` - returns ``true`` if the provided solution is valid,
otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0", "method":"mc_submitWork", "params":["0x0000000000000001", "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef", "0xD1GE5700000000000000000000000000D1GE5700000000000000000000000000"],"id":101}'

    // Result
    {
      "id":101,
      "jsonrpc":"2.0",
      "result": true
    }

--------------

ADMIN
'''''

**admin\_addPeer**

.. _admin_addPeer:

The addPeer administrative method requests adding a new remote node to the list of tracked static nodes. The node will try to maintain connectivity to these nodes at all times, reconnecting every once in a while if the remote connection goes down.

The method accepts a single argument, the enode URL of the remote peer to start tracking and returns a BOOL indicating whether the peer was accepted for tracking or some error occurred.

*Parameters*

``string``: ``url``, 20 Bytes - address from which the VNODE settings
in the vnodeconfig.json file.

*Returns*

``Boolean`` - returns ``true`` if the operation is successfully, otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_addPeer", "params": ["enode://a979fb575495b8d6db44f750317d0f4622bf4c2aa3365d6af7c284339968eef29b69ad0dce72a4d8db5ebb4968de0e3bec910127f134779fbcb0cb6d3331163c@2.1.188.187:30333"]}' localhost:8545

    // Result
    true

--------------
**admin\_datadir**

.. _admin_datadir:

The datadir administrative property can be queried for the absolute path the running MOAC VNODE node currently uses to store all its databases.

*Parameters*

none

*Returns*

``string`` - returns the absolute path running the MOAC VNODE.

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_datadir", "params": [],"id":106}' localhost:8545

    // Result
    {"jsonrpc":"2.0","id":101,"result":"/Users/moac/src/github.com/innowells/moac-vnode/test101"}

--------------

**admin\_nodeInfo**

.. _admin_nodeInfo:

The nodeInfo administrative property can be queried for all the information known about the running MOAC VNODE at the networking granularity. These include general information about the node itself as a participant of the ÐΞVp2p P2P overlay protocol, as well as specialized information added by each of the running application protocols.

*Parameters*

none

*Returns*

``object`` - returns ``true`` if the operation is successfully, otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_nodeInfo", "params": [],"id":101}' localhost:8545

    // Result
    {"jsonrpc":"2.0","id":101,"result":{"id":"a6f486af99679e00ec1a2bf77304e8c7f183987c8138a7c515a08ee42c5bebbda9f01474d43ba7176f891989dbdb78a6cbade67a941e6c2d5a83751039adba36","name":"Moac/v1.0.11-rc-2b24668f/darwin-amd64/go1.13.1","enode":"enode://a6f486af99679e00ec1a2bf77304e8c7f183987c8138a7c515a08ee42c5bebbda9f01474d43ba7176f891989dbdb78a6cbade67a941e6c2d5a83751039adba36@71.166.46.131:30336?servicecfgport=:50062\u0026showtopublic=true\u0026beneficialaddress=0xD814F2ac2c4cA49b33066582E4e97EBae02F2aB9\u0026ip=","ip":"71.166.46.131","ports":{"discovery":30336,"listener":30336},"listenAddr":"[::]:30336","protocols":{"mc":{"network":101,"difficulty":1476985363965,"genesis":"0x4e2972df43453f5b658656de1f2af40866b6d86b4e11b0c49eb1fc1a854d9796","head":"0x941bb3a1c9a8a26e0bc2f747c2a7ea805135e1e995464957aa7e09814a1575d7"}}}}

--------------

**admin\_peers**

.. _admin_peers:

The addPeer administrative method requests adding a new remote node to the list of tracked static nodes. The node will try to maintain connectivity to these nodes at all times, reconnecting every once in a while if the remote connection goes down.

The method accepts a single argument, the enode URL of the remote peer to start tracking and returns a BOOL indicating whether the peer was accepted for tracking or some error occurred.

*Parameters*

none

*Returns*

``Boolean`` - returns ``true`` if the operation is successfully, otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_addPeer", "params": ["enode://a979fb575495b8d6db44f750317d0f4622bf4c2aa3365d6af7c284339968eef29b69ad0dce72a4d8db5ebb4968de0e3bec910127f134779fbcb0cb6d3331163c@2.1.188.187:30333"]}' localhost:8545

    // Result
    true

--------------

**admin\_startRPC**

.. _admin_startRPC:

The startRPC administrative method starts an HTTP based JSON RPC API webserver to handle client requests. All the parameters are optional:

host: network interface to open the listener socket on (defaults to "localhost")
port: network port to open the listener socket on (defaults to 8545)
cors: cross-origin resource sharing header to use (defaults to "")
apis: API modules to offer over this interface (defaults to "mc,net,chain3")

*Parameters*

``string``: ``url``, 20 Bytes - address from which the VNODE settings
in the vnodeconfig.json file.

*Returns*

``Boolean`` - returns ``true`` if the HTTP RPC listener was opened successfully, otherwise ``false``. Please note, only one HTTP endpoint is allowed to be active at any time. So this is a dump method, you can't use it :).

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_startRPC", "params": ["127.0.0.1",8546], "id":101}' localhost:8545

    // Result
    {"jsonrpc":"2.0","id":106,"error":{"code":-32000,"message":"HTTP RPC already running on 127.0.0.1:8545"}}

--------------

**admin\_stopRPC**

.. _admin_stopRPC:

The stopRPC administrative method closes the currently open HTTP RPC endpoint. As the node can only have a single HTTP endpoint running, this method takes no parameters, returning a boolean whether the endpoint was closed or not.

*Parameters*

 none

*Returns*

``Boolean`` - returns ``true`` if the operation is successfully, otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_stopRPC", }' localhost:8545

    // Result
    true

--------------

**admin\_addSubnetP2P**

.. _admin_addSubnetP2P:

The addSubnetP2P is adding the Vnode to subnet's dedicated p2p network
as a result, this node can join multiple p2p networks including the mainnet one.

*Parameters*

``string``: ``Address``, 20 Bytes - address of the AppChain/SubChain the VNODE need to connect.

``number``: ``Block number``, the block number that when the Vnode will join the network, suggest to be current blocknumber +100.

*Returns*

``null`` - returns ``null`` if the operation is successfully

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_addSubnetP2P", "params": ["0xD814F2ac2c4cA49b33066582E4e97EBae02F2aB9",1999], "id":101}' localhost:8545

    // Result
    null

--------------

**admin\_removeSubnetP2P**

.. _admin_removeSubnetP2P:

The removeSubnetP2P administrative method remove the Vnode from the P2P network of the AppChain.

*Parameters*

``string``: ``Address``, 20 Bytes - address of the AppChain/SubChain the VNODE need to connect.

``number``: ``Block number``, the block number that when the Vnode will join the network, suggest to be current blocknumber +100.

*Returns*

``Boolean`` - returns ``true`` if the operation is successfully, otherwise ``false``.

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_removeSubnetP2P", "params": ["0xD814F2ac2c4cA49b33066582E4e97EBae02F2aB9",1999], "id":101}' localhost:8545

    // Result
    true

--------------

**admin\_getSubnetP2PList**

.. _admin_getSubnetP2PList:

List the Subnet P2P network currently joining.

*Parameters*

none

*Returns*

``array`` - returns a list of the netowrks.

Example


.. code:: js

    // Request
    curl -X POST --data '{"method": "admin_getSubnetP2PList"}' localhost:8545

    // Result
        // Result
    {
      "id":101,
      "jsonrpc":"2.0",
      "result": [{
        "blockEnd": "0x1b4", // 436
        "blockStart":"0x1", // 1
        "peers": "0",
        "subnet": "0x16c5785ac562ff41e2dcfdf829c5a142f1fccd7d"
        },{
          ...
        }]
    }

--------------

TXPOOL
'''''

**txpool\_content**

.. _txpool_content:

The content inspection property can be queried to list the exact details of all the transactions currently pending for inclusion in the next block(s), as well as the ones that are being scheduled for future execution only.

The result is an object with two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with actual transactions.

Please note, there may be multiple transactions associated with the same account and nonce. This can happen if the user broadcast mutliple ones with varying gas allowances (or even complerely different transactions).

*Parameters*

none

*Returns*


``result``: Object with two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with actual transactions.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"txpool_content","params":[],"id":106}' localhost:8545

    // Result
    {
    "jsonrpc":"2.0",
    "id":101,
    "result":{"pending":{"0xf6a36118751C50F8932d31d6d092B11Cc28f2258":{"349":{"blockHash":"0x0000000000000000000000000000000000000000000000000000000000000000","blockNumber":null,"from":"0xf6a36118751c50f8932d31d6d092b11cc28f2258","gas":"0x1e8480","gasPrice":"0x4a817c800","hash":"0xdf81d14a6857549d11a82aba6471874af6410e04e73eac003161fabc9220f953","input":"0x6bbded70","nonce":"0x15d","syscnt":"0x0","to":"0x7097302333beef4813eef53d12dff987241e3c3c","transactionIndex":"0x0","value":"0xde0b6b3a7640000","v":"0xf8","r":"0xfbafb9051495c3b8f8626bb50f0b17b1bf4d611ce12403940765a74e0f2c96e2","s":"0x4c8d2b5f6dc014edfed908c8ca5149bd7dd4270753b44edc55f1693aff589866","shardingFlag":"0x0"}}},"queued":{}}
    }

--------------

**txpool\_status**

.. _txpool_status:

The status inspection property can be queried for the number of transactions currently pending for inclusion in the next block(s), as well as the ones that are being scheduled for future execution only.

The result is an object with two fields pending and queued, each of which is a counter representing the number of transactions in that particular state.

*Parameters*

none

*Returns*

``result``: Object with two fields pending and queued. Each of which is a counter representing the number of transactions in that particular state.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"txpool_status","params":[],"id":106}' localhost:8545

    // Result
    {
    "jsonrpc":"2.0",
    "id":101,
    "result":{"pending":"0x1","queued":"0x0"}
    }

--------------

**txpool\_inspect**

.. _txpool_inspect:

The inspect inspection property can be queried to list a textual summary of all the transactions currently pending for inclusion in the next block(s), as well as the ones that are being scheduled for future execution only. This is a method specifically tailored to developers to quickly see the transactions in the pool and find any potential issues.

The result is an object with two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with transactions summary strings.

Please note, there may be multiple transactions associated with the same account and nonce. This can happen if the user broadcast mutliple ones with varying gas allowances (or even complerely different transactions).

*Parameters*

none

*Returns*

``result``: Object with two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with transactions summary strings.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"txpool_inspect","params":[],"id":106}' localhost:8545

    // Result
    {
    "jsonrpc":"2.0",
    "id":101,
    "result":{"pending":{"0xf6a36118751C50F8932d31d6d092B11Cc28f2258":{"349":"0x7097302333Beef4813EEf53d12dfF987241E3C3C: 1000000000000000000 sha + 2000000 × 20000000000 gas"}},"queued":{}
    }

--------------

VNODE
'''''

**vnode\_address**

.. _vnode_address:

Returns the VNODE benificial address. This is needed for SCS to use in
the communication.

*Parameters*

none

*Returns*

``address``: ``DATA``, 20 Bytes - address from which the VNODE settings
in the vnodeconfig.json file.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"vnode_address","params":[],"id":101}' localhost:8545

    // Result
    {
    "jsonrpc":"2.0",
    "id":101,
    "result":"0xa8863fc8ce3816411378685223c03daae9770ebb"
    }

--------------

**vnode\_scsService**

.. _vnode_scsService:

Returns if the VNODE enable the service for SCS servers.

*Parameters*


none

*Returns*


``Bool`` - true, enable the SCS service; false, not open.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"vnode_scsService","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "jsonrpc":"2.0",
      "id":101,
      "result":true
    }

--------------

**vnode\_serviceCfg**

.. _vnode_serviceCfg:

Returns the VNODE SCS service ports for connecting with.

*Parameters*

none

*Returns*


``String`` - The current service port set in the vnodeconfig.json.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"vnode_serviceCfg","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "jsonrpc":"2.0",
      "id":101,
      "result":":50062"
    }

--------------

**vnode\_showToPublic**

.. _vnode_showToPublic:

Returns if the VNODE enable the public view.

*Parameters*

none

*Returns*


``Bool`` - true, open to the public; false, not open.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"vnode_showToPublic","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "jsonrpc":"2.0",
      "id":101,
      "result":true
    }

--------------

**vnode\_vnodeIP**

.. _vnode_vnodeIP:

Returns VNODE IP for users to access. Note for cloud servers, this could
be different from the cloud server IP.

*Parameters*


none

*Returns*


``String`` - The current IP that can be used to access. This is set in
the vnodeconfig.json.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"vnode_vnodeIP","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "jsonrpc":"2.0",
      "id":101,
      "result":"127.0.0.1"
    }

--------------

SCS
'''

.. _scs_directcall:

**scs_directCall**

Executes a new constant call of the MicroChain Dapp function without
creating a transaction on the MicroChain. This RPC call is used by
API/lib to call MicroChain Dapp functions.

*Parameters*


``Object`` - The transaction call object

- ``from``: ``DATA``, 20 Bytes - (optional) The address the transaction is sent from. 
- ``to``: ``DATA``, 20 Bytes - The address the transaction is directed to. This
parameter is the MicroChain address. 
- ``data``: ``DATA`` - (optional) Hash of the method signature and encoded parameters. For details see
`Ethereum Contract ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`

*Returns*


``DATA`` - the return value of executed Dapp constant function call.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_directCall","params":[{see above}],"id":101}' localhost:8545

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x"
    }

--------------

**scs\_getBlock**

.. _scs_getblock:

Returns information about a block on the MicroChain by block number.

*Parameters*


1. ``String`` - the address of the MicroChain that Dapp is on.
2. ``QUANTITY|TAG`` - integer of a block number, or the string
   ``"earliest"`` or ``"latest"``, as in the `default block
   parameter <#the-default-block-parameter>`. Note, scs\_getBlock does
   not support ``"pending"``.

*Returns*


``DATA`` - Data in the block on the MicroChain.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getBlock","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x1"],"id":101}' localhost:8548

    // Result
    {"jsonrpc":"2.0","id":101,"result":{"extraData":"0x","hash":"0xc80cbe08bc266b1236f22a8d0b310faae3135961dbef6ad8b6ad4e8cd9537309","number":"0x1","parentHash":"0x0000000000000000000000000000000000000000000000000000000000000000","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0x1a065207da60d8e7a44db2f3b5ed9d3e81052a3059e4108c84701d0bf6a62292","timestamp":"0x0","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"}}

--------------

**scs\_getBlockList**

.. _scs_getblocklist:

Returns information about multiple MicroChain blocks by block number.

*Parameters*


1. ``String`` - the address of the MicroChain that Dapp is on.
2. ``QUANTITY`` - integer of the start block number.
3. ``QUANTITY`` - integer of the end block number, need to be larger or equal the start block number.

*Returns*


``ARRAY`` - Array of the block infromation on the MicroChain.

Example


.. code:: js

    // Request
    curl -X POST --data curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getBlock","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x370", "0x373"],"id":101}' localhost:8548

    // Result
    {"jsonrpc":"2.0","id":101,"result":{"blockList":[{"extraData":"0x","hash":"0x56075838e0fffe6576add14783b957239d4f3c57989bc3a7b7728a3b57eb305a","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x370","parentHash":"0x56352a3a8bd0901608041115817204cbce943606e406d233d7d0359f449bd4c2","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c1c","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0xbc3f5791ec039cba99c37310a4f30a68030dd2ab79efb47d23fd9ac5343f54e5","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x371","parentHash":"0x56075838e0fffe6576add14783b957239d4f3c57989bc3a7b7728a3b57eb305a","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c3a","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0x601be17c47cb4684053457d1d5f70a6dbeb853b27cda08d160555f857f2da33b","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x372","parentHash":"0xbc3f5791ec039cba99c37310a4f30a68030dd2ab79efb47d23fd9ac5343f54e5","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c58","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0x8a0bea649bcdbd2b525690ff485e56d5a83443e9013fcdccd1a0adee56ba4092","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x373","parentHash":"0x601be17c47cb4684053457d1d5f70a6dbeb853b27cda08d160555f857f2da33b","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c76","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"}],"endBlk":"0x373","microchainAddress":"0x7D0CbA876cB9Da5fa310A54d29F4687f5dd93fD7","startBlk":"0x370"}}

--------------

**scs\_getBlockNumber**

.. _scs_getblocknumber:

Returns the number of most recent block .

*Parameters*

1. ``String`` - the address of the MicroChain that Dapp is on.

*Returns*

``QUANTITY`` - integer of the current block number the client is on.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getBlockNumber","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x4b7" // 1207
    }
--------------

**scs\_getDappList**

.. _scs_getdapplist:

Returns the Dapp addresses on the MicroChain. For nuwa 1.0.8 and later version only, 

*Parameters*

1. ``String`` - the address of the MicroChain that has Dapps.

*Returns*

``ARRAY`` - Array of the DAPP addresses on the MicroChain.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getDappList","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": ["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x7cfd775c7a97aa632846eff35dcf9dbcf502d0f3"]
    }

--------------

**scs\_getDappState**

.. _scs_getdappstate:

Returns the Dapp state on the MicroChain.

*Parameters*

1. ``String`` - the address of the MicroChain that Dapp is on.

*Returns*

``QUANTITY`` - 0, no DAPP is deployed on the MicroChain; 1, DAPP is
deployed on the MicroChain.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getDappState","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": 1
    }

--------------

**scs\_getMicroChainInfo**

.. _scs_getmicrochaininfo:

Returns the requested MicroChain information on the connecting SCS. This information is the same as the information defined in the MicroChain contract.

*Parameters*

1. `String` - the address of the MicroChain on the SCS.

*Returns*

``Object`` A Micro Chain information object as defined in the MicroChain contract:

-  ``balance``: ``Number`` - the native token amount in the MicroChain.
-  ``blockReward``: ``Number`` - the reward amount at each block for the MicroChain, unit is in Sha = 1e-18 moac.
-  ``bondLimit``: ``Number`` - the token amount needed as deposit in the MicroChain, unit is in Sha = 1e-18 moac.
-  ``owner``: ``String``, 20 Bytes - the address of the beneficiary to
   whom the mining rewards were given.
-  ``scsList``: ``Array``, List of SCS addresses, 20 Bytes each - the address of the SCS to
   whom the mining rewards were given.
-  ``txReward``: ``Number`` - the reward provided to the TX for the MicroChain, unit is in Sha = 1e-18 moac.
-  ``viaReward``: ``Number`` - the reward provided to the VNODE proxy for the MicroChain, unit is in Sha = 1e-18 moac.   


Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getMicroChainInfo","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {"balance":"0x0","blockReward":"0x1c6bf52634000","bondLimit":"0xde0b6b3a7640000","owner":"0xa8863fc8Ce3816411378685223C03DAae9770ebB","scsList":["0xECd1e094Ee13d0B47b72F5c940C17bD0c7630326","0x50C15fafb95968132d1a6ee3617E99cCa1FCF059","0x1b65cE1A393FFd5960D2ce11E7fd6fDB9e991945"],"txReward":"0x174876e800","viaReward":"0x9184e72a000"}
    }

--------------

**scs\_getMicroChainList**

.. _scs_getmicrochainlist:

Returns the list of MicroChains on the SCS that is connecting with. 

*Parameters*

None

*Returns*

``Array`` - A list of Micro Chain addresses on the SCS.

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getMicroChainList","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": ["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8","0x7cfd775c7a97aa632846eff35dcf9dbcf502d0f3"]
    }

--------------

**scs\_getNonce**

.. _scs_getNonce:

Returns the account nonce on the MicroChain. 

*Parameters*


1. ``String`` - the address of the MicroChain that Dapp is on.
2. ``String`` - the address of the accountn.

*Returns*


``QUANTITY`` integer of the number of transactions send from this address on the MicroChain; 

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getNonce","params":["0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8", "0x7312F4B8A4457a36827f185325Fd6B66a3f8BB8B"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": 1
    }

--------------

**scs\_getSCSId**

.. _scs_getSCSId:

Returns the SCS id.

*Parameters*

None

*Returns*


``String`` - SCS id in the scskeystore directory, used for SCS
identification to send deposit and receive MicroChain mining rewards.

Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getSCSId","params":[],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x9d711986ccc8c89db2dfaf0894acadeb5a383ee8"
    }

--------------

**scs\_getReceiptByHash**

.. _scs_getReceiptByHash:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The MicroChain address. 
2. ``String`` - The transaction hash.

*Returns*

``Object`` - A transaction receipt object, or ``null`` when no receipt
was found:

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.

-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.
- ``failed``: ``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getReceiptByHash","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**scs\_getReceiptByNonce**

.. _scs_getReceiptByNonce:

Returns the transaction result by address and nonce on the MicroChain. Note That the nonce is the nonce on the MicroChain. This nonce can be checked using scs_getNonce. 

*Parameters*


1. ``String`` - The MicroChain address. 
1. ``String`` - The transaction nonce.
1. ``QUANTITY`` - The nonce of the transaction.

*Returns*

``Object`` - A transaction receipt object, or ``null`` when no receipt
was found:

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.

-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.
- ``failed``: ``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)


Example

.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getReceiptByNonce","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0xa8863fc8ce3816411378685223c03daae9770ebb", 0],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**scs\_getTransactionByHash**

.. _scs_gettransactionbyhash:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The MicroChain address. 
2. ``String`` - The transaction hash.

*Returns*


``Object`` - A transaction object, or null when no transaction was found.

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.

-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.
- ``failed``: ``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)

Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getTransactionByHash","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**scs\_getTransactionByNonce**

.. _scs_gettransactionbynonce:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The MicroChain address. 
1. ``String`` - The transaction nonce.
1. ``QUANTITY`` - The nonce of the transaction.

*Returns*



``Object`` - A transaction object, or null when no transaction was found.

-  ``transactionHash``: ``DATA``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``QUANTITY`` - integer of the transactions
   index position in the block.
-  ``blockHash``: ``DATA``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``QUANTITY`` - block number where this transaction
   was in.

-  ``contractAddress``: ``DATA``, 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.
-  ``logsBloom``: ``DATA``, 256 Bytes - Bloom filter for light clients
   to quickly retrieve related logs.
- ``failed``: ``Boolean`` - ``true`` if the filter was successfully uninstalled,
otherwise ``false``.
-  ``status``: ``QUANTITY`` either ``1`` (success) or ``0`` (failure)


Example


.. code:: js

    // Request
    curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getTransactionByNonce","params":["0x299afff2da4a57e7e0a0a16bf626f8822b8a3158","0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"],"id":101}' 'localhost:8545'

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    
--------------

**scs\_getExchangeByAddress**

.. _scs_getExchangeByAddress:

Returns the Withdraw/Deposit exchange records between MicroChain and MotherChain for a certain address. This command returns both the ongoing exchanges and processed exchanges. To check all the ongoing exchanges, please use scs_getExchangeInfo. 


*Parameters*

1. `String` - The MicroChain address.
1. `String` - The address to be checked.
1. `Int` - Index of Deposit records >= 0.
1. `Int` - Number of Deposit records extracted.
1. `Int` - Index of Depositing records >= 0.
1. `Int` - Number of Depositing records extracted.
1. `Int` - Index of Withdraw records >= 0.
1. `Int` - Number of Withdraw records extracted.
1. `Int` - Index of Withdrawing records >= 0.
1. `Int` - Number of Withdrawing records extracted.

*Returns*


``Object`` - A JSON format object contains the token exchange info.

Example


.. code:: js

  // Request
  curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getExchangeByAddress","params":["0x2e4694875de2a7da9c3186a176c52760d58694e4","0xa8863fc8ce3816411378685223c03daae9770ebb", 0,10,0,10,0,10,0,10],"id":100}' localhost:8548

  // Result
  {"jsonrpc":"2.0","id":100,"result":{"DepositRecordCount":2,"DepositRecords":[null,null,{"DepositAmt":"0x18abedda5a37000","Deposittime":"0x5c7f03c4"},{"DepositAmt":"0x2bdbb64bc09000","Deposittime":"0x5c7e8aaa"}],"DepositingRecordCount":0,"DepositingRecords":null,"WithdrawRecordCount":0,"WithdrawRecords":null,"WithdrawingRecordCount":0,"WithdrawingRecords":null,"microchain":"0x2e4694875de2a7da9c3186a176c52760d58694e4","sender":"0xa8863fc8ce3816411378685223c03daae9770ebb"}}


--------------

**scs\_getExchangeInfo**

.. _scs_getExchangeInfo:

Returns the Withdraw/Deposit exchange records between MicroChain and MotherChain for a certain address. This command returns both the ongoing exchanges and processed exchanges. To check all the ongoing exchanges, please use scs_getExchangeInfo. 


*Parameters*

1. `String` - The MicroChain address.
1. `String` - The transaction hash.
1. `Int` - Index of Depositing records >= 0.
1. `Int` - Number of Depositing records extracted.
1. `Int` - Index of Withdrawing records >= 0.
1. `Int` - Number of Withdrawing records extracted.

*Returns*


``Object`` - A JSON format object contains the token exchange info.

Example


.. code:: js

  // Request
  curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getExchangeInfo","params":["0x2e4694875de2a7da9c3186a176c52760d58694e4",0,10,0,10],"id":101}' 'localhost:8548'

  // Result

  {"jsonrpc":"2.0","id":100,"result":{"DepositingRecordCount":0,"DepositingRecords":null,"WithdrawingRecordCount":0,"WithdrawingRecords":null,"microchain":"0x2e4694875de2a7da9c3186a176c52760d58694e4","scsid":"0x50c15fafb95968132d1a6ee3617e99cca1fcf059"}}

--------------

**scs\_getTxpool**

.. _scs_gettxpool:

Returns the ongoing transactions in the MicroChain. 

*Parameters*

1. `String` - The MicroChain address.

*Returns*

``Object`` - A JSON format object contains two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with actual transactions.

Example


.. code:: js

  // Request
  curl -X POST --data '{"jsonrpc":"2.0","method":"scs_getTxpool","params":["0x25b0102b5826efa7ac469782f54f40ffa72154f5"],"id":101}' 'localhost:8548'

  // Result

  {"jsonrpc":"2.0","id":100,"result":{"pending":{},"queued":{}}}

--------------

