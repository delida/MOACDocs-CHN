.. _rand-drop:

RandDrop 应用链
--------------


RandDrop应用链是指采用BLS签名，支持多合约部署的MOAC应用链。
RandDrop目前还在测试阶段，将会支持两种原子跨链交换，完成母链原生通证或者ERC20通证和应用链原生通证之间的互换。

RandDrop 共识
====================

共识算法作为区块链系统的核心技术，决定了整个系统的性能和可扩展性，一直是当前区块链关键技术研究的重点。
目前最新的共识机制研究中有很多对于PoS和PBFT的改进，尤其是使用随机数算法来提高共识的效率和增加参与共识的节点数目。 

RandDrop采用BLS签名，从共识层支持多个节点的签名片段进行合并得到阈值签名，以此为基础产生随机数。随机数可以在RandDrop的智能合约里面直接调用。RandDrop随机数的优点是可以杜绝单个节点对最终签名的可操作性，更加安全可靠。同时，RandDrop的信息量是O(n)，比其他类似的随机数区块链具有较大的优势。

RandDrop应用链的验证过程由支持RandDrop应用链的客户端（SCS）完成，ProcWind和RandDrop的SCS节点不能混用。

应用链本身是以智能合约的方式部署到MOAC平台上，其共识方式、节点组成和业务逻辑都在应用链合约中定义。

* 应用链节点控制合约（ScsProtocolBase），用于定义SCS节点共识方式和如何包括SCS节点矿工加入应用链;
* 可验证秘密共享（VssBase）合约，构造函数需要提供１个threshold参数，该参数表示阀值签名的阀值。部署后，记录下vssbase合约的部署地址vssbaseAddress;
* 应用链逻辑控制合约（AppChainBase）：用于应用链控制逻辑，应用链生成前和生成后的一系列控制逻辑;
* 应用链合约控制（DappBase）：用于控制合约在应用链上的部署，一条应用链可以部署多个合约；目前有两类控制合约，一类是仅允许应用链的部署帐号，即拥有者（owner）在应用链上部署合约，而另一类则没有这个限制;
* 应用链DAPP智能合约：用于部署应用链业务逻辑的合约，每个应用链可以部署多个DAPP合约;

与ProcWind不同的是，在部署完VssBase和RandDropChainBase的合约后，需要在基础链上，调用VssBase合约的setCaller方法，
传入之前的RandDrop合约地址 subchainbaseAddress。
此方法调用后，保证了VssBase合约的部分关键函数只能由subchainbase合约调用，而无法由外部普通账户调用。

RandDrop 验证节点
================

RandDrop节点客户端与ProcWind不同，称智能合约服务器 Smart Contract Server(SCS-VSS) 是支持BLS共识的节点软件。
SCS-VSS也通过VNODE代理节点接入MOAC母链，每个运行的SCS可以支持多条应用链，也可以动态接入不同VNODE。
目前有两种应用链客户端，分别支持对应的应用链ProcWind和RandDrop。

当前，按在应用链中的功能分，有如下几种SCS节点类型：

* 参与业务逻辑的SCS
* 用于业务监控的SCS
* 准备参与业务逻辑的SCS

节点的操作可以参考：

:doc:`Setup`

RandDrop 通证
====================

RandDrop 应用链也支持应用链上的原生通证（TOKEN），其发行方式是在应用链合约中设定，这点和ProcWind相同，
详细介绍请参考：
:ref:`RandDrop 应用链的部署<rand-drop-setup>` 

关于RandDrop上通证的转移，也是采用shardingFlag=2的方式，例子如下：
::
    //Example to transfer the AppChain tokens
    function sendAppChainToken(baseaddr,basepsd,appchainaddr,amount,code,sf,n)
    {       
        //unlock the 
        chain3.personal.unlockAccount(baseaddr,basepsd,0);

        //transfer the appchain token
        chain3.mc.sendTransaction(
        {       
                from: baseaddr,
                value:chain3.toSha(amount,'mc'), // note this value is the appchain token value, not mc
                to: appchainaddr,
                gas: '0',//'200000',
                gasPrice: '0',//chain3.mc.gasPrice,
                ShardingFlag: sf,
                data: code,
                nonce: n,
                via:via,
        });
                
        console.log('sending from:' + baseaddr + ' to:' + appchainaddr  + ' with nonce:' + n);
    }
    // Call the function to transafer
    var amount = 1;
    //函数输入参数说明
    //baseaddr:转出地址  
    //basepsd：转出地址的密码 
    //appchainaddr:应用链地址 
    //amount:转账金额 
    //receive: 转入的地址 
    //'0x2'： shardingFlag 设为2为应用链原生货币转换
    //n: 转出地址的nonce
    sendAppChainToken(baseaddr,basename,appchainaddr,amount,receive,'0x2',n)

