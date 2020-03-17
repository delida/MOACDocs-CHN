.. _scs_console:

交互命令行（console）
-------------------

从v1.1.1开始，应用链客户端SCS也支持使用了和以太坊类似的交互式命令行。用户可以在命令行（console）中执行内置的JAVA script命令或者利用脚本（script），输出结果显示在命令行中。
这里使用的chain3对象，是MOAC参考以太坊，而开发的一套javascript库，目的是让应用程序能够与MOAC的VNODE和SCS节点进行通信。注意，这里有两层，moac启动了一个MOAC VNODE节点，console参数开启了一个javascript的控制台，这个控制台注入了chain3.js这个库，以使我们可以通过chain3对象与MOAC SCS不同应用链节点做交互。


SCS和用户交互的命令行界面
=======================

MOAC 应用链节点的客户端（SCS）构建了一个可以和用户交互的JavaScript命令行界面。启动这个命令行首先需要启动一个SCS客户端，并确保打开RPC。
::
    $ ./scsserver –rpc –rpcport 8546


然后使用下面命令
::
    $ scsserver attach

scs客户端会去连接一个已经运行的SCS节点并显示命令行界面，默认路径为./scsdata。
::
    $ scsserver attach ./scsdata/scs.ipc


请注意，默认情况下，MOAC SCS节点不会启动RPC服务。 


交互式使用示例
=================

以下是主要的两个交互式命令功能包：scs和appchain。


scs
========

主要提供SCS节点相关信息，可以使用RPC方法与应用链通讯，具体方法可以参考 JSONRPC 接口。

.. _scs_getSCSId:


**scs.getSCSId**

获取SCS的ID

输入参数：无

示例如下：

.. code:: js

    > scs.getSCSId()
    "0x76449055dc3cf91090c11f7c544b60363bf896cb"


**scs.getAppChainList**

返回SCS接入的应用链列表。

输入参数：无

示例如下：

.. code:: js
    > scs.getAppChainList()
    ["0xaa4c98c7efb244f4104bf3bb437cd761df4e648c"]

scs.sendRawTransaction

发送已加签好的交易

输入参数：
   加签交易数据

示例如下：
::
    > scs.sendRawTransaction("0xf86a33808504a817c8008094aa4c98c7efb244f4104bf3bb437cd761df4e648c8084d826f88f018081efa0ec2607997ed3d35adf7821983f996b37495b8d42b47286ec74d166d4f51b5a27a071de43031ce69aaed8c1e66b19f9a5953364503ba2b67599ab218117b3182a30")
    "0xa629303563c97821fcaa2ec4bdb1ba7e2963cb06692704d84fbc05946e5576a5"

appchain
========

应用链对象，内置地址，类似SCS的相应方法，可以省略做为地址的第一个参数。

**appchain.setAddress**

设置appchain模块的内置参数address，即应用链地址

输入参数：
    Addr:应用链地址

示例如下：
::
    > appchain.setAddress("0xc2a0423fac6d1aee906c9bd8560288edc94c00ee") 


**appchain.getAddress**

返回appchain模块的内置参数address，即应用链地址

输入参数：
    Addr:应用链地址

示例如下：
::
    > appchain.getAddress()
    "0xc2a0423fac6d1aee906c9bd8560288edc94c00ee" 



**appchain.anyCall**

获取dapp合约函数的返回值，调用此接口前必须将dapp注册入dappbase

输入参数：
    Params： 第一个参数是调用的方法，之后是方法传入参数
    Sender：查询账号
    DappAddr:应用链业务逻辑地址

示例如下：
::
    > appchain.anyCall({Sender: "0xc2a0423fac6d1aee906c9bd8560288edc94c00ee", DappAddr:"0xe5d7da7a374663218294c43a7e16575f18e4b746", Params:['getCurNodeList']})
    "{\"nodeList\":[\"0x76449055dc3cf91090c11f7c544b60363bf896cb\",\"0xc2a0423fac6d1aee906c9bd8560288edc94c00ee\"]}"

**appchain.getAppChainInfo**

返回应用链信息。此信息与应用链合约（subchainbase）中定义的信息相同。

输入参数：无

