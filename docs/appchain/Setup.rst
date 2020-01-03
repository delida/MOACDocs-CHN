.. _scs-setup:

SCS节点快速启动指南
^^^^^^^^^^^^^^^^^^

官方版本下载
------------

最新的MOAC SCS软件可以从官方的发布地址下载 `release
link <https://github.com/MOACChain/moac-core/releases>`__，

解压后可以看到有scs的目录，其中有userconfig.json文件，其内容如下：

"VnodeServiceCfg":
  所连接的SCS的VNODE端的IP和端口，默认为localhost：50062;

"DataDir": 
  存储appChain数据的数据路径，默认为“./scsdata”;

"LogPath": 
  存储日志数据的数据路径，默认为“./_logs”;

"Beneficiary":
  从appChains获得奖励的有益帐户地址。这与scsserver地址不同。

"VnodechainId":
  所连接的MOAC链的chainId，99 - mainnet，100-devnet，101 -testnet;

"Capability":
  此SCS服务器可以加入的appChains数量，默认为10;
  SCS服务器加入的appChain越多，它可以获得的奖励就越多;
  但同时要求的网络带宽也有相应增加，如果加入的appChain过多而SCS处理能力不够，会造成数据丢失而被惩罚；

"ReconnectInterval":
  如果SCS服务器处于脱机状态，它将尝试连接回来。这是它尝试连接的块数。

"LogLevel":
  记录详细程度：0 =无声，1 =仅显示错误，2 =显示警告信息，3 =显示所有普通信息，4 =显示调试信息，5 =显示所有详细信息（默认值：4）

"BondLimit":
   加入一个appChain的最低存款额，以moac为单位，默认为每个appChain 2 moac; 如果appChain的要求高于这个限制，那么SCS不会加入；

"ReWardMin":
  加入一个appChain的最低奖励限制，以moac为单位，默认为每块0.0001 moac; 如果一个appChain提供的奖励低于这个限制，SCS不会加入；


Debian/Ubuntu/CentOS
---------------------

1. 使用 tar 来解压缩，可以看到scs目录下有三个文件：
README          - 说明文件;
scsserver       - 可执行程序;
userconfig.json - 配置文件；

在当前目录下运行：
 ./scsserver

如果要看帮助信息, 使用help参数:
./scsserver --help

SCS OPTIONS:
  --datadir "/Users/zpli/go/src/github.com/innowells/releases/nuwa1.0.11/mac/scs/scsdata"  Data directory for the microchain databases
  --password value                                                                         password of the SCS keystore (default: "moacscsofflineaccountpwd")
  
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
-------

解压文件，应该可以看到目录内的三个文件：

scsserver.exe

打开命令（cmd）终端，转到SCS解压目录，在命令行中执行：

::

    D:\scs>scsserver.exe --help

其它操作参考 Debian/Ubuntu/CentOS。




