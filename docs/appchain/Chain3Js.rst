.. _chain3js0.1:

Chain3 JavaScript 软件库 0.1.x
------------------------------


Chain3 JavaScript 软件库是MOAC开发的一套javascript库，目的是让应用程序能够使用简便的方式与MOAC节点进行通信。
注意，这里有两层，moac启动了一个MOAC母链节点，console参数开启了一个javascript的控制台，这个控制台注入了chain3.js这个库，以使我们可以通过chain3对象与MOAC节点的JSON-RPC接口做交互。

目前Chain3的JavaScript 软件库有两个版本，分别参考以太坊的web3 0.20 和 1.0版本。这里面讲解的是基于0.20版本完成的Chain3，
最新版本号是0.1.22。

开发者可以通过``chain3``对象来对DApp的函数进行调用。chain3 通过 `RPC
calls <https://github.com/MOACChain/moac-core/wiki/RPC>`__ 来和母链服务器 VNODE. 
但注意要求连接的VNODE服务器打开 RPC 接口：

.. code:: 

./scsserver --rpc --rpcport 8548


``chain3`` 还提供了和应用链客户端 SCS 交互的对象 ``scs``，本文档中进行了接口的说明，更多的例子可以在源文件中的example下找到：
`here <https://github.com/MOACChain/Chain3/tree/master/example>`__.

使用回调函数（CALLBACK）
=====================

默认的API调用方式是HTTP端口的同步调用，
如果需要进行异步调用，可以把异步调用的函数作为callback的参数传入，可以参考 `error first
callback <http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/>`__
style:

.. code:: js

    chain3.mc.getBlock(48, function(error, result){
        if(!error)
            console.log(result)
        else
            console.error(error);
    })



Chain3 Javascript Ðapp API Reference
====================================

-  `chain3 <#chain3>`__

-  scs

   -  :ref:`scs_directcall <jsscs_directcall>`
   -  :ref:`scs_getblock <jsscs_getblock>`
   -  :ref:`scs_getBlockNumber <jsscs_getblocknumber>`
   -  :ref:`scs_getDappList <jsscs_getdapplist>`
   -  :ref:`scs_getDappState <jsscs_getdappstate>`
   -  :ref:`scs_getMicroChainInfo <jsscs_getmicrochaininfo>`
   -  :ref:`scs_getMicroChainList <jsscs_getmicrochainlist>`
   -  :ref:`scs\_getNonce <jsscs_getnonce>`
   -  :ref:`scs\_getSCSId <jsscs_getscsid>`
   -  :ref:`scs\_getTransactionByHash <jsscs_gettransactionbyhash>`
   -  :ref:`scs\_getTransactionByNonce <jsscs_gettransactionbynonce>`
   -  :ref:`scs\_getReceiptByHash <jsscs_getreceiptbyhash>`
   -  :ref:`scs\_getReceiptByNonce <jsscs_getreceiptbynonce>`
   -  :ref:`scs\_getExchangeByAddress <jsscs_getexchangebyaddress>`
   -  :ref:`scs\_getExchangeInfo <jsscs_getexchangeinfo>`
   -  :ref:`scs\_getTxpool <jsscs_gettxpool>`

SCS
========

用户可以使用 chain3 中的 SCS 对象与应用链节点的JSON-RPC接口进行通信的。与母链节点一样，需要SCS节点打开RPC接口：

::

    ./scsserver --rpc  


在调用时，也需要初始化chain3对象：

.. code:: js

    var Chain3 = require('chain3');
    var chain3 = new Chain3();

    //Create a chain3 object to link with SCS
    //Setup the SCS monitor with JSON 2.0 commands
    chain3.setScsProvider(new chain3.providers.HttpProvider('http://localhost:8548'));


.. _jsscs_directcall:

**scs_directCall**


执行应用链上的合约调用（constant call），仅能查询合约变量状态，不改变合约状态。


*Parameters*


``Object`` - The transaction call object 

- ``from``: ``DATA``, 20 Bytes - (optional) The address the transaction is sent from. 

- ``to``: ``DATA``, 20 Bytes - The address the transaction is directed to. This parameter is the MicroChain address. 

- ``data``: ``DATA`` - (optional) Hash of the method signature and encoded parameters. For details see `Ethereum Contract ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`

*Returns*


``String`` - the return value of executed Dapp constant function call.

Example


