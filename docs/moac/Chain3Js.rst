Chain3 JavaScript 软件库
=======================


Chain3 JavaScript 软件库是MOAC开发的一套javascript库，目的是让应用程序能够使用简便的方式与MOAC节点进行通信。
注意，这里有两层，moac启动了一个MOAC母链节点，console参数开启了一个javascript的控制台，这个控制台注入了chain3.js这个库，以使我们可以通过chain3对象与MOAC节点的JSON-RPC接口做交互。


开发者可以通过``chain3``对象来对DApp的函数进行调用。chain3 通过 `RPC
calls <https://github.com/MOACChain/moac-core/wiki/RPC>`__ 来和母链服务器 VNODE. 
但注意要求连接的VNODE服务器打开 RPC 接口：

.. code:: 

./moac --rpc --rpcport 8545


``chain3`` 除了包含 ``mc`` 对象 - ``chain3.mc`` 与母链节点互动之外，在 accounts 下面的文件中提供了交易签名功能，
以及对消息签名和验证的功能，以方便用户使用。
``chain3`` 还提供了和子链客户端 SCS 交互的对象 ``scs``
Over time we'll introduce other objects such as SCS for the other chain3
protocols. 更多的例子可以在源文件中的example下找到：
`here <https://github.com/MOACChain/Chain3/tree/master/example>`__.

使用回调函数（CALLBACK）
---------------

As this API is designed to work with a local RPC node and all its
functions are by default use synchronous HTTP requests.con

If you want to make an asynchronous request, you can pass an optional
callback as the last parameter to most functions. All callbacks are
using an `error first
callback <http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/>`__
style:

.. code:: js

    chain3.mc.getBlock(48, function(error, result){
        if(!error)
            console.log(result)
        else
            console.error(error);
    })

批处理执行命令
--------------

Batch requests allow queuing up requests and processing them at once.

.. code:: js

    var batch = chain3.createBatch();
    batch.add(chain3.mc.getBalance.request('0x0000000000000000000000000000000000000000', 'latest', callback));
    batch.add(chain3.mc.contract(abi).at(address).balance.request(address, callback2));
    batch.execute();

A note on big numbers in chain3.js
----------------------------------

You will always get a BigNumber object for balance values as JavaScript
is not able to handle big numbers correctly. Look at the following
examples:

.. code:: js

    "101010100324325345346456456456456456456"
    // "101010100324325345346456456456456456456"
    101010100324325345346456456456456456456
    // 1.0101010032432535e+38

chain3.js depends on the `BigNumber
Library <https://github.com/MikeMcl/bignumber.js/>`__ and adds it
automatically.

.. code:: js

    var balance = new BigNumber('123456786979284780000000000');
    // or var balance = chain3.mc.getBalance(someAddress);

    balance.plus(21).toString(10); // toString(10) converts it to a number string
    // "123456786979284780000000021"

The next example wouldn't work as we have more than 20 floating points,
therefore it is recommended that you always keep your balance in *sha*
and only transform it to other units when presenting to the user:

.. code:: js

    var balance = new BigNumber('13124.234435346456466666457455567456');

    balance.plus(21).toString(10); // toString(10) converts it to a number string, but can only show max 20 floating points
    // "13145.23443534645646666646" // you number would be cut after the 20 floating point

Chain3 Javascript Ðapp API Reference
------------------------------------

-  `chain3 <#chain3>`__
   
   -  `version <#chain3versionapi>`__

       -  `api <#chain3versionapi>`__
       -  `node <#chain3versionnode>`__
       -  `network <#chain3versionnetwork>`__
       -  `moac <#chain3versionmoac>`__

   -  `isConnected() <####chain3isconnected>`__
   -  `setProvider(provider) <#chain3setprovider>`__
   -  `currentProvider <#chain3currentprovider>`__
   -  `reset() <#chain3reset>`__
   -  `sha3(string) <#chain3sha3>`__
   -  `toHex(stringOrNumber) <#chain3tohex>`__
   -  `toAscii(hexString) <#chain3toascii>`__
   -  `fromAscii(textString, [padding]) <#chain3fromascii>`__
   -  `toDecimal(hexString) <#chain3todecimal>`__
   -  `toChecksumAddress(string) <#chain3tochecksumaddress>`__
   -  `fromDecimal(number) <#chain3fromdecimal>`__
   -  `fromSha(numberStringOrBigNumber, unit) <#chain3fromsha>`__
   -  `toSha(numberStringOrBigNumber, unit) <#chain3tosha>`__
   -  `toBigNumber(numberOrHexString) <#chain3tobignumber>`__
   -  `isAddress(hexString) <#chain3isAddress>`__
-  `net <#chain3net>`__

   -  `listening/getListening <#chain3netlistening>`__
   -  `peerCount/getPeerCount <#chain3mcpeercount>`__

-  `mc <#chain3mc>`__

   -  `defaultAccount <#chain3mcdefaultaccount>`__
   -  `defaultBlock <#chain3mcdefaultblock>`__
   -  `syncing/getSyncing <#chain3mcsyncing>`__
   -  `isSyncing <#chain3mcissyncing>`__
   -  `coinbase/getCoinbase <#chain3mccoinbase>`__
   -  `hashrate/getHashrate <#chain3mchashrate>`__
   -  `gasPrice/getGasPrice <#chain3mcgasprice>`__
   -  `accounts/getAccounts <#chain3mcaccounts>`__
   -  `mining/getMining <#chain3mcmining>`__
   -  `blockNumber/getBlockNumber <#chain3mcblocknumber>`__
   -  `getBalance(address) <#chain3mcgetbalance>`__
   -  `getStorageAt(address, position) <#chain3mcgetstorageat>`__
   -  `getCode(address) <#chain3mcgetcode>`__
   -  `getBlock(hash/number) <#chain3mcgetblock>`__
   -  `getBlockTransactionCount(hash/number) <#chain3mcgetblocktransactioncount>`__
   -  `getUncle(hash/number) <#chain3mcgetuncle>`__
   -  `getBlockUncleCount(hash/number) <#chain3mcgetblockunclecount>`__
   -  `getTransaction(hash) <#chain3mcgettransaction>`__
   -  `getTransactionFromBlock(hashOrNumber,
      indexNumber) <#chain3mcgettransactionfromblock>`__
   -  `getTransactionReceipt(hash) <#chain3mcgettransactionreceipt>`__
   -  `getTransactionCount(address) <#chain3mcgettransactioncount>`__
   -  `sendTransaction(object) <#chain3mcsendtransaction>`__
   -  `call(object) <#chain3mccall>`__
   -  `estimateGas(object) <#chain3mcestimategas>`__
   -  `filter(array (, options) ) <#chain3mcfilter>`__

      -  `watch(callback) <#chain3mcfilter>`__
      -  `stopWatching(callback) <#chain3mcfilter>`__
      -  `get() <#chain3mcfilter>`__

   -  `contract(abiArray) <#chain3mccontract>`__
   -  `contract.myMethod() <#contract-methods>`__
   -  `contract.myEvent() <#contract-events>`__
   -  `contract.allEvents() <#contract-allevents>`__
   -  `encodeParams <#chain3encodeParams>`__
   -  `namereg <#chain3mcnamereg>`__
   -  `sendIBANTransaction <#chain3mcsendibantransaction>`__
   -  `iban <#chain3mciban>`__
   -  `fromAddress <#chain3mcibanfromaddress>`__
   -  `fromBban <#chain3mcibanfrombban>`__
   -  `createIndirect <#chain3mcibancreateindirect>`__
   -  `isValid <#chain3mcibanisvalid>`__
   -  `isDirect <#chain3mcibanisdirect>`__
   -  `isIndirect <#chain3mcibanisindirect>`__
   -  `checksum <#chain3mcibanchecksum>`__
   -  `institution <#chain3mcibaninstitution>`__
   -  `client <#chain3mcibanclient>`__
   -  `address <#chain3mcibanaddress>`__
   -  `toString <#chain3mcibantostring>`__

-  vnode

   -  :ref:`address <vnode_address>`
   -  :ref:`scsService <vnode_scsservice>`
   -  :ref:`serviceCfg <vnode_servicecfg>`
   -  :ref:`showToPublic <vnode_showtopublic>`
   -  :ref:`vnodeIP <vnode_vnodeip>`

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

Chain3
~~~~~~



.. raw:: html

   <h4 id="chain3">

chain3.version.api

.. raw:: html

   </h4>

The ``chain3`` object provides all methods.

Example
'''''''

.. code:: js

    var Chain3 = require('chain3');
    // create an instance of chain3 using the HTTP provider.
    var chain3 = new Chain3(new Chain3.providers.HttpProvider("http://localhost:8545"));

--------------

.. raw:: html

   <h4 id="chain3versionapi">

