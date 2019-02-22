Restful API 简介
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

MOAC以Restful API提供给用户的一种接入方式。包括了对钱包，主链，主链，各交易查询的一系列封装。

DAPP用户当调用客户端SDK有困难时，可以通过服务端调用的方式实现对MOAC的接入。


URL设计
---------------------------

使用http作为API的通信协议，目前采用较多的POST方法。

url格式:    http(s)://server.com/api/{method}

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


节点控制
---------------------------
目前设计需要vnode节点和monitor节点的相关API，先通过参数传入地址和端口的方式以区别测试环境和正式环境。

以后可能扩展为请求token时一次性设置，并在整个token生命周期有效。



