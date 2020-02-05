Restful API 接口
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

管理模块
---------------------------

API认证
=====================

请求访问token，提供权限调用API的其他接口

方法：auth

参数:
::
	account:  授权账号
	pwd:  授权账号密码
	
调用示例：
::
	POST: http://139.198.126.104:8080/auth
	BODY：account=******&pwd=******

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": "token内容"
	}

	
账户模块
---------------------------

账户注册
=====================

方法：register

参数:
::
	pwd:  账户密码
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/account/v1.0/register
	BODY：pwd=********&token=********************************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"address": 账户地址,
		"encode": 账户加密串,
		"keystore": 账户keystore信息,
		"privateKey": 账户私钥
	}
	
账户登录
=====================

方法：login

参数:
::
	address:  账户地址
	pwd:  账户密码
	encode:  账户加密串
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/account/v1.0/login
	BODY：address=0x********&pwd=*****&encode=*******&token=************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户地址
	}	

账户导入
=====================

方法：import   将账户通过keystore导入系统

参数:
::
	address:  账户地址
	pwd:  账户密码
	keystore:  账户keystore
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/account/v1.0/import
	BODY：address=0x********&pwd=*****&keystore={*******}&token=************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"address": 账户地址,
		"encode": 账户加密串,
		"privateKey": 账户私钥
	}	
	
主网模块
---------------------------


账户余额
=====================

方法：getBalance

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账号地址
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getBalance
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x******&token=*****************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户余额 (单位 moac)	
	}
	
区块高度
=====================

方法：getBlockNumber

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getBlockNumber
	BODY：vnodeip=127.0.0.1&vnodeport=8545&token=***************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 区块高度
	}	
	
区块信息
=====================

方法：getBlockInfo

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	block:  区块号或者区块hash
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getBlockInfo
	BODY：vnodeip=127.0.0.1&vnodeport=8545&block=2002326&token=******************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 区块信息
	}	

交易明细
=====================

方法：getTransactionByHash

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	hash:  交易hash
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getTransactionByHash
	BODY：vnodeip=127.0.0.1&vnodeport=8545&hash=0x**&token=******************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易明细
	}

交易详情
=====================

方法：getTransactionReceiptByHash

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	hash:  交易hash
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getTransactionReceiptByHash
	BODY：vnodeip=127.0.0.1&vnodeport=8545&hash=0x**&token=******************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易详情
	}	
	
转账
=====================

方法：sendRawTransaction

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	from:  源账号地址
	to:  目标账号地址
	amount:  数量（单位 moac）
	method:  dapp合约方法 比如：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 比如：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	privatekey:  源账号私钥 （传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	gasprice: 可选参数，默认gasprice为chain3的gasPrice，当交易堵塞时，需要传原交易的110%进行覆盖。
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/sendRawTransaction
	BODY：vnodeip=127.0.0.1&vnodeport=8545&from=0x**&to=0x***&amount=10&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&privatekey=0x**&token=*******

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

调用智能合约
=====================

方法：callContract

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	contractaddress:  合约地址
	method:  dapp合约方法 比如：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 比如：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/callContract
	BODY：vnodeip=127.0.0.1&vnodeport=8545&contractaddress=0x*****&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]0x****&token=***************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 调用合约返回结果
	}		

erc20转账
=====================

方法：transferErc

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	from:  源账号地址
	to:  目标账号地址
	contractaddress:  erc20合约地址
	amount:  erc20代币数量
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/transferErc
	BODY：vnodeip=&vnodeport=&from=0x**&to=0x**&contractaddress=0x**&amount=10&privatekey=0x**&token=*******

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	
	
erc20余额
=====================

方法：getErcBalance

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	contractaddress:  erc20合约地址
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/getErcBalance
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x*****&contractaddress=0x**&token=*********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 余额（最小精度，10进制）
	}	
	
erc20授权给子链
=====================

方法：ercApprove

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	amount:  授权erc20数量
	privatekey:  账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	microchainaddress			子链地址
	contractaddress:  erc20合约地址
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/ercApprove
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x*****&amount=***&privatekey=0x***&microchainaddress=0x***&contractaddress=0x**&token=*********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	


充值子链  erc20兑换子链原生币
=====================

方法：buyErcMintToken   注：前提是erc20对应数量已经授权给子链

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	microchainaddress:  子链地址
	method:  dapp合约方法 默认为：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 默认为：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/buyErcMintToken
	BODY：vnodeip=&vnodeport=&address=0x**&privatekey=0x**&microchainaddress=0x**&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&token=****

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

充值子链  moac兑换子链原生币
=====================

方法：buyMoacMintToken

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	address:  账户地址
	privatekey:  源账号私钥
	pwd： 账户密码
	encode：账户加密串
	microChainaddress:  子链地址
	method:  dapp合约方法 默认为：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 默认为：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/v1.0/buyMoacMintToken
	BODY：vnodeip=&vnodeport=&address=0x**&privatekey=0x**&microChainaddress=0x**&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&token=****

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}		
	
