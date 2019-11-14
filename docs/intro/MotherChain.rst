基础链
^^^^^^^^^^^


基础链（BaseChain），或称母链（MotherChain）是一个使用工作量证明为共识算法的区块链，可为智能合约和DApp解决数据存储和计算处理工作，是MOAC的主要部分，可以支持多种应用链（AppChain），又称子链（MicroChain）。

工作量证明 Proof-of-Work（PoW）算法是一种行之有效的措施，可以阻止并最终禁止第三方干扰，包括拒绝服务攻击，其他服务以及网络滥用（如垃圾邮件）。 PoW要求服务请求者提供一些工作量证明，通常是在规定的处理时间内由计算机完成特定处理任务，从而消除错误的系统威胁。
在MOAC平台上，母链是处理交易和其他区块链操作、共识和数据访问的公共区块链层。MOAC还支持使用应用链来实现其他共识算法 。

除了POW对事务和数据存储集的共识之外，每个POW节点都与一个或多个智能合约服务器（SCS）相关联。 SCS节点可以是POW节点的本地节点，也可以是远程节点。 SCS标识可由相应的POW节点或验证节点（VNODE）完全验证。

MOAC 系统的区块大概每10秒钟达成一次共识，挖出区块的奖励在最初为2个MOAC币。

这一奖励每12,500,000个区块减半，大致相当于约四年时间。

在挖出15,000,000个区块之后，奖励变为常数，即每个区块0.1 MOAC。 

MOAC母链每年将通过挖矿大概产生600万个通证，进一步增加流通中的供应量。到2058年，总供应量预计将达到2.1亿枚。

在MOAC系统中， 1 MOAC = 1,000,000 Sand. 1 Sand = 1,000 Xiao.


+---------------------------+------------------------------------+
| Block#                    | Reward (1 MOAC = 1,000,000 Sand)   |
+===========================+====================================+
| 1-12,500,000              | 2 MOAC                             |
+---------------------------+------------------------------------+
| 12,500,001 - 25,000,000   | 1 MOAC                             |
+---------------------------+------------------------------------+
| 25,000,000 - 37,500,000   | 0.5 MOAC                           |
+---------------------------+------------------------------------+
| 37,500,001 - 50,00,000    | 0.25 MOAC                          |
+---------------------------+------------------------------------+
| 50,000,001 - 62,500,000   | 0.125 MOAC                         |
+---------------------------+------------------------------------+
| > 62,500,001              | 0.1 MOAC                           |
+---------------------------+------------------------------------+

Transaction fee is paid in two ways. One is through Transaction. The
other is for Smart Contract or sub chain. Smart Contract Call cost is
set lower than underlying transaction in purpose, thus encouraging the
usage of SCS. This can alleviate the pressure on the underlying layer,
and also benefit the SCS providers.


基础链交易数据结构
-----------------

MOAC母链的交易数据结构与以太坊的相比，加入了三个新变量：

+-------------------------+-----------+-------------------------------------------+
| Txdata field            | Json | Usage                                       |
+=========================+===========+===========================================+
| **AccountNonce**        | nonce     | transaction sequence number fr the sending account  |
+-------------------------+-----------+-------------------------------------------+
| **Price**      | price   | price you are offering to pay, in unit of Sha |
+-------------------------+-----------+-------------------------------------------+
| **GasLimit**       | gaslimit   | maximum amount of gas allowed for the transaction, should less than 9,000,000|
+-------------------------+-----------+-------------------------------------------+
| **Recipient**         | to   | destination address (account or contract address)                           |
+-------------------------+-----------+-------------------------------------------+
| **Amount**      | amount  | moac to transfer to the destination, if any, in unit of Sha  |
+-------------------------+-----------+-------------------------------------------+
| **Payload**           | data  |  Information aobut Global Contract call, MicroChain Dapp transactions, etc.|
+-------------------------+-----------+-------------------------------------------+
| **ShardingFlag**             | shardingFlag  | 用于鉴别交易种类: 0 - 母链交易 TX; 1 - 应用链合约调用；2 - 应用链原生币交易; 3 - 应用链合约部署；     |
+-------------------------+-----------+-------------------------------------------+
| **Via**           | via  | 仅在ShardingFlag不为0的时候起作用，是应用链交易发送到的母链节点的标识地址；|
+-------------------------+-----------+-------------------------------------------+
| **SystemContract**           | systemFlag  | 系统合约标识，默认为0，不需要改动；   |
+-------------------------+-----------+-------------------------------------------+


::

  type txdata struct {
    AccountNonce   `json:"nonce"    gencodec:"required"`
    SystemContract `json:"syscnt" gencodec:"required"`
              `json:"gasPrice" gencodec:"required"`
    GasLimit       `json:"gas"      gencodec:"required"`
    Recipient      `json:"to"       rlp:"nil"` // nil means contract creation
    Amount         `json:"value"    gencodec:"required"`
    Payload        `json:"input"    gencodec:"required"`
    ShardingFlag   `json:"shardingFlag" gencodec:"required"`
    Via            `json:"via"       rlp:"nil"`

    // Signature values
    V `json:"v" gencodec:"required"`
    R `json:"r" gencodec:"required"`
    S `json:"s" gencodec:"required"`

  }


基础链节点客户端（VNODE）
----------------------

VNODE 客户端是接入MOAC母链的软件。VNODE 客户端可以进行母链的POW共识，并和应用链的SCS客户端相连，以支持应用链的共识。 


外部链接
--------------
1. `MOAC <http://www.moacfoundation.org/>`__
   
2. `Mainnet Explorer <http://explorer.moac.io/home>`__
   
3. `Testnet Explorer <http://testnet.moac.io/home>`__
   
4. `MoacWalletOnline <https://moacwalletonline.com>`__
   
5. `TokenPocket <https://www.mytokenpocket.vip/en>`__

6. `MOACMask <https://github.com/MOACChain/MOACMask/releases>`__

