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
	BODY：address=0x******************&pwd=********&keyStore={****************}&token=********************************

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
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x******&token=********************************

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
	BODY：vnodeip=127.0.0.1&vnodeport=8545&token=********************************

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
	BODY：vnodeip=127.0.0.1&vnodeport=8545&block=2002326&token=********************************

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
	vnodeip： 	vnode节点地址
	vnodeport：	vnode节点端口
	from：		源账号地址
	to：		目标账号地址
	amount：	数量（单位 moac）
	data：		备注
	privateKey：源账号私钥
	token：		auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api/vnode/sendRawTransaction
	BODY：vnodeip=127.0.0.1&vnodeport=8545&from=0x*******&to=0x*******&amount=10&data=&privateKey=0x*******&token=********************************

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
	BODY：vnodeip=127.0.0.1&vnodeport=8545&address=0x*******&data=0x*******&token=********************************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 调用合约返回结果
	}		
	
	
子链模块
---------------------------

获得子链区块高度
=====================

方法：getBlockNumber

参数:
::
	vnodeip： 			vnode节点地址
	vnodeport：			vnode节点端口
	microip： 			monitor节点地址
	microport：			monitor节点端口
	contractaddress： 	子链SubChain地址
	token：				auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/getBlockNumber
	BODY：vnodeip=127.0.0.1&vnodeport=8545&microip=127.0.0.1&microport=8546&contractaddress=0x*******&token=********************************

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
	vnodeip： 			vnode节点地址
	vnodeport：			vnode节点端口
	microip： 			monitor节点地址
	microport：			monitor节点端口
	contractaddress： 	子链SubChain地址
	blocknum:			块号
	token：				auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/getBlock
	BODY：vnodeip=127.0.0.1&vnodeport=8545&microip=127.0.0.1&microport=8546&contractaddress=0x*******&blocknum=*****&token=********************************

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
	vnodeip： 			vnode节点地址
	vnodeport：			vnode节点端口
	microip： 			monitor节点地址
	microport：			monitor节点端口
	contractaddress： 	子链SubChain地址
	address				账户地址
	token：				auth返回的授权token
	
	
调用示例：
::
	POST: http://139.198.126.104:8080/api//micro/getBalance
	BODY：vnodeip=127.0.0.1&vnodeport=8545&microip=127.0.0.1&microport=8546&contractaddress=0x*******&address=0x*******&token=********************************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 账户余额
	}	

	