子链模块
---------------------------

获得子链区块高度
=====================

方法：getBlockNumber

参数:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getBlockNumber
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&token=***********
返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链区块高度
	}	
	
获得子链dapp地址列表
=====================

方法：getDappAddrList

参数:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getDappAddrList
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&token=***********
返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链dapp地址列表（按合约注册次序）
	}		
	
获取子链区块信息
=====================

方法：getBlock

参数:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	blocknum:  块号
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getBlock
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&blocknum=*****&token=***********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链区块信息
	}	
	
获得子链对应Hash的交易信息 
=====================

方法：getTransactionByHash

参数:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	hash:  交易hash
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getTransactionByHash
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&hash=0x**&token=***********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链交易信息
	}	
	
获得子链对应Hash的交易明细
=====================

方法：getTransactionReceiptByHash

参数:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	hash:  交易hash
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getTransactionReceiptByHash
	BODY：microip=127.0.0.1&microport=8546&microchainaddress=0x***&hash=0x**&token=***********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链交易明细，其中主要字段描述如下：
		        failed：交易是否成功  false表示成功
				result：如执行合约方法，retrun的数据
				transactionHash：子链hash
				contractAddress：当部署合约时，返回合约地址
				
	}	
		

获取子链账户余额
=====================

方法：getBalance

参数:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	address:  账户地址
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/getBalance
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x*****&address=0x*****&token=**************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户余额
	}	

	
子链原生币转账
=====================

方法：transferCoin

注意，这个方法中用户有两种方法授权，一种是直接发送私钥（private key），简单但是有泄露风险；另一种是使用账户密码和账户加密串，需要调用帐号模块来生成后使用。

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	via:  子链收益账号
	from:  源账户地址
	to:  目标账户地址
	amount:  原生币数量
	memo: 备注 （交易的Input Data会由目标账户地址+memo内容组成）
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	memo: 备注 （交易的Input Data会由目标账户地址+memo内容组成）
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/transferCoin
	// Send Private Key
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&via=0x**&from=0x**&to=0x**&amount=**&memo=&privatekey=0x***&token=*****
    // or Send pwd and encode
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&via=0x**&from=0x**&to=0x**&amount=**&memo=&pwd=***&encode=***&token=*****

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

子链加签交易  
=====================

方法：sendRawTransaction   调用dapp合约涉及修改数据的方法

参数:
::
	vnodeip: vnode节点地址
	vnodeport:  vnode节点端口
	microip:  monitor节点地址
	microport:  monitor节点端口
	from: 发送交易账户地址
	microchainaddress:  子链SubChain地址
	via:  子链收益账号
	amount:	 payable对应金额	
	dappaddress:  dapp合约地址
	method:  dapp合约方法 比如：buyMintToken(uint256)
	paramtypes:  dapp合约方法对应的参数类型 比如：["uint256"]
	paramvalues:  dapp合约方法对应的参数值   比如：[100000000]
	privatekey: 源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token: auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/sendRawTransaction
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&from=0x**&microchainaddress=0x***&via=0x**&amount=**&dappaddress=0x***&method=buyMintToken(uint256)&paramtypes=["uint256"]&paramvalues=[100000000]&privatekey=0x***&token=*****

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链交易hash
	}
	
子链合约调用 
=====================

方法：callContract 针对public方法和变量，不涉及数据修改

参数:
::
	microip:  monitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	dappaddress:  dapp合约地址
	data:  字符串数组，如合约方法getTopicList(uint pageNum, uint pageSize)，则传入["getTopicList", "0", "20"]
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/callContract
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x*****&dappaddress=0x**&data=&token=********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 合约返回结果
	}	
	
子链ERC提币 
=====================

方法：redeemErcMintToken     原生币转erc20

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	microipHmonitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	dappbaseaddress:  dappbase合约地址
	via:  子链收益账号
	address:  提币账户地址
	amount:  提取原生币数量
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/redeemErcMintToken
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&dappbaseaddress=0x**&via=0x**&address=0x**&amount=**&data=****&privatekey=0x**&token=********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	
	
子链MOAC提币 
=====================

方法：redeemMoacMintToken     原生币转moac

参数:
::
	vnodeip:  vnode节点地址
	vnodeport:  vnode节点端口
	microipHmonitor节点地址
	microport:  monitor节点端口
	microchainaddress:  子链SubChain地址
	dappbaseaddress:  dappbase合约地址
	via:  子链收益账号
	address:  提币账户地址
	amount:  提取原生币数量
	privatekey:  源账号私钥（传privatekey，可忽略参数pwd和encode，不传privatekey，则必须传pwd和encode认证）
	pwd： 账户密码
	encode：账户加密串
	token:  auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/micro/v1.0/redeemMoacMintToken
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&dappbaseaddress=0x**&via=0x**&address=0x**&amount=**&data=****&privatekey=0x**&token=********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	