MOAC通证
***********


MOAC通证的起始供应总量为1.5亿枚。MOAC基金会本身持有6300万，MOAC区块链科技公司持有3100万个通证。3100万用于资助正在进行的群链技术发展。MOAC通证的价值可以在CoinMarketcap.com上查询。

MOAC通证最初在2017年7月以ERC20形式在以太坊平台发行。MOAC主网在2018年4月上线以后，已经将以太坊上的ERC20合约关闭，按地址将ERC20通证全部1：1转换成为MOAC主网的原生通证。

MOAC区块链每年将通过挖矿产生600万个通证，进一步增加流通中的供应量。4年后，产量减半至300万，接下来的4年再减半。到2058年，总供应量将达到2.1亿枚。

MOAC 的小写 moac 或者 mc 是MOAC平台上原生通证的名字，如同以太坊的ether，井通平台的swt一样。原生通证被用来支付母链上交易和合约执行的手续费，和子链的运行费用。可以参考 :ref:`gas-and-mc`.

计量系统
--------------------------------------------------------

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


MOAC平台里面使用mc作为系统运行的燃料（gas），类似比特币中的BTC和以太坊中的ETH. Beyond transaction fees, gas is a central part of every network request and requires the sender to pay for the computing resources consumed. The gas cost is dynamically calculated, based on the volume and complexity of the request and multiplied by the current gas price. Its value as a cryptofuel has the effect of increasing the stability and long-term  demand for mc and MOAC as a whole. 

.. _gas-and-mc:

Gas 费用和 mc
=============================


Gas is supposed to be the constant cost of network resources/utilisation. You want the real cost of sending a transaction to always be the same, so you can't really expect Gas to be issued, currencies in general are volatile.

So instead, we issue mc whose value is supposed to vary, but also implement a Gas Price in terms of MOAC. If the price of mc goes up, the Gas Price in terms of mc should go down to keep the real cost of Gas the same.

Gas has multiple associated terms with it: Gas Prices, Gas Cost, Gas Limit, and Gas Fees. The principle behind Gas is to have a stable value for how much a transaction or computation costs on the Ethereum network.

* Gas Cost is a static value for how much a computation costs in terms of Gas, and the intent is that the real value of the Gas never changes, so this cost should always stay stable over time.
* Gas Price is how much Gas costs in terms of another currency or token like MOAC. To stabilise the value of gas, the Gas Price is a floating value such that if the cost of tokens or currency fluctuates, the Gas Price changes to keep the same real value. The Gas Price is set by the equilibrium price of how much users are willing to spend, and how much processing nodes are willing to accept.
* Gas Limit is the maximum amount of Gas that can be used per block, it is considered the maximum computational load, transaction volume, or block size of a block, and miners can slowly change this value over time.
* Gas Fee is effectively the amount of Gas needed to be paid to run a particular transaction or program (called a contract). The Gas Fees of a block can be used to imply the computational load, transaction volume, or size of a block. The gas fees are paid to the miners (or bonded contractors in PoS).