chain3.version.api

.. raw:: html

   </h4>

.. code:: js

    chain3.version.api
    // or async
    chain3.version.getApi(callback(error, result){ ... })

Returns
'''''''

``String`` - The moac js api version.

Example
'''''''

.. code:: js

    var version = chain3.version.api;
    console.log(version); // "0.2.0"

--------------

.. raw:: html

   <h4 id="chain3versionnode">

chain3.version.node

.. raw:: html

   </h4>

::

    chain3.version.node
    // or async
    chain3.version.getClient(callback(error, result){ ... })

Returns
'''''''

``String`` - The client/node version.

Example
'''''''

.. code:: js

    var version = chain3.version.node;
    console.log(version); // "Moac/v0.1.0-develop/darwin-amd64/go1.9"

--------------

.. raw:: html

   <h4 id="chain3versionnetwork">

chain3.version.network

.. raw:: html

   </h4>

::

    chain3.version.network
    // or async
    chain3.version.getNetwork(callback(error, result){ ... })

Returns
'''''''

``String`` - The network protocol version.

Example
'''''''

.. code:: js

    var version = chain3.version.network;
    console.log(version); // 54

--------------

.. raw:: html

   <h4 id="chain3versionnetmoac">

chain3.version.moac

.. raw:: html

   </h4>

::

    chain3.version.moac
    // or async
    chain3.version.getMoac(callback(error, result){ ... })

Returns
'''''''

``String`` - The moac protocol version.

Example
'''''''

.. code:: js

    var version = chain3.version.moac;
    console.log(version); // 0x3f

--------------

.. raw:: html

   <h4 id="chain3isconnected">

chain3.isConnected

.. raw:: html

   </h4>

::

    chain3.isConnected()

Should be called to check if a connection to a node exists

Parameters
''''''''''

none

Returns
'''''''

``Boolean``

Example
'''''''

.. code:: js

    if(!chain3.isConnected()) {

       // show some dialog to ask the user to start a node

    } else {

       // start chain3 filters, calls, etc

    }

--------------

.. raw:: html

   <h4 id="chain3setprovider">

chain3.setProvider

.. raw:: html

   </h4>

::

    chain3.setProvider(provider)

Should be called to set provider.

Parameters
''''''''''

none

Returns
'''''''

``undefined``

Example
'''''''

.. code:: js

    chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545')); // 8545 for go/moac

--------------

.. raw:: html

   <h4 id="chain3currentprovider">

chain3.currentProvider

.. raw:: html

   </h4>

::

    chain3.currentProvider

Will contain the current provider, if one is set. This can be used to
check if moac etc. set already a provider.

Returns
'''''''

``Object`` - The provider set or ``null``;

Example
'''''''

.. code:: js

    // Check if moac etc. already set a provider
    if(!chain3.currentProvider)
        chain3.setProvider(new chain3.providers.HttpProvider("http://localhost:8545"));

--------------

.. raw:: html

   <h4 id="chain3currentreset">

chain3.reset

.. raw:: html

   </h4>

::

    chain3.reset(keepIsSyncing)

Should be called to reset state of chain3. Resets everything except
manager. Uninstalls all filters. Stops polling.

Parameters
''''''''''

1. ``Boolean`` - If ``true`` it will uninstall all filters, but will
   keep the `chain3.mc.isSyncing() <#chain3mcissyncing>`__ polls

Returns
'''''''

``undefined``

Example
'''''''

.. code:: js

    chain3.reset();

--------------

.. raw:: html

   <h4 id="chain3sha3">

chain3.sha3

.. raw:: html

   </h4>

::

    chain3.sha3(string [, callback])

Parameters
''''''''''

1. ``String`` - The string to hash using the SHA3 algorithm
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The SHA3 of the given data.

Example
'''''''

.. code:: js

    var str = chain3.sha3("Some ASCII string to be hashed in MOAC");
    console.log(str); // "0xbfa24877cd68e6734710402a2af3e29cf18bd6d2f304aa528ffa3a32fa7652d2"

--------------

.. raw:: html

   <h4 id="chain3tohex">

chain3.toHex

.. raw:: html

   </h4>

::

    chain3.toHex(mixed);

Converts any value into HEX.

Parameters
''''''''''

1. ``String|Number|Object|Array|BigNumber`` - The value to parse to HEX.
   If its an object or array it will be ``JSON.stringify`` first. If its
   a BigNumber it will make it the HEX value of a number.

Returns
'''''''

``String`` - The hex string of ``mixed``.

Example
'''''''

.. code:: js

    var str = chain3.toHex("moac network");
    console.log(str); // '0x6d6f6163206e6574776f726b'

    console.log(chain3.toHex({moac: 'test'}));
    //'0x7b226d6f6163223a2274657374227d'

--------------

.. raw:: html

   <h4 id="chain3toascii">

chain3.toAscii

.. raw:: html

   </h4>

chain3.toAscii
^^^^^^^^^^^^^^

::

    chain3.toAscii(hexString);

Converts a HEX string into a ASCII string.

Parameters
''''''''''

1. ``String`` - A HEX string to be converted to ascii.

Returns
'''''''

``String`` - An ASCII string made from the given ``hexString``.

Example
'''''''

.. code:: js

    var str = chain3.toAscii("0x0x6d6f6163206e6574776f726b");
    console.log(str); // "moac network"

--------------

.. raw:: html

   <h4 id="chain3fromascii">

chain3.fromAscii

.. raw:: html

   </h4>

::

    chain3.fromAscii(string);

Converts any ASCII string to a HEX string.

Parameters
''''''''''

``String`` - An ASCII string to be converted to HEX.

Returns
'''''''

``String`` - The converted HEX string.

Example
'''''''

.. code:: js

    var str = chain3.fromAscii('moac network');
    console.log(str); // "0x6d6f6163206e6574776f726b"

--------------

.. raw:: html

   <h4 id="chain3tochecksumaddress">

chain3.toChecksumAddress

.. raw:: html

   </h4>

::

    chain3.toChecksumAddress(hexString);

Converts a string to the checksummed address equivalent.

Parameters
''''''''''

1. ``String`` - A string to be converted to a checksummed address.

Returns
'''''''

``String`` - A string containing the checksummed address.

Example
'''''''

.. code:: js

    var myAddress = chain3.toChecksumAddress('0xa0c876ec9f2d817c4304a727536f36363840c02c');
    console.log(myAddress); // '0xA0C876eC9F2d817c4304A727536f36363840c02c'

--------------

.. raw:: html

   <h4 id="chain3todecimal">

chain3.toDecimal

.. raw:: html

   </h4>

::

    chain3.toDecimal(hexString);

Converts a HEX string to its number representation.

Parameters
''''''''''

1. ``String`` - An HEX string to be converted to a number.

Returns
'''''''

``Number`` - The number representing the data ``hexString``.

Example
'''''''

.. code:: js

    var number = chain3.toDecimal('0x15');
    console.log(number); // 21

--------------

.. raw:: html

   <h4 id="chain3fromdecimal">

chain3.fromDecimal

.. raw:: html

   </h4>

::

    chain3.fromDecimal(number);

Converts a number or number string to its HEX representation.

Parameters
''''''''''

1. ``Number|String`` - A number to be converted to a HEX string.

Returns
'''''''

``String`` - The HEX string representing of the given ``number``.

Example
'''''''

.. code:: js

    var value = chain3.fromDecimal('21');
    console.log(value); // "0x15"

--------------

.. raw:: html

   <h4 id="chain3fromsha">

chain3.fromSha

.. raw:: html

   </h4>

::

    chain3.fromSha(number, unit)

Converts a number of sha into the following moac units:

-  ``ksha``/``femtomc``
-  ``msha``/``picomc``
-  ``gsha``/``nano``/``xiao``
-  ``micro``/``sand``
-  ``milli``
-  ``mc``
-  ``kmc``/``grand``
-  ``mmc``
-  ``gmc``
-  ``tmc``

Parameters
''''''''''

1. ``Number|String|BigNumber`` - A number or BigNumber instance.
2. ``String`` - One of the above moac units.

Returns
'''''''

``String|BigNumber`` - Either a number string, or a BigNumber instance,
depending on the given ``number`` parameter.

Example
'''''''

.. code:: js

    var value = chain3.fromSha('21000000000000', 'Xiao');
    console.log(value); // "21000"

--------------

.. raw:: html

   <h4 id="chain3tosha">

chain3.toSha

.. raw:: html

   </h4>

::

    chain3.toSha(number, unit)

Converts a moac unit into sha. Possible units are:

-  ``ksha``/``femtomc``
-  ``msha``/``picomc``
-  ``gsha``/``nano``/``xiao``
-  ``micro``/``sand``
-  ``milli``
-  ``mc``
-  ``kmc``/``grand``
-  ``mmc``
-  ``gmc``
-  ``tmc``

Parameters
''''''''''

