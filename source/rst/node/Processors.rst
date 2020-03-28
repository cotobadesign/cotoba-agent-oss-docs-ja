pre/post processor
============================

| 対話エンジン内にはpre-processorとpost-processorの2種のプロセッサがあります。
| 発話文に対し、対話エンジン内での対話処理前に加工する必要がある場合、対話エンジンからの応答文に対し返却前に加工が必要である場合に用います。

-  `Pre Processors <#pre-processors>`__ は、対話エンジン内部で対話処理を行う前に、文字列の前処理を行うプロセッサです。
-  `Post Processors <#post-processors>`__ は、対話エンジン内部で対話処理を行った応答文に対し、APIで返却を行う前に文字列の後処理を行うプロセッサです。


Pre Processors
-----------------------------

Pre Processorsは、下記の抽象基本クラスから継承します。

.. code:: python

       programy.processors.processing.PreProcessor


このクラスには単一のメソッドprocessがあり、入力されたstringに対し前処理を行い、処理された文字列を返却します。
返却された文字列を対話エンジン内で利用し対話を進行します。

.. code:: python

       class PreProcessor(Processor):

           def __init__(self):
               Processor.__init__(self)

           @abstractmethod
           def process(self, bot, clientid, string):
               pass

Post Processors
-----------------------------

Post Processorsは、下記の抽象基本クラスから継承します。

.. code:: python

       programy.processors.processing.PostProcessor

このクラスには単一のメソッドprocessがあり、入力されたstringに対し前処理を行い、処理された文字列を返却します。
返却された文字列をエンドユーザに対する応答文として使用します。

.. code:: python

       class PostProcessor(Processor):
           def __init__(self):
               Processor.__init__(self)

           @abstractmethod
           def process(self, bot, clientid, string):
               pass
