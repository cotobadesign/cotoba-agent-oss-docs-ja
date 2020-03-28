SubAgent
=======================================

概要
----------------------------------------

サブエージェント(SubAgent)は、sraix要素を使用して呼び出す外部のサービスです。
sraixで指定する外部サービスの呼び出し方法を説明します。

外部サービスの呼び出しの方法には、次の3種類があります。

* 汎用RESTインタフェース
    属性で、"botId"／"service"が未指定の場合、子要素のhost,header,query,body等を利用して、REST-APIとして外部サービスを呼び出します。
* 対話プラットフォームで、公開されているbot呼び出し
    属性に、"botId"を指定した場合、対話プラットフォームで公開されているbotを呼び出します。
* カスタム外部サービス実装
    属性に、"service"を指定した場合、カスタム実装を行なった処理を利用して、外部サービスを呼び出します。


* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "`未指定 <#rest>`__","","No","子要素による汎用REST API呼び出し。"
    ":ref:`service<subagent_custom>`","文字列","No","カスタム外部サービスのサービス名。"
    "`botId <#cotoba-designbot>`__","文字列","No","対話プラットフォームで公開されているbot ID。"
    "`host <#cotoba-designbot>`__","文字列","No","対話プラットフォーム、もしくは、ホスト名。"
    "`default <#default>`__","文字列","No","外部呼び出し失敗時の応答文。"

| SubAgentからのレスポンスのボディは、シナリオ(AIML)で利用可能なローカル変数(var)に展開されます。AIMLで処理できるのはテキストのみなので、バイナリデータの戻り値はサポートしていません。
| レスポンスボディの内容がJSONの場合、 :ref:`json<template_json>` タグでJSON内部のパラメータを取得できます。
| ローカル変数(var)に展開された内容は、AIMLのcategoryの範囲のみで有効なため、継続して他のcategoryで利用する場合(sraiを含む)、別途、グローバル変数(name/data)に代入してください。
| また、category内で、複数回、サブエージェントの呼び出しを行った場合、サブエージェントからの戻り値が上書きされる場合があるため、必要なレスポンスの内容は個別に変数に代入してください。

| "default"属性は外部サービスの呼び出し方法の3種ともに指定でき、呼び出しが失敗した場合に、設定された内容を戻り値として返します。
| "host"属性は、対話プラットフォームのみで利用するものであり、子要素の"host"とは用途が異なります。


汎用RESTインタフェース
----------------------------------------

sraixの属性"botId"、"service"が未指定の場合、子要素のhost, header, query, body等を利用して、REST-APIとして外部サービスを呼び出します。


送信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 子要素

.. csv-table::
    :header: "タグ","必須","説明"
    :widths: 10,5,75

    "host","Yes","接続先のURL。"
    "method","No","HTTPのメソッド。未指定時はGETで、その他、POST/PUT/DELETEに対応します。"
    "query","No","queryとして指定するパラメータを連想配列で指定します。"
    "header","No","ヘッダに指定するキーと値を連想配列で指定します。"
    "body","No","ボディに設定する内容を指定します。"


リクエスト時は下記のように指定します。
bodyには文字列を設定します。

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <think>
                <sraix>
                    <host>http://www.***.com/ask</host>
                    <method>POST</method>
                    <query>"userid":"1234567890","q":"question"</query>
                    <header>"Authorization":"yyyyyyyyyyyyyyyyy","Content-Type":"application/json;charset=UTF-8"</header>
                    <body>{"question": "Ask this question"}</body>
                </sraix>
            </think>
        </template>
    </category>

送信内容

..  code:: json

    POST /ask?userid=1234567890&q=question HTTP/1.1
    Host: www.***.com
    Content-Type: application/json;charset=UTF-8
    Authorization: yyyyyyyyyyyyyyyyy

    {
        "question": "Ask this question"
    }

対話APIで指定された ``metadata`` をボディに指定する場合は、jsonタグで ``__USER_METADATA__`` を取得し、子要素"body"に設定します。

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <think>
                <sraix>
                    <host>http://somehost.com</host>
                    <method>POST</method>
                    <query>"userid":"1234567890","q":"question"</query>
                    <header>"Authorization":"yyyyyyyyyyyyyyyyy","Content-Type":"application/json;charset=UTF-8"</header>
                    <body><json var="__USER_METADATA__" /></body>
                </sraix>
            </think>
        </template>
    </category>


受信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 受信結果のボディ内容を、sraixの結果として返します。
| AIMLで処理できるのはテキストのみなので、バイナリのボディはサポートしていません。
| 受信結果は、ローカル変数(var)： ``__SUBAGENT_BODY__`` にも展開します。 getで、<get var="__SUBAGENT_BODY__" >を指定することで、ボディの文字列を取得できます。
| ローカル変数(var)の内容はcategory単位で保持されるため、継続してレスポンスの内容を利用する場合、別途グローバル変数(name/data)に代入してください。
| また、category内で、複数回、汎用RESTインタフェース呼び出した場合、``__SUBAGENT_BODY__`` は上書きされるため、必要なレスポンス内容は変数に代入してください。