示例如下：
::
    > appchain.getAppChainInfo()
    {
      balance: "0x4674edea40000000",
      blockReward: "0x1c6acb234f400",
      bondLimit: "0xde0b6b3a7640000",
      owner: "0xf180041c895a6aA8b2F38500A277cEA3e20C2CBE",
      scsList: ["0x76449055dC3cF91090c11F7C544B60363Bf896cB", "0xC2A0423Fac6d1aeE906c9BD8560288eDC94C00eE"],
      txReward: "0x174782c680",
      viaReward: "0x23857dec231000"
    }

**appchain.getBalance**

获得对应账号在应用链中的原生币余额。

输入参数：

* 账号地址

示例如下：
::
    > appchain.getBalance('0xf180041c895a6aa8b2f38500a277cea3e20c2cbe')
    11000000000000000000

**appchain.getBlock**

获得当前应用链的指定的区块信息。

输入参数：

* 块号

示例如下：
::
    > appchain.getBlock(37)
    {
      extraData: "0xa77c68b8d817377d61c23bcea32c57eab4db51bd216f066f8440aa6cca256ff80301401c1d7423d512504cc588ab68dd36f90fcd1642b1dfb0a79564f44432311a5fd00ef252dbe79d002929aa42ddaf1650a7b8f18c59edfaa9189d025923f1617a510209313538323638373437034062e8e2de7b1d5af97dc66e51fc93102ebdb5768431e36f65d8ff02af61c7016ba97b07d36cbd22d0ce8da0d7c0c8a13aa6d9fd42ab274d588610442a62d2b00e",
      hash: "0xfe8f215cb8b70d43d50c185fb0b7592a97545de2914c443ac064d436807e39a7",
      miner: "0xc2a0423fac6d1aee906c9bd8560288edc94c00ee",
      number: "0x25",
      parentHash: "0xc881942213aad1bad5403c47f9598798343f0a1793006eab6ce84272da5428ce",
      receiptsRoot: "0xa07cd7906ea8bb65b5341afaa57a7cd068c6f2acd7a165cf599537022b9d8fc4",
      stateRoot: "0xd6f76d1619ae985dfc31b9a583dd7c530569d864323d16d248b8cd3f4581e40e",
      timestamp: "0x5e55e4ee",
      transactions: ["0x36249ce5e68f9565c4149719c5d3f4e7e63d805416d5c038a611b111cd9a43a9", "0xaaf14ac0152fb12f71cce01fb8e244cc0c5fc4d0cbb27cc64f3f78b2720bca6b"],
      transactionsRoot: "0xb7ece0cd0fcc52501c23062dc25fc66529fa5093ae30859f91a11298676bc0f0"
    }

**appchain.getBlockList**

获取某一区间内的区块信息

输入参数：

* 起始块号
* 结束块号

