options
=====================

An Chain3 module does provide several options for configuring the transaction confirmation worklfow or for defining default values.
These are the currently available option properties on a Chain3 module:

.. _chain3-module-options:

--------------
Module Options
--------------

:ref:`defaultAccount <chain3-module-defaultaccount>`

:ref:`defaultBlock <chain3-module-defaultblock>`

:ref:`defaultGas <chain3-module-defaultgas>`

:ref:`defaultGasPrice <chain3-module-defaultaccount>`

:ref:`transactionBlockTimeout <chain3-module-transactionblocktimeout>`

:ref:`transactionConfirmationBlocks <chain3-module-transactionconfirmationblocks>`

:ref:`transactionPollingTimeout <chain3-module-transactionpollingtimeout>`

:ref:`transactionSigner <chain3-module-transactionSigner>`

-------
Example
-------

.. code-block:: javascript

    import Chain3 from 'chain3';

    const options = {
        defaultAccount: '0x0',
        defaultBlock: 'latest',
        defaultGas: 1,
        defaultGasPrice: 0,
        transactionBlockTimeout: 50,
        transactionConfirmationBlocks: 24,
        transactionPollingTimeout: 480,
        transactionSigner: new CustomTransactionSigner()
    }

    const chain3 = new Chain3('http://localhost:8545', null, options);

------------------------------------------------------------------------------

.. _chain3-module-defaultblock:

defaultBlock
=====================

.. code-block:: javascript

    chain3.defaultBlock
    chain3.mc.defaultBlock
    ...

The default block is used for all methods which have a block parameter.
You can override it by passing the block parameter if a block is required.

Example:

- :ref:`chain3.mc.getBalance() <eth-getbalance>`
- :ref:`chain3.mc.getCode() <eth-code>`
- :ref:`chain3.mc.getTransactionCount() <eth-gettransactioncount>`
- :ref:`chain3.mc.getStorageAt() <eth-getstorageat>`
- :ref:`chain3.mc.call() <eth-call>`
- :ref:`new chain3.mc.Contract() -> myContract.methods.myMethod().call() <contract-call>`

-------
Returns
-------

The ``defaultBlock`` property can return the following values:

- ``Number``: A block number
- ``"genesis"`` - ``String``: The genesis block
- ``"latest"`` - ``String``: The latest block (current head of the blockchain)
- ``"pending"`` - ``String``: The currently mined block (including pending transactions)

Default is ``"latest"``

------------------------------------------------------------------------------

.. _chain3-module-defaultaccount:

defaultAccount
=====================

.. code-block:: javascript

    chain3.defaultAccount
    chain3.mc.defaultAccount
    ...

This default address is used as the default ``"from"`` property, if no ``"from"`` property is specified.

-------
Returns
-------

``String`` - 20 Bytes: Any Ethereum address. You need to have the private key for that address in your node or keystore. (Default is ``undefined``)

------------------------------------------------------------------------------

.. _chain3-module-defaultgasprice:

defaultGasPrice
=====================

.. code-block:: javascript

    chain3.defaultGasPrice
    chain3.mc.defaultGasPrice
    ...

The default gas price which will be used for a request.

-------
Returns
-------

``string|number``: The current value of the defaultGasPrice property.


------------------------------------------------------------------------------

.. _chain3-module-defaultgas:

defaultGas
=====================

.. code-block:: javascript

    chain3.defaultGas
    chain3.mc.defaultGas
    ...

The default gas which will be used for a request.

-------
Returns
-------

``string|number``: The current value of the defaultGas property.

------------------------------------------------------------------------------

.. _chain3-module-transactionblocktimeout:

transactionBlockTimeout
=====================

.. code-block:: javascript

    chain3.transactionBlockTimeout
    chain3.mc.transactionBlockTimeout
    ...

The ``transactionBlockTimeout`` will be used over a socket based connection. This option does define the amount of new blocks it should wait until the first confirmation happens.
This means the PromiEvent rejects with a timeout error when the timeout got exceeded.


-------
Returns
-------

``number``: The current value of transactionBlockTimeout

------------------------------------------------------------------------------

.. _chain3-module-transactionconfirmationblocks:

transactionConfirmationBlocks
=====================

.. code-block:: javascript

    chain3.transactionConfirmationBlocks
    chain3.mc.transactionConfirmationBlocks
    ...

This defines the number of blocks it requires until a transaction will be handled as confirmed.


-------
Returns
-------

``number``: The current value of transactionConfirmationBlocks

------------------------------------------------------------------------------


.. _chain3-module-transactionpollingtimeout:

transactionPollingTimeout
=====================

.. code-block:: javascript

    chain3.transactionPollingTimeout
    chain3.mc.transactionPollingTimeout
    ...

The ``transactionPollingTimeout``  will be used over a HTTP connection.
This option does define the amount of polls (each second) it should wait until the first confirmation happens.


-------
Returns
-------

``number``: The current value of transactionPollingTimeout

------------------------------------------------------------------------------


.. _chain3-module-transactionSigner:

transactionSigner
=================

.. code-block:: javascript

    chain3.mc.transactionSigner
    ...



The ``transactionSigner`` property does provide us the possibility to customize the signing process
of the ``Mc`` module and the related sub-modules.

