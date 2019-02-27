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
	account： 	授权账号
	pwd：		授权账号密码
	
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
	pwd： 		账户密码
	token：		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/account/register
	BODY：pwd=********&token=********************************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户信息
	}
	
账户登录
=====================

方法：login

参数:
::
	address：	账户地址
	pwd： 		账户密码
	keyStore：	账户的keyStore信息
	token：		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/account/login
	BODY：address=0x********&pwd=*****&keyStore={*******}&token=************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户地址
	}	

主网模块
---------------------------


账户余额
=====================

方法：getBalance

参数:
::
	vnodeip： 	vnode节点地址
	vnodeport：	vnode节点端口
	address：	账号地址
	token：		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/getBalance
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x******&token=*****************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户余额
	}
	
区块高度
=====================

方法：getBlockNumber

参数:
::
	vnodeip： 	vnode节点地址
	vnodeport：	vnode节点端口
	token：		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/getBlockNumber
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
	vnodeip： 	vnode节点地址
	vnodeport：	vnode节点端口
	block：		区块号或者区块hash
	token：		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/getBlockInfo
	BODY：vnodeip=127.0.0.1&vnodeport=8545&block=2002326&token=******************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 区块信息
	}		
	
转账
=====================

方法：sendRawTransaction

参数:
::
	vnodeip： 		vnode节点地址
	vnodeport：		vnode节点端口
	from：			源账号地址
	to：			目标账号地址
	amount：			数量（单位 moac）
	data：			备注
	privatekey：		源账号私钥
	token：			auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/sendRawTransaction
	BODY：vnodeip=127.0.0.1&vnodeport=8545&from=0x**&to=0x***&amount=10&data=&privatekey=0x**&token=*******

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
	vnodeip： 	vnode节点地址
	vnodeport：	vnode节点端口
	address：	合约地址
	data：		调用合约参数
	token：		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/callContract
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x*****&data=0x****&token=***************

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
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	from：					源账号地址
	to：					目标账号地址
	contractaddress:			erc20合约地址
	amount：					erc20代币数量
	privatekey：				源账号私钥
	abistring：				erc20合约abi
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/transferErc
	BODY：vnodeip=&vnodeport=&from=0x**&to=0x**&contractaddress=0x**&amount=10&privatekey=0x**&abistring=****&token=*******

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
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	address：				账户地址
	contractaddress:			erc20合约地址
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/getErcBalance
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x*****&contractaddress=0x**&token=*********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 余额
	}	

充值子链  兑换子链原生币
=====================

方法：buyMintToken

参数:
::
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	address：				账户地址
	amount：					兑换数量
	privatekey：				源账号私钥
	microChainaddress:			子链地址
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/buyMintToken
	BODY：vnodeip=&vnodeport=&address=0x**&amount=**&privatekey=0x**&microChainaddress=0x**&token=****

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
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	microip： 				monitor节点地址
	microport：				monitor节点端口
	microchainaddress： 			子链SubChain地址
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/getBlockNumber
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x***&token=***********
返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链区块高度
	}	
	
获取子链区块信息
=====================

方法：getBlock

参数:
::
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	microip： 				monitor节点地址
	microport：				monitor节点端口
	microchainaddress： 			子链SubChain地址
	blocknum:				块号
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/getBlock
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x***&blocknum=*****&token=***********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 子链区块信息
	}	

获取子链账户余额
=====================

方法：getBalance

参数:
::
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	microip： 				monitor节点地址
	microport：				monitor节点端口
	microchainaddress： 			子链SubChain地址
	address：				账户地址
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/getBalance
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

参数:
::
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	microip： 				monitor节点地址
	microport：				monitor节点端口
	microchainaddress： 			子链SubChain地址
	via：					子链收益账号
	from：					源账户地址
	to：					目标账户地址
	amount：					原生币数量
	privatekey：				源账号私钥
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/transferCoin
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&via=0x**&from=0x**&to=0x**&amount=**&privatekey=0x***&token=*****

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	

子链加签交易
=====================

方法：sendRawTransaction

参数:
::
	vnodeip： 				vnode节点地址
	vnodeport：				vnode节点端口
	microip： 				monitor节点地址
	microport：				monitor节点端口
	from：					发送交易账户地址
	microchainaddress： 			子链SubChain地址
	amount:					payable对应金额	
	dappaddress:				dapp合约地址
	data 						调用合约参数
	privatekey：				源账号私钥
	token：					auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/sendRawTransaction
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&from=0x**&microchainaddress=0x***&amount=**&dappaddress=0x***dataStr=**&privatekey=0x***&token=*****

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}
	
子链合约调用 针对public方法和变量
=====================

方法：callContract

参数:
::
	vnodeip： 					vnode节点地址
	vnodeport：					vnode节点端口
	microip： 					monitor节点地址
	microport：					monitor节点端口
	microchainaddress： 				子链SubChain地址
	dappbaseaddress:					dappbase合约地址
	data 						调用合约参数   比如  ["方法名", 参数1, 参数2]
	token：						auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/callContract
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x*****&dappbaseaddress=0x**&data=****&token=********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	
	
子链提币 原生币转erc20
=====================

方法：redeemMintToken

参数:
::
	vnodeip： 					vnode节点地址
	vnodeport：					vnode节点端口
	microip： 					monitor节点地址
	microport：					monitor节点端口
	microchainaddress： 				子链SubChain地址
	dappbaseaddress:					dappbase合约地址
	address：					提币账户地址
	amount 						提取原生币数量
	privatekey：					源账号私钥
	token：						auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/redeemMintToken
	BODY：vnodeip=&vnodeport=&microip=127.0.0.1&microport=8546&microchainaddress=0x**&dappbaseaddress=0x**&address=0x**&amount=**&data=****&privatekey=0x**&token=********

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 交易hash
	}	