示例如下：
::
    > appchain.getBlockList(36, 37)
    {
      blockList: [{
          extraData: "0xf0c9626d45789730f1fcb773907f6bae0d47b002f96aee3372867e219d1c73b203014021fd3e788fdb4c10c11e321e3388fb8f48eef828236e5e7c063806f9fda3ed8d893c01ae99335f678658512c9b92c369ce33c42a98808428ad52c018858052a702093135383236383734360340ef9223f63d268f02fb67c09d3c458223842ee8dcff581a21b7b53ccc6837d5c4ba720cb9b373cae71180f4c39f7fd3daf67ee61d79f8e239c3b14ed160f8a606",
          hash: "0xc881942213aad1bad5403c47f9598798343f0a1793006eab6ce84272da5428ce",
          miner: "0xc2a0423fac6d1aee906c9bd8560288edc94c00ee",
          number: "0x24",
          parentHash: "0xc5dd98df039f1d5e9c70b70b7f1c197b666c1db038da2f225bd8a9772de966d4",
          receiptsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
          stateRoot: "0x90766f90ee1d7dfc02909c68030f7ca24cfcb94d6f370b78ff522b3066235325",
          timestamp: "0x5e55e4e4",
          transactions: [],
          transactionsRoot: "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421"
      }, {
          extraData: "0xa77c68b8d817377d61c23bcea32c57eab4db51bd216f066f8440aa6cca256ff80301401c1d7423d512504cc588ab68dd36f90fcd1642b1dfb0a79564f44432311a5fd00ef252dbe79d002929aa42ddaf1650a7b8f18c59edfaa9189d025923f1617a510209313538323638373437034062e8e2de7b1d5af97dc66e51fc93102ebdb5768431e36f65d8ff02af61c7016ba97b07d36cbd22d0ce8da0d7c0c8a13aa6d9fd42ab274d588610442a62d2b00e",
          hash: "0xfe8f215cb8b70d43d50c185fb0b7592a97545de2914c443ac064d436807e39a7",
          miner: "0xc2a0423fac6d1aee906c9bd8560288edc94c00ee",
          number: "0x25",
          parentHash: "0xc881942213aad1bad5403c47f9598798343f0a1793006eab6ce84272da5428ce",
          receiptsRoot: "0xa07cd7906ea8bb65b5341afaa57a7cd068c6f2acd7a165cf599537022b9d8fc4",
          stateRoot: "0xd6f76d1619ae985dfc31b9a583dd7c530569d864323d16d248b8cd3f4581e40e",
          timestamp: "0x5e55e4ee",
          transactions: ["0x36249ce5e68f9565c4149719c5d3f4e7e63d805416d5c038a611b111cd9a43a9", "0xaaf14ac0152fb12f71cce01fb8e244cc0c5fc4d0cbb27cc64f3f78b2720bca6b"],
          transactionsRoot: "0xb7ece0cd0fcc52501c23062dc25fc66529fa5093ae30859f91a11298676bc0f0"
      }],
      endBlk: "0x25",
      microchainAddress: "0xAA4c98C7efb244f4104Bf3bB437CD761Df4e648C",
      startBlk: "0x24"
    }

**appchain.getBlockNumber**

获得当前应用链的区块高度

输入参数：无

示例如下：
::
    > appchain.getBlockNumber()
    2245


**appchain.getContractAddrList**

通过应用链地址获取应用链内所有多合约的地址列表，需要应用链业务逻辑合约调用基础合约registerDapp方法后才能生效

输入参数：无

示例如下：
::
    > appchain.getContractAddrList()
    ["0xe5d7da7a374663218294c43a7e16575f18e4b746"]

**appchain.getExchangeByAddress**

获得应用链指定账号指定数量的充提信息
输入参数：

* 需要查询的账号地址
* 已经充值记录的起始位置(0)
* 已经充值记录的长度
* 已经提币记录的起始位置(0)
* 已经提币记录的长度
* 正在充值记录的起始位置(0)
* 正在充值记录的长度
* 正在提币记录的起始位置(0)
* 正在提币记录的长度

示例如下：
::
    > appchain.getExchangeByAddress('0xf180041c895a6aa8b2f38500a277cea3e20c2cbe',0,5,0,5,0,5,0,5)
    {
      DepositRecordCount: 5,
      DepositRecords: [null, null, null, null, null, {
          DepositAmt: "0xde0b6b3a7640000",
          Deposittime: "0x5e56244a"
      }, {
          DepositAmt: "0xde0b6b3a7640000",
          Deposittime: "0x5e56244a"
      }, {
          DepositAmt: "0xde0b6b3a7640000",
          Deposittime: "0x5e56244a"
      }, {
          DepositAmt: "0xde0b6b3a7640000",
          Deposittime: "0x5e56244a"
      }, {
          DepositAmt: "0xde0b6b3a7640000",
          Deposittime: "0x5e5623a0"
      }],
      DepositingRecordCount: 0,
      DepositingRecords: null,
      WithdrawRecordCount: 0,
      WithdrawRecords: null,
      WithdrawingRecordCount: 0,
      WithdrawingRecords: null,
      microchain: "0xaa4c98c7efb244f4104bf3bb437cd761df4e648c",
      sender: "0xf180041c895a6aa8b2f38500a277cea3e20c2cbe"
    }

**appchain.getExchangeInfo**

获得应用链指定数量正在充提的信息

输入参数：

* 正在充值记录的起始位置(0) 
* 正在充值记录的长度 
* 正在提币记录的起始位置(0) 
* 正在提币记录的长度

示例如下：
::
    > appchain.getExchangeInfo(0,5,0,5)
    {
      DepositingRecordCount: 0,
      DepositingRecords: null,
      WithdrawingRecordCount: 0,
      WithdrawingRecords: null,
      microchain: "0xaa4c98c7efb244f4104bf3bb437cd761df4e648c",
      scsid: "0x76449055dc3cf91090c11f7c544b60363bf896cb"
    }

