VNODE节点命令行
=============

常用的命令行参数：
---------------

::

    --testnet: connect to MOAC testnet (networkid = 101);

    --rpc: initiate RPC service of HTTP, so as to nonlocally access the specific MOAC node service;

    --rpcaddr value: default as "localhost", only local accessibility; can be reset to "0.0.0.0", so as to nonlocally access the specific MOAC node service, but the current RPC service is based on HTTP using plaintext，so users need to be aware of the safety risks;

    --rpcport value: default as"8545", normally no changes are needed, users can use the default port;

    --rpcapi value: command RPC as to which API service will be open，default as "chain3,mc,net"，the commonly used will also be configured such as: personal,admin,debug,miner,txpool,db,shh and so on，but because of RPC services' cleartext nature，if users choose to use personal，they should be aware of the safety risks；

    --rpccorsdomain value: in general，if users wish to use this option, they can just use ""; to be more specifical, users are recommended to search up "Cross-origin resource sharing" to further understand some key phrases;

    --jspath loadScript: default value is ".", the main directory of loadScript when installing javascript file；

As shown below：
~~~~~~~~~~

初始化启动VNODE节点，默认连接到 MOAC 主网(network ID = 99)：

::

    ./moac      

Through local ipc port connecting to MOAC mainnet nodes and start the command line

::

    ./moac attach

Initiate MOAC nodes and connect to testnet (network ID = 101)
初始化启动VNODE节点，默认连接到 MOAC 测试网(network ID = 99)：

::

    ./moac --testnet                      

Initiate MOAC testnet nodes and interactive command line

::

    ./moac --testnet console    

Through local ipc port connecting to MOAC nodes

::

    ./moac attach /xx/xxx/moac.ipc

Through HTTP-based rpc port connecting to local or remote MOAC nodes

::

    ./moac attach http://xxx.xxx.xxx.xxx:8545       

Initiate MOAC testnet nodes and RPC service

::

    ./moac --testnet --rpc                  

Initiate MOAC testnet nodes which is nonlocally accessible; personal and debug services can also be nonlocally used while providing cross-domain resource sharing service.

::

    ./moac --testnet --rpc --rpcaddr=0.0.0.0 --rpcapi="db,mc,net,chain3,personal,debug" --rpccorsdomain=""                  

显示所有的命令行参数
--------------------

在命令行下，输入：

::

    $ ./moac help

或者

::

    $ ./moac --help

NAME:
^^^^^

::

    moac - the MOAC-core command line interface

    Copyright 2017 The MOAC Authors

USAGE:

::

    moac [options] command [command options] [arguments...]

VERSION:

1.0.9-stable-f98b7fea

COMMANDS:
^^^^^^^^^

::

        account     Manage accounts
        attach      Start an interactive JavaScript environment (connect to node)
        bug         opens a window to report a bug on the moac repo
        console     Start an interactive JavaScript environment
        dump        Dump a specific block from storage
        dumpconfig  Show configuration values
        export      Export blockchain into file
        import      Import a blockchain file
        init        Bootstrap and initialize a new genesis block
        js          Execute the specified JavaScript files
        license     Display license information
        makecache   Generate ethash verification cache (for testing)
        makedag     Generate ethash mining DAG (for testing)
        monitor     Monitor and visualize node metrics
        removedb    Remove blockchain and state databases
        version     Print version numbers
        wallet      Manage MoacNode presale wallets
        help, h     Shows a list of commands or help for one command

MOAC CORE OPTIONS:
^^^^^^^^^^^^^^^^^^

::

       --config value                            TOML configuration file
       --datadir "/Users/zpli/Library/MoacNode"  Data directory for the databases and keystore
       --keystore                                Directory for the keystore (default = inside the datadir)
       --nousb                                   Disables monitoring for and managing USB hardware wallets
       --networkid value                         Network identifier (integer, 99=mainnet, 101=testnet, 100=devnet) (default: 99)
       --testnet                                 MOAC test network: pre-configured proof-of-work test network
       --dev                                     Developer mode: pre-configured private network with several debugging flags
       --syncmode "fast"                         Blockchain sync mode ("fast", "full", or "light")
       --mcstats value                           Reporting URL of a mcstats service (nodename:secret@host:port)
       --identity value                          Custom node name
       --lightserv value                         Maximum percentage of time allowed for serving LES requests (0-90) (default: 0)
       --lightpeers value                        Maximum number of LES client peers (default: 20)
       --lightkdf                                Reduce key-derivation RAM & CPU usage at some expense of KDF strength

ETHASH OPTIONS:
^^^^^^^^^^^^^^^

::

       --ethash.cachedir                      Directory to store the ethash verification caches (default = inside the datadir)
       --ethash.cachesinmem value             Number of recent ethash caches to keep in memory (16MB each) (default: 2)
       --ethash.cachesondisk value            Number of recent ethash caches to keep on disk (16MB each) (default: 3)
       --ethash.dagdir "/Users/zpli/.ethash"  Directory to store the ethash mining DAGs (default = inside home folder)
       --ethash.dagsinmem value               Number of recent ethash mining DAGs to keep in memory (1+GB each) (default: 1)
       --ethash.dagsondisk value              Number of recent ethash mining DAGs to keep on disk (1+GB each) (default: 2)

