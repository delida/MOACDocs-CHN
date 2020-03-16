
.. include:: include_announcement.rst

=================================
Chain3 JavaScript 软件库 1.0.x 
=================================

chain3.js 1.0.x 是基于以太坊 web3 1.2.1版本设计的JavaScript软件库，可以用于处理和MOAC母链及应用链节点的数据交换。
目前还在测试阶段，用户有问题可以到 `Chain3 JS 代码库 <https://github.com/MOACChain/chain3/issues>`_ 提交问题。
与0.1.x版本不同的地方，在于本版本使用了web3 1.2.1里面的多个软件库，如web3-eth-accounts，在web3.js基础上，

chain3.js 1.0.x 使用了web3.js库的多个函数，支持异步调用中Promise和Events实现。

The following documentation will guide you through :ref:`installing and running chain3.js <adding-chain3>`,
as well as providing a :ref:`API reference documentation <api-ref>` with examples.

Contents:

:ref:`Keyword Index <genindex>`, :ref:`Search Page <search>`

.. toctree::
   :maxdepth: 2
   :caption: User Documentation

   getting-started
   callbacks-promises-events
   glossary


.. toctree::
    :maxdepth: 2
    :caption: API Reference

    chain3
    chain3-mc
    chain3-mc-contract
    chain3-mc-accounts
    chain3-core-method

