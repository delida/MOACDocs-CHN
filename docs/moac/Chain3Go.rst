Chain3 GO 软件库
================

MOAC Go API was built for MOAC chain. It was developed from MOAC RPC API, which can be used to develop Ðapp on MOAC chain. It supports both VNODE and SCS JSON RPC API methods in MOAC network.

Chain3Go Installation
---------------------

setup $GOPATH
''''''''''

``
export GOPATH=/Users/[user]/go
``

go get

``bash
go get -u github.com/MOACChain/Chain3Go
``

MOAC Configuration
---------------------

Install MOAC
''''''''''''

Download latest MOAC Vnode and SCS Releases from here: https://github.com/MOACChain/moac-core/releases

Run MOAC
''''''''''

Run moac vnode on testnet


.. code:: js

./moac --testnet

Run moac scs to connect the VNODE locally


.. code:: js

./scsserver

Create new accounts and send transactions


.. code:: js

    mc.coinbase
    mc.accounts
    personal.newAccount()
    passphrase:
    repeat passphrase:

    miner.start()
    --wait a few seconds
    miner.stop()

    personal.unlockAccount("0x18833df6ba69b4d50acc744e8294d128ed8db1f1")
    mc.sendTransaction({from: '0x18833df6ba69b4d50acc744e8294d128ed8db1f1', to: '0x2a022eb956d1962d867dcebd8fed6ae71ee4385a', value: chain3.toSha(12, "moac")})


Chain3Go Execution
''''''''''''''''''

.. code::js

go run main.go


Requirements
---------------------

go ^1.8.3


[Go installation instructions.](https://golang.org/doc/install)