ボディの内容がJSONの場合、 :ref:`json<template_json>` タグでJSON内部のパラメータを取得できます。
ボディの内容が、

..  code:: json

    {
        "transportation": {
            "station": {
                "departure": "東京",
                "arrival": "京都"
            },
            "time": {
                "departure": "2018/11/1 11:00",
                "arrival": "2018/11/1 13:30"
            },
            "facility": ["鹿苑寺", "清水寺", "伏見稲荷大社"]
        }
    }

の場合、

.. code:: xml

    <json var="__SUBAGENT_BODY__.transportation.station.departure" />
    <json var="__SUBAGENT_BODY__.facility" function="len" />
    <json var="__SUBAGENT_BODY__.facility"><index>1</index></json>

という記述で、JSONタグによりボディの内部情報を取得することができます。


通信失敗時の応答文(default)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

通信失敗時には、"default"属性で指定した、文字列をsraixの戻り値として返します。
:ref:`カスタム外部サービス実装<subagent_custom>` へのアクセスでも同様です。
以下は、カスタム外部サービス実装を利用する場合の例です。

.. code:: xml

   <category>
       <pattern>botステータスチェック *</pattern>
       <template>
           <star />のステータスは、<sraix service="sameBot" default="通信失敗"><star/></sraix>です。
        </template>
   </category>

| Input: botステータスチェック 公開bot
| Output: 公開botのステータスは、通信失敗です。



.. _subagent_cotoba_design_pf:

対話プラットフォームで、公開されているbot呼び出し
--------------------------------------------------------------------------------

sraixの属性に、"botId"を指定した場合、対話プラットフォームで公開されているbot(公開Bot)を呼び出します。
"botId"には、対話プラットフォームで規定されているbotのIDを指定し、sraixの内容を公開botへの入力文(発話文)として送信します。
公開botからの戻ってくる内容は、:ref:`対話API<coversation_api>` の受信データで規定されたJSON形式であり、sraixの戻り値では、その中のresponse要素を返します。


送信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

下記の例は、"OK"をレスポンスとして返す公開Botの場合の動作例です。

.. code:: xml

   <category>
       <pattern>botステータスチェック *</pattern>
       <template>
           <star />のステータスは、<sraix botId="sameBot"><star/></sraix>です。
        </template>
   </category>

| Input: botステータスチェック 公開bot
| Output: 公開botのステータスはOKです。


| 公開bot利用時のパラメータは子要素として記載します。子要素の内容は、:ref:`対話API<coversation_api>` のボディの内容として送信されます。
| 子要素の意味は、:ref:`対話API<coversation_api>` を参照してください。
| 未指定の場合に、対話APIで指定された内容を引き継ぐ要素もあります。該当要素が引き継ぎ不要の場合、子要素の設定(空文字列等)が必要です。
| (sraixで、ユーザIDが未指定の場合、対話APIで指定されたユーザIDを元に生成した別のIDを使用します。）
| 公開Botを利用する場合、sraixにはユーザ発話を設定する子要素はなく、sraixの内容が公開Botに通知するutteranceとして扱われます。

* 子要素

.. csv-table::
    :header: "項目","タグ名","型","必須","対話APIからの引き継ぎ"
    :widths: 30,30,20,20,60

    "ロケール","locale","string","No","Yes"
    "時間情報","time","string","No","Yes"
    "ユーザID","userId","string","Yes","No (別のユーザIDを生成)"
    "トピックID","topic","string","No","No"
    "タスク変数削除","deleteVariable","boolean","No","No"
    "メタデータ","metadata","string","No","Yes"
    "コンフィグ","config","","No","No"
    "","logLevel","string","No","No"


下記の例は、topic、deleteVariable、metadata、configをシナリオで指定し、locale、timeは、呼び出し元のリクエストの内容を引き継ぐ場合の設定例です。

.. code:: xml

    <category>
        <pattern>botステータスチェック *</pattern>
        <template>
            <think>
                <json var="askSubagent.郵便番号">222-0033</json>
                <json var="config.logLevel">debug</json>
            </think>
            <sraix botId="sameBot">
                <star/>
                <topic>*</topic>
                <deleteVariable>true</deleteVariable>
                <metadata><json var="askSubagent"/></metadata>
                <config><json var="config"/></config>
            </sraix>
        </template>
    </category>

| Input: botステータスチェック 郵便番号検索
| Output: 新横浜


受信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

公開Botからの受信したボディ(JSON形式)の中の”response"要素が、sraixの戻り値として設定されます。
下記例の、sameBotからの受信データが

..  code:: json

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "response": "こんにちは、今日もいい天気ですね",
        "topic": "greeting"
    }

だった場合、下記のAIMLの結果は、

.. code:: xml

   <category>
        <pattern>*</pattern>
        <template>
           <sraix botId="sameBot"><star/></sraix>
        </template>
   </category>

| Input: こんにちは
| Output: こんにちは、今日もいい天気ですね

になります。


公開Botからの受信内容は、ローカル変数(var) ``__SUBAGENT_EXTBOT__.ボットID`` に展開され、getで取得することができます。
尚、該当変数はcategory単位に保持されるため、継続利用の場合は、グローバル変数(name/data)への代入が必要です。

