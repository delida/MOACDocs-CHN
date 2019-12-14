.. _scs-pool:

SCS节点池部署  
^^^^^^^^^^^^
		
目前针对不同的共识协议，可以创建对应的应用链节点池，接受对应SCS的注册，并缴纳保证金，进入节点池后，成为应用链的候选节点

如果加入现成的应用链节点池，则可以忽略此步骤
		
部署SubChainProtocolBase.sol示例:    共识:POR  最低保证金: 2 moac 
::		     
	> chain3 = require('chain3')
	> solc = require('solc')
	> chain3 = new chain3();
	> chain3.setProvider(new chain3.providers.HttpProvider('http://localhost:8545'));
	> solfile = 'SubChainProtocolBase.sol';
	> contract = fs.readFileSync(solfile, 'utf8');
	> output = solc.compile(contract, 1);                     
	> abi = output.contracts[':SubChainProtocolBase'].interface;
	> bin = output.contracts[':SubChainProtocolBase'].bytecode;
	> subchainprotocolbaseContract = chain3.mc.contract(JSON.parse(abi));
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> subchainprotocolbase = subchainprotocolbaseContract.new( "POR",  2, { from: chain3.mc.accounts[0],  data: '0x' + bin,  gas: '5000000'});
	> chain3.mc.getTransactionReceipt(subchainprotocolbase.transactionHash).contractAddress
	
部署完毕后，获得应用链节点池合约地址  0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac


设置启动SCS 
----------

这里我们设置三个scs节点

确认 userconfig.json配置
::
	VnodeServiceCfg为代理vnode地址: 192.168.10.209:50062
	Beneficiary为收益账号: 
		0xa934198916cd993c73c1aa6e0c0e7b21ce7c735b 
		0x2e7c076dbf6e61207a0ddb1b942ef7da8fd139f0
		0xea1a118e94344be69f02753d1e6f7fe19dda89ac
		
分别通过命令启动  scsserver-windows-4.0-amd64 --password "123456"   （生成scs keystore的密码）
		
然后在scskeystore目录内生成的keystore文件中分别获得 scs 地址  
::
	0xd4057328a35f34507dbcd295d43ed0cccf9c368a 
	0x3e21ba36b396936c6cc9adc3674655b912e5fa54
	0x03c74ecc8ad9493a6a3d14f4e48d5eb551fe1be5

最后给scs转入moac以支付必要的交易费用
::		
	> amount = 20;
	> scsaddr = '0xd4057328a35f34507dbcd295d43ed0cccf9c368a';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	> scsaddr = '0x3e21ba36b396936c6cc9adc3674655b912e5fa54';
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	> scsaddr = '0x03c74ecc8ad9493a6a3d14f4e48d5eb551fe1be5';
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:chain3.toSha(amount,'mc'), to: scsaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: ''});
	
可以通过查询余额进行验证  
::		
	> chain3.mc.getBalance('0xd4057328a35f34507dbcd295d43ed0cccf9c368a')
	> chain3.mc.getBalance('0x3e21ba36b396936c6cc9adc3674655b912e5fa54')
	> chain3.mc.getBalance('0x03c74ecc8ad9493a6a3d14f4e48d5eb551fe1be5')
	
SCS 加入应用链节点池
----------------------

调用应用链节点池合约register方法加入节点池
			
参数:
::
	from： 应用链测试账号    
	value：押金，必须大于节点池合约的设置值  
	to: 应用链节点池合约地址  
	data: register(address) 
	
关于data传递调用register参数说明:	
::	
	根据ABI chain3.sha3("register(address)") = 0x4420e4869750c98a56ac621854d2d00e598698ac87193cdfcbb6ed1164e9cbcd 
		取前4个字节 0x4420e486  
	参数address传scs 地址    d4057328a35f34507dbcd295d43ed0cccf9c368a  （前面补24个0， 凑足32个字节）  
		000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a
	data = '0x4420e486000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a'		

调用示例:
::
	> amount = chain3.toSha(5,'mc')
	> data = '0x4420e486000000000000000000000000d4057328a35f34507dbcd295d43ed0cccf9c368a';
	> chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: '0xe42f4f566aedc3b6dd61ea4f70cc78d396130fac', gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });
	
