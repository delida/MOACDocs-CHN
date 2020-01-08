交互命令行
===========


MOAC母链客户端使用了和以太坊类似的交互式命令行。用户可以在命令行（console）中执行内置的JAVA script命令或者利用脚本（script），输出结果显示在命令行中。
这里使用的chain3对象，是MOAC参考以太坊，而开发的一套javascript库，目的是让应用程序能够与MOAC的VNODE和SCS节点进行通信。注意，这里有两层，moac启动了一个MOAC VNODE节点，console参数开启了一个javascript的控制台，这个控制台注入了chain3.js这个库，以使我们可以通过chain3对象与MOAC VNODE节点做交互。

.. _vnode-console:

VNODE和用户交互的命令行界面
--------------------------------------

MOAC 母链节点的客户端（VNODE）有一个可以和用户交互的JavaScript命令行界面。启动这个命令行可以使用下面两个命令
::
    $ moac console
    $ moac attach

如果使用console参数，moac客户端会首先启动VNODE节点并自动进入命令行界面。 而使用attach参数，moac客户端不会启动VNODE节点，而是去连接一个已经运行的VNODE节点并显示命令行界面。
::
    $ moac attach ipc:/some/custom/path
    $ moac attach http://127.0.0.1:8545
    $ moac attach ws://127.0.0.1:8546

参数说明
--datadir  数据库和秘钥存储路径
--dev 开发环境，采用权威证明(PoA)，开发帐号有预存金额，自动封装交易数据到区块链
--rpc 开启HTTP-RPC服务，默认8545端口
--rpccorsdomain 允许连接到rpc的url匹配模式 *代表任意
console 启动一个交互式的JavaScript环境

请注意，默认情况下，MOAC VNODE节点不会启动http和weboscket服务，并且由于安全原因，并非所有功能都通过这些接口提供。 当启动VNODE节点时使用--rpcapi和--wsapi参数，或者使用admin.startRPC和admin.startWS，可以覆盖这些默认值。

log 参数会在终端显示更多的信息:
::

$ moac --verbosity 4 console 2>> /tmp/vnode.log

如果不想被太多的系统信息干扰命令行的界面，可以关掉系统输出：
::

$ moac console 2>> /dev/null

or

::

$ moac --verbosity 0 console

MOAC支持在交互命令行（console）中通过--preload参数使用用户定义的JavaScript脚本文件。

::

$ moac --preload "/my/scripts/folder/utils.js,/my/scripts/folder/contracts.js" console

非交互式使用: JSRE 脚本模式
-------------------------

VNODE也可以执行文件到JavaScript解释器。 console和attach子命令接受--exec参数，这是一个javascript语句。
::

$ moac --exec "mc.blockNumber" attach

这将打印正在运行的MOAC VNODE上的当前区块号。
或通过http在远程节点上执行带有更复杂语句的本地脚本：
::

$ moac --exec 'loadScript("/tmp/checkbalances.js")' attach http://127.0.0.1:8545
$ moac --jspath "/tmp" --exec 'loadScript("checkbalances.js")' attach http://127.0.0.1:8545

使用--jspath <path / to / my / js / root>为您的js脚本设置一个libdir。 相对于该目录，将理解没有绝对路径的loadScript（）参数。

您可以通过键入exit或直接使用CTRL-C退出控制台。

注意
------

MOAC's JSRE 和以太坊的客户端一样采取了Otto JS VM 虚拟机并具有以下限制:

"use strict" 语句可以编译通过，但没有相应效果。
正则表达式引擎（re2 / regexp）与ECMA5规范不完全兼容。
注意，Otto的另一个已知限制（即缺少计时器）已得到解决。 以太坊JSRE同时实现setTimeout和setInterval。 除此之外，控制台还提供admin.sleep（seconds）以及“ blocktime sleep”方法admin.sleepBlocks（number）。

chain3.js 自动加载 bignumber.js 的软件库(MIT Expat Licence)用于大数处理。

时钟（Timers）
--------------

除了JS的全部功能（按照ECMA5）之外，MOAC VNODE的JSRE还增加了各种计时器。 它实现了在浏览器窗口中使用的setInterval，clearInterval，setTimeout，clearTimeout。 它还提供了admin.sleep（seconds）和基于块的计时器admin.sleepBlocks（n）的实现，该定时器一直休眠直到添加的新块的数量等于或大于n为止，认为“等待n个确认”。

高级管理 APIs
---------------

除了官方的API界面外，VNODE还支持其他管理API。 这些API是使用JSON-RPC提供的，并且遵循与DApp API中使用的相同约定。 VNODE软件包随附一个控制台客户端，该客户端支持所有其他API。