1. ``Number|String|BigNumber`` - A number or BigNumber instance.
2. ``String`` - One of the above moac units.

Returns
'''''''

``String|BigNumber`` - Either a number string, or a BigNumber instance,
depending on the given ``number`` parameter.

Example
'''''''

.. code:: js

    var value = chain3.toSha('1', 'mc');
    console.log(value); // "1000000000000000000" = 1e18

--------------

.. raw:: html

   <h4 id="chain3tobignumber">

chain3.toBigNumber

.. raw:: html

   </h4>

::

    chain3.toBigNumber(numberOrHexString);

Converts a given number into a BigNumber instance.

See the `note on BigNumber <#a-note-on-big-numbers-in-javascript>`__.

Parameters
''''''''''

1. ``Number|String`` - A number, number string or HEX string of a
   number.

Returns
'''''''

``BigNumber`` - A BigNumber instance representing the given value.

Example
'''''''

.. code:: js

    var value = chain3.toBigNumber('200000000000000000000001');
    console.log(value); // instanceOf BigNumber, { [String: '2.00000000000000000000001e+23'] s: 1, e: 23, c: [ 2000000000, 1 ] }
    console.log(value.toNumber()); // 2.0000000000000002e+23
    console.log(value.toString(10)); // '200000000000000000000001'

--------------

Net
~~~

.. raw:: html

   <h3 id="chain3net">

chain3.net

.. raw:: html

   </h3>

.. raw:: html

   <h4 id="chain3netlistening">

chain3.net.listening

.. raw:: html

   </h4>

::

    chain3.net.listening
    // or async
    chain3.net.getListening(callback(error, result){ ... })

This property is read only and says whether the node is actively
listening for network connections or not.

Returns
'''''''

``Boolean`` - ``true`` if the client is actively listening for network
connections, otherwise ``false``.

Example
'''''''

.. code:: js

    var listening = chain3.net.listening;
    console.log(listening); // true of false

--------------

.. raw:: html

   <h4 id="chain3netpeercount">

chain3.net.peerCount

.. raw:: html

   </h4>

::

    chain3.net.peerCount
    // or async
    chain3.net.getPeerCount(callback(error, result){ ... })

This property is read only and returns the number of connected peers.

Returns
'''''''

``Number`` - The number of peers currently connected to the client.

Example
'''''''

.. code:: js

    var peerCount = chain3.net.peerCount;
    console.log(peerCount); // 4

--------------

Mc
~~~

.. raw:: html

   <h3 id="chain3mc">

chain3.mc

.. raw:: html

   </h3>

Contains the MOAC blockchain related methods.

Example
'''''''

.. code:: js

    var mc = chain3.mc;

--------------

.. raw:: html

   <h4 id="chain3mcdefaultaccount">

chain3.mc.defaultAccount

.. raw:: html

   </h4>

::

    chain3.mc.defaultAccount

This default address is used for the following methods (optionally you
can overwrite it by specifying the ``from`` property):

-  `chain3.mc.sendTransaction() <#chain3mcsendtransaction>`__
-  `chain3.mc.call() <#chain3mccall>`__

Values
''''''

``String``, 20 Bytes - Any address you own, or where you have the
private key for.

*Default is* ``undefined``.

Returns
'''''''

``String``, 20 Bytes - The currently set default address.

Example
'''''''

.. code:: js

    var defaultAccount = chain3.mc.defaultAccount;
    console.log(defaultAccount); // 'undefined'

    // set the default block
    chain3.mc.defaultAccount = '0x8888f1f195afa192cfee860698584c030f4c9db1';
    console.log(chain3.mc.defaultAccount);//'0x8888f1f195afa192cfee860698584c030f4c9db1'

--------------

.. raw:: html

   <h4 id="chain3mcdefaultblock">

chain3.mc.defaultBlock

.. raw:: html

   </h4>

::

    chain3.mc.defaultBlock

This default block is used for the following methods (optionally you can
overwrite the defaultBlock by passing it as the last parameter):

-  `chain3.mc.getBalance() <#chain3mcgetbalance>`__
-  `chain3.mc.getCode() <#chain3mcgetcode>`__
-  `chain3.mc.getTransactionCount() <#chain3mcgettransactioncount>`__
-  `chain3.mc.getStorageAt() <#chain3mcgetstorageat>`__
-  `chain3.mc.call() <#chain3mccall>`__

Values
''''''

Default block parameters can be one of the following:

-  ``Number`` - a block number
-  ``String`` - ``"earliest"``, the genisis block
-  ``String`` - ``"latest"``, the latest block (current head of the
   blockchain)
-  ``String`` - ``"pending"``, the currently mined block (including
   pending transactions)

*Default is* ``latest``

Returns
'''''''

``Number|String`` - The default block number to use when querying a
state.

Example
'''''''

.. code:: js

    var defaultBlock = chain3.mc.defaultBlock;
    console.log(defaultBlock); // 'latest'

    // set the default block
    chain3.mc.defaultBlock = 12345;
    console.log(chain3.mc.defaultAccount);//'12345'

--------------

.. raw:: html

   <h4 id="chain3mcsyncing">

chain3.mc.syncing

.. raw:: html

   </h4>

::

    chain3.mc.syncing
    // or async
    chain3.mc.getSyncing(callback(error, result){ ... })

This property is read only and returns the either a sync object, when
the node is syncing or ``false``.

Returns
'''''''

``Object|Boolean`` - A sync object as follows, when the node is
currently syncing or ``false``: - ``startingBlock``: ``Number`` - The
block number where the sync started. - ``currentBlock``: ``Number`` -
The block number where at which block the node currently synced to
already. - ``highestBlock``: ``Number`` - The estimated block number to
sync to.

Example
'''''''

.. code:: js

    var sync = chain3.mc.syncing;
    console.log(sync);
    /*
    {
       startingBlock: 300,
       currentBlock: 312,
       highestBlock: 512
    }
    */

--------------

.. raw:: html

   <h4 id="chain3mcissyncing">

chain3.mc.isSyncing

.. raw:: html

   </h4>

::

    chain3.mc.isSyncing(callback);

This convenience function calls the ``callback`` everytime a sync
starts, updates and stops.

Returns
'''''''

``Object`` - a isSyncing object with the following methods:

-  ``syncing.addCallback()``: Adds another callback, which will be
   called when the node starts or stops syncing.
-  ``syncing.stopWatching()``: Stops the syncing callbacks.

Callback return value
'''''''''''''''''''''

-  ``Boolean`` - The callback will be fired with ``true`` when the
   syncing starts and with ``false`` when it stopped.
-  ``Object`` - While syncing it will return the syncing object:
-  ``startingBlock``: ``Number`` - The block number where the sync
   started.
-  ``currentBlock``: ``Number`` - The block number where at which block
   the node currently synced to already.
-  ``highestBlock``: ``Number`` - The estimated block number to sync to.

Example
'''''''

.. code:: js

    chain3.mc.isSyncing(function(error, sync){
        if(!error) {
            // stop all app activity
            if(sync === true) {
               // we use `true`, so it stops all filters, but not the chain3.mc.syncing polling
               chain3.reset(true);

            // show sync info
            } else if(sync) {
               console.log(sync.currentBlock);

            // re-gain app operation
            } else {
                // run your app init function...
            }
        }
    });

--------------

.. raw:: html

   <h4 id="chain3mccoinbase">

chain3.mc.coinbase

.. raw:: html

   </h4>

::

    chain3.mc.coinbase
    // or async
    chain3.mc.getCoinbase(callback(error, result){ ... })

This property is read only and returns the coinbase address were the
mining rewards go to.

Returns
'''''''

``String`` - The coinbase address of the client.

Example
'''''''

.. code:: js

    var coinbase = chain3.mc.coinbase;
    console.log(coinbase); // "0x407d73d8a49eeb85d32cf465507dd71d507100c1"

--------------

.. raw:: html

   <h4 id="chain3mcmining">

chain3.mc.mining

.. raw:: html

   </h4>

::

    chain3.mc.mining
    // or async
    chain3.mc.getMining(callback(error, result){ ... })

This property is read only and says whether the node is mining or not.

Returns
'''''''

``Boolean`` - ``true`` if the client is mining, otherwise ``false``.

Example
'''''''

.. code:: js

    var mining = chain3.mc.mining;
    console.log(mining); // true or false

--------------

.. raw:: html

   <h4 id="chain3mchashrate">

chain3.mc.hashrate

.. raw:: html

   </h4>

::

    chain3.mc.hashrate
    // or async
    chain3.mc.getHashrate(callback(error, result){ ... })