验证： 访问应用链节点池合约的scsCount
::		
	> subchainprotocolbase.scsCount()

同上将另两个scs也加入应用链节点池

.. _scs-join-appchain:

SCS节点添加
----------

应用链合约提供了registerAdd方法来支持应用链添加，必须由应用链部署账号来发送交易请求。

需要对应SubChainProtocolBase节点池合约有等待加入的scs节点。

应用链收到请求后，在节点池合约选取scs，开始同步应用链区块，等一轮flush后生效，正式加入应用链。

registerAdd参数:
::
	nodeToAdd： 当前scs数+需要加入scs数

调用示例:
::	
	> data = subchainbase.registerAdd.getData(20)
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});

验证：scs对应日志开始同步区块，合约公共变量nodeCount更新为scs最新数量
::		
	> SubChainBase.nodeCount()
	
.. _scs-exit-appchain:

SCS节点退出应用链
---------------

SCS节点退出应用链有两种方式：

1. 当应用链工作正常时，调用子类合约requestRelease方法请求退出应用链，等待一轮flush后生效。

requestRelease参数:
::
	senderType：	1：scs发起请求       2：收益账号发出请求
	index： 		scs序号（参考ScsRPCMethod.GetSubChainInfo中scs的列表）

调用示例（在NODEJS 交互环境下）:	
::	
	> data = subchainbase.requestRelease.getData(senderType, index)
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});
	
验证：等待一轮flush后，关注合约公共变量nodeCount是否变化
::		
	> SubChainBase.nodeCount()

	
2. 当应用链工作不正常时，可以调用子类合约requestReleaseImmediate方法请求立即退出应用链。

requestReleaseImmediate参数:
::
	senderType：	1：scs发起请求       2：收益账号发出请求
	index： 		scs序号（参考ScsRPCMethod.GetSubChainInfo中scs的列表）

调用示例:	
::	
	> data = subchainbase.requestReleaseImmediate.getData(senderType, index)
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> chain3.personal.unlockAccount(chain3.mc.accounts[0], '123456');
	> chain3.mc.sendTransaction( { from: chain3.mc.accounts[0], value:0, to: subchainaddr, gas: "2000000", gasPrice: chain3.mc.gasPrice, data: data});
	
验证：合约公共变量nodeCount是否变化
::		
	> SubChainBase.nodeCount()

.. _scs-monitor:

SCS Monitor
----------------------

Monitor是一种特殊的应用链SCS节点，其主要可以用于监控应用链的状态和数据。

Monitor不参与应用链的交易共识，只是同步区块数据，提供数据查询

应用链启动的方式与scs区别在于参数不同，主要定义了rpc接口的访问控制
::	
	scsserver-windows-4.0-amd64 --password "123456" --rpcdebug --rpcaddr 0.0.0.0 --rpcport 2345 --rpccorsdomain "*"

应用链运行后，Monitor可以调用应用链控制合约subchainbase中的registerAsMonitor方法进行注册

调用registerAsMonitor参数说明:	
::	
	> data = subchainbase.registerAsMonitor.getData('0xd135afa5c8d96ba11c40cf0b52952d54bce57363','127.0.0.1:2345')   
	

调用示例:
::
	> subchainbase = SubChainBaseContract.at('0xb877bf4e4cc94fd9168313e00047b77217760930')
	> amount = chain3.toSha(1,'mc')
	> subchainaddr = '0x1195cd9769692a69220312e95192e0dcb6a4ec09';
	> data = subchainbase.registerAsMonitor.getData('0xd135afa5c8d96ba11c40cf0b52952d54bce57363','127.0.0.1:2345')
	> chain3.mc.sendTransaction({ from: chain3.mc.accounts[0], value:amount, to: subchainaddr, gas: "5000000", gasPrice: chain3.mc.gasPrice, data: data });

验证： 观察SCS monitor concole界面开始同步应用链区块，或者调用应用链合约的getMonitorInfo方法
::
	> subchainbase = SubChainBaseContract.at('0xb877bf4e4cc94fd9168313e00047b77217760930')	
	> subchainbase.getMonitorInfo.call()

