MOAC通证
^^^^^^^^


MOAC通证的起始供应总量为1.5亿枚。MOAC基金会本身持有6300万，MOAC区块链科技公司持有3100万个通证。3100万用于资助正在进行的群链技术发展。MOAC通证的价值可以在CoinMarketcap.com上查询。

MOAC通证最初在2017年7月以ERC20形式在以太坊平台发行。MOAC主网在2018年4月上线以后，已经将以太坊上的ERC20合约关闭，按地址将ERC20通证全部1：1转换成为MOAC主网的原生通证。

MOAC区块链每年将通过挖矿产生600万个通证，进一步增加流通中的供应量。4年后，产量减半至300万，接下来的4年再减半。到2058年，总供应量将达到2.1亿枚。

MOAC 的小写 moac 或者 mc 是MOAC平台上原生通证的名字，如同以太坊的ether，井通平台的swt一样。原生通证被用来支付母链上交易和合约执行的手续费，和子链的运行费用。可以参考 :ref:`gas-and-mc`.

计量系统
----------

MOAC具有用作mc单位的计量系统。 mc的最小单位，又称为基本单位为Sha，1 mc = 1e+18 Sha，其它的额度有自己的名称。 以下是额度名称的列表
他们在沙的价值。 按照共同的模式，mc还指定货币的单位（1e + 18或一个quintillion Sha）。 

+-------------------------+-----------+-------------------------------------------+
| Unit                    | Sha Value | Sha                                       |
+=========================+===========+===========================================+
| **sha**                 | 1 sha     | 1                                         |
+-------------------------+-----------+-------------------------------------------+
| **Ksha (femtomc)**      | 1e3 sha   | 1,000                                     |
+-------------------------+-----------+-------------------------------------------+
| **Msha (picomc)**       | 1e6 sha   | 1,000,000                                 |
+-------------------------+-----------+-------------------------------------------+
| **Gsha (xiao)**         | 1e9 sha   | 1,000,000,000                             |
+-------------------------+-----------+-------------------------------------------+
| **micromc (sand)**      | 1e12 sha  | 1,000,000,000,000                         |
+-------------------------+-----------+-------------------------------------------+
| **millimc**             | 1e15 sha  | 1,000,000,000,000,000                     |
+-------------------------+-----------+-------------------------------------------+
| **mc (moac)**           | 1e18 sha  | 1,000,000,000,000,000,000                 |
+-------------------------+-----------+-------------------------------------------+



如何获取MOAC通证
================================================================================

用户可以通过交易所交换MOAC，目前主要交易所在这里可以找 `here <https://coinmarketcap.com/currencies/moac/#markets>`_.；
或者使用矿机参与MOAC主网母链挖矿、应用链挖矿，也可以通过参与MOAC子链的活动；

交易MOAC
===================================================================

MOAC平台提供了一个在线钱包 `MOAC Wallet  <https://www.moacwalletonline.com/>`_  来方便用户使用简单的交易功能。

用户也可以通过 :ref:`MOAC VNODE 操作界面 <vnode-console>`来发送mc.

.. code-block:: console

    > var sender = mc.accounts[0];
    > var des = mc.accounts[1];
    > var amount = chain3.toSha(0.01, "mc")
    > mc.sendTransaction({from:sender, to:des, value: amount})


MOAC平台里面使用mc作为母链系统运行的燃料（gas），类似比特币中的BTC和以太坊中的ETH。除了交易费用外，gas是每个网络交易请求的核心部分，需要发送方为消耗的计算资源付费。 gas成本是根据网络请求的数量和复杂性动态计算的，并乘以当前的gas价格。 它作为一种加密燃料的价值具有提高MOAC平台的稳定性和对mc长期需求的作用。
除了在母链系统中的作用，MOAC平台的应用链也需要使用mc作为部署中需要支付的保证金和部署费用，同时，应用链的部署方还需要持续向应用链注入mc，以支付应用链矿工节点的验证费用，这套机制同时保证了mc的实用价值。

.. _gas-and-mc:

Gas 费用和 mc 之间的关系
=============================

如被认为是网络资源/利用的不变成本。您希望发送交易的实际成本始终是相同的，因此您不能真正预期将发行天然气，而总体而言，货币是易变的。

因此，我们发行的mc的价值应该有所不同，但还要根据MOAC实施汽油价格。如果mc的价格上涨，以mc表示的天然气价格应下降，以使天然气的实际成本保持不变。

gas（燃料）具有多个相关术语：燃料价格（），燃料成本（Gas​​ Cost），燃料限额（Gas Limit）和燃料费用（Gas Fees）。 
Gas背后的原理是对于MOAC网络上的交易或计算成本有稳定的价值。


* Gas Price是指以另一种货币或代币（如MOAC）表示的天然气成本。为了稳定天然气的价格，天然气价格是一个浮动值，这样，如果代币或货币的价格波动，天然气价格就会变化以保持相同的实际价值。天然气价格由平衡价格决定，该平衡价格是用户愿意花费多少，节点愿意接受多少。
* Gas​​ Cost 是关于Gas的计算成本的静态值，其目的是Gas的实际值永远不变，因此该成本应始终保持稳定。
* Gas​​ Limit 是每个块可以使用的最大Gas量，被视为最大计算负荷，交易量或一个块的块大小，矿工可以随着时间的推移慢慢更改此值。
* Gas Fees 实际上是运行特定交易或程序（称为合同）所需支付的燃料量。区块的Gas Fees可用于暗示区块的计算量，交易量或大小。Gas Fees也是支付给母链矿工的验证费用。

Gas is supposed to be the constant cost of network resources/utilisation. You want the real cost of sending a transaction to always be the same, so you can't really expect Gas to be issued, currencies in general are volatile.

So instead, we issue mc whose value is supposed to vary, but also implement a Gas Price in terms of MOAC. If the price of mc goes up, the Gas Price in terms of mc should go down to keep the real cost of Gas the same.

Gas has multiple associated terms with it: Gas Prices, Gas Cost, Gas Limit, and Gas Fees. The principle behind Gas is to have a stable value for how much a transaction or computation costs on the Ethereum network.

* Gas Cost is a static value for how much a computation costs in terms of Gas, and the intent is that the real value of the Gas never changes, so this cost should always stay stable over time.
* Gas Price is how much Gas costs in terms of another currency or token like MOAC. To stabilise the value of gas, the Gas Price is a floating value such that if the cost of tokens or currency fluctuates, the Gas Price changes to keep the same real value. The Gas Price is set by the equilibrium price of how much users are willing to spend, and how much processing nodes are willing to accept.
* Gas Limit is the maximum amount of Gas that can be used per block, it is considered the maximum computational load, transaction volume, or block size of a block, and miners can slowly change this value over time.
* Gas Fee is effectively the amount of Gas needed to be paid to run a particular transaction or program (called a contract). The Gas Fees of a block can be used to imply the computational load, transaction volume, or size of a block. The gas fees are paid to the miners (or bonded contractors in PoS).