TRANSACTION POOL OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --txpool.nolocals            Disables price exemptions for locally submitted transactions
       --txpool.journal value       Disk journal for local transaction to survive node restarts (default: "transactions.rlp")
       --txpool.rejournal value     Time interval to regenerate the local transaction journal (default: 1h0m0s)
       --txpool.pricelimit value    Minimum gas price limit to enforce for acceptance into the pool (default: 1)
       --txpool.pricebump value     Price bump percentage to replace an already existing transaction (default: 10)
       --txpool.accountslots value  Minimum number of executable transaction slots guaranteed per account (default: 16)
       --txpool.globalslots value   Maximum number of executable transaction slots for all accounts (default: 40960)
       --txpool.accountqueue value  Maximum number of non-executable transaction slots permitted per account (default: 64)
       --txpool.globalqueue value   Maximum number of non-executable transaction slots for all accounts (default: 1024)
       --txpool.lifetime value      Maximum amount of time non-executable transaction are queued (default: 3h0m0s)

PERFORMANCE TUNING OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --cache value            Megabytes of memory allocated to internal caching (min 16MB / database forced) (default: 128)
       --trie-cache-gens value  Number of trie node generations to keep in memory (default: 120)

ACCOUNT OPTIONS:
^^^^^^^^^^^^^^^^

::

       --unlock value    Comma separated list of accounts to unlock
       --password value  Password file to use for non-inteactive password input

API和交互命令行选项:
^^^^^^^^^^^^^^^^^^^^^^^^

::

       --rpc                  Enable the HTTP-RPC server
       --rpcaddr value        HTTP-RPC server listening interface (default: "localhost")
       --rpcport value        HTTP-RPC server listening port (default: 8545)
       --rpcapi value         API's offered over the HTTP-RPC interface
       --ws                   Enable the WS-RPC server
       --wsaddr value         WS-RPC server listening interface (default: "localhost")
       --wsport value         WS-RPC server listening port (default: 8546)
       --wsapi value          API's offered over the WS-RPC interface
       --wsorigins value      Origins from which to accept websockets requests
       --ipcdisable           Disable the IPC-RPC server
       --ipcpath              Filename for IPC socket/pipe within the datadir (explicit paths escape it)
       --rpccorsdomain value  Comma separated list of domains from which to accept cross origin requests (browser enforced)
       --jspath loadScript    JavaScript root path for loadScript (default: ".")
       --exec value           Execute JavaScript statement
       --preload value        Comma separated list of JavaScript files to preload into the console

NETWORKING OPTIONS:
^^^^^^^^^^^^^^^^^^^

::

       --bootnodes value     Comma separated enode URLs for P2P discovery bootstrap (set v4+v5 instead for light servers)
       --bootnodesv4 value   Comma separated enode URLs for P2P v4 discovery bootstrap (light server, full nodes)
       --bootnodesv5 value   Comma separated enode URLs for P2P v5 discovery bootstrap (light server, light nodes)
       --port value          Network listening port (default: 30333)
       --maxpeers value      Maximum number of network peers (network disabled if set to 0) (default: 25)
       --maxpendpeers value  Maximum number of pending connection attempts (defaults used if set to 0) (default: 0)
       --nat value           NAT port mapping mechanism (any|none|upnp|pmp|extip:<IP>) (default: "any")
       --nodiscover          Disables the peer discovery mechanism (manual peer addition)
       --v5disc              Enables the experimental RLPx V5 (Topic Discovery) mechanism
       --netrestrict value   Restricts network communication to the given IP networks (CIDR masks)
       --nodekey value       P2P node key file
       --nodekeyhex value    P2P node key as hex (for testing)

挖矿选项:
^^^^^^^^^^^^^^

::

     --mine                    开始节点挖矿
     --minerthreads value      指定用来挖矿的CPU（默认值：8）
     --moacbase value          挖矿收益的Public address for block mining rewards (default = first account created) (default: "0")
     --targetgaslimit value    Target gas limit sets the artificial target gas floor for the blocks to mine (default: 9000000)
     --gasprice "18000000000"  Minimal gas price to accept for mining a transactions
     --extradata value         Block extra data set by the miner (default = client version)

GAS PRICE ORACLE OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --gpoblocks value      Number of recent blocks to check for gas prices (default: 10)
       --gpopercentile value  Suggested gas price is the given percentile of a set of recent transaction gas prices (default: 50)

VIRTUAL MACHINE OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^

::

       --vmdebug  Record information useful for VM and contract debugging
      

LOGGING AND DEBUGGING OPTIONS:
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

       --metrics                 Enable metrics collection and reporting
       --fakepow                 Disables proof-of-work verification
       --nocompaction            Disables db compaction after import
       --verbosity value         Logging verbosity: 0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=detail (default: 3)
       --vmodule value           Per-module verbosity: comma-separated list of <pattern>=<level> (e.g. mc/=5,p2p=4)
       --backtrace value         Request a stack trace at a specific logging statement (e.g. "block.go:271")
       --debug                   Prepends log messages with call-site location (file and line number)
       --pprof                   Enable the pprof HTTP server
       --pprofaddr value         pprof HTTP server listening interface (default: "127.0.0.1")
       --pprofport value         pprof HTTP server listening port (default: 6060)
       --memprofilerate value    Turn on memory profiling with the given rate (default: 524288)
       --blockprofilerate value  Turn on block profiling with the given rate (default: 0)
       --cpuprofile value        Write CPU profile to the given file
       --trace value             Write execution trace to the given file

MISC OPTIONS:
^^^^^^^^^^^^^

::

       --fast      Enable fast syncing through state downloads
       --light     使用轻节点模式（）
       --help, -h  show help
      

COPYRIGHT:
^^^^^^^^^^

::

       Copyright 2017-2019 MOAC Authors