This property is read only and returns the number of hashes per second
that the node is mining with.

Returns
'''''''

``Number`` - number of hashes per second.

Example
'''''''

.. code:: js

    var hashrate = chain3.mc.hashrate;
    console.log(hashrate); // 493736

--------------

.. raw:: html

   <h4 id="chain3mcgasprice">

chain3.mc.gasPrice

.. raw:: html

   </h4>

::

    chain3.mc.gasPrice
    // or async
    chain3.mc.getGasPrice(callback(error, result){ ... })

This property is read only and returns the current gas price. The gas
price is determined by the x latest blocks median gas price.

Returns
'''''''

``BigNumber`` - A BigNumber instance of the current gas price in sha.

See the `note on BigNumber <#a-note-on-big-numbers-in-javascript>`__.

Example
'''''''

.. code:: js

    var gasPrice = chain3.mc.gasPrice;
    console.log(gasPrice.toString(10)); // "20000000000"

--------------

.. raw:: html

   <h4 id="chain3mcaccounts">

chain3.mc.accounts

.. raw:: html

   </h4>

::

    chain3.mc.accounts
    // or async
    chain3.mc.getAccounts(callback(error, result){ ... })

This property is read only and returns a list of accounts the node
controls.

Returns
'''''''

``Array`` - An array of addresses controlled by client.

Example
'''''''

.. code:: js

    var accounts = chain3.mc.accounts;
    console.log(accounts); // ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]

--------------

.. raw:: html

   <h4 id="chain3mcblocknumber">

chain3.mc.blockNumber

.. raw:: html

   </h4>

::

    chain3.mc.blockNumber
    // or async
    chain3.mc.getBlockNumber(callback(error, result){ ... })

This property is read only and returns the current block number.

Returns
'''''''

``Number`` - The number of the most recent block.

Example
'''''''

.. code:: js

    var number = chain3.mc.blockNumber;
    console.log(number); // 2744

--------------

.. raw:: html

   <h4 id="chain3mcgetbalance">

chain3.mc.getBalance

.. raw:: html

   </h4>

::

    chain3.mc.getBalance(addressHexString [, defaultBlock] [, callback])

Get the balance of an address at a given block.

Parameters
''''''''''

1. ``String`` - The address to get the balance of.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - A BigNumber instance of the current balance for the given
address in sha.

See the `note on BigNumber <#a-note-on-big-numbers-in-javascript>`__.

Example
'''''''

.. code:: js

    var balance = chain3.mc.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
    console.log(balance); // instanceof BigNumber
    console.log(balance.toString(10)); // '1000000000000'
    console.log(balance.toNumber()); // 1000000000000

--------------

.. raw:: html

   <h4 id="chain3mcgetstorageat">

chain3.mc.getStorageAt

.. raw:: html

   </h4>

::

    chain3.mc.getStorageAt(addressHexString, position [, defaultBlock] [, callback])

Get the storage at a specific position of an address.

Parameters
''''''''''

1. ``String`` - The address to get the storage from.
2. ``Number`` - The index position of the storage.
3. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
4. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The value in storage at the given position.

Example
'''''''

.. code:: js

    var state = chain3.mc.getStorageAt("0x407d73d8a49eeb85d32cf465507dd71d507100c1", 0);
    console.log(state); // "0x03"

--------------

.. raw:: html

   <h4 id="chain3mcgetcode">

chain3.mc.getCode

.. raw:: html

   </h4>

::

    chain3.mc.getCode(addressHexString [, defaultBlock] [, callback])

Get the code at a specific address. For a contract address, return the
binary code of the contract.

Parameters
''''''''''

1. ``String`` - The address to get the code from.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The data at given address ``addressHexString``.

Example
'''''''

.. code:: js

    var code = chain3.mc.getCode("0x1d12aec505caa2b8513cdad9a13e9d4806a1b704");
    console.log(code); // "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"

--------------

.. raw:: html

   <h4 id="chain3mcgetblock">

chain3.mc.getBlock

.. raw:: html

   </h4>

::

     chain3.mc.getBlock(blockHashOrBlockNumber [, returnTransactionObjects] [, callback])

Returns a block matching the block number or block hash.

Parameters
''''''''''

1. ``String|Number`` - The block number or hash. Or the string
   ``"earliest"``, ``"latest"`` or ``"pending"`` as in the `default
   block parameter <#chain3mcdefaultblock>`__.
2. ``Boolean`` - (optional, default ``false``) If ``true``, the returned
   block will contain all transactions as objects, if ``false`` it will
   only contains the transaction hashes.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - The block object:

-  ``number``: ``Number`` - the block number. ``null`` when its pending
   block.
-  ``hash``: ``String``, 32 Bytes - hash of the block. ``null`` when its
   pending block.
-  ``parentHash``: ``String``, 32 Bytes - hash of the parent block.
-  ``nonce``: ``String``, 8 Bytes - hash of the generated proof-of-work.
   ``null`` when its pending block.
-  ``sha3Uncles``: ``String``, 32 Bytes - SHA3 of the uncles data in the
   block.
-  ``logsBloom``: ``String``, 256 Bytes - the bloom filter for the logs
   of the block. ``null`` when its pending block.
-  ``transactionsRoot``: ``String``, 32 Bytes - the root of the
   transaction trie of the block
-  ``stateRoot``: ``String``, 32 Bytes - the root of the final state
   trie of the block.
-  ``miner``: ``String``, 20 Bytes - the address of the beneficiary to
   whom the mining rewards were given.
-  ``difficulty``: ``BigNumber`` - integer of the difficulty for this
   block.
-  ``totalDifficulty``: ``BigNumber`` - integer of the total difficulty
   of the chain until this block.
-  ``extraData``: ``String`` - the "extra data" field of this block.
-  ``size``: ``Number`` - integer the size of this block in bytes.
-  ``gasLimit``: ``Number`` - the maximum gas allowed in this block.
-  ``gasUsed``: ``Number`` - the total used gas by all transactions in
   this block.
-  ``timestamp``: ``Number`` - the unix timestamp for when the block was
   collated.
-  ``transactions``: ``Array`` - Array of transaction objects, or 32
   Bytes transaction hashes depending on the last given parameter.
-  ``uncles``: ``Array`` - Array of uncle hashes.

Example
'''''''

.. code:: js

    var info = chain3.mc.getBlock(0);
    console.log(info);
    /*
    { difficulty: { [String: '100000000000'] s: 1, e: 11, c: [ 100000000000 ] },
      extraData: '0x31393639415250414e4554373354435049503039425443323031384d4f4143',
      gasLimit: 9000000,
      gasUsed: 0,
      hash: '0x6b9661646439fab926ffc9bccdf3abb572d5209ae59e3390abf76aee4e2d49cd',
      logsBloom: '0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000',
      miner: '0x0000000000000000000000000000000000000000',
      mixHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
      nonce: '0x0000000000000042',
      number: 0,
      parentHash: '0x0000000000000000000000000000000000000000000000000000000000000000',
      receiptsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
      sha3Uncles: '0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347',
      size: 540,
      stateRoot: '0xe221c9e4ad19514d7ce3e6b0bec3ad7f6cc293336e59c301cda293cfbda83df6',
      timestamp: 0,
      totalDifficulty: { [String: '100000000000'] s: 1, e: 11, c: [ 100000000000 ] },
      transactions: [],
      transactionsRoot: '0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421',
      uncles: [] }
    */

--------------

.. raw:: html

   <h4 id="chain3mcgetblocktransactioncount">

chain3.mc.getBlockTransactionCount

.. raw:: html

   </h4>

::

    chain3.mc.getBlockTransactionCount(hashStringOrBlockNumber [, callback])

Returns the number of transaction in a given block.

Parameters
''''''''''

1. ``String|Number`` - The block number or hash. Or the string
   ``"earliest"``, ``"latest"`` or ``"pending"`` as in the `default
   block parameter <#chain3mcdefaultblock>`__.
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Number`` - The number of transactions in the given block.

Example
'''''''

.. code:: js

    var number = chain3.mc.getBlockTransactionCount("0xddfb8508bff841242099e640efe59f5e5252be1a60fa701d333e1a8bfdee6263");
    console.log(number); // 1

--------------

.. raw:: html

   <h4 id="chain3mcgetuncle">

chain3.mc.getUncle

.. raw:: html

   </h4>

::

    chain3.mc.getUncle(blockHashStringOrNumber, uncleNumber [, returnTransactionObjects] [, callback])

Returns a blocks uncle by a given uncle index position.

Parameters
''''''''''

1. ``String|Number`` - The block number or hash. Or the string
   ``"earliest"``, ``"latest"`` or ``"pending"`` as in the `default
   block parameter <#chain3mcdefaultblock>`__.