The interface of a ``TransactionSigner``:

.. code-block:: javascript

    interface TransactionSigner {
        sign(txObject: Transaction): Promise<SignedTransaction>
    }

    interface SignedTransaction {
        messageHash: string,
        v: string,
        r: string,
        s: string,
        rawTransaction: string
    }



-------
Returns
-------

``TransactionSigner``: A JavaScript class of type TransactionSigner.

------------------------------------------------------------------------------

setProvider
=====================

.. code-block:: javascript

    chain3.setProvider(myProvider)
    chain3.mc.setProvider(myProvider)
    ...

Will change the provider for its module.

.. note:: When called on the umbrella package ``chain3`` it will also set the provider for all sub modules ``chain3.eth``, ``chain3.shh``, etc.

----------
Parameters
----------

1. ``Object|String`` - ``provider``: a valid provider
2. ``Net`` - ``net``: (optional) the node.js Net package. This is only required for the IPC provider.

-------
Returns
-------

``Boolean``

-------
Example
-------

.. code-block:: javascript

    import Chain3 from 'chain3';

    const chain3 = new Chain3('http://localhost:8545');

    // or
    const chain3 = new Chain3(new Chain3.providers.HttpProvider('http://localhost:8545'));

    // change provider
    chain3.setProvider('ws://localhost:8546');
    // or
    chain3.setProvider(new Chain3.providers.WebsocketProvider('ws://localhost:8546'));

    // Using the IPC provider in node.js
    const net = require('net');
    const chain3 = new Chain3('/Users/myuser/Library/MoacNode/moac.ipc', net); // mac os path

    // or
    const chain3 = new Chain3(new Chain3.providers.IpcProvider('/Users/myuser/Library/MoacNode/moac.ipc', net)); // mac os path
    // on windows the path is: '\\\\.\\pipe\\moac.ipc'
    // on linux the path is: '/users/myuser/.moac/moac.ipc'

------------------------------------------------------------------------------

providers
=====================

.. code-block:: javascript

    Chain3.providers
    Mc.providers
    ...

Contains the current available providers.

----------
Value
----------

``Object`` with the following providers:

    - ``Object`` - ``HttpProvider``: The HTTP provider is **deprecated**, as it won't work for subscriptions.
    - ``Object`` - ``WebsocketProvider``: The Websocket provider is the standard for usage in legacy browsers.
    - ``Object`` - ``IpcProvider``: The IPC provider is used node.js dapps when running a local node. Gives the most secure connection.

-------
Example
-------

.. code-block:: javascript

    const Chain3 = require('chain3');
    // use the given Provider, e.g in Mist, or instantiate a new websocket provider
    const chain3 = new Chain3(Chain3.givenProvider || 'ws://localhost:8546');
    // or
    const chain3 = new Chain3(Chain3.givenProvider || new Chain3.providers.WebsocketProvider('ws://localhost:8546'));

    // Using the IPC provider in node.js
    const net = require('net');

    const chain3 = new Chain3('/Users/myuser/Library/Ethereum/moac.ipc', net); // mac os path
    // or
    const chain3 = new Chain3(new Chain3.providers.IpcProvider('/Users/myuser/Library/Ethereum/moac.ipc', net)); // mac os path
    // on windows the path is: '\\\\.\\pipe\\moac.ipc'
    // on linux the path is: '/users/myuser/.moadnode/moac.ipc'

------------------------------------------------------------------------------

givenProvider
=====================

.. code-block:: javascript

    Chain3.givenProvider
    chain3.mc.givenProvider
    chain3.shh.givenProvider
    ...

When using chain3.js in an Ethereum compatible browser, it will set with the current native provider by that browser.
Will return the given provider by the (browser) environment, otherwise ``null``.


-------
Returns
-------

``Object``: The given provider set or ``false``.

-------
Example
-------

.. code-block:: javascript

    chain3.setProvider(Chain3.givenProvider || 'ws://localhost:8546');


------------------------------------------------------------------------------


currentProvider
=====================

.. code-block:: javascript

    chain3.currentProvider
    chain3.mc.currentProvider
    chain3.shh.currentProvider
    ...

Will return the current provider.


-------
Returns
-------

``Object``: The current provider set.

-------
Example
-------

.. code-block:: javascript

    if (!chain3.currentProvider) {
        chain3.setProvider('http://localhost:8545');
    }

------------------------------------------------------------------------------

BatchRequest
=====================

.. code-block:: javascript

    new chain3.BatchRequest()
    new chain3.mc.BatchRequest()
    ...

Class to create and execute batch requests.

----------
Parameters
----------

none

-------
Returns
-------

``Object``: With the following methods:

    - ``add(request)``: To add a request object to the batch call.
    - ``execute()``: Will execute the batch request.

-------
Example
-------

.. code-block:: javascript

    const contract = new chain3.mc.Contract(abi, address);

    const batch = new chain3.BatchRequest();
    batch.add(chain3.mc.getBalance.request('0x0000000000000000000000000000000000000000', 'latest'));
    batch.add(contract.methods.balance(address).call.request({from: '0x0000000000000000000000000000000000000000'}));
    batch.execute().then(...);
