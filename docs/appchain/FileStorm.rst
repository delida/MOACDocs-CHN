FileStorm 应用链
----------------
.. _file-storm:

FileStorm 是个基于MOAC应用链实现的IPFS存储平台。

IPFS是一个去中心化多节点存储的重要协议。这个协议的目标是取代传统的互联网协议HTTP，让我们的互联网速度更快，更安全，更开放。但IPFS还需要通过基于区块链实现的奖励机制，来鼓励节点参与IPFS的网络存储，提供服务并获取回报。但是IPFS只是一个协议，它还不足以建立一个分布式存储平台。在这个协议之上，还缺少一个文件存储的奖励机制。有了这个机制，有存储需求的用户使用代币，提供存储空间的矿工通过竞争来存储文件获取代币。这样大家才会愿意把富余的硬盘拿出来存储别人的文件，才能搭建一个可商用的全球化的存储空间，将来IPFS才有可能取代HTTP协议，打造一个全新的互联网。

所以，基于IPFS协议搭建的存储平台需要做下面这些事情：

确保文件被复制到多个节点上。
查找并修复缺损文件。
文件存储提供方可以得到奖励。
文件存储可以被证明。证明分两块：
复制证明（proof-of-replication ）- 存储提供节点必须能证明文件确实被存储在了它的物理硬盘上。
时空证明（proof-of-spacetime）- 存储提供节点必须能证明文件在一个确定的时间里被存储了。
墨客是一个为去中心化应用而生的多层次区块链，每一个子链都将会用来支持一个应用，而大部分的应用都有存储需求，这是区块链应用不可或缺的一部分。FileStorm就是这样一条子链。用户可以通过调用智能合约把文件写入和读出IPFS网络。FileStorm子链定期做文件验证确保文件都被存储，然后给存储节点支付收益。因为底层协议一致，通过FileStorm存储的文件跟IPFS主网是相连的，所以实现了IPFS存储的墨客，将成为一个完美的去中心化应用开发平台。

而基于IPFS协议搭建的存储平台需要的所有条件，都可以通过墨客子链来轻松实现：

建立一条由多个可以提供存储功能的节点组成的子链。
通过子链自带的节点更新，备份，和刷新功能实现文件修复。
子链节点通过出块得到奖励。
FileStorm共识机制从时间和空间上证明了文件被复制。


FileStorm 共识
==============

FileStorm共识是在ProcWind共识之上实现的一个dPOS共识。DPOS，Delegated proof of stake，委托权益共识需要选出一组矿工节点来做区块的生产和调度。这些权益所有者需要抵押通证。任何节点如果不正常出块或者不对其他节点的出块做验证，抵押通证将被扣除。这些节点被称之为验证节点，或出块节点。验证节点按顺序轮流出块，每n秒钟一个块，n可设置，现在设置为10。FileStorm的不同之处在于，每个FileStorm子链的区块头上多了两个随机数。这两个随机数是用来做文件验证的。证明每个节点上都存了该存的文件。在FileStorm子链上，节点分成两种：出块节点和存储节点。出块节点负责打包交易，出块，并且产生文件验证随机数。存储节点存储文件，并且对文件进行验证。

出块节点被分到不同的组，每个组是一个存储的基本单位，相当于数据库里的一个Shard(分片)。每个分片里的节点存储空间一样大，存储的文件也是一样。这样一个分片里的多个节点可以互相验证。所有的文件的信息和读写交易都写在区块链上，而文件内容则存在存储节点的IPFS中。

每个存储节点都要定期对本地文件进行验证。初始设定为每40个区块。轮到验证的区块根据两个随机数产生一组文件，然后用第一个随机数找到每个文件中的一个起始位置。从这个起始位置往后数256个字节，得到一组随机的字符串组。然后对这组字符串进行哈希，直到得出一个根哈希值。同时，同分片里的其他节点也要做同样的操作。得到哈希值。被验证节点先将自己的哈希值以交易的方式发到链上。同分片的其他节点收到交易后，跟自己生成的根哈希进行比较，然后将比较结果也以交易的方式发出来。最后进行统计，如果超过半数的节点给出的哈希值跟被验证节点相同。我们认为被验证节点存了所有的文件，我们就会给它奖励。

每个存储节点都会轮流被验证，验证通过得到奖励。文件验证会在每个分片里进行。

用户文件存到链上，会根据一定的算法平均的分配到不同的分片上。

FileStorm 验证节点
====================

验证节点主要安装下面这个程序来产生和验证区块。并且获取区块奖励。

SCSServer - 墨客子链节点程序。
FileStorm 存储节点
存储节点主要安装下面这些程序来对存储的文件进行验证。验证成功即可获取存储收益。

StormCatcher - 文件管理助手，用于对文件读写删除和验证的操作。
IPFS Daemon - 文件以IPFS的方式存储的主要平台。
redis - 本地数据库，用于缓存文件调用的请求。
IPFS Daemon由IPFS源代码生成，没有改动。所以FileStorm子链是一种开放式架构，可以和其他基于IPFS的存储设备兼容。

FileStorm 通证
====================

FileStorm Token (FST)是发行在墨客区块链上的一个ERC20代币。在FileStorm平台上流通。用户通过支付FST获取平台的使用权，存储提供方提供存储设备得到FST收益。

FileStorm 应用链的部署
====================


FileStorm Dapp开发指南
=====================

详细介绍可参看这篇：

:ref:`FileStorm Dapp开发指南 <file-storm-dapps>` 





