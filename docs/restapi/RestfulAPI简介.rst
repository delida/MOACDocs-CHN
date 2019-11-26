Restful API 简介
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

为了简化DAPP的应用，MOAC以Restful API提供给用户的一种接入方式。包括了对钱包，主链，子链，各交易查询的一系列封装。

DAPP用户当调用客户端SDK有困难时，可以通过服务端调用的方式实现对MOAC的接入。


URL设计
---------------------------

使用http作为API的通信协议，目前采用较多的POST方法。

url格式:    http(s)://server.com/api/{module}/{version}/{method}

关于version，各模块支持多版本，默认version为v1.0

POST提交格式采用form表单参数    Content-Type: application/x-www-form-urlencoded (a=name&b=666)


结构返回
---------------------------
返回体格式采用json格式
::	
	{
		"success": true,
		"message": "",
		"data": "*********"
	}
	success:  true - 成功    false - 失败
	message： 失败时的错误信息
	data：	  接口返回数据

相关状态码
200 OK
403 token权限受限


接口访问控制
---------------------------

API访问控制，需先请求访问的用户名和密码，先调用auth接口请求访问token。

各api接口需要token参数调用，不然会返回403错误。

token有过期机制，目前是2小时有效。

私钥安全
---------------------------

对于账号私钥的安全提供了两种方案。

1. dapp注册账户后，收到返回的私钥（privatekey），后续发送交易直接传递私钥
2. dapp注册账户后，也会返回一个加密串（encode），后续发送交易传递加密串和账户密码，系统会解码获得私钥进行签名。


节点控制
---------------------------

测试环境地址：http://139.198.126.104:8080

测试环境获取access token，可使用测试账号：test    密码：123456

正式环境地址：https://api.moac.io:8080

正式环境获取access token，请联系开发团队：moacapi@mossglobal.net。

目前设计需要vnode节点的相关API，可通过参数传入地址和端口的方式指定连接的节点。
参数传入为空的情况下，会使用平台默认的节点信息（测试环境对应testnet的默认节点，正式环境默认主网节点）。