2. ``Number`` - The index position of the uncle.
3. ``Boolean`` - (optional, default ``false``) If ``true``, the returned
   block will contain all transactions as objects, if ``false`` it will
   only contains the transaction hashes.
4. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - the returned uncle. For a return value see
`chain3.mc.getBlock() <#chain3mcgetblock>`__.

**Note**: An uncle doesn't contain individual transactions.

Example
'''''''

.. code:: js

    var uncle = chain3.mc.getUncle(500, 0);
    console.log(uncle); // see chain3.mc.getBlock

--------------

.. raw:: html

   <h4 id="chain3mcgettransaction">

chain3.mc.getTransaction

.. raw:: html

   </h4>

::

    chain3.mc.getTransaction(transactionHash [, callback])

Returns a transaction matching the given transaction hash.

Parameters
''''''''''

1. ``String`` - The transaction hash.
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - A transaction object its hash ``transactionHash``:

-  ``hash``: ``String``, 32 Bytes - hash of the transaction.
-  ``nonce``: ``Number`` - the number of transactions made by the sender
   prior to this one.
-  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
   transaction was in. ``null`` when its pending.
-  ``blockNumber``: ``Number`` - block number where this transaction was
   in. ``null`` when its pending.
-  ``transactionIndex``: ``Number`` - integer of the transactions index
   position in the block. ``null`` when its pending.
-  ``from``: ``String``, 20 Bytes - address of the sender.
-  ``to``: ``String``, 20 Bytes - address of the receiver. ``null`` when
   its a contract creation transaction.
-  ``value``: ``BigNumber`` - value transferred in Sha.
-  ``gasPrice``: ``BigNumber`` - gas price provided by the sender in
   Sha.
-  ``gas``: ``Number`` - gas provided by the sender.
-  ``input``: ``String`` - the data sent along with the transaction.

Example
'''''''

.. code:: js

    var txhash = "0x687751dd47684f4b5df263ae4ec39f54f057d0e2a1dde56f9d52766849c9c7fe";

    var transaction = chain3.mc.getTransaction(txhash);
    console.log(transaction);
    /*
    { blockHash: '0x716c602d49055b4e24fbe7da952c1ad81820ad0401f3cb3ce12e832fbcc368f5',
      blockNumber: 57259,
      from: '0x7cfd775c7a97aa632846eff35dcf9dbcf502d0f3',
      gas: 1000,
      gasPrice: { [String: '200000000000'] s: 1, e: 11, c: [ 200000000000 ] },
      hash: '0xf1c1771204431c1c584e793b49d41586a923c370be93673aac42d66252bc8d0a',
      input: '0x',
      nonce: 840,
      syscnt: '0x0',
      to: '0x3435410589ebd06b74079a1e141759d8502aeb8b',
      transactionIndex: 689,
      value: { [String: '1000000000'] s: 1, e: 9, c: [ 1000000000 ] },
      v: '0xea',
      r: '0x77aa72d0900e436b60f8be377e43dead3567241a85fbeb5517283e2dc8aca2b4',
      s: '0x24477ec130fc4101289b37e6107acfe64026c898994273110faa8f19b8ea6985',
      shardingFlag: '0x0' }
    */

--------------

.. raw:: html

   <h4 id="chain3mcgettransactionfromBlock">

chain3.mc.getTransactionFromBlock

.. raw:: html

   </h4>

::

    getTransactionFromBlock(hashStringOrNumber, indexNumber [, callback])

Returns a transaction based on a block hash or number and the
transactions index position.

Parameters
''''''''''

1. ``String`` - A block number or hash. Or the string ``"earliest"``,
   ``"latest"`` or ``"pending"`` as in the `default block
   parameter <#chain3mcdefaultblock>`__.
2. ``Number`` - The transactions index position.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - A transaction object, see
`chain3.mc.getTransaction <#chain3mcgettransaction>`__:

Example
'''''''

.. code:: js

    var transaction = chain3.mc.getTransactionFromBlock(921, 2);
    console.log(transaction); // see chain3.mc.getTransaction

--------------

.. raw:: html

   <h4 id="chain3mcgettransactionreceipt">

chain3.mc.getTransactionReceipt

.. raw:: html

   </h4>

::

    chain3.mc.getTransactionReceipt(hashString [, callback])

Returns the receipt of a transaction by transaction hash.

**Note** That the receipt is not available for pending transactions.

Parameters
''''''''''

1. ``String`` - The transaction hash.
2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Object`` - A transaction receipt object, or ``null`` when no receipt
was found:

-  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
   transaction was in.
-  ``blockNumber``: ``Number`` - block number where this transaction was
   in.
-  ``transactionHash``: ``String``, 32 Bytes - hash of the transaction.
-  ``transactionIndex``: ``Number`` - integer of the transactions index
   position in the block.
-  ``from``: ``String``, 20 Bytes - address of the sender.
-  ``to``: ``String``, 20 Bytes - address of the receiver. ``null`` when
   its a contract creation transaction.
-  ``cumulativeGasUsed``: ``Number`` - The total amount of gas used when
   this transaction was executed in the block.
-  ``gasUsed``: ``Number`` - The amount of gas used by this specific
   transaction alone.
-  ``contractAddress``: ``String`` - 20 Bytes - The contract address
   created, if the transaction was a contract creation, otherwise
   ``null``.
-  ``logs``: ``Array`` - Array of log objects, which this transaction
   generated.

Example
'''''''

.. code:: js

    var receipt = chain3.mc.getTransactionReceipt('0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b');
    console.log(receipt);
    {
      "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
      "transactionIndex": 0,
      "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
      "blockNumber": 3,
      "contractAddress": "0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b",
      "cumulativeGasUsed": 314159,
      "gasUsed": 30234,
      "logs": [{
             // logs as returned by getFilterLogs, etc.
         }, ...]
    }

--------------

.. raw:: html

   <h4 id="chain3mcgettransactioncount">

chain3.mc.getTransactionCount

.. raw:: html

   </h4>

::

    chain3.mc.getTransactionCount(addressHexString [, defaultBlock] [, callback])

Get the numbers of transactions sent from this address.

Parameters
''''''''''

1. ``String`` - The address to get the numbers of transactions from.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``Number`` - The number of transactions sent from the given address.

Example
'''''''

.. code:: js

    var number = chain3.mc.getTransactionCount("0x407d73d8a49eeb85d32cf465507dd71d507100c1");
    console.log(number); // 1

--------------

.. raw:: html

   <h4 id="chain3mcsendtransaction">

chain3.mc.sendTransaction

.. raw:: html

   </h4>

::

    chain3.mc.sendTransaction(transactionObject [, callback])

Sends a transaction to the network.

Parameters
''''''''''

1. ``Object`` - The transaction object to send:

-  ``from``: ``String`` - The address for the sending account. Uses the
   `chain3.mc.defaultAccount <#chain3mcdefaultaccount>`__ property, if
   not specified.
-  ``to``: ``String`` - (optional) The destination address of the
   message, can be an account address or a contract address, left
   undefined for a contract-creation transaction.
-  ``value``: ``Number|String|BigNumber`` - (optional) The value
   transferred for the transaction in Sha, also the endowment if it's a
   contract-creation transaction.
-  ``gas``: ``Number|String|BigNumber`` - (optional, default:
   To-Be-Determined) The amount of gas to use for the transaction
   (unused gas is refunded).
-  ``gasPrice``: ``Number|String|BigNumber`` - (optional, default:
   To-Be-Determined) The price of gas for this transaction in sha,
   defaults to the mean network gas price.
-  ``data``: ``String`` - (optional) Either a byte string containing the
   associated data of the message, or in the case of a contract-creation
   transaction, the initialisation code.
-  ``nonce``: ``Number`` - (optional) Integer of a nonce. This allows to
   overwrite your own pending transactions that use the same nonce.
-  ``shardingFlag``: ``Number|String`` - (optional) Set to 1 for
   micro/subchain direct contract-call, 0 or undefine for global
   contract-creation transaction.

2. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The 32 Bytes transaction hash as HEX string.

If the transaction was a contract creation use
`chain3.mc.getTransactionReceipt() <#chain3mcgettransactionreceipt>`__
to get the contract address, after the transaction was mined.

Example
'''''''

.. code:: js


    // compiled solidity source code using https://chriseth.github.io/cpp-ethereum/
    var code = "603d80600c6000396000f3007c01000000000000000000000000000000000000000000000000000000006000350463c6888fa18114602d57005b600760043502
    8060005260206000f3";

    chain3.mc.sendTransaction({data: code}, function(err, address) {
      if (!err)
        console.log(address); // "0x7f9fade1c0d57a7af66ab4ead7c2eb7b11a91385"
    });

--------------

.. raw:: html

   <h4 id="chain3mccall">

chain3.mc.call

.. raw:: html

   </h4>

::

    chain3.mc.call(callObject [, defaultBlock] [, callback])

Executes a message call transaction, which is directly executed in the
VM of the node, but never mined into the blockchain.

Parameters
''''''''''

