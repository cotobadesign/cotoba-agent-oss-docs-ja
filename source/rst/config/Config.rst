コンフィグレーション
=====================================

.. _configuration_file:

コンフィグレーションファイル
-----------------------------

| コンフィグレーションファイルに記載した内容は各種パラメータとして利用します。
| 対話エンジン起動時の、--configオプションでコンフィグファイルを指定します。
| コンフィグファイルで指定できるフォーマットは、*.yaml、*.json、*.xmlのいずれかです。
| -–cformatオプションで形式を指定するか、-–configオプションで指定するコンフィグファイルの拡張子からフォーマットが決まります。

設定例

::

   python3 ../../src/programy/clients/console.py --config ./config.yaml --cformat yaml --logging ./logging.yaml 


.. _configuration_command_line_subsitutions:

コマンドラインオプション
-----------------------------

| 起動の制御に使用されるコマンドラインオプションあります。
| このコマンドラインオプションは全てのclientで同じオプション指定を行うことができます。

-  -–config [コンフィグファイルパス] client実行時に使用するコンフィグファイルのパスを指定します。
-  -–cformat [yaml|json|xml] コンフィグファイルのフォーマットを指定します。指定はオプションで未指定の場合、コンフィグファイルの拡張子から自動判別します。
-  -–logging [ロギングコンフィグレーション] ロギング用コンフィグファイルパスを指定します。
-  -–noloop 対話処理ループを実行しないオプションです。コンフィグファイルの読み込み、AIMLの読み込みを行い、対話エンジンを終了します。対話エンジンの起動確認ように利用します。
-  -—subs 代替え引数設定ファイル指定。コンフィグファイル内に記載された代数をsubsで指定された内容で置換します。詳細は、 :ref:`コマンドラインオプション置換<config_subsitutions>` を参照してください。

| コンフィグの説明はyamlフォーマットを例に説明します。
| jsonおよびxmlでも記載方法が異なるだけで名称は同じとなるため、jsonおよびxmlを用いる場合は読み替えてください。


コンフィグセクション
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

コンフィグファイルは、複数のセクションに基づいて構成されます。
これらのセクションは、client、bot、brainがどの構成に基づいて構成されるかを記載しています。

基本となる構成はclientです。
これらは、REST、コンソール等様々なインタフェースの実装を提供します。
clientでは、主に下記の内容を設定します。

-  Client

   -  Bots
   -  Storage

botの主な構成は下記のとおりです。

-  :doc:`Bot <Config_Bot>`

   -  Brains
   -  Conversations
   -  Sentence Splitting/Joining
   -  Spelling
   -  Translations
   -  Sentiment Analysis

brainの主な構成は下記のとおりです。

-  :doc:`Brain <Config_Brain>`

   -  Overrides
   -  Default Responses
   -  Initial Questions
   -  Binaries
   -  Braintree
   -  Services
   -  Security
   -  OOB
   -  Dynamic Maps and Sets
   -  Tokenizers
   -  Debug Files

ログの構成は下記のファイルで指定します。

-  :ref:`Logging <config_logging>`