.. code:: js

    // Request
    // Prepare the data for the contract call
    var payload = {
    to: "0xecd1e094ee13d0b47b72f5c940c17bd0c7630326",
    data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
    }
    
    var output = this._scs.directCall(payload, defaultBlock);

    // Result
    console.log(output); // "0x0000000000000000000000000000000000000000000000000000000000000015"

--------------

**chain3.scs.getBlock**

.. _jsscs_getblock:

Returns information about a block on the AppChain by block number.

*Parameters*


1. ``String`` - the address of the MicroChain that Dapp is on.
2. ``QUANTITY|TAG`` - integer of a block number, or the string
   ``"earliest"`` or ``"latest"``, as in the `default block
   parameter <#the-default-block-parameter>`. Note, scs\_getBlock does
   not support ``"pending"``.

*Returns*

``Object`` - The MicroChain block object:

-  ``number``: ``Number`` - the block number. ``null`` when its pending
   block.
-  ``hash``: ``String``, 32 Bytes - hash of the block. ``null`` when its
   pending block.
-  ``parentHash``: ``String``, 32 Bytes - hash of the parent block.
-  ``nonce``: ``String``, 8 Bytes - hash of the generated proof-of-work.
   ``null`` when its pending block.
-  ``transactionsRoot``: ``String``, 32 Bytes - the root of the
   transaction trie of the block
-  ``stateRoot``: ``String``, 32 Bytes - the root of the final state
   trie of the block.
-  ``miner``: ``String``, 20 Bytes - the address of the beneficiary to
   whom the mining rewards were given.
-  ``extraData``: ``String`` - the "extra data" field of this block.
-  ``timestamp``: ``Number`` - the unix timestamp for when the block was
   collated.
-  ``transactions``: ``Array`` - Array of transaction objects, or 32
   Bytes transaction hashes depending on the last given parameter.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList();
    mcAddress = mclist[0];
    console.log("SCS block:", chain3.scs.getBlock(mcAddress, 1));

    // Result
    {"extraData":"0x","hash":"0xc80cbe08bc266b1236f22a8d0b310faae3135961dbef6ad8b6ad4e8cd9537309","number":"0x1","parentHash":"0x0000000000000000000000000000000000000000000000000000000000000000","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0x1a065207da60d8e7a44db2f3b5ed9d3e81052a3059e4108c84701d0bf6a62292","timestamp":"0x0","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"}

--------------

**chain3.scs.getBlockList**

.. _jsscs_getblocklist:

Returns information about multiple MicroChain blocks by block number.

*Parameters*


1. ``String`` - the address of the MicroChain that Dapp is on.
2. ``QUANTITY`` - integer of the start block number.
3. ``QUANTITY`` - integer of the end block number, need to be larger or equal the start block number.

*Returns*

``Object`` - The MicroChain blockList object:

-  ``blockList``: ``ARRAY``, Array of the MicroChain block objects.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("SCS blockList 1 - 3:", chain3.scs.getBlockList(mcAddress, 1, 3));

    // Result
    {"blockList":[{"extraData":"0x","hash":"0x56075838e0fffe6576add14783b957239d4f3c57989bc3a7b7728a3b57eb305a","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x370","parentHash":"0x56352a3a8bd0901608041115817204cbce943606e406d233d7d0359f449bd4c2","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c1c","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0xbc3f5791ec039cba99c37310a4f30a68030dd2ab79efb47d23fd9ac5343f54e5","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x371","parentHash":"0x56075838e0fffe6576add14783b957239d4f3c57989bc3a7b7728a3b57eb305a","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c3a","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0x601be17c47cb4684053457d1d5f70a6dbeb853b27cda08d160555f857f2da33b","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x372","parentHash":"0xbc3f5791ec039cba99c37310a4f30a68030dd2ab79efb47d23fd9ac5343f54e5","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c58","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"},{"extraData":"0x","hash":"0x8a0bea649bcdbd2b525690ff485e56d5a83443e9013fcdccd1a0adee56ba4092","miner":"0xecd1e094ee13d0b47b72f5c940c17bd0c7630326","number":"0x373","parentHash":"0x601be17c47cb4684053457d1d5f70a6dbeb853b27cda08d160555f857f2da33b","receiptsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","stateRoot":"0xde741a2f6b4a3c865e8f6fc9ba11eadaa1fa04c61d660bcdf0fa1195029699f6","timestamp":"0x5bfb7c76","transactions":[],"transactionsRoot":"0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"}],"endBlk":"0x373","microchainAddress":"0x7D0CbA876cB9Da5fa310A54d29F4687f5dd93fD7","startBlk":"0x370"}}