**appchain.getNonce**

获得对应账号在应用链的nonce，这是调用应用链DAPP合约的必要参数之一，每当应用链交易发送后会自动加1。

输入参数：

* 账号地址

示例如下：
::
    > appchain.getNonce('0xf180041c895a6aa8b2f38500a277cea3e20c2cbe')


**appchain.getReceiptByHash**

通过交易hash获取应用链的tx执行结果

输入参数：

* 交易哈希

示例如下：
::
    > appchain.getReceiptByHash('0x9cf04d4f00998ba09c2c614dd14fd85b173a406baedde414f08d88df5c992034')
    {
      contractAddress: "0x0000000000000000000000000000000000000000",
      failed: false,
      logs: [],
      logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      queryInBlock: 0,
      result: "",
      transactionHash: "0x9cf04d4f00998ba09c2c614dd14fd85b173a406baedde414f08d88df5c992034"
    }

**appchain.getReceiptByNonce**

通过账号和Nonce获取应用链的tx执行结果

输入参数：

* 账号地址
* Nonce

示例如下：
::
    > appchain.getReceiptByNonce('0xf180041c895a6aA8b2F38500A277cEA3e20C2CBE',2)
    {
      contractAddress: "0x0000000000000000000000000000000000000000",
      failed: false,
      logs: [],
      logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
      queryInBlock: 0,
      result: "",
      transactionHash: "0x9cf04d4f00998ba09c2c614dd14fd85b173a406baedde414f08d88df5c992034"
    }


**appchain.getTransactionByHash**

通过交易HASH获取应用链的交易信息

输入参数：

*交易哈希

示例如下：
::
    > appchain.getTransactionByHash('0x9cf04d4f00998ba09c2c614dd14fd85b173a406baedde414f08d88df5c992034')
    {
      blockHash: "0xdaf05facc57d218528e6c73df4ef98dac51eeed54a9fae9e8be4353c23c58ec6",
      blockNumber: "0x910",
      from: "0xf180041c895a6aa8b2f38500a277cea3e20c2cbe",
      gas: null,
      gasPrice: null,
      hash: "0x9cf04d4f00998ba09c2c614dd14fd85b173a406baedde414f08d88df5c992034",
      input: "0x3876c45f5b6f81a997661cf5ebcb075e400a571e",
      nonce: "0x2",
      r: "0x5c1aeea440bdf02cdf6462ed292eb33c5ed29d42a41c59b299262ffb23a4a5e7",
      s: "0x7dac198898963a33a69af708b9b7aab94328c0c03215467032055eb2f26b716d",
      shardingFlag: "0x2",
      syscnt: "0x0",
      to: "0xaa4c98c7efb244f4104bf3bb437cd761df4e648c",
      transactionIndex: "0x0",
      v: "0xf0",
      value: "0xde0b6b3a7640000"
    }


**appchain.getTransactionByNonce**

通过账号和Nonce获取应用链的交易信息

输入参数：
    账号地址
    Nonce

示例如下：
::
    > appchain.getTransactionByNonce('0xf180041c895a6aA8b2F38500A277cEA3e20C2CBE',2)
    {
      blockHash: "0xdaf05facc57d218528e6c73df4ef98dac51eeed54a9fae9e8be4353c23c58ec6",
      blockNumber: "0x910",
      from: "0xf180041c895a6aa8b2f38500a277cea3e20c2cbe",
      gas: null,
      gasPrice: null,
      hash: "0x9cf04d4f00998ba09c2c614dd14fd85b173a406baedde414f08d88df5c992034",
      input: "0x3876c45f5b6f81a997661cf5ebcb075e400a571e",
      nonce: "0x2",
      r: "0x5c1aeea440bdf02cdf6462ed292eb33c5ed29d42a41c59b299262ffb23a4a5e7",
      s: "0x7dac198898963a33a69af708b9b7aab94328c0c03215467032055eb2f26b716d",
      shardingFlag: "0x2",
      syscnt: "0x0",
      to: "0xaa4c98c7efb244f4104bf3bb437cd761df4e648c",
      transactionIndex: "0x0",
      v: "0xf0",
      value: "0xde0b6b3a7640000"
    }

