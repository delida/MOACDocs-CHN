.. _rand-drop:

RandDrop 应用链
--------------

RandDrop 是采用基于阈值签名技术提供的可验证随机数来实现系统随机出块并达成共识的MOAC应用子链，其优点是可以杜绝单个节点对随机数的可操作性，保证系统随机数的不可预测性，保证了区块链的整体安全性。

阈值密码学是当前区块链技术发展的前沿。阈值密码学分为阈值加密和阈值签名（也可称为门限加密或门限签名），一般见于随机预言机、防审查、减少通信复杂度（HotStuff）、共识网络中防拜占庭（HoneyBadgerBFT）以及作为分布式伪随机数生成器的重要原语（Primitive），其优越的资产协同防盗特性也慢慢被新兴数字资产托管机制所重视。理想的阈值签名系统是可以在异步的网络环境里做到容错容灾不可伪造（non-forgeability），并且拥有极度可靠安全的消息传输通道，签名份额的生成和验证是完全非交互式的。
阈值签名系统的基础是参与的多个节点能够在初始阶段实现防止拜占庭行为的异步分布式密钥生成（DKG）机制，其中重要的第一步是将私钥片段安全地发送给多个参与者，即可验证秘密共享（Verifiable Secret Sharing - VSS）技术。最早的基于多项式的VSS是由1979年由MIT的Adi Shamir提出的,并由 Paul Feldman 完善。但是该算法基于的假设是存在一个可信的中心节点用来产生其余各个节点所需要的秘密片段。一旦该中心节点被攻击，或者被贿赂，则系统安全性就无法保证，因此风险较高。Pedersen在Feldman算法的基础上，通过去除中心节点，将秘密片段的生成方式改为分布式，显著的提高了秘密分享的安全性。

以DKG基础实现的阈值签名在区块链的应用中属于前沿的关键技术，目前开展研发工作的主要是各个大学和研究所。区块链平台中Dfinity项目和Facebook公司的Libra项目也在进行研究，但目前都还没有投入实用。此外，在过往的研究里，签名的生成和验证大多是交互式的，并且依赖一个同步通信网络和广播通道。节点们在某种设定下接收到特定消息后便同时启动签名协议，并严格遵循超时机制。而在实际互联网环境和区块链网络里，对网络假设的限定是有限的，所以阈值系统要成功运作除了需要构造真正的 DKG 协议和非交互式签名机制外，还需要具备商用级的网络系统以及被验证过的成熟代码。

RandDrop应用链使用的阈值签名技术是BLS（Boneh-Lynn-Shacham）签名算法，是由斯坦福大学的著名密码学教授Dan Boneh提出的，BLS算法是他和另外两位合作者的姓氏缩写。
BLS签名算法是一种可以实现签名聚合和密钥聚合的算法，它可以把一笔交易中的所有签名和公钥合并成单个签名和公钥，且合并过程无从追溯这个签名或公钥是否通过合并而来。阈值签名本质是m-of-n的签名方式，在知道m个签名的条件下，可以合成唯一一个合法的签名。任意m个签名片段的组合都是同一个可验证的签名。而且，由于每个人只有一个私钥片段，需要m个私钥片段组合在一起才能形成一个合法的完整的私钥。如果少于m个私钥片段在网络中共享的话，就不会有任何一个个人知道这个完整的私钥。这一点，保证了系统运行的安全性，不会由于少数节点被攻击而造成组私钥泄露。

RandDrop区块链系统中的密钥分发方案 DKG 采用Pedersen算法，可以在分布式协作下生成秘密，避免单点泄漏风险。系统中的多节点同时各自选择秘密值并运行自己的 VSS，每个节点收集来自其他节点的秘密片段完成组装，组装后的结果便是节点各自的私钥片段，而各个合法节点的私钥片段如果聚合起来便是最终的组私钥。注意该组私钥只是理论上存在，在实际的系统部署中，严格禁止节点共享任何私钥片段信息。
具体的阈值签名实现流程如下：
1.  各个节点中通过上述DKG算法，将其余节点所需要的秘密片段分发至网络中。网络中的各个节点在收到属于自己的片段后，可以本地还原出属于自己的私钥片段和组公钥。
2.  各个节点使用自己的私钥片段对某个公开的信息m签名，并将m的哈希和该签名片段一同广播到网络中。
3.  节点收集到超过阈值的签名片段后，可以将签名片段合并得到组签名。节点可以用1中还原的组公钥验证签名的有效性。

这里要解决以下2个关键的技术问题：
1)  私钥泄密的问题：在投票节点使用私钥签名的时候，需要m个私钥片段的合成。如果节点公布私钥片段，那么一旦大于m个私钥片段公布在网络中，任何一个节点就可以算出来全局私钥，导致私钥泄密。这样如果再次使用同样私钥就会为系统带来安全风险。而如果每个私钥只使用一次，那么发布私钥和收集签名的过程将会多次重复，导致系统效率低下。RandDrop这里的做法是，节点不应该公布共享私钥信息，而只是公布签名信息。这样，共享私钥可以一直使用，每个人可以验证签名的有效性，而全局私钥却没有人能够知道，从而在提高效率的同时，保证安全性。这样在系统中每个节点有自己的私钥片段，并可以使用它对某个信息（交易集合，或者预定义信息）进行签名并对全网公布。单个节点在收集>m个签名片段后，就可以合成全局签名。这个签名可以很容易通过全面公钥来验证。在这个过程中，全局私钥没有任何人知道，但是这个签名的结果是多个节点认可的结果，从而完成系统共识。
2)  节点在DKG过程中的作弊问题：当系统中的节点在DKG的过程中存在作弊问题时，如果作弊节点数量少于系统阈值，则系统应该有能力自主恢复并保证DKG流程可以继续进行下去。RandDrop使用智能合约技术管理整个DKG流程，做到关键数据与证据均上链保存。通过设计合理的slash机制，及时剔除作弊节点和无效节点，保证系统运行的活性和安全性。