.. code:: xml

    <json var="__SUBAGENT_EXTBOT__.sameBot" />

公開Botからのボディの内容はJSONのため、 :ref:`json<template_json>` タグでJSON内部のパラメータを取得できます。
また、``metadata`` の内容がJSONである場合、JSONタグで ``metadata`` 内のパラメータも取得できます。

metadataの内容が、

.. code:: json

        "metadata":{"broadcaster":"OBS","title":"午後のニュース"}

の場合、

.. code:: xml

    <json var="__SUBAGENT_EXTBOT__.sameBot.response" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.utterance" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.topic" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.metadata" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.metadata.broadcaster" />
    <json var="__SUBAGENT_EXTBOT__.sameBot.metadata.title" />

として、公開botからの戻り値、及び、metadataの情報を取得することができます。


.. _subagent_custom:

カスタム外部サービス実装
----------------------------------------

属性に、"service"を指定した場合、カスタム実装を行なった処理を利用して、外部サービスを呼び出すことができます。
カスタム外部サービスは、利用するサービス(SubAgent)毎に実装が必要な呼び出し方法があるため、下記の基底クラスを継承して個別に実装します。

.. code:: python

    programy.services.service.Service

処理クラスの実装では、基底クラスを継承したクラスを作成し、ask_question()関数として、発話データに相当する"question"引数を利用して、結果の文字列を返す処理を実装します。
外部サービスとの連携を行う場合、ask_question()内に、REST通信機能を実装することになります。

.. code:: python

    from programy.services.service import Service

    class StatusCheck(Service):
       __metaclass__ = ABCMeta

       def __init__(self, config: BrainServiceConfiguration):
           self._config = config

       @property
       def configuration(self):
           return self._config

       def load_additional_config(self, service_config):
           pass

       @abstractmethod
       def def ask_question(self, client_context, question: str):
           return "OK"
           

次に、構成定義：config.yamlの``services``セクションに、次のようにカスタム外部サービスのエントリを追加することで、sraixのサービス名として利用できるようになります。

.. code:: yaml

           myService:
               classname: programy.services.myService.StatusCheck
               url: http://myService.com/api/statuscheck


AIMLで利用する場合には、以下の例のように、sraixの属性"service"に、カスタム外部サービスのエントリ名を指定します。
sraixのカスタム外部サービスの処理としては、カスタム外部サービスのエントリのclassnameで定義されたクラスをロードし、関数：ask_question()が呼び出します。
関数：ask_question()の戻り値が、sraixの結果になります。

.. code:: xml

   <category>
       <pattern>ステータスチェック *</pattern>
       <template>
           <star />のステータスは、<sraix service="myService"><star/></sraix>です。
       </template>
   </category>

| Input: ステータスチェック カスタム
| Output: カスタムのステータスはOKです。

カスタム外部サービスへの引数および戻り値
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

引数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

sraix service="myService"がカスタム外部サービス呼び出しで、sraix要素内を引数として扱います。
引数の定義は個々のカスタム外部サービスの引数I/Fに依存しており、個々のサービスに合わせ実装を行う必要があります。 
下記の例ではmyServiceという外部サービスを利用する例で、引数を4つ設定する前提で記載されています。

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <set var="text">
                    <sraix service="myService">
                        <star/>
                        <json var="__USER_METADATA__.arg1" />
                        <json var="__USER_METADATA__.arg2" />
                        <json var="__USER_METADATA__.arg3" />
                    </sraix>
                </set>
                <think>
                    <set name="departure"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name="arrival"><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                <get name="departure">から<get name="arrival">までを検索します。
            </template>
        </category>
    </aiml>


戻り値
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

カスタム外部サービスの戻り値、つまり、個別実装のask_question()関数の戻り値は、ローカル変数(var) ``__SUBAGENT__.サービス名`` に展開されます。
該当変数はcategory単位に保持されるため、継続利用の場合は、グローバル変数(name/data)への代入が必要です。

変数に格納される形式は、テキスト、または、JSON形式となり、カスタム実装でもバイナリは使用できません。

以下の例では、myServiceに対する処理の戻り値が ``__SUBAGENT__.myService`` に展開されていますが、その内容がJSON形式で、

..  code:: json

    {
        "transportation": {
            "station": {
                "departure" :"東京",
                "arrival" : "京都"
            },
            "time": {
                "departure": "2018/11/1 11:00",
                "arrival": "2018/11/1 13:30"
            },
            "facility": ["鹿苑寺", "清水寺", "伏見稲荷大社"]
        }
    }

の場合、

.. code:: xml

    <json var="__SUBAGENT__.myService.transportation.station.departure" />
    <json var="__SUBAGENT__.myService.transportation.station.arrival" />

として、jsonタグを利用して、ボディの内部情報を取得することができます。
``__SUBAGENT__.myService`` の内容がテキストの場合は、getタグで取得することになります。


関連項目: :doc:`Metadata <Metadata>`, :doc:`対話API <../Api>`, :doc:`JSON <JSON>`