**appchain.syncing**

返回SCS应用链同步区块状态

示例如下：
::
    > appchain.syncing()
    {
    startingBlock: '0x384',
    currentBlock: '0x386',
    highestBlock: '0x454'
    }
    >
    "startingBlock": bc.syncCache.Start,
                "currentBlock":  bc.CurrentBlock().NumberU64(),
                "highestBlock":  bc.syncCache.High,
txpool
=======

**txpool.content**

获得应用链交易池交易的详细信息。

示例如下：
::
    > txpool.content()
    {
      pending: {},
      queued: {
        0xf180041c895a6aA8b2F38500A277cEA3e20C2CBE: {
          3: {
            blockHash: "0x0000000000000000000000000000000000000000000000000000000000000000",
            blockNumber: null,
            from: "0xf180041c895a6aa8b2f38500a277cea3e20c2cbe",
            gas: null,
            gasPrice: null,
            hash: "0x58eb432ff88f766613e990b6c5d92f0a577e6e769712f1a2356fc1c25bde9110",
            input: "0x3876c45f5b6f81a997661cf5ebcb075e400a571e",
            nonce: "0x3",
            r: "0x3c79d4f4b1336565db6234afad3ae24f8f7dda2c1aee1a3107bf92e97361c0b3",
            s: "0x7f582d62b3e99f5dad7d34dcc8c117ddb37f70811aa90c645a1fdd433f349338",
            shardingFlag: "0x2",
            syscnt: "0x0",
            to: "0xaa4c98c7efb244f4104bf3bb437cd761df4e648c",
            transactionIndex: "0x0",
            v: "0xf0",
            value: "0x0"
          }
        }
      }
    }                           

**txpool.inspect**

获得应用链交易池交易的简要信息。

示例如下：
::
    > txpool.inspect()
    {
      pending: {},
      queued: {
        0xf180041c895a6aA8b2F38500A277cEA3e20C2CBE: {
          3: "0xAA4c98C7efb244f4104Bf3bB437CD761Df4e648C: 0 sha + 0 × 20000000000 gas"
        }
      }
    }

txpool.status

获得应用链交易池交易的交易数量。

示例如下：
::
    > txpool.status()
    {
      pending: 0,
      queued: 1
    }


debug
========

**debug\_traceTransaction**

.. _debug_traceTransaction:

    返回交易在EVM执行期间创建的结构化日志，并将它们作为JSON对象返回。

    输入参数：交易哈希

示例如下：
::
    > debug.traceTransaction("1a0da52d43d42223cd4170a40b4ea9505cdc6aba732a74100f7357f0a0128340", "callTracer")
    "{\"type\":\"CALL\",\"from\":\"0xc2a0423fac6d1aee906c9bd8560288edc94c00ee\",\"to\":\"0xe5d7da7a374663218294c43a7e16575f18e4b746\",\"value\":\"0x0\",\"gas\":\"0x337be36\",\"gasUsed\":\"0x0\",\"input\":\"0x12df94120000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000f180041c895a6aa8b2f38500a277cea3e20c2cbe00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000de0b6b3a76400000000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000005e562360\",\"output\":\"0x\",\"time\":\"0s\",\"calls\":[{\"type\":\"CALL\",\"from\":\"0xe5d7da7a374663218294c43a7e16575f18e4b746\",\"to\":\"0xf180041c895a6aa8b2f38500a277cea3e20c2cbe\",\"value\":\"0xde0b6b3a7640000\",\"input\":\"0x\"}]}"


注意
=====

MOAC SCS's JSRE 和以太坊的客户端一样采取了Otto JS VM 虚拟机并具有以下限制:

"use strict" 语句可以编译通过，但没有相应效果。
正则表达式引擎（re2 / regexp）与ECMA5规范不完全兼容。
注意，Otto的另一个已知限制（即缺少计时器）已得到解决。 以太坊JSRE同时实现setTimeout和setInterval。 除此之外，控制台还提供admin.sleep（seconds）以及“ blocktime sleep”方法admin.sleepBlocks（number）。

chain3.js 自动加载 bignumber.js 的软件库(MIT Expat Licence)用于大数处理。


