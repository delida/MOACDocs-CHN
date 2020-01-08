VNODE节点快速上手指南
=====================

官方版本下载
------------

最新的MOAC VNODE软件可以从官方的发布地址下载 `release
link <https://github.com/MOACChain/moac-core/releases>`__

Debian/Ubuntu/CentOS
~~~~~~~~~~~~~~~~~~~~

1. 使用 tar 来解压缩，在当前目录下运行：
::
  ./moac

如果要看帮助信息, 使用help参数: ./moac --help

如果要启动交互命令行，运行console参数: ./moac console

主网的存储目录会自动生成，在默认的 $HOME/.moac/ 下面，可以看到下面的信息:

::

    INFO [04-24|11:24:26.506] 86161:IPC endpoint closed: /home/user/.moac/moac.ipc 

再打开另一个命令行终端，再次运行：

::

    ./moac attach 

或者制定ipc文件：

::

    ./moac attach $HOME/.moac/moac.ipc

2. 进入命令行（console）后，产生一个新帐号：

   ::

       >personal.newAccount()

3. 在命令行提示符下，开始挖矿：

   ::

       >miner.start()

4. 在另一个终端上，再次运行MOAC VNODE以连接运行中的节点：

   ::

       ./moac attach

5. 在命令行提示符，加载脚本：

   ::

       >loadScript("mctest.js")

6. 查看之前挖矿是否获得了mc，注意如果是连接的主网或者测试网，用户仅使用CPU挖矿的话，可能余额没有变化：

   ::

       >mc.accounts

7. 产生一个新账号：

   ::

       >personal.newAccount()

8. 尝试从帐号0发送交易到帐号1:

   ::

       >Send(mc.accounts[0], '', mc.accounts[1], 0.1)

WINDOWS
^^^^^^^

将文件解压缩到目录下，运行程序
::
  D:\moac-win>moac.exe


2.1 查看moac帮助
''''''''''''''''

打开命令（cmd）终端，转到MOAC解压目录，在命令行中执行：

::

    D:\moac-win>moac --help

显示帮助信息，包含但不限于以下内容：

查看moac帮助

打开命令（cmd）终端，转到MOAC解压目录，在命令行中执行：

::

    D:\ moac-win>moac --help

显示帮助信息，包含但不限于以下内容：

::

    Start MOAC.... 2
    NAME:
     moac - the MOAC-vnode command line interface
     Copyright 2017-2019 The MOAC Foundation
    USAGE:
     moac [options] command [command options] [arguments...]
    VERSION:
     1.0.12-rc-2b24668f
    MOAC CORE OPTIONS:
    --config value                      TOML configuration file
    --datadir "C:\Users\[userName]\AppData\Roaming\MoacNode" Data directory for the databases and keystore
    --keystore                         Directory for the keystore (default = inside the datadir)
    --nousb                           Disables monitoring for and managing USB hardware wallets
    --networkid value                   Network identifier (integer, 1=Pangu, 2=Testnet) (default: 1)
    --testnet                          MOAC test network: pre-configured proof-of-work test network

2.2 运行节点
''''''''''''

打开命令（cmd）终端，转到MOAC当前目录，在命令行中执行：

::

    D:\ moac-win>moac.exe

显示如下信息：

.. figure:: ../image/moac_install_win_0.png
   :alt: moac\_install\_win\_0


至最后三行显示如下：

::

    INFO [04-01|20:44:42.851] 1:[node/node.go->Node.startIPC]
    INFO [04-01|20:44:42.852] 145:IPC endpoint opened: \\.\pipe\moac.ipc
    INFO [04-01|20:45:12.846] 152:Block synchronisation started

表示节点安装成功，如果网络正常，就开始同步区块。

系统将MOAC节点默认安装在目录：

::

    C:\Users\[userName]\AppData\Roaming\MoacNode\

该目录下包含两个文件夹：moac和keystore。

2.3 进入MOAC console界面
''''''''''''''''''''''''

系统关机或主动关闭运行中的节点后，如果需要重新启动节点，在命令行中执行：

::

    D:\ moac-win>moac console

之后一直滚屏以同步区块数据。

打开另一个命令（cmd）终端，转到MOAC当前目录，在命令行中执行：

::

    D:\ moac-win>moac attach

.. figure:: ../image/moac_install_win_1.png
   :alt: moac\_install\_win\_1


该命令行不会主动滚屏，而是等待命令。

3. 挖矿
^^^^^^^

3.1 建立新账户
''''''''''''''

挖矿前必须建立一个自己的账户。

进入MOAC console界面，执行命令：

::

    > personal.newAccount()

系统会提示输入一个密码，例如"passwd"，并再次输入相同密码确认后，会显示一个以0x开头的字符串，即为MOAC帐号的公开地址。

.. figure:: ../image/moac_install_win_2.png
   :alt: moac\_install\_win\_2


系统同时会在以下目录：

::

    C:\Users\[userName]\AppData\Roaming\MoacNode\testnet\keystore

记录一个账号文件。请保存好该文件，并牢记密码，之后用于解密帐号和操作。

3.2 查看账户
''''''''''''

进入MOAC console界面，执行命令：

::

    > mc.accounts

可以查看本节点下的所有账号。

3.3 查看账户余额
''''''''''''''''

