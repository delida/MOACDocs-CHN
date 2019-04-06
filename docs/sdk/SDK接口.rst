SDK 接口
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

钱包模块
---------------------------

注册
=====================

Node.JS Example

参数:
::
	pwd：钱包账户密码

代码:
::

	var account = require("moac-api").account;
	var wallet = account.register(pwd);

返回:
::

	wallet：
	{ address: '钱包地址....',
	  privateKey: '私钥....',
	  keyStore: 'keyStore内容...' 
	}
  
登录
=====================

Node.JS Example

参数:
::

	addr：钱包地址
	pwd：钱包密码
	keyStore：keyStore

代码:
::

	var account = require("moac-api").account;
	var status = account.login(addr, pwd, keyStore);

返回:
::

	status：0 //登录失败
	status：1 //登录成功
	status：2 //密码错误


主链模块
---------------------------


实例化主链对象
=========================

Node.JS Example

参数:
::
	vnodeAddress：主链访问地址 //http://47.106.69.61:8989
	
代码:
::
	var VnodeChain = require("moac-api").vnodeChain;
	var vc = new VnodeChain(vnodeAddress);

获取主链区块高度
===========================================

Node.JS Example


代码:
::
	var blockNumber = vc.getBlockNumber();

返回:
::
	blockNumber：主链区块高度
	
获取主链某一区块信息
====================================

Node.JS Example

参数:
::
	hashOrNumber：区块hash或区块高度

代码:
::
	var blockInfo = vc.getBlockInfo(hashOrNumber);

返回:
::
	blockInfo：某一区块信息

获取主链交易详情
=====================================

Node.JS Example

参数:
::
	hash：交易hash

代码:
::
	var tradeInfo = vc.getTransactionByHash(hash);

返回:
::
	tradeInfo：交易详情
	
获取合约实例
===========================

Node.JS Example

参数:
::
	microChainAddress：子链地址
	versionKey：版本号（默认0.1版本）

代码:
::
	var data = vc.getSubChainBaseInstance(microChainAddress, versionKey);

返回:
::
	data：合约实例
	
获取主链账户余额
=====================================

Node.JS Example

参数:
::
	addr：钱包账户地址 
	
代码:
::
	var balance = vc.getBalance(addr);
	
返回:
::
	balance：主链账户余额（单位为moac）

获取主链账户ERC代币余额
=============================================

Node.JS Example

参数:
::
	addr：钱包账户地址 
	contractAddress：合约地址
	
代码:
::
	var balance = vc.getErcBalance(addr, contractAddress);
	
返回:
::
	balance：账户ERC代币余额（erc20最小单位）
	
获取主链合约实例
================================

Node.JS Example

参数:
::
	abiObj：abi对象
	contractAddress：合约地址
	
代码:
::
	var object = vc.getContractInstance(abiObj, contractAddress);
	
返回:
::
	object：主链合约实例对象
	
获取交易Data
=========================

参数:
::
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）

代码:
::
	var data = mc.getData(method,paramTypes,paramValues);

返回:
::
	data：data字符串
	
主链加签交易
=========================

Node.JS Example

参数:
::
	from：交易发送人
	to：交易接受者（可以为个人地址，或者主链上的合约地址）
	amount：交易金额
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
	privateKey：交易发起人私钥字符串
	gasPrice：gas费用（默认为0，如返回错误为gas过低，请在返回的gas基础上加上整数gas重新提交）
	
代码:
::
	vc.sendRawTransaction(from, to, amount, method, paramTypes, paramValues, privateKey, gasPrice).then((hash) => {
		console.log(hash);
	});
	
返回:
::
	hash：交易hash
	
主链MOAC转账
=========================

参数:
::
	from：转账人地址
	to：收款人地址
	amount：交易金额（单位为moac）
	privatekey：转账人私钥

代码:
::
	vc.transferMoac(from, to, amount, privatekey).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash
	
主链ERC代币转账
==============================

参数:
::
	from：转账人地址
	to：收款人地址
	contractAddress：erc代币合约地址
	amount：交易金额（单位为moac）
	privateKey：转账人私钥

代码:
::
	vc.transferErc(from, to, contractAddress, amount, privateKey).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash
	
调用主链合约
=========================

参数:
::
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
	contractAddress：合约地址

代码:
::
	var callRes = vc.callContract(method, paramTypes, paramValues, contractAddress);

返回:
::
	callRes：调用合约返回信息
	
ERC20充值
=========================

参数:
::
	addr：钱包地址
	privateKey：钱包私钥
	microChainAddress：子链地址
	method：方法 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 ['address','uint256']
	paramValues：paramValues 参数值数组 ['0x.....',10000]（需要传金额的入参为erc20最小单位）

代码:
::
	vc.buyErcMintToken(addr, privateKey, microChainAddress, method, paramTypes, paramValues).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash

MOAC充值
=========================

参数:
::
	addr：钱包地址
	privateKey：钱包私钥
	microChainAddress：子链地址
	method：方法 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 ['address','uint256']
	paramValues：paramValues 参数值数组 ['0x.....',10000]（金额单位为moac）