--------------

**chain3.scs.getBlockNumber**

.. _jsscs_getblocknumber:

Returns the number of most recent block .

*Parameters*

1. ``String`` - the address of the MicroChain that Dapp is on.

*Returns*

``QUANTITY`` - integer of the current block number the client is on.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("SCS blockNumber:", chain3.scs.getBlockNumber(mcAddress));

    // Result
    SCS block number: 903

--------------

**chain3.scs.getDappList**

.. _jsscs_getdapplist:

Returns the Dapp addresses on the MicroChain. For nuwa 1.0.8 and later version only, 

*Parameters*

1. ``String`` - the address of the MicroChain that has Dapps.

*Returns*

``ARRAY`` - Array of the DAPP addresses on the MicroChain.

Example

.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("SCS dapp:", chain3.scs.getDappAddrList(mcAddress));

    // Result
    SCS dapp: [ '0xa6e4e429e48d97a3dd4309d96cabc836f3bb4283' ]

--------------

**chain3.scs.getDappState**

.. _jsscs_getdappstate:

Returns the Dapp state on the MicroChain.

*Parameters*

1. ``String`` - the address of the MicroChain that Dapp is on.

*Returns*

``QUANTITY`` - 0, no DAPP is deployed on the MicroChain; 1, DAPP is
deployed on the MicroChain.

Example

.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("SCS state:", chain3.scs.getDappState(mcAddress));

    // Result
    SCS state: 1


--------------------------------

**chain3.scs.getMicroChainInfo**

.. _jsscs_getmicrochaininfo:

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
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("MC info:", chain3.scs.getMicroChainInfo(mcAddress));

    // Result
    MC info: {"balance":"0x0","blockReward":"0x1c6bf52634000","bondLimit":"0xde0b6b3a7640000","owner":"0xa8863fc8Ce3816411378685223C03DAae9770ebB","scsList":["0xECd1e094Ee13d0B47b72F5c940C17bD0c7630326","0x50C15fafb95968132d1a6ee3617E99cCa1FCF059","0x1b65cE1A393FFd5960D2ce11E7fd6fDB9e991945"],"txReward":"0x174876e800","viaReward":"0x9184e72a000"}

--------------

**chain3.scs.getMicroChainList**

.. _jsscs_getmicrochainlist:

Returns the list of MicroChains on the SCS that is connecting with. 

*Parameters*

None

*Returns*

``Array`` - A list of Micro Chain addresses on the SCS.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    console.log("SCS MicroChain List:", mclist);


    // Result
    SCS MicroChain List: [ '0x25b0102b5826efa7ac469782f54f40ffa72154f5', '0x7cfd775c7a97aa632846eff35dcf9dbcf502d0f3' ]

--------------

**chain3.scs.getNonce**

.. _jsscs_getNonce:

Returns the account nonce on the MicroChain. 

*Parameters*


1. ``String`` - the address of the MicroChain.
2. ``String`` - the address of the account.

*Returns*


``QUANTITY`` integer of the number of transactions send from this address on the MicroChain; 

Example

.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    tAddress="0xf6a36118751c50f8932d31d6d092b11cc28f2258";
    console.log("SCS nonce of:", tAddress, " is ", chain3.scs.getNonce(mcAddress,tAddress));

    // Result
    SCS nonce of: 0xf6a36118751c50f8932d31d6d092b11cc28f2258  is  3

--------------

**chain3.scs.getSCSId**

.. _jsscs_getSCSId:

Returns the SCS id.

*Parameters*

None

*Returns*


``String`` - SCS id in the scskeystore directory, used for SCS
identification to send deposit and receive MicroChain mining rewards.

Example

.. code:: js

    // Request
    console.log("SCS ID:", chain3.scs.getSCSId());

    // Result
    SCS ID: 0xecd1e094ee13d0b47b72f5c940c17bd0c7630326

--------------

**chain3.scs.getReceiptByHash**

.. _jsscs_getReceiptByHash:

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
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    txhash1="0x688456221f7729f5c2c17006bbe4df163d09bea70c1a1ebb66b9b53ca10563df";
    console.log("TX Receipt:", chain3.scs.getReceiptByHash(mcAddress, txhash1));

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**chain3.scs.getReceiptByNonce**

.. _jsscs_getReceiptByNonce:

Returns the transaction result by address and nonce on the MicroChain. Note That the nonce is the nonce on the MicroChain. This nonce can be checked using scs_getNonce. 

