.. _scs-setup:

SCS节点快速启动指南
^^^^^^^^^^^^^^^^^^

官方版本下载
====================

最新的MOAC SCS软件可以从官方的发布地址下载 `release
link <https://github.com/MOACChain/moac-core/releases>`__，

.. _setup-configfile:

解压后可以看到有scs的目录，其中有userconfig.json文件，其内容如下：

.. code:: js

  {
      "VnodeServiceCfg": "localhost:50062",
      "DataDir": "./scsdata",
      "LogPath": "./_logs",
      "Beneficiary": "0xd814f2ac2c4ca49b33066582e4e97ebae02f2ab9",
      "VnodechainId": 101,
      "LogLevel": 4,
      "BondLimit": 1,
      "ReWardMin":0.0001,
      "Capability": 10,
      "ReconnectInterval": 5
  }

其中参数说明如下：

"VnodeServiceCfg":
  所连接的SCS的VNODE端的IP和端口，默认为localhost：50062；

"DataDir": 
  存储appChain数据的数据路径，默认为“./scsdata”；

"LogPath": 
  存储日志数据的数据路径，默认为“./_logs”；

"Beneficiary":
  从appChains获得奖励的有益帐户地址，与scsserver地址不同；

"VnodechainId":
  所连接的MOAC链的chainId，99 - mainnet，100-devnet，101 -testnet;

"LogLevel":
  记录详细程度：0 =无声，1 =仅显示错误，2 =显示警告信息，3 =显示所有普通信息，4 =显示调试信息，5 =显示所有详细信息（默认值：4）；

"BondLimit":
   加入一个appChain的最低存款额，以moac为单位，默认为每个appChain 2 moac; 如果appChain的要求高于这个限制，那么SCS不会加入；

"ReWardMin":
  加入一个appChain的最低奖励限制，以moac为单位，默认为每块0.0001 moac; 如果一个appChain提供的奖励低于这个限制，SCS不会加入；

"Capability":
  此SCS服务器可以加入的appChains数量，默认为10;
  SCS服务器加入的appChain越多，它可以获得的奖励就越多;
  但同时要求的网络带宽也有相应增加，如果加入的appChain过多而SCS处理能力不够，会造成数据丢失而被惩罚；

"ReconnectInterval":
  如果SCS服务器处于脱机状态，它将尝试连接回来。这是它尝试连接的块数。

以上文件配置可以在运行时通过命令行参数覆盖。

Debian/Ubuntu/CentOS
====================

1. 使用 tar 来解压缩，可以看到scs目录下有三个文件：

README          - 说明文件;
scsserver       - 可执行程序;
userconfig.json - 配置文件；

在当前目录下运行：
::
 ./scsserver

如果要看帮助信息, 使用help参数:
::

./scsserver --help

SCS OPTIONS:
  --datadir "/Users/mac/scs/scsdata"  Data directory for the SCS databases
  --password value                    password of the SCS keystore (default: "moacscsofflineaccountpwd")
  
API OPTIONS:
  --rpc                  Enable the HTTP JSON-RPC server
  --rpcdebug             Enable the HTTP-RPC Debug server
  --rpcaddr value        HTTP-RPC server listening interface (default: "127.0.0.1")
  --rpcport value        HTTP-RPC server listening port (default: 8548)
  --rpcapi value         API's offered over the HTTP-RPC interface
  --rpccorsdomain value  Comma separated list of domains from which to accept cross origin requests (browser enforced)
  --userconfig value     userconfig [file path]
  --verbosity value      sets the verbosity level, override loglevel in userconfig.json (default: 2)
  --help, -h             show help
  --version, -v          print the version


WINDOWS
==========

解压文件，应该可以看到目录内的三个文件：

scsserver.exe

打开命令（cmd）终端，转到SCS解压目录，在命令行中执行：

::

    D:\scs>scsserver.exe --help

其它操作参考 Debian/Ubuntu/CentOS。

.. _setup-logfile:

操作日志
=========

SCS在运行时会产生操作日志，配置日志的路径以及日志的级别字段分别为LogPath和LogLevel，:ref:`SCS配置文件 <setup-configfile>` 。

日志级别一般分为2为包含error日志，3包含info日志，4代表包含debug日志。高级别包含低级别，启动前配置好即可，一般配置为3即可。
如果有特殊需求，临时重启scs，需要打印一段日志级别为4的debug日志，可以不修改配置文件
只需用在命令行运行的时候添加一个命令即可。
“--verbosity”拥有优先权，包含--verbosity字段的，会自动忽略LogLevel的参数

命令示例
::

    ./scsserver --verbosity 4


日志文件是按照小时进行切割的，所以可以根据用户的需要来保留。

当需要分析应用链运行时问题产生的原因，或者了解详细的程序执行过程，需要将日志级别调整为4。这样导致的一个问题就是日志量会比较大，需要及时清理，避免硬盘塞满而影响应用链运行。