进入MOAC console界面，执行命令：

::

    > mc.getBalance(mc.accounts[0])

可以查看本节点下的账号余额。0表示第一个账户，也是默认挖矿账户。

或者：导入“mctest.js”的情况下（见4.1），执行命令：

::

    > checkBalance()

该命令用于查看当前节点所有账号的余额。

3.4 查看挖矿状态
''''''''''''''''

进入MOAC console界面，执行命令：

::

    > mc.mining

返回true表明节点正在挖矿，false表明节点没有挖矿。

3.5 开始挖矿
''''''''''''

进入MOAC console界面，执行命令：

::

    > miner.start()

挖矿状态下，数据显示有明显不同。

.. figure:: ../image/moac_install_win_4.png
   :alt: moac\_install\_win\_4


挖到矿之后，可以查看余额

.. figure:: ../image/moac_install_win_5.png
   :alt: moac\_install\_win\_5

登录MOAC区块链浏览器页面： http://explorer.moac.io。

.. figure:: ../image/moacExplorerMain.png

在搜索栏输入你的挖矿账号地址，会显示该账号的余额等信息。

.. figure:: ../image/moacExplorerAccount.png


在搜索栏输入你挖到矿的区块号，会显示该区块的信息。

Miner正是你的账号地址。

.. figure:: ../image/moacExplorerBlock.png


3.6 停止挖矿
''''''''''''

进入MOAC console界面，执行命令：

::

    > miner.stop()

4. 交易
^^^^^^^

4.1 读入测试函数
''''''''''''''''

部分功能程序存储在mctest.js里。

进入MOAC console界面，执行命令：

::

    > loadScript("mctest.js")

4.2 交易条件
''''''''''''

为执行交易，需要至少两个帐号，其中一个有足够的mc。

如果没有目标账号，可以用步骤2.3.1的命令创建一个本地账号。并用命令：

::

    > mc.accounts

显示当前节点中存储的账号，应该至少有一个挖矿账号。

4.3 交易
''''''''

进入MOAC console界面，执行命令：

::

    > Send(mc.accounts[0], 'passwd', mc.accounts[1], 0.1)

这个过程需要第一个账号的密码。比如'passwd'，发送额为0.1 mc。

.. figure:: ../image/moac_install_win_6.png
   :alt: moac\_install\_win\_6


在系统挖矿的情况下，发送应该在下一个区块产生时完成。

系统显示的是以 **sha（Sand）** 为单位的余额， **1 mc = 1e18 sha。**
