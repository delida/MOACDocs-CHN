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
	account： 授权账号
	pwd：授权账号密码
	
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
	POST: http://139.198.126.104:8080/api/getBalance
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
	POST: http://139.198.126.104:8080/api/getBlockNumber
	BODY：vnodeip=127.0.0.1&vnodeport=8545&token=********************************

返回数据示例	
::	
	{
		"success": true,
		"message": "",
		"data": 区块高度
	}	