1. ``Object`` - A transaction object see
   `chain3.mc.sendTransaction <#chain3mcsendtransaction>`__, with the
   difference that for calls the ``from`` property is optional as well.
2. ``Number|String`` - (optional) If you pass this parameter it will not
   use the default block set with
   `chain3.mc.defaultBlock <#chain3mcdefaultblock>`__.
3. ``Function`` - (optional) If you pass a callback the HTTP request is
   made asynchronous. See `this note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - The returned data of the call, e.g. a codes functions
return value.

Example
'''''''

.. code:: js

    var result = chain3.mc.call({
        to: "0xc4abd0339eb8d57087278718986382264244252f",
        data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
    });
    console.log(result); // "0x0000000000000000000000000000000000000000000000000000000000000015"

--------------

.. raw:: html

   <h4 id="chain3mcestimategas">

chain3.mc.estimateGas

.. raw:: html

   </h4>

::

    chain3.mc.estimateGas(callObject [, defaultBlock] [, callback])

Executes a message call or transaction, which is directly executed in
the VM of the node, but never mined into the blockchain and returns the
amount of the gas used.

Parameters
''''''''''

See `chain3.mc.sendTransaction <#chain3mcsendtransaction>`__, expect
that all properties are optional.

Returns
'''''''

``Number`` - the used gas for the simulated call/transaction.

Example
'''''''

.. code:: js

    var result = chain3.mc.estimateGas({
        to: "0xc4abd0339eb8d57087278718986382264244252f",
        data: "0xc6888fa10000000000000000000000000000000000000000000000000000000000000003"
    });
    console.log(result); // "1465"

--------------

.. raw:: html

   <h4 id="chain3mcfilter">

chain3.mc.filter

.. raw:: html

   </h4>

.. code:: js

    // can be 'latest' or 'pending'
    var filter = chain3.mc.filter(filterString);
    // OR object are log filter options
    var filter = chain3.mc.filter(options);

    // watch for changes
    filter.watch(function(error, result){
      if (!error)
        console.log(result);
    });

    // Additionally you can start watching right away, by passing a callback:
    chain3.mc.filter(options, function(error, result){
      if (!error)
        console.log(result);
    });

Parameters
''''''''''

1. ``String|Object`` - The string ``"latest"`` or ``"pending"`` to watch
   for changes in the latest block or pending transactions respectively.
   Or a filter options object as follows:

-  ``fromBlock``: ``Number|String`` - The number of the earliest block
   (``latest`` may be given to mean the most recent and ``pending``
   currently mining, block). By default ``latest``.
-  ``toBlock``: ``Number|String`` - The number of the latest block
   (``latest`` may be given to mean the most recent and ``pending``
   currently mining, block). By default ``latest``.
-  ``address``: ``String`` - An address or a list of addresses to only
   get logs from particular account(s).
-  ``topics``: ``Array of Strings`` - An array of values which must each
   appear in the log entries. The order is important, if you want to
   leave topics out use ``null``, e.g. ``[null, '0x00...']``. You can
   also pass another array for each topic with options for that topic
   e.g. ``[null, ['option1', 'option2']]``

Returns
'''''''

``Object`` - A filter object with the following methods:

-  ``filter.get(callback)``: Returns all of the log entries that fit the
   filter.
-  ``filter.watch(callback)``: Watches for state changes that fit the
   filter and calls the callback. See `this note <#using-callbacks>`__
   for details.
-  ``filter.stopWatching()``: Stops the watch and uninstalls the filter
   in the node. Should always be called once it is done.

Watch callback return value
'''''''''''''''''''''''''''

-  ``String`` - When using the ``"latest"`` parameter, it returns the
   block hash of the last incoming block.
-  ``String`` - When using the ``"pending"`` parameter, it returns a
   transaction hash of the last add pending transaction.
-  ``Object`` - When using manual filter options, it returns a log
   object as follows:

   -  ``logIndex``: ``Number`` - integer of the log index position in
      the block. ``null`` when its pending log.
   -  ``transactionIndex``: ``Number`` - integer of the transactions
      index position log was created from. ``null`` when its pending
      log.
   -  ``transactionHash``: ``String``, 32 Bytes - hash of the
      transactions this log was created from. ``null`` when its pending
      log.
   -  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
      log was in. ``null`` when its pending. ``null`` when its pending
      log.
   -  ``blockNumber``: ``Number`` - the block number where this log was
      in. ``null`` when its pending. ``null`` when its pending log.
   -  ``address``: ``String``, 32 Bytes - address from which this log
      originated.
   -  ``data``: ``String`` - contains one or more 32 Bytes non-indexed
      arguments of the log.
   -  ``topics``: ``Array of Strings`` - Array of 0 to 4 32 Bytes
      ``DATA`` of indexed log arguments. (In *solidity*: The first topic
      is the *hash* of the signature of the event (e.g.
      ``Deposit(address,bytes32,uint256)``), except you declared the
      event with the ``anonymous`` specifier.)

**Note** For event filter return values see `Contract
Events <#contract-events>`__

Example
'''''''

.. code:: js

    var filter = chain3.mc.filter('pending');

    filter.watch(function (error, log) {
      console.log(log); //  {"address":"0x0000000000000000000000000000000000000000", "data":"0x0000000000000000000000000000000000000000000000000000000000000000", ...}
    });

    // get all past logs again.
    var myResults = filter.get(function(error, logs){ ... });

    ...

    // stops and uninstalls the filter
    filter.stopWatching();

--------------

.. raw:: html

   <h4 id="chain3mccontract">

chain3.mc.contract

.. raw:: html

   </h4>

::

    chain3.mc.contract(abiArray)

Creates a contract object for a solidity contract, which can be used to
initiate contracts on an address. You can read more about events
`here <https://github.com/MOACChain/wiki/wiki/MOAC-Contract-ABI#example-javascript-usage>`__.

Parameters
''''''''''

1. ``Array`` - ABI array with descriptions of functions and events of
   the contract.

Returns
'''''''

``Object`` - A contract object, which can be initiated as follows:

.. code:: js

    var MyContract = chain3.mc.contract(abiArray);

    // instantiate by address
    var contractInstance = MyContract.at([address]);

    // deploy new contract
    var contractInstance = MyContract.new([contructorParam1] [, contructorParam2], {data: '0x12345...', from: myAccount, gas: 1000000});

    // Get the data to deploy the contract manually
    var contractData = MyContract.new.getData([contructorParam1] [, contructorParam2], {data: '0x12345...'});
    // contractData = '0x12345643213456000000000023434234'

And then you can either initiate an existing contract on an address, or
deploy the contract using the compiled byte code:

.. code:: js

    // Instantiate from an existing address:
    var myContractInstance = MyContract.at(myContractAddress);


    // Or deploy a new contract:

    // Deploy the contract asyncronous:
    var myContractReturned = MyContract.new(param1, param2, {
       data: myContractCode,
       gas: 300000,
       from: mySenderAddress}, function(err, myContract){
        if(!err) {
           // NOTE: The callback will fire twice!
           // Once the contract has the transactionHash property set and once its deployed on an address.

           // e.g. check tx hash on the first call (transaction send)
           if(!myContract.address) {
               console.log(myContract.transactionHash) // The hash of the transaction, which deploys the contract

           // check address on the second call (contract deployed)
           } else {
               console.log(myContract.address) // the contract address
           }

           // Note that the returned "myContractReturned" === "myContract",
           // so the returned "myContractReturned" object will also get the address set.
        }
      });

    // Deploy contract syncronous: The address will be added as soon as the contract is mined.
    // Additionally you can watch the transaction by using the "transactionHash" property
    var myContractInstance = MyContract.new(param1, param2, {data: myContractCode, gas: 300000, from: mySenderAddress});
    myContractInstance.transactionHash // The hash of the transaction, which created the contract
    myContractInstance.address // undefined at start, but will be auto-filled later

**Note** When you deploy a new contract, you should check for the next
12 blocks or so if the contract code is still at the address (using
`chain3.mc.getCode() <#chain3mcgetcode>`__), to make sure a fork didn't
change that.

Example
'''''''

.. code:: js

    // contract abi
    var abi = [{
         name: 'myConstantMethod',
         type: 'function',
         constant: true,
         inputs: [{ name: 'a', type: 'string' }],
         outputs: [{name: 'd', type: 'string' }]
    }, {
         name: 'myStateChangingMethod',
         type: 'function',
         constant: false,
         inputs: [{ name: 'a', type: 'string' }, { name: 'b', type: 'int' }],
         outputs: []
    }, {
         name: 'myEvent',
         type: 'event',
         inputs: [{name: 'a', type: 'int', indexed: true},{name: 'b', type: 'bool', indexed: false]
    }];

    // creation of contract object
    var MyContract = chain3.mc.contract(abi);

    // initiate contract for an address
    var myContractInstance = MyContract.at('0xc4abd0339eb8d57087278718986382264244252f');

    // call constant function
    var result = myContractInstance.myConstantMethod('myParam');
    console.log(result) // '0x25434534534'

    // send a transaction to a function
    myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000});

    // short hand style
    chain3.mc.contract(abi).at(address).myAwesomeMethod(...);

    // create filter
    var filter = myContractInstance.myEvent({a: 5}, function (error, result) {
      if (!error)
        console.log(result);
        /*
        {
            address: '0x8718986382264244252fc4abd0339eb8d5708727',
            topics: "0x12345678901234567890123456789012", "0x0000000000000000000000000000000000000000000000000000000000000005",
            data: "0x0000000000000000000000000000000000000000000000000000000000000001",
            ...
        }
        */
    });

--------------

Contract Methods
^^^^^^^^^^^^^^^^

.. code:: js

    // Automatically determines the use of call or sendTransaction based on the method type
    myContractInstance.myMethod(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Explicitly calling this method
    myContractInstance.myMethod.call(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Explicitly sending a transaction to this method
    myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Explicitly sending a transaction to this method
    myContractInstance.myMethod.sendTransaction(param1 [, param2, ...] [, transactionObject] [, callback]);

    // Get the call data, so you can call the contract through some other means
    var myCallData = myContractInstance.myMethod.getData(param1 [, param2, ...]);
    // myCallData = '0x45ff3ff6000000000004545345345345..'

The contract object exposes the contracts methods, which can be called
using parameters and a transaction object.

Parameters
''''''''''