代码:
::
	vc.buyMoacMintToken(addr, privateKey, microChainAddress, method, paramTypes, paramValues).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash
	
子链模块
---------------------------

实例化子链对象
=================================

Node.JS Example

参数:
::
	vnodeAddress：主链访问地址 //http://47.106.69.61:8989
	monitorAddress：子链访问地址 //http://47.106.89.22:8546
	microChainAddress：子链地址
	via：子链via

代码:
::
	var MicroChain = require("moac-api").microChain;
	var mc = new MicroChain(vnodeAddress, monitorAddress, microChainAddress, via);

获取子链区块高度
=========================

Node.JS Example

代码:
::
	mc.getBlockNumber().then((blockNumber) => {
		console.log(blockNumber);
	});

返回:
::
	blockNumber：子链区块高度
	
获取某一区间内的多个区块信息
=================================================

Node.JS Example

参数:
::
	start：开始高度
	end：结束高度

代码:
::
	mc.getBlocks(start, end).then((blockListInfo) => {
		console.log(blockListInfo);
	});

返回:
::
	blockListInfo：区块信息List
	
获取子链某一区块信息
==========================================

Node.JS Example

参数:
::
	blockNumber：区块高度

代码:
::
	mc.getBlock(blockNumber).then((blockInfo) => {
		console.log(blockInfo);
	});

返回:
::
	blockInfo：某一区块信息
	
获取子链交易详情
=========================

Node.JS Example

参数:
::
	transactionHash：交易hash

代码:
::
	mc.getTransactionByHash(transactionHash).then((transactionInfo) => {
		console.log(transactionInfo);
	});

返回:
::
	transactionInfo：交易详情
	
获取子链账户余额
=========================

Node.JS Example

参数:
::
	addr：钱包地址

代码:
::
	mc.getBalance(addr).then((balance) => {
		console.log(balance);
	});

返回:
::
	data：子链账户余额（erc20最小单位）
	
获取子链详细信息
=========================

Node.JS Example

代码:
::
	mc.getMicroChainInfo().then((microChainInfo) => {
		console.log(microChainInfo);
	});;

返回:
::
	microChainInfo：子链信息
	
获取子链DAPP状态
=========================

Node.JS Example

代码:
::
	mc.getDappState().then((state) => {
		console.log(state);
	});;

返回:
::
	state：1//正常
	state：0//异常

获取Nonce
=========================

Node.JS Example

参数:
::
	addr：账户钱包地址

代码:
::
	mc.getNonce(addr).then((nonce) => {
		console.log(nonce);
	});;

返回:
::
	nonce：得到的nonce
	
获取子链DAPP合约实例
============================================

参数:
::
	dappContractAddress：dapp合约地址
	dappAbi：dapp合约的Abi对象

代码:
::
	var dapp = getDappInstance(dappContractAddress, dappAbi);

返回:
::
	dapp：dapp实例

获取交易Data
=========================

参数:
::
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）

代码:
::
	var data = mc.getData(method,paramTypes,paramValues);

返回:
::
	data：data字符串


子链加签交易
=========================

Node.JS Example

参数:
::
	from：发送方的钱包地址
	microChainAddress：子链地址
	amount：交易金额
	dappAddress：dapp地址
	method：方法 例 "issue(address,uint256)"
	paramTypes：paramTypes 参数类型数组 例['address','uint256']
	paramValues：paramValues 参数值数组 例['0x.....',10000]（如需要传金额的入参为erc20最小单位）
	privateKey：发送方钱包私钥

代码:
::
	mc.sendRawTransaction(from, microChainAddress, amount, dappAddress, method, paramTypes, paramValues, privateKey).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash
	
子链转账
=========================

Node.JS Example

参数:
::
	from：发送方的钱包地址
	to：接收方的钱包地址
	amount：交易金额（erc20最小单位）
	privateKey：钱包私钥
	

代码:
::
	mc.transferCoin(from, to, amount, privateKey).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash
	
调用子链合约
=========================

参数:
::
	contractAddress：dapp合约地址
	param：例如合约中存在一个无参的方法getDechatInfo，则传入["getDechatInfo"];
     	     存在一个有参的方法getTopicList(uint pageNum, uint pageSize), 则传入["getTopicList", 0, 20]

代码:
::
	mc.callContract(contractAddress, param).then((data) => {
		console.log(data);
	});

返回:
::
	data：调用合约返回信息
	
提币（MOAC）
=========================

参数:
::
	addr：钱包地址
	amount：金额（单位为moac）
	privateKey：钱包私钥

代码:
::
	mc.redeemMoacMintToken(addr, amount, privateKey).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash

提币（ERC20）
=========================

参数:
::
	addr：钱包地址
	amount：金额（erc20最小单位）
	privateKey：钱包私钥

代码:
::
	mc.redeemErcMintToken(addr, amount,privateKey).then((hash) => {
		console.log(hash);
	});

返回:
::
	hash：交易hash


