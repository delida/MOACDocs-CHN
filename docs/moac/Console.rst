交互命令行
===========


墨客母链客户端使用了和以太坊类似的交互式命令行。用户可以在命令行（console）中执行内置的JAVA script命令或者利用脚本（script），输出结果显示在命令行中。
这里使用的chain3对象，是MOAC参考以太坊，而开发的一套javascript库，目的是让应用程序能够与MOAC的VNODE和SCS节点进行通信。注意，这里有两层，moac启动了一个MOAC VNODE节点，console参数开启了一个javascript的控制台，这个控制台注入了chain3.js这个库，以使我们可以通过chain3对象与MOAC VNODE节点做交互。


Interactive use: the JSRE REPL Console
--------------------------------------

The MOAC VNODE CLI executable moac has a JavaScript console (a Read, Evaluate & Print Loop = REPL exposing the JSRE), which can be started with the console or attach subcommand. The console subcommands starts the moac node and then opens the console. The attach subcommand will not start the moac node but instead tries to open the console on a running moac instance.

::

$ moac console
$ moac attach

The attach node accepts an endpoint in case the moac node is running with a non default ipc endpoint or you would like to connect over the rpc interface.
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



Note that by default the moac node doesn't start the http and weboscket service and not all functionality is provided over these interfaces due to security reasons. These defaults can be overridden when the --rpcapi and --wsapi arguments when the moac node is started, or with admin.startRPC and admin.startWS.

If you need log information, start with:

::

$ moac --verbosity 4 console 2>> /tmp/vnode.log

Otherwise mute your logs, so that it does not pollute your console:
::

$ moac console 2>> /dev/null

or

::

$ moac --verbosity 0 console

Geth has support to load custom JavaScript files into the console through the --preload argument. This can be used to load often used functions, setup chain3 contract objects, or

::

$ moac --preload "/my/scripts/folder/utils.js,/my/scripts/folder/contracts.js" console

Non-interactive use: JSRE script mode
-------------------------------------

It's also possible to execute files to the JavaScript interpreter. The console and attach subcommand accept the --exec argument which is a javascript statement.

::

$ moac --exec "mc.blockNumber" attach

This prints the current block number of a running moac instance.

Or execute a local script with more complex statements on a remote node over http:

::

$ moac --exec 'loadScript("/tmp/checkbalances.js")' attach http://127.0.0.1:8545
$ moac --jspath "/tmp" --exec 'loadScript("checkbalances.js")' attach http://127.0.0.1:8545

Use the --jspath <path/to/my/js/root> to set a libdir for your js scripts. Parameters to loadScript() with no absolute path will be understood relative to this directory.

You can exit the console cleanly by typing exit or simply with CTRL-C.

Caveat
------

Just like go-ethereum, MOAC's JSRE uses the Otto JS VM which has some limitations:

"use strict" will parse, but does nothing.
The regular expression engine (re2/regexp) is not fully compatible with the ECMA5 specification.
Note that the other known limitation of Otto (namely the lack of timers) is taken care of. Ethereum JSRE implements both setTimeout and setInterval. In addition to this, the console provides admin.sleep(seconds) as well as a "blocktime sleep" method admin.sleepBlocks(number).

Since chain3.js uses the bignumber.js library (MIT Expat Licence), it is also autoloded.

Timers
------

In addition to the full functionality of JS (as per ECMA5), the moac JSRE is augmented with various timers. It implements setInterval, clearInterval, setTimeout, clearTimeout you may be used to using in browser windows. It also provides implementation for admin.sleep(seconds) and a block based timer, admin.sleepBlocks(n) which sleeps till the number of new blocks added is equal to or greater than n, think "wait for n confirmations".

Management APIs
---------------

Beside the official DApp API interface the VNODE has support for additional management API's. These API's are offered using JSON-RPC and follow the same conventions as used in the DApp API. The VNODE package comes with a console client which has support for all additional API's.