-  ``String|Number`` - (optional) Zero or more parameters of the
   function.
-  ``Object`` - (optional) The (previous) last parameter can be a
   transaction object, see
   `chain3.mc.sendTransaction <#chain3mcsendtransaction>`__ parameter 1
   for more.
-  ``Function`` - (optional) If you pass a callback as the last
   parameter the HTTP request is made asynchronous. See `this
   note <#using-callbacks>`__ for details.

Returns
'''''''

``String`` - If its a call the result data, if its a send transaction a
created contract address, or the transaction hash, see
`chain3.mc.sendTransaction <#chain3mcsendtransaction>`__ for details.

Example
'''''''

.. code:: js

    // creation of contract object
    var MyContract = chain3.mc.contract(abi);

    // initiate contract for an address
    var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

    var result = myContractInstance.myConstantMethod('myParam');
    console.log(result) // '0x25434534534'

    myContractInstance.myStateChangingMethod('someParam1', 23, {value: 200, gas: 2000}, function(err, result){ ... });

--------------

Contract Events
^^^^^^^^^^^^^^^

.. code:: js

    var event = myContractInstance.MyEvent({valueA: 23} [, additionalFilterObject])

    // watch for changes
    event.watch(function(error, result){
      if (!error)
        console.log(result);
    });

    // Or pass a callback to start watching immediately
    var event = myContractInstance.MyEvent([{valueA: 23}] [, additionalFilterObject] , function(error, result){
      if (!error)
        console.log(result);
    });

You can use events like `filters <#chain3mcfilter>`__ and they have the
same methods, but you pass different objects to create the event filter.

Parameters
''''''''''

1. ``Object`` - Indexed return values you want to filter the logs by,
   e.g. ``{'valueA': 1, 'valueB': [myFirstAddress, mySecondAddress]}``.
   By default all filter values are set to ``null``. It means, that they
   will match any event of given type sent from this contract.
2. ``Object`` - Additional filter options, see
   `filters <#chain3mcfilter>`__ parameter 1 for more. By default
   filterObject has field 'address' set to address of the contract. Also
   first topic is the signature of event.
3. ``Function`` - (optional) If you pass a callback as the last
   parameter it will immediately start watching and you don't need to
   call ``myEvent.watch(function(){})``. See `this
   note <#using-callbacks>`__ for details.

Callback return
'''''''''''''''

``Object`` - An event object as follows:

-  ``args``: ``Object`` - The arguments coming from the event.
-  ``event``: ``String`` - The event name.
-  ``logIndex``: ``Number`` - integer of the log index position in the
   block.
-  ``transactionIndex``: ``Number`` - integer of the transactions index
   position log was created from.
-  ``transactionHash``: ``String``, 32 Bytes - hash of the transactions
   this log was created from.
-  ``address``: ``String``, 32 Bytes - address from which this log
   originated.
-  ``blockHash``: ``String``, 32 Bytes - hash of the block where this
   log was in. ``null`` when its pending.
-  ``blockNumber``: ``Number`` - the block number where this log was in.
   ``null`` when its pending.

Example
'''''''

.. code:: js

    var MyContract = chain3.mc.contract(abi);
    var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

    // watch for an event with {some: 'args'}
    var myEvent = myContractInstance.MyEvent({some: 'args'}, {fromBlock: 0, toBlock: 'latest'});
    myEvent.watch(function(error, result){
       ...
    });

    // would get all past logs again.
    var myResults = myEvent.get(function(error, logs){ ... });

    ...

    // would stop and uninstall the filter
    myEvent.stopWatching();

--------------

Contract allEvents
^^^^^^^^^^^^^^^^^^

.. code:: js

    var events = myContractInstance.allEvents([additionalFilterObject]);

    // watch for changes
    events.watch(function(error, event){
      if (!error)
        console.log(event);
    });

    // Or pass a callback to start watching immediately
    var events = myContractInstance.allEvents([additionalFilterObject,] function(error, log){
      if (!error)
        console.log(log);
    });

Will call the callback for all events which are created by this
contract.

Parameters
''''''''''

1. ``Object`` - Additional filter options, see
   `filters <#chain3mcfilter>`__ parameter 1 for more. By default
   filterObject has field 'address' set to address of the contract. Also
   first topic is the signature of event.
2. ``Function`` - (optional) If you pass a callback as the last
   parameter it will immediately start watching and you don't need to
   call ``myEvent.watch(function(){})``. See `this
   note <#using-callbacks>`__ for details.

Callback return
'''''''''''''''

``Object`` - See `Contract Events <#contract-events>`__ for more.

Example
'''''''

.. code:: js

    var MyContract = chain3.mc.contract(abi);
    var myContractInstance = MyContract.at('0x78e97bcc5b5dd9ed228fed7a4887c0d7287344a9');

    // watch for an event with {some: 'args'}
    var events = myContractInstance.allEvents({fromBlock: 0, toBlock: 'latest'});
    events.watch(function(error, result){
       ...
    });

    // would get all past logs again.
    events.get(function(error, logs){ ... });

    ...

    // would stop and uninstall the filter
    myEvent.stopWatching();

--------------

.. raw:: html

   <h4 id="chain3encodeParams">

chain3.encodeParams

.. raw:: html

   </h4>

::

    chain3.encodeParams

Encode a list of parameters array into HEX codes. ##### Parameters

-  ``types`` - list of types of params
-  ``params`` - list of values of params

Example
'''''''

.. code:: js

    var types = ['int','string'];
    var args = [100, '4000'];

    var dataHex = '0x' + chain3.encodeParams(types, args);
    console.log("encoded params:", dataHex);

    // outputs
    encoded params:0x0000000000000000000000000000000000000000000000000000000000000064000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000043430303000000000000000000000000000000000000000000000000000000000

--------------

.. raw:: html

   <h4 id="chain3mcnamereg">

chain3.mc.namereg

.. raw:: html

   </h4>

::

    chain3.mc.namereg

Returns GlobalRegistrar object.

Usage
'''''

see
`namereg <https://github.com/MOACChain/chain3.js/blob/master/example/namereg.html>`__
example

--------------

.. raw:: html

   <h4 id="chain3mcsendibantransaction">

chain3.mc.sendIBANTransaction

.. raw:: html

   </h4>

.. code:: js

    var txHash = chain3.mc.sendIBANTransaction('0x00c5496aee77c1ba1f0854206a26dda82a81d6d8', 'XE66MOACXREGGAVOFYORK', 0x100);

Sends IBAN transaction from user account to destination IBAN address.

Parameters
''''''''''

-  ``string`` - address from which we want to send transaction
-  ``string`` - IBAN address to which we want to send transaction
-  ``value`` - value that we want to send in IBAN transaction

--------------

.. raw:: html

   <h4 id="chain3mciban">

chain3.mc.iban

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE81ETHXREGGAVOFYORK");

--------------

.. raw:: html

   <h4 id="chain3mcibanfromaddress">