同时，交易量较高的情况下，日志级别较高的情况下，会影响到系统的IO性能，也可能会因此造成执行正常任务性能下降而造成分叉（小概率）。

当生产环境中，应用链运行稳定后，建议将日志级别调整为3即可。

如果是测试环境上测试功能，希望您将日志级别调整为4，方便查询问题，分析问题。

如果是测试压力的情况下，建议您将日志级别调整为3，测试相关性能，分析当前配置下，能够支持的性能情况。

具体日志保留时间，可以按照SCS运行环境的硬盘大小以及其业务模型规模等进行保留。若单纯分析问题一般至少一周的日志，正常建议至少保存一个月。

日志常见问题
===========

SCS正常出块的log内容如下：
::

  ### SendBkToVnode Block Number:1726854 ###

  block.Hash:       0xb3f7e5f3fb50060df479d3560e1b3cd439e79e5160456184e344a07c8caf1401

  block.ParentHash: 0x02ceefe0b79afb319e04a36a7488f539e424d5996bdbeb5abc26831efc65cf89

  SubchainAddr:     0xac7c54e2b6bae6768bbc90afc51b022e9200a4dc

  Sender:           0x63c2c5c4dda393c9f288534d2bb660f5b905734d

  #####################################


如果没有收到其他SCS区块，而是一直自己在出块，很可能出现网络问题，此时检查SCS所连接的Vnode的P2P连接是否正常。


接受区块时的正常情况：
::

  ### Insert Block Number:1726858 ###

  block.Hash:       0xc0b1d9d92a84377eb83ecae9dd8915ac128ba1d910ce4829f7996d0403c40d93

  block.ParentHash: 0xcbd7e13a9f9dfc64dbdf58403a8528e9083b8f824aef23fce906527a3812cc11

  SubchainAddr:     0xac7c54e2b6bae6768bbc90afc51b022e9200a4dc

  Sender:           0xd1910e0c1581f64e1891db15accb4afe289bdc4e

  ##############################

  DEBUG[12-19|11:53:27.780] 6756094:Finalize/IntermediateRoot(true) num: 1726858, root: 0x4785f2b62412d7c90cd99c93ff266e866f8773cd14a0c6ef5e71afce8def46c7 

  DEBUG[12-19|11:53:27.780] 6756094:Trie cache stats after commit    misses=7714769 unloads=887

  INFO [12-19|11:53:27.780] 6756094:insertChain/return success 



其中insertChain/return success 表示一个区块被共识并写入levelDB。


接受区块异常情况
收块异常情况都会报告BAB BLOCK：
::

  ########## BAD BLOCK #########

  Number:      1726860

  Hash:      0x80624814f555fdeb99403ae456a4c27c2a9dbb18e3829c4f5bcd8c2614e95345

  ParentHash:  0x1f7ef6036051f3936778e8012ba33b5bc034082aaa7789b1894967e8398012e5

  SubchainAddr:0xac7c54e2b6bae6768bbc90afc51b022e9200a4dc

  Error:       the received block was too slow

  ##############################



情况一：the received block was too slow

假设出块时间间隔为T(一般T=10)，如果块创建的时间与收到该块时SCS本地时间相差大于T-1，那么会报此错误。

情况二：收到重复的区块：block already known

此情况一般不会有影响，该块会被丢弃。

情况三：打包区块时爆出交易错误：nonce too high

这类情况都是发送交易时Nonce字段写错了。

情况四：unknown ancestor

该区块的父ParentHash在Merkle Tree中不存在

情况五：block in the future

收到大于本地区块号+1的区块，报此错误

情况六：invalid block number

收到小于本地区块号的区块，报此错误

请求区块数据相关过程的log

当连续多次报“block in the future”错误，并收集到大于等于8个高区块，次数启动请求区块模式，此SCS向部署在主网的VnodeProtocolBase合约随机获取一个vnode的IP和Port，

如果获取成功会打印log：
::

    getRandomProxyServer/getProperProxy proxy:56.151.161.171:50062

返回的IP和Port是用户注册进去的。

SCS上传区块成功、失败、性能值，上传成功的log:
::

  Succeedfully uploaded the blocks Start:32, End:33, sender:..., proxy:56.151.161.171:50062, performance:5

上传失败的log：Failed to upload the blocks to proxy: 56.151.161.171:50062, performance:4

SCS下载区块成功、失败、性能值

下载成功的log:
::

  SubChain:0xac7c54e2b6bae6768bbc90afc51b022e9200a4dc download block 32 from 56.151.161.171:50062

下载失败的log：
::

  Failed to download the block:32, performance:4,

性能值performance
performance最大为5，上传区块失败一次减1，下载失败减1，减到0则不提供服务，但定期复活一次。
