SDK 简介
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

为了方便用户接入，MOAC官方提供nodejs 版本的SDK，官方暂不考虑提供其他版本的SDK。

Node.JS SDK下载安装
::
	npm install moac-api
	
Node.JS SDK异常处理说明：

应用方根据自己业务逻辑对sdk方法进行 try catch 异常处理

示例：
::
	var VnodeChain = require("moac-api").vnodeChain;

	try{
		var vc = new VnodeChain("http://47.106.69.61:8989");
		var blockNumber = vc.getBlockNumber();
		console.log(blockNumber);
	}catch (e){
		console.log(e);
	}
	
	