chain3.mc.iban.fromAddress

.. raw:: html

   </h4>

.. code:: js

    var i = chain3.mc.iban.fromAddress('0xd814f2ac2c4ca49b33066582e4e97ebae02f2ab9');
    console.log(i.toString()); // 'XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT

--------------

.. raw:: html

   <h4 id="chain3mcibanfrombban">

chain3.mc.iban.fromBban

.. raw:: html

   </h4>

.. code:: js

    var i = chain3.mc.iban.fromBban('XE66MOACXREGGAVOFYORK');
    console.log(i.toString()); // "XE71XE66MOACXREGGAVOFYORK"

--------------

.. raw:: html

   <h4 id="chain3mcibancreateindirect">

chain3.mc.iban.createIndirect

.. raw:: html

   </h4>

.. code:: js

    var i = chain3.mc.iban.createIndirect({
      institution: "XREG",
      identifier: "GAVOFYORK"
    });
    console.log(i.toString()); // "XE66MOACXREGGAVOFYORK"

--------------

.. raw:: html

   <h4 id="chain3mcibanisvalid">

chain3.mc.iban.isValid

.. raw:: html

   </h4>

.. code:: js

    var valid = chain3.mc.iban.isValid("XE66MOACXREGGAVOFYORK");
    console.log(valid); // true

    var valid2 = chain3.mc.iban.isValid("XE76MOACXREGGAVOFYORK");
    console.log(valid2); // false, cause checksum is incorrect

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var valid3 = i.isValid();
    console.log(valid3); // true

--------------

.. raw:: html

   <h4 id="chain3mcibanisdirect">

chain3.mc.iban.isDirect

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var direct = i.isDirect();
    console.log(direct); // false

--------------

.. raw:: html

   <h4 id="chain3mcibanisindirect">

chain3.mc.iban.isIndirect

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var indirect = i.isIndirect();
    console.log(indirect); // true

--------------

.. raw:: html

   <h4 id="chain3mcibanchecksum">

chain3.mc.iban.checksum

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var checksum = i.checksum();
    console.log(checksum); // "66"

--------------

.. raw:: html

   <h4 id="chain3mcibaninstitution">

chain3.mc.iban.institution

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var institution = i.institution();
    console.log(institution); // 'XREG'

--------------

.. raw:: html

   <h4 id="chain3mcibanclient">

chain3.mc.iban.client

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban("XE66MOACXREGGAVOFYORK");
    var client = i.client();
    console.log(client); // 'GAVOFYORK'

--------------

.. raw:: html

   <h4 id="chain3mcibanaddress">

chain3.mc.iban.address

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban('XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT');
    var address = i.address();
    console.log(address); // 'd814f2ac2c4ca49b33066582e4e97ebae02f2ab9'

--------------

.. raw:: html

   <h4 id="chain3mcibantostring">

chain3.mc.iban.toString

.. raw:: html

   </h4>

.. code:: js

    var i = new chain3.mc.iban('XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT');
    console.log(i.toString()); // 'XE72P8O19KRSWXUGDY294PZ66T4ZGF89INT'

--------------

VNODE
~~~~~

**chain3.vnode.address**

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
    console.log(chain3.vnode.address); // '0xa8863fc8ce3816411378685223c03daae9770ebb'

--------------

**chain3.vnode.scsService**

.. _vnode_scsService:

Returns if the VNODE enable the service for SCS servers.

*Parameters*


none

*Returns*


``Bool`` - true, enable the SCS service; false, not open.

Example


.. code:: js

    // Request
    console.log(chain3.vnode.address()); // true/false


--------------

**chain3.vnode.serviceCfg**

.. _vnode_serviceCfg:

Returns the VNODE SCS service ports for connecting with.

*Parameters*

none

*Returns*


``String`` - The current service port set in the vnodeconfig.json.

Example


.. code:: js

    // Request
    console.log("VNODE serviceCfg:", chain3.vnode.serviceCfg);//

--------------

**chain3.vnode.showToPublic**

.. _vnode_showToPublic:

Returns if the VNODE enable the public view.

*Parameters*

none

*Returns*


``Bool`` - true, open to the public; false, not open.

Example


.. code:: js

    // Request
    console.log("VNODE showtoPublic:", chain3.vnode.showToPublic);

--------------

**chain3.vnode.ip**

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
    console.log("VNODE IP:", chain3.vnode.ip);// return null for local host

--------------

SCS
~~~~~

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


.. _scs_directcall:

**scs_directCall**

Executes a new constant call of the MicroChain Dapp function without
creating a transaction on the MicroChain. This RPC call is used by
API/lib to call MicroChain Dapp functions.

*Parameters*


``Object`` - The transaction call object 

- ``from``: ``DATA``, 20 Bytes - (optional) The address the transaction is sent from. 

- ``to``: ``DATA``, 20 Bytes - The address the transaction is directed to. This parameter is the MicroChain address. 

- ``data``: ``DATA`` - (optional) Hash of the method signature and encoded parameters. For details see `Ethereum Contract ABI <https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI>`

*Returns*


``DATA`` - the return value of executed Dapp constant function call.

Example


.. code:: js

    // Request
    var mclist = chain3.scs.getMicroChainList();
    mcAddress = mclist[0];
    console.log("SCS block:", chain3.scs.getBlock(mcAddress, 1));

    // Result
    {
      "id":101,
      "jsonrpc": "2.0",
      "result": "0x"
    }

--------------

**chain3.scs.getBlock**

.. _scs_getblock:

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

.. _scs_getblocklist:

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

.. _scs_getblocknumber:

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

.. _scs_getdapplist:

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
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("SCS state:", chain3.scs.getDappState(mcAddress));

    // Result
    SCS state: 1

--------------
**chain3.scs.getMicroChainInfo**

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
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    console.log("MC info:", chain3.scs.getMicroChainInfo(mcAddress));

    // Result
    MC info: {"balance":"0x0","blockReward":"0x1c6bf52634000","bondLimit":"0xde0b6b3a7640000","owner":"0xa8863fc8Ce3816411378685223C03DAae9770ebB","scsList":["0xECd1e094Ee13d0B47b72F5c940C17bD0c7630326","0x50C15fafb95968132d1a6ee3617E99cCa1FCF059","0x1b65cE1A393FFd5960D2ce11E7fd6fDB9e991945"],"txReward":"0x174876e800","viaReward":"0x9184e72a000"}

--------------
**chain3.scs.getMicroChainList**

.. _scs_getmicrochainlist:

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
    var mclist = chain3.scs.getMicroChainList(); //find the MicroChain on the SCS
    mcAddress = mclist[0]; //locate the 1st MicroChain
    tAddress="0xf6a36118751c50f8932d31d6d092b11cc28f2258";
    console.log("SCS nonce of:", tAddress, " is ", chain3.scs.getNonce(mcAddress,tAddress));

    // Result
    SCS nonce of: 0xf6a36118751c50f8932d31d6d092b11cc28f2258  is  3

--------------

**chain3.scs.getSCSId**

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
    console.log("SCS ID:", chain3.scs.getSCSId());

    // Result
    SCS ID: 0xecd1e094ee13d0b47b72f5c940c17bd0c7630326

--------------

**chain3.scs.getReceiptByHash**

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

.. _scs_getReceiptByNonce:

Returns the transaction result by address and nonce on the MicroChain. Note That the nonce is the nonce on the MicroChain. This nonce can be checked using scs_getNonce. 

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
    console.log("SCS receipt:", chain3.scs.getReceiptByNonce(mcAddress, tAddress, 0));

    // Result
    SCS receipt: {contractAddress: '0x0a674edac2ccd47ae2a3197ea3163aa81087fbd1',
  failed: false,"logs":[{"address":"0x2328537bc943ab1a89fe94a4b562ee7a7b013634","topics":["0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef","0x000000000000000000000000a8863fc8ce3816411378685223c03daae9770ebb","0x0000000000000000000000007312f4b8a4457a36827f185325fd6b66a3f8bb8b"],"data":"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGQ=","blockNumber":0,"transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69","transactionIndex":0,"blockHash":"0x78f092ca81a891ad6c467caa2881d00d8e19c8925ddfd71d793294fbfc5f15fe","logIndex":0,"removed":false}],"logsBloom":"0x00000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000008000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000800000000000080000000000000000000000000002000000000000000000000000000000000000080100002000000000000000000000000000000000000000000000000000000000000000000000000000","status":"0x1","transactionHash":"0x67bfaa5a704e77a31d5e7eb866f8c662fa8313a7882d13d0d23e377cd66d2a69"}
    

--------------

**chain3.scs.getTransactionByHash**

.. _scs_gettransactionbyhash:

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

.. _scs_gettransactionbynonce:

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

.. _scs_gettxpool:

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