RandDrop目前支持两种原子跨链交换，完成母链原生通证（ASM）或者ERC20通证（AST，待发布）和应用链原生通证之间的互换。

RandDrop 共识
====================

共识算法作为区块链系统的核心技术，决定了整个系统的性能和可扩展性，一直是当前区块链关键技术研究的重点。
目前最新的共识机制研究中有很多对于PoS和PBFT的改进，尤其是使用随机数算法来提高共识的效率和增加参与共识的节点数目。 

RandDrop采用BLS签名，从共识层支持多个节点的签名片段进行合并得到阈值签名，以此为基础产生随机数。随机数可以在RandDrop的智能合约里面直接调用。RandDrop随机数的优点是可以杜绝单个节点对最终签名的可操作性，更加安全可靠。同时，RandDrop的信息量是O(n)，比其他类似的随机数区块链具有较大的优势。

RandDrop应用链的验证过程由支持RandDrop应用链的客户端（SCS）完成，ProcWind和RandDrop的SCS节点不能混用。

应用链本身是以智能合约的方式部署到MOAC平台上，其共识方式、节点组成和业务逻辑都在应用链合约中定义。

* 应用链节点控制合约（ScsProtocolBase），用于定义SCS节点共识方式和如何包括SCS节点矿工加入应用链;
* 可验证秘密共享（VssBase）合约，构造函数需要提供１个threshold参数，该参数表示阈值签名的阈值。部署后，记录下vssbase合约的部署地址vssbaseAddress;
* 应用链逻辑控制合约（AppChainBase）：用于应用链控制逻辑，应用链生成前和生成后的一系列控制逻辑;
* 应用链合约控制（DappBase）：用于控制合约在应用链上的部署，一条应用链可以部署多个合约；目前有两类控制合约，一类是仅允许应用链的部署帐号，即拥有者（owner）在应用链上部署合约，而另一类则没有这个限制;
* 应用链DAPP智能合约：用于部署应用链业务逻辑的合约，每个应用链可以部署多个DAPP合约;

与ProcWind不同的是，在部署完VssBase和RandDropChainBase的合约后，需要在基础链上，调用VssBase合约的setCaller方法，
传入之前的RandDrop合约地址 subchainbaseAddress。
此方法调用后，保证了VssBase合约的部分关键函数只能由subchainbase合约调用，而无法由外部普通账户调用。

RandDrop 中随机数的使用
======================

RandDrop应用链可以产生随机数。随机数可以在RandDrop的智能合约里面直接调用。
::
    pragma solidity ^0.4.8;

    contract precompileBLS {
        function f() public returns(bytes32);
    }

    contract Lottery {
      mapping (uint8 => address[]) playersByNumber ;
      address owner;
      enum LotteryState { Accepting, Finished }
      LotteryState state;

      function Lottery() public {
          owner = msg.sender;
          state = LotteryState.Accepting;
      }

      function enter(uint8 number) public payable {
          require(number<=255);
          require(state == LotteryState.Accepting);
          playersByNumber[number].push(msg.sender);
      }

      function determineWinner() public {
          require(msg.sender == owner);
          state = LotteryState.Finished;
          uint8 winningNumber = random();
          distributeFunds(winningNumber);
      }

      function resetLotteryState() public {
          require(msg.sender == owner);
          state = LotteryState.Accepting;
      }

      function distributeFunds(uint8 winningNumber) private returns(uint256) {
          uint256 winnerCount = playersByNumber[winningNumber].length;
          uint256 balanceToDistribute = this.balance/(2*winnerCount);
          for (uint i = 0; i<winnerCount; i++) {
              playersByNumber[winningNumber][i].transfer(balanceToDistribute);
          }

          return this.balance;
      }

      function random() private view returns (uint8) {
          precompileBLS bls = precompileBLS(0x20);
          // get the 256-bit random number
          bytes32 r = bls.f();
          // return its first 8 bits, range from 0-255
          return uint8(r[0]);
      }
    }    
       

RandDrop 验证节点
================

RandDrop节点客户端SCS与ProcWind的不同，两者不能互用，必须使用支持分布式私钥共享（VSS）和BLS共识的智能合约服务器 SCS-VSS 客户端。
SCS-VSS也通过VNODE代理节点接入MOAC母链，每个运行的SCS可以支持多条应用链，也可以动态接入不同VNODE。

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
在发布目录中，也有脚本调用的例子，crossChainASM.js 和 crossChainAST.js。
具体做法可以参考：

:doc:`CrossChain`

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


