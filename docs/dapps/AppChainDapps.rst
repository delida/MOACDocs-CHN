.. _proc-wind-dapps:

应用链Dapp开发指南
^^^^^^^^^^^^^^^^^^^^^

同基础链相同，应用链上业务逻辑的实现也通过智能合约的方式。

在1.0.8及之后的版本中，应用链已经支持多合约的部署，建议用户使用nuwa 1.0.8 以后的SCS节点来构建应用链并部署合约。
在1.0.12及之后的版本，应用链可以支持solidity EVM 0.5.x，但需要部署相应DappBase.sol，参见 :ref:`ProcWind 支持solidity 0.5.X <procwind-dappbase0.5.x>`.。


多合约部署准备工作
--------------------
需要有一条已经在运行的ProcWind应用链，节点软件版本nuwa 1.0.8以上，具体部署方法可参见 :ref:`ProcWind 应用链 <proc-wind>`。

假设有两个业务逻辑合约dapp1.sol和dapp2.sol。

从发布链接下载多合约基础合约dappbase.sol


.. _procwind-dappbase:

DAPP智能合约的部署
--------------------


DAPP智能合约也通过主链的sendTransaction发送交易到 proxy vnode 的方式进行部署。

参数：
::
  to: 应用链控制合约subchainbase的地址
  gas: 不需要消耗gas费用，传值：0
  shardingflag：表示操作应用链， 传值：0x3，请注意，多合约版本部署任何合约shardingflag都为0x3  
  via: 对应 proxy vnode 的收益地址
  
STEP1：在应用链上部署多合约基础合约 DappBase.sol， 在 nuwa1.0.10 中可能是 DappBasePrivate.sol 或者 DappBasePublic.sol，两者的不同是
DappBasePublic 允许除应用链拥有者(owner)之外的用户在应用链上部署DAPPs。之前默认的都是仅有应用链owner才能部署DAPP。

!!!特别注意!!!：目前应用链原生币支持moac和erc20两种兑换方式，交易values分别对应subchainbase的tokensupply和erc20的totalsupply，这个值必须对应，否则将会导致dappbase部署失败。细节详见 :ref:`ProcWind 应用链货币交互 <proc-wind-as>`.

部署示例（以下在nodeJs console中进行）：
::
  > chain3 = require('chain3')
  > solc = require('solc')
  > chain3 = new chain3();
  > chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
  > solfile = 'DappBase.sol';
  > contract = fs.readFileSync(solfile, 'utf8');
  > output = solc.compile(contract, 1);                    
  > abi = output.contracts[':DappBase'].interface;
  > bin = output.contracts[':DappBase'].bytecode;
  > subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
  > via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';  
  > chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
  > amount = tokensupply // 注意：amount分别对应subchainbase的tokensupply和erc20的totalsupply，细节详见母应用链货币交互章节
  > chain3.mc.sendTransaction({from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: subchainaddr, gas:0, shardingFlag: "0x3", data: '0x' + bin, nonce: 0, via: via, });
      
验证: 
  合约部署成功后，Nonce值应该是1  
  可调用monitor的rpc接口ScsRPCMethod.GetNonce进行检查，具体详见应用链接口调用部分。
  
  dapp合约地址:6ab296062d8a147297851719682fb5ffe081f1d3
  dapp合约地址可调用monitor的rpc接口ScsRPCMethod.GetReceipt，传入对应Nonce，获得contractAddress字段内容


.. _procwind-dapp:

STEP2：在应用链上部署业务逻辑合约 dapp1.sol，部署方法和上面雷同
  合约地址可调用monitor的rpc接口ScsRPCMethod.GetReceipt，传入对应Nonce，获得contractAddress字段内容

STEP3：在应用链上部署业务逻辑合约 dapp2.sol，部署方法和上面雷同
  合约地址可调用monitor的rpc接口ScsRPCMethod.GetReceipt，传入对应Nonce，获得contractAddress字段内容
    

DAPP智能合约的调用
----------------------

DAPP智能合约的调用也通过主链的sendTransaction发送交易到 proxy vnode 的方式进行。

在多合约版本中，调用dapp方法前，需要先调用dappbase中的registerDapp方法来注册每一个dapp，具体方式如下：

**请注意，与母链调用不同，应用链的任何调用需要在data前加上dapp的合约地址！！**

dappbase.sol有个方法 registerDapp(address,address,string)

参数：
::
  to: 应用链控制合约subchainbase的地址
  nonce：调用monitor的rpc接口ScsRPCMethod.GetNonce获得
  gas: 0 不需要消耗gas费用
  shardingflag： 0x1  表示应用链调用操作
  via: 对应 proxy vnode 的收益地址
  data: 调用合约地址 + registerDapp(address,address,string)对应参数

registerDapp中，第一个参数是想要注册的dapp的地址（dapp1和dapp2的地址），可以通过RPC getReciipt方法获得部署时contract address；第二个参数是创建dappbase时的from，也就是只有创建dappbase的人才能调用此方法；第三个参数是这个dapp的ABI。
  
调用示例：
::
  > nonce = 3 
  > addr_dapp = 需要注册dapp的合约地址
  > abi = 需要注册dapp的abi
  > data = dappbase.address + dappbase.registerDapp.getData(addr_dapp, chain3.mc.accounts[0], abi).substring(2)   
  > subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
  > via = '0xf103bc1c054babcecd13e7ac1cf34f029647b08c';
  > chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
  > chain3.mc.sendTransaction( { nonce: nonce, from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas:0, shardingFlag:'0x1', data: data, via: via,});
  
验证：
  每次操作成功后，Nonce会自动增加1
  或者直接调用monitor的rpc接口ScsRPCMethod.GetDappAddrList获得合约注册列表的方式进行验证。

以部署dapp1和dapp2为例，需要将这两个业务逻辑合约注册到dappbase中去：

STEP4： 调用dappbase中的registerDapp方法来注册dapp1

STEP5： 调用dappbase中的registerDapp方法来注册dapp2

STEPX： 调用dapp1或dapp2中的业务逻辑

.. _procwind-dappbase0.5.x:

支持solidity 0.5.x智能合约的部署
------------------------------

在部署完应用链合约后，可以通过部署新的应用链控制合约来完成对solidity 0.5.x版本的支持。

可以在MOAC的发布网站或者开发包中找到以下两个文件：

`ASM DappBasePrivate <https://github.com/MOACChain/moac-core/blob/master/procwind/asm/DappBasePrivate_0.5.sol>`_

`ASM DappBasePublic <https://github.com/MOACChain/moac-core/blob/master/procwind/asm/DappBasePublic_0.5.sol>`_

`AST DappBasePrivate <https://github.com/MOACChain/moac-core/blob/master/procwind/ast/DappBasePrivate_0.5.sol>`_

`AST DappBasePublic <https://github.com/MOACChain/moac-core/blob/master/procwind/ast/DappBasePublic_0.5.sol>`_


注意，如果是从源文件编译合约，solidity 编译器需要使用相应版本，在Node.Js里面是需要solcjs版本大于0.5.0。

这些合约的部署过程也是通过主链的sendTransaction发送交易到 proxy vnode 的方式进行部署。具体可以参考 :ref:`应用链控制合约部署 <procwind-dappbase>`。

参数：
::
  to: 应用链控制合约subchainbase的地址
  gas: 不需要消耗gas费用，传值：0
  shardingflag：表示操作应用链， 传值：0x3，请注意，多合约版本部署任何合约shardingflag都为0x3  
  via: 对应 proxy vnode 的收益地址
  