RandDrop 跨链
====================

应用链通证可以和母链的原生货币或者ERC20代币直接进行兑换，只需要部署不同的应用链合约并执行相应功能调用即可完成。
具有与母链原生货币（moac）进行跨链交换功能合约的名称为ASM（Atomic Swap of Moac）。
具有与母链ERC20代币进行跨链交换功能合约的名称为AST（Atomic Swap of Token）。
具体做法可以参考：

:doc:`ProcWindExchange`

RandDrop 应用链的参数和设置
=========================

目前采用RandDrop共识的应用也分为两种：ASM和AST。
在MOAC发布的目录可以看到合约内容，主要的不同是需要加入VssBase.sol的部署地址。

ASM的合约构建函数为：
:: 
    function ChainBaseASM(
    address scsPoolAddr, 
    address vnodeProtocolBaseAddr, 
    uint min, 
    uint max, 
    uint thousandth, 
    uint flushRound, 
    uint256 tokensupply, 
    uint256 exchangerate,
    address vssBaseAddr
    )

其中的参数含义为：

* address scsPoolAddr - SCS节点池地址；
* address vnodeProtocolBaseAddr - Vnode节点池合约地址；
* uint min - 应用链需要SCS的最小数量，需要从如下值中选择：1，3，5，7；
* uint max - 应用链需要SCS的最大数量，需要从如下值中选择：11，21，31，51，99
* uint thousandth - 控制选择scs的概率，建议设为1，对于大型应用链节点池才有效；
* uint flushRound - 应用链刷新周期  单位是主链block生成对应数量的时间，当前的取值范围是40-99；
* uint256 tokensupply - 应用链的原生货币数量；
* uint256 exchangerate - 应用链原生货币和母链moac的兑换比例；
* address vssBaseAddr - VSSBase的部署地址；

注意，这里输入参数tokensupply和应用链的BALANCE相对映，
BALANCE = tokensupply * 1e18
例如，tokensupply = 1000，结果的BALANCE应该是10的21次方。

AST的合约构建函数为：
:: 
    function ChainBaseAST(
    address scsPoolAddr, 
    address vnodeProtocolBaseAddr, 
    address ercAddr,  
    uint ercRate,
    uint min, 
    uint max, 
    uint thousandth, 
    uint flushRound,
    uint256 exchangerate,
    address vssBaseAddr
    )

其中的参数含义为：

* address proto - SCS节点池地址；
* address vnodeProtocolBaseAddr - Vnode节点池合约地址；
* address ercAddr - 基础链ERC20合约地址；
* uint ercRate - 应用链原生货币和基础链ERC20 token的兑换比例；
* uint min - 应用链需要SCS的最小数量，需要从如下值中选择：1，3，5，7；
* uint max - 应用链需要SCS的最大数量，需要从如下值中选择：11，21，31，51，99
* uint thousandth - 控制选择scs的概率，建议设为1，对于大型应用链节点池才有效；
* uint flushRound - 应用链刷新周期  单位是主链block生成对应数量的时间，当前的取值范围是40-99；
* uint256 exchangerate - 应用链原生货币和母链moac的兑换比例；
* address vssBaseAddr - VSSBase的部署地址；

注意，AST应用链的BALANCE不能设定，而是由ERC20 token里面totalSupply所决定的，
BALANCE = tokenSupply * ERCRate * (10 ** (ERCDecimals));

用户可以根据需要调试输入参数，之后的应用链部署步骤请参考：

:doc:`RandDropSetup`

:ref:`RandDrop 应用链推荐设置 <randdrop-optimize>` 

如果遇到问题，可以参考

:ref:`应用链部署常见问题 <faq-all>` 

RandDrop Dapp开发指南
====================

RandDrop应用链的开发基本与ProcWind相同，
详细介绍可参看这篇：

:ref:`ProcWind Dapp开发指南 <proc-wind-dapps>` 


