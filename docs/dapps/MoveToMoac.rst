移植到MOAC平台
============

MOAC的基础链节点是基于
`以太坊 <https://github.com/ethereum/go-ethereum>`__ 项目开发的，具有类似的借口环境。
在MOAC基础链上开发分布式应用（DAPP）


1. 在MOAC基础链上部署Solidity编写的智能合约，确保有能够接入MOAC链的节点，
可以使用主网公共节点（http://gateway.moac.io/mainnet）或者测试网公共节点（http://gateway.moac.io/testnet）。

   -  `Remix <https://remix.ethereum.org/>`__, a browser compiler for
      Solidity.
   -  `wallet.moac.io <http://wallet.moac.io/>`__, a browser Dapp
      connect with local nodes to deploy contracts.
   -  `Node.Js Solidity
      compiler <https://www.npmjs.com/package/solc>`__, a JavaScript
      bindings for the Solidity compiler.

   在MOAC的WIKI上面有两个基于基础链的示例：

   `ERC20 <https://github.com/MOACChain/moac-core/wiki/ERC20>`__

   `ERC721 <https://github.com/MOACChain/moac-core/wiki/ERC721>`__

2. 调用基础链的智能合约可以通过chain3的相应工具来进行:

   -  `chain3 <https://github.com/MOACChain/chain3>`__, developed based
      on web3.js 0.2.x, is the JavaScript library supported by MOAC.

3. 在MOAC应用链上部署Solidity编写的智能合约，需要获得MOAC应用链的地址，或者通过示例子链的 :ref:`REST API <restapi-ref>` 和 :ref:`SDK <sdk-ref>` 进行，
也可以通过以下工具进行部署：
   -  `wallet.moac.io <http://wallet.moac.io/>`__, a browser Dapp
      connect with local nodes to deploy contracts.
   -  `Node.Js Solidity
      compiler <https://www.npmjs.com/package/solc>`__, a JavaScript
      bindings for the Solidity compiler.

示例
~~~~~~~

Web3.js:

.. code:: js

    var Web3 = require('../index.js');
    var web3 = new Web3();
    
    web3.setProvider(new web3.providers.HttpProvider('http://localhost:8545'));
    
    var coinbase = web3.eth.coinbase;
    console.log(coinbase);
    
    var balance = web3.eth.getBalance(coinbase);
    console.log(balance.toString(10));
    
Chain3:

.. code:: js


    var Chain3 = require('../index.js');
    var chain3 = new Chain3();
    
    chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
    
    var coinbase = chain3.mc.accounts[0];
    console.log(coinbase);
    
    var balance = chain3.mc.getBalance(coinbase);
    console.log(balance.toString(10));
    


智能合约Solidity
----------------

-  `Solidity: a smart contract programming language sharing similarities
   with Javascript and
   C++ <https://solidity.readthedocs.org/en/latest/>`__
-  `Solidity
   Glossary <https://github.com/ethereum/wiki/wiki/Solidity-Glossary>`__
-  `Solidity
   Features <https://github.com/ethereum/wiki/wiki/Solidity-Features>`__
-  `Solidity
   Collections <https://github.com/ethereum/wiki/wiki/Solidity-Collections>`__
-  `Solidity
   Cheat-sheet <https://github.com/manojpramesh/solidity-cheatsheet>`__

其它编写智能合约的工具和示例
-------------------------

-  `Safety <https://github.com/ethereum/wiki/wiki/Safety>`__
-  `Ethereum ÐApp Developer
   Resources <https://github.com/ethereum/wiki/wiki/Dapp-Developer-Resources>`__
-  `Ethereum JavaScript
   API <https://github.com/ethereum/wiki/wiki/JavaScript-API>`__
-  `Ethereum JSON RPC
   API <https://github.com/ethereum/wiki/wiki/JSON-RPC>`__
-  `Standardized Contract
   APIs <https://github.com/ethereum/wiki/wiki/Standardized_Contract_APIs>`__
-  `Ethereum development
   tutorial <https://github.com/ethereum/wiki/wiki/Ethereum-Development-Tutorial>`__
-  `ÐApp using
   Meteor <https://github.com/ethereum/wiki/wiki/Dapp-using-Meteor>`__
-  `Dapp Insight: dapp statistics <https://dappinsight.com>`__
