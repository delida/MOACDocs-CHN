母链
^^^^^^^^^^^


母链是一个使用工作量证明为共识算法的区块链，可为智能合约和DApp解决数据存储和计算处理工作，是墨客的主要部分。

工作量证明 Proof-of-Work（PoW）算法是一种行之有效的措施，可以阻止并最终禁止第三方干扰，包括拒绝服务攻击，其他服务以及网络滥用（如垃圾邮件）。 PoW要求服务请求者提供一些工作量证明，通常是在规定的处理时间内由计算机完成特定处理任务，从而消除错误的系统威胁。
在墨客平台上，母链是处理交易和其他区块链操作、共识和数据访问的公共区块链层。墨客还支持使用子链来实现其他共识算法 。

Besides the POW consensus on transaction and data store set, each POW node is associated with one or more Smart Contract Server(SCS). SCS node could be local to the POW node, or it could be a remote node. The SCS identity is fully verifiable by the corresponding POW node, or Validating Node (VNODE). 

Block is mined every 10s, with reward of 2 MOAC coins per block.

The reward schedule halves every 12,500,000 blocks, equivalent to
approx. four years.

After block 15,000,000, the reward will be constant of 0.1 MOAC per
block. See below.

We define 1 MOAC = 1,000,000 Sand. 1 Sand = 1,000 Xiao.


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


母链交易数据结构
--------------

墨客母链的交易数据结构与以太坊的相比，加入了三个新变量：

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
| **ShardingFlag**             | shardingFlag  | 用于鉴别交易种类: 0 - 母链交易 TX; 1 - 子链合约调用；2 - 子链原生币交易; 3 - 子链合约部署；     |
+-------------------------+-----------+-------------------------------------------+
| **Via**           | via  | 仅在ShardingFlag不为0的时候起作用，是子链交易发送到的母链节点的标识地址；|
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


母链节点客户端（VNODE）
--------------

VNODE client is used to form the MotherChain network. It used a Proof of Work consensus method similar to Ethereum. VNODE not only handles data storage and compute processing for smart contracts but also pass data for the MicroChain Dapps. 


外部链接
--------------
1. `MOAC <http://www.moacfoundation.org/>`__
   
2. `Mainnet Explorer <http://explorer.moac.io/home>`__
   
3. `Testnet Explorer <http://testnet.moac.io/home>`__
   
4. `MoacWalletOnline <https://moacwalletonline.com>`__
   
5. `TokenPocket <https://www.mytokenpocket.vip/en>`__

6. `MOACMask <https://github.com/MOACChain/MOACMask/releases>`__