*Parameters*


1. ``String`` - The MicroChain address. 
2. ``String`` - The transaction nonce.
3. ``QUANTITY`` - The nonce of the transaction.

*Returns*

``Object`` - A transaction receipt object, or null when no receipt was
found:.

Example

.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    tAddress="0xf6a36118751c50f8932d31d6d092b11cc28f2258";
    console.log("SCS receipt:", chain3.scs.getReceiptByNonce(mcAddress, tAddress, 0));

    // Result
    SCS receipt: {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    

--------------

**chain3.scs.getTransactionByHash**

.. _jsscs_gettransactionbyhash:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The MicroChain address. 
2. ``String`` - The transaction hash.

*Returns*


``Object`` - A transaction object, or null when no transaction was found.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    txhash1="0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69";
    console.log("TX by hash:", chain3.scs.getTransactionByHash(mcAddress, txhash1));

    // Result
    TX by hash: {
      "id":101,
      "jsonrpc": "2.0",
      "result": {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    }

--------------

**chain3.scs.getTransactionByNonce**

.. _jsscs_gettransactionbynonce:

Returns the receipt of a transaction by transaction hash. Note That the
receipt is not available for pending transactions.

*Parameters*

1. ``String`` - The MicroChain address. 
1. ``String`` - The transaction nonce.
1. ``QUANTITY`` - The nonce of the transaction.

*Returns*


``Object`` - A transaction receipt object, or null when no receipt was
found:.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    tAddress="0xf6a36118751c50f8932d31d6d092b11cc28f2258";
    console.log("SCS TX:", chain3.scs.getTransactionByNonce(mcAddress, tAddress, 0));


    // Result
    SCS TX: { blockHash: '0x45ab47bde3a7caa62d80e8c38bef21ada499d52331e574f3a09d4d943aa133fa',
  blockNumber: 66,
  from: '0xf6a36118751c50f8932d31d6d092b11cc28f2258', input: '.....', nonce: 0,
  r: 1.1336589614028917e+77,
  s: 1.8585853533200337e+76,
  shardingFlag: 3,
  to: '0x25b0102b5826efa7ac469782f54f40ffa72154f5',
  transactionHash: '0x6eb3d33fab53317007927368238aef5bc00d1d1d9bf082930c372e3dabca507c',
  transactionIndex: 0,
  v: 248,
  value: BigNumber { s: 1, e: 21, c: [ 10000000 ] },
  gas: 0,
  gasPrice: BigNumber { s: 1, e: 0, c: [ 0 ] } }
    
--------------

**chain3.scs.getExchangeByAddress**

.. _jsscs_getExchangeByAddress:

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
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    tAddress="0xf6a36118751c50f8932d31d6d092b11cc28f2258";
    console.log("SCS token address exchange:", chain3.scs.getExchangeByAddress(mcAddress, tAddress));

    // Result
    SCS token address exchange: { DepositRecordCount: 0,
    DepositRecords: null,
    DepositingRecordCount: 0,
    DepositingRecords: null,
    WithdrawRecordCount: 0,
    WithdrawRecords: null,
    WithdrawingRecordCount: 0,
    WithdrawingRecords: null,
    microchain: '0x25b0102b5826efa7ac469782f54f40ffa72154f5',
    sender: '0xf6a36118751c50f8932d31d6d092b11cc28f2258' }


--------------

**chain3.scs.getExchangeInfo**

.. _jsscs_getExchangeInfo:

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
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("SCS token exchanging info:", chain3.scs.getExchangeInfo(mcAddress));

    // Result
    SCS token exchanging info: { DepositingRecordCount: 0,
      DepositingRecords: null,
      WithdrawingRecordCount: 0,
      WithdrawingRecords: null,
      microchain: '0x25b0102b5826efa7ac469782f54f40ffa72154f5',
      scsid: '0xecd1e094ee13d0b47b72f5c940c17bd0c7630326' }

--------------

**chain3.scs.getTxpool**

.. _jsscs_gettxpool:

Returns the ongoing transactions in the MicroChain. 

*Parameters*

1. `String` - The MicroChain address.

*Returns*

``Object`` - A JSON format object contains two fields pending and queued. Each of these fields are associative arrays, in which each entry maps an origin-address to a batch of scheduled transactions. These batches themselves are maps associating nonces with actual transactions.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("SCS TXpool:", chain3.scs.getTxpool(mcAddress));

    // Result

    SCS TXpool: {"pending":{},"queued":{}}

--------------

