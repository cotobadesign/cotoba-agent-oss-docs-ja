SubAgent
=======================================

概要
----------------------------------------

サブエージェント(SubAgent)は、sraix要素を使用して呼び出す外部のサービスです。
sraixで指定する外部サービスの呼び出し方法を説明します。

外部サービスの呼び出しの方法には、次の４種類があります。

* 汎用RESTインタフェース
    属性に、"template"を指定した場合、または、属性で、"botName"、"nlu"、"service"が未指定の場合、子要素のhost、header、query、body等を利用して、REST APIとして外部サービスを呼び出します。
* 対話プラットフォームで、公開されているbot呼び出し
    属性に、"botName"を指定した場合、対話プラットフォームで公開されているbotを呼び出します。
* NLU通信インタフェース
    属性に、"nlu"を指定した場合、意図解釈エンジン（NLUサーバ）を呼び出します。
* カスタム外部サービス実装
    属性に、"service"を指定した場合、カスタム実装を行なった処理を利用して、外部サービスを呼び出します。


* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    ":ref:`未指定 <subagent_rest>`","","No","子要素による汎用REST API呼び出し。"
    ":ref:`template <subagent_rest>`","string","No","汎用REST通信用のテンプレート名。"
    ":ref:`botName <subagent_cotoba_design_pf>`","string","No","公開Bot通信用のエイリアス名。"
    ":ref:`nlu <subagent_nlu>`","string","No","NLU通信用のエイリアス名。"
    ":ref:`service <subagent_custom>`","string","No","カスタム外部サービスのサービス名。"
    "`default <#default>`__","string","No","外部呼び出し失敗時の応答文。"
    "`timeout <#timeout>`__","string","No","通信タイムアウト時間（秒単位）を '1' 以上の整数で指定。未指定時は '10'。"

| SubAgentからのレスポンスボディは、シナリオ(AIML)で利用可能なローカル変数(var)に展開されます。AIMLで処理できるのはテキストのみなので、バイナリデータの戻り値はサポートしていません。
| レスポンスボディの内容がJSONの場合、 :ref:`json<template_json>` タグでJSON内部のパラメータを取得できます。
| ローカル変数(var)に展開された内容は、AIMLのcategoryの範囲のみで有効なため、継続して他のcategoryで利用する場合(sraiを含む)、別途、グローバル変数(name/data)に代入してください。
| また、category内で、複数回、サブエージェントの呼び出しを行った場合、サブエージェントからの戻り値が上書きされる場合があるため、必要なレスポンスの内容は個別に変数に代入してください。

| "default"属性は４種類の外部サービスの呼び出し方法に共通で指定でき、呼び出しに失敗した場合に、設定された内容を戻り値として返します。
| "timeout"属性も４種類の外部サービスの呼び出し方法に共通で指定でき、通信要求に対しての送受信待ち時間を制限します。


共通仕様
----------------------------------------

各外部サービス呼び出しに共通する機能として、通信失敗の検出と要因解析のための機能があります。

通信失敗時の応答文(default)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

通信失敗時には、sraixの戻り値として””（空文字）が設定されますが、"default"属性を指定した場合、指定された文字列をsraixの戻り値として返します。

以下は、カスタム外部サービス実装を利用する場合の例です。

.. code:: xml

   <category>
       <pattern>botステータスチェック *</pattern>
       <template>
           <star />のステータスは、<sraix service="someBot" default="通信失敗"><star /></sraix>です。
        </template>
   </category>

| Input: botステータスチェック 公開bot
| Output: 公開botのステータスは、通信失敗です。

タイムアウト指定(timeout)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 外部サービスとの通信時間を指定された時間で制限します。タイムアウトが発生した場合には、sraixの戻り値として””（空文字）が設定されます。
| "default"属性を指定することで、指定された文字列をsraixの戻り値として返すこともできます。
| 指定は秒単位（省略値：10秒）で行いますが、動作環境によっては指定時間までに異常を検出する場合があります。３０秒程度までの値を指定されること推奨します。

以下は、カスタム外部サービス実装を利用しタイムアウトが発生した場合の例です。

.. code:: xml

   <category>
       <pattern>botステータスチェック *</pattern>
       <template>
           <star />のステータスは、<sraix service="someBot" timeout="10"><star /></sraix>です。
        </template>
   </category>

| Input: botステータスチェック 公開bot
| Output: 公開botのステータスは、です。

HTTPステータスコードの取得
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

通信失敗の要因には、パラメータの指定異常などを含め各種ありますが、通信処理の結果として、
ローカル変数(var)： ``__SUBAGENT_STATUS_CODE__`` に、HTTPステータスコードの値が文字列として設定されます。

- 取得失敗 : 通信処理が行われなかった。
- 000 : 通信要求に問題があり通信が行われなかった、または、通信結果として、ステータスコードが取得できなかった。
- 001 : 通信タイムアウトが発生した。
- 200 : 通信が正常に行われた。
- その他 : 通信結果として、異常を示すステータスコードが通知された。

以下は、REST通信のhostに接続できないURLを指定した場合の例です。

.. code:: xml

   <category>
       <pattern>不正RESTサーバ指定</pattern>
       <template>
           <think>
               <sraix><host>https://otherhost.com:5000</host><body>data</body></sraix>
           </think>
           ステータスコードは、<get var="__SUBAGENT_STATUS_CODE__" />です
        </template>
   </category>

| Input: 不正RESTサーバ指定
| Output: ステータスコードは、000です。

レイテンシの取得
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 外部サービスとの通信時間（レイテンシ）は動作環境やネットワーク状況によって変動します。
| 送受信に要した時間を確認できる様に、ローカル変数(var)： ``__SUBAGENT_LATENCY__`` に、送受信時間を秒単位の小数点付き数値で設定します。
| 通信が行われなかった場合には、変数値の取得に失敗します。

以下は、REST通信のhostに通信できないURLを指定してタイムアウトが発生した場合の例です。

.. code:: xml

   <category>
       <pattern>通信タイムアウト</pattern>
       <template>
           <think>
               <sraix timeout="1"><host>https://anyhost.com</host><body>data</body></sraix>
           </think>
           通信時間は、<get var="__SUBAGENT_LATENCY__" />秒です
        </template>
   </category>

| Input: 通信タイムアウト
| Output: 通信時間は、1.010000秒です。


.. _subagent_rest:

汎用RESTインタフェース
----------------------------------------

sraixの属性に"template"を指定した場合、または、属性で、"botName"、"nlu"、"service"が未指定の場合、
子要素のhost, header, query, body等を利用して、REST APIとして外部サービスを呼び出します。

| 属性に"template"を指定した場合の値には、:ref:`rest_templates<storage_rest_templates>` ファイルで登録したテンプレート名を指定します。
| （登録されていないテンプレート名を指定した場合には、AIML展開時に異常を検出し、該当のcategoryは無効になります。）
| "template"指定の場合にも子要素の指定を行うことで、送信する内容を変更することができます。


送信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "host","string","No","接続先のURLを指定します。属性に’template’を指定しない場合は必須です。"
    "method","string","No","HTTPのメソッド。未指定時はGETで、その他、POST/PUT/DELETE/PATCHに対応します。"
    "query","string","No","queryとして指定するパラメータを連想配列で指定します。"
    "header","string","No","ヘッダに指定するキーと値を連想配列で指定します。"
    "body","string","No","ボディに設定する内容を指定します。"

| sraixの処理実行時に子要素の不正を検出した場合には、処理例外が発生します。この場合、通信処理は行われず、対話としての応答文は空文字になります。
| （例外発生時の応答文は、:ref:`properties<storage_file_properties>` の ``exception-response`` で指定できます。）

汎用RESTインタフェースでのリクエストは、以下のように指定します。
bodyには文字列を設定します。

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <sraix>
                <host>https://www.***.com/ask</host>
                <method>POST</method>
                <query>"userid":"1234567890","q":"question"</query>
                <header>"Authorization":"yyyyyyyyyyyyyyyyy","Content-Type":"application/json;charset=UTF-8"</header>
                <body>{"question": "Ask this question"}</body>
            </sraix>
        </template>
    </category>

送信内容

.. code::

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
            <sraix>
                <host>https://otherhost.com/ask</host>
                <method>POST</method>
                <query>"userid":"1234567890","q":"question"</query>
                <header>"Authorization":"yyyyyyyyyyyyyyyyy","Content-Type":"application/json;charset=UTF-8"</header>
                <body><json var="__USER_METADATA__" /></body>
            </sraix>
        </template>
    </category>


テンプレートを使用した送信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

rest_templatesファイルで以下の定義を行った場合のテンプレートの使用例を示します。

.. code:: yaml

  rest:
    テンプレート:
      host: 'https://otherhost.com/ask'
      method: POST
      query:  '"item":"1234"'
      header: '"Content-Type": "applicaton/json"'
      body: '{"key": "Send Data"}'


| sraixの記述で子要素を指定しない場合、テンプレートの定義内容で送信を行います。
| テンプレートの定義を無効にする場合、sraixの該当子要素に空文字を指定します。
| 以下の例では、queryを無効化しています。

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <sraix template="テンプレート">
                <query></quesy>
            <sraix>
        </template>
    </category>

送信内容

.. code::

    POST /ask HTTP/1.1
    Host: otherhost.com
    Content-Type: application/json

    {
      "key": "Send Data"
    }


| sraixで子要素を指定した場合、テンプレートの定義より優先して設定されますが、query、headerは、指定された要素を結合した状態にして送信します。（同じ要素名の場合は置換します。）
| テンプレートのquery、header定義がある場合に、一部のキーの情報を無効にする場合、sraixの記述で該当項目に対して {対象キー: None} を設定します。
| bodyについては、テンプレート定義とsraixの子要素指定の両方がJSON形式の場合に限り、結合した形式で送信します。但し、置換を行う場合は、第一階層のキーからの指定が必要です。
| テンプレート定義のbodyがJSON形式で、一部のキーを除外して送信したい場合、子要素をJSON形式で指定し、{対象キー: null} を設定します。（第一階層のキーのみが対象となります）。
| 尚、テンプレート定義として hostの指定は必須ですが、sraixの記述で置き換えることができます。
| 以下の例では、hostを置換し、methodとともに、queryの"item"要素を無効化しています。また、header・bodyには、要素を追加しています。

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <sraix template="テンプレート">
                <host>https://otherhost.com/ask</host>
                <method></method>
                <query>"item": None, "userid":"1234567890"</query>
                <header>"Authorization":"yyyyyyyyyyyyyyyyy"</header>
                <body>{"key2": "added data"}</body>
            <sraix>
        </template>
    </category>

送信内容

.. code::

    GET /ask?userid=1234567890 HTTP/1.1
    Host: otherhost.com
    Content-Type: application/json
    Authorization: yyyyyyyyyyyyyyyyy

    {
      "key": "Send Data", "key2": "added data"
    }


受信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 受信結果のボディ内容を、sraixの結果として返します。
| AIMLで処理できるのはテキストのみなので、バイナリのボディはサポートしていません。
| 受信結果は、ローカル変数(var)： ``__SUBAGENT_BODY__`` にも展開します。 getで、<get var="__SUBAGENT_BODY__" />を指定することで、ボディの文字列を取得できます。
| ローカル変数(var)の内容はcategory単位で保持されるため、継続してレスポンスの内容を利用する場合、別途グローバル変数(name/data)に代入してください。
| また、category内で、複数回、汎用RESTインタフェース呼び出した場合、``__SUBAGENT_BODY__`` は上書きされるため、必要なレスポンス内容は他の変数に代入してください。

sraixの結果と、ローカル変数の格納値は同じ形式なので、以下の２つの記述の結果は同じになります。

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <sraix template="テンプレート" />
        </template>
    </category>

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <think>
                <sraix template="テンプレート" />
            </think>
            <get var="__SUBAGENT_BODY__" />
        </template>
    </category>


受信結果のボディ内容がJSONの場合、 :ref:`json<template_json>` タグでJSON内部のパラメータを取得できます。

.. code:: json

    {
        "transportation": {
            "station": {
                "departure": "東京",
                "arrival": "京都"
            },
            "time": {
                "departure": "2018/11/1 11:00",
                "arrival": "2018/11/1 13:30"
            }
        },
        "facility": ["鹿苑寺", "清水寺", "伏見稲荷大社"]
     }

というボディ内容の場合、以下の記述で、JSONタグによりボディの内部情報を取得することができます。

.. code:: xml

    <json var="__SUBAGENT_BODY__.transportation.station.departure" />   <!-- 取得結果： 東京 -->
    <json var="__SUBAGENT_BODY__.facility" function="len" />            <!-- 取得結果： 3 -->
    <json var="__SUBAGENT_BODY__.facility"><index>1</index></json>      <!-- 取得結果： 清水寺 -->


.. _subagent_cotoba_design_pf:

対話プラットフォームで、公開されているbot呼び出し
--------------------------------------------------------------------------------

| sraixの属性に、"botName"を指定した場合、対話プラットフォームで公開されているbot(公開Bot)を呼び出します。
| 属性"botName"の値には、:ref:`bot_names<storage_bot_names>` ファイルで登録したエイリアス名を指定します。
| sraixの内容が公開botへの入力文(発話文)として送信されますが、入力文がない（含む、空文字）場合は通信処理を行いません。
| （登録されていないエイリアス名を指定した場合には、AIML展開時に異常を検出し、該当のcategoryは無効になります。）
| エイリアス名の定義で各種パラメータを指定した場合にも、子要素の指定を行うことで、送信する内容を変更することができます。
| 公開botからの戻ってくる内容は、:ref:`対話API<coversation_api>` の受信データで規定されたJSON形式であり、sraixの戻り値に、その中のresponse要素を返します。


送信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 公開bot利用時のパラメータは子要素として記載します。子要素の内容は、:ref:`対話API<coversation_api>` のボディの内容として送信されます。
| 子要素については、:ref:`対話API<coversation_api>` を参照してください。
| sraixで、ユーザIDが未指定の場合、対話APIで指定されたユーザIDを引き継いで使用します。
| 公開Botを利用する場合、sraixにはユーザ発話を設定する子要素はなく、sraixの内容が公開Botに通知するutteranceとして扱われます。
| sraixの内容が無い（含む、空文字）の場合、公開Botへの通信は行われず、属性：defaultが未設定の場合のsraixの戻り値は空文字になります。

* 子要素

.. csv-table::
    :header: "パラメータ","","タイプ","必須","説明"
    :widths: 10,10,10,5,75

    "userId","","string","No","ユーザID。未指定の場合、対話APIの指定値を引き継ぎます。"
    "locale","","string","No","ロケール。言語指定ISO-639 言語コードとISO-3166 国コードをハイフン繋いだ組み合わせを指定します。"
    "time","","string","No","時間情報。ISO8601(RFC3339)形式で指定します。"
    "topic","","string","No","トピックID。topic情報を変更する場合にのみ指定します。"
    "deleteVariable","","boolean","No","タスク変数削除指定。'true'が指定された場合のみに送信します。"
    "metadata","","string","No","メタデータ。Text形式、または、JSON形式で指定します。"
    "config","","","No","コンフィグ指定。JSON形式で指定します。"
    "","logLevel","string","No","ログレベル。'none','error','warning','info','debug'のいずれかを指定します。"

| userId以外の子要素が未指定の場合、該当の要素は送信されません。
| sraixの処理実行時に子要素の不正を検出した場合には、処理例外が発生します。この場合、通信処理は行われず、対話としての応答文は空文字になります。
| （例外発生時の応答文は、:ref:`properties<storage_file_properties>` の ``exception-response`` で指定できます。）

以下の例は、userId, topic、deleteVariable、metadata、configをシナリオで指定し、locale、timeは、指定しない場合の例です。
尚、:ref:`bot_names<storage_bot_names>` ファイルでは、URLとapikeyのみを指定した場合になります。

.. code:: yaml

  bot:
    someBot:
      url: https://somebot.com/bots/botId_1/ask
      apikey: test_apikey

.. code:: xml

    <category>
        <pattern>botステータスチェック *</pattern>
        <template>
            <think>
                <json var="askSubagent.郵便番号">222-0033</json>
                <json var="config.logLevel">debug</json>
            </think>
            <sraix botName="someBot">
                <star />
                <userId>someUser</userId>
                <topic>test</topic>
                <deleteVariable>true</deleteVariable>
                <metadata><json var="askSubagent"/></metadata>
                <config><json var="config"/></config>
            </sraix>
        </template>
    </category>

| Input: botステータスチェック 郵便番号検索
| Output: 新横浜

送信内容

.. code::

    POST /bots/botId_1/ask HTTP/1.1
    Host: somebot.com
    Content-Type: application/json;charset=UTF-8
    x-api-key: test_apikey

    {
        "userId": "someUser",
        "topic": "test",
        "deleteVariable": true,
        "metadata": {"郵便番号": "222-0033"},
        "config": {"logLevel": "debug"},
        "utterance": "郵便番号検索"
    }


bot_namesでパラメータを設定した場合の送信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

bot_namesファイルで以下の定義を行った場合の送信例を示します。

.. code:: yaml

  bot:
    someBot:
      url: https://somebot.com/bots/botId_1/ask
      locale: ja-JP
      time: 2018-07-01T12:18:45+09:00
      topic: test
      deleteVariable: false
      config: '("loglevel": "info"}'
      metadata: Send Data


| sraixの記述で子要素を指定しない場合、bot_namesの定義内容で送信を行います。
| bot_namesの定義を無効にする場合、sraixの該当子要素に空文字を指定します。
| 以下の例では、time、topicを無効化しています（deleteVariableは'false'のため送信されません）。
| 尚、userIdはbot_namesでは設定できないため、個別に指定します。(省略時には、対話APIの変数（var型）の ``__USER_USERID__`` の値を使用します。)

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <sraix botName="someBot">
                <userId>someUser</userId>
                <time></time>
                <topic></topic>
                こんにちは
            <sraix>
        </template>
    </category>

送信内容

.. code::

    POST /bots/botId_1/ask HTTP/1.1
    Host: somebot.com
    Content-Type: application/json;charset=UTF-8
    x-api-key: 

    {
        "userId": "someUser",
        "locale": "ja-JP",
        "config": {"logLevel": "info"},
        "utterance": "こんにちは",
        "metadata": "Send Data"
    }


| sraixで子要素を指定した場合、bot_namesの定義より優先して設定されます。
| metadataについては、bot_names定義とsraixの子要素指定の両方がJSON形式の場合に限り、結合した形式で送信します。但し、置換を行う場合は、第一階層のキーからの指定が必要です。
| bot_names定義のmetadataがJSON形式で、一部のキーを除外して送信したい場合、子要素をJSON形式で指定し、該当キーの値として 直値:'null' を設定します。（第一階層のキーのみが対象となります）。
| 以下の例では、topicとdeleteVariableを置換し、metadataを編集した値を送信しています。
| 尚、metadataに関するJSON結合処理例を示すため、前述のbot_names定義のmetadataを、次の様にJSON形式の値に変更しているものとします。

.. code:: yaml

        metadata: '{"key1":"value1", "key2": "value2", "key3": "value3"}'

.. code:: xml

    <category>
        <pattern>XXX</pattern>
        <template>
            <sraix botName="someBot">
                <userId>someUser</userId>
                <topic>test</topic>
                <deleteVariable>true</deleteVariable>
                <metadata>{"key1": null, "key2": {"modify": "data"}, "key4": "added"}</metadata>
                こんにちは
            </sraix>
        </template>
    </category>

送信内容

.. code::

    POST /bots/botId_1/ask HTTP/1.1
    Host: somebot.com
    Content-Type: application/json;charset=UTF-8
    x-api-key: 

    {
        "userId": "someUser",
        "locale": "ja-JP",
        "time": "2018-07-01T12:18:45+09:00",
        "topic": "test",
        "deleteVariable": true,
        "config": {"logLevel": "info"},
        "utterance": "こんにちは",
        "metadata": {"key2":{"modify": "data"}, "key3": "value3", "key4": "added"}
    }

受信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 公開Botからの受信したボディ(JSON形式)の中の response要素が、sraixの戻り値として設定されます。
| 受信したボディがJSON形式でない場合や、JSON内に response要素が無い場合には公開Botとの通信として異常とし、属性：defaultが未設定の場合のsraixの戻り値は空文字になります。

以下のシナリオで、

.. code:: xml

   <category>
        <pattern>*</pattern>
        <template>
           <sraix botName="someBot"><star /></sraix>
        </template>
   </category>

公開Bot：someBotからの受信データが

.. code::

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "response": "こんにちは、今日もいい天気ですね。",
        "topic": "greeting"
    }

だった場合、結果は、

| Input: こんにちは
| Output: こんにちは、今日もいい天気ですね。

になります。


| 公開Botからの受信内容は、ローカル変数(var) ``__SUBAGENT_EXTBOT__.エイリアス名`` にJSON形式で展開され、jsonタグで取得することができます。
| 尚、該当変数はcategory単位に保持されるため、継続利用の場合は、グローバル変数(name/data)への代入が必要です。
| また、category内で、複数回、公開Botを呼び出した場合、``__SUBAGENT_EXTBOT__`` は上書きされるため、必要なレスポンス内容は他の変数に代入してください。

.. code:: xml

   <category>
        <pattern>*</pattern>
        <template>
           <think>
               <sraix botName="someBot"><star /></sraix>
           </think>
           <json var="__SUBAGENT_EXTBOT__.someBot" />
        </template>
   </category>

| Input: こんにちは
| Output: {"response": "こんにちは、今日もいい天気ですね。", "topic": "greeting"}。

getタグで取得する場合には、変数名のみを指定する必要があり、第一階層のキーとしてエイリアス名が設定されたJSON全体を取得することになります。
前述のjsonタグをgetタグに変更すると、以下の様になります。

.. code:: xml

    <get var="__SUBAGENT_EXTBOT__" />

| Input: こんにちは
| Output: {"someBot": {"response": "こんにちは、今日もいい天気ですね。", "topic": "greeting"}}。


公開Botからの受信ボディの内容はJSONのため、 :ref:`json<template_json>` タグでJSON内部のパラメータを取得できます。
また、``metadata`` の内容がJSONである場合、JSONタグで ``metadata`` 内のパラメータも取得できます。

公開Botからのボディの内容が、

.. code:: json

    {
        "utterance": "こんにちは",
        "response": "こんにちは、今日もいい天気ですね。",
        "topic": "greeting"
        "metadata":{"broadcaster":"OBS","title":"午後のニュース"}
    }

の場合、

.. code:: xml

    <json var="__SUBAGENT_EXTBOT__.someBot.response" />              <!-- 取得結果： こんにちは、今日もいい天気ですね。 -->
    <json var="__SUBAGENT_EXTBOT__.someBot.utterance" />             <!-- 取得結果： こんにちは -->
    <json var="__SUBAGENT_EXTBOT__.someBot.topic" />                 <!-- 取得結果： greeting -->
    <json var="__SUBAGENT_EXTBOT__.someBot.metadata" />              <!-- 取得結果： {"broadcaster":"OBS","title":"午後のニュース"} -->
    <json var="__SUBAGENT_EXTBOT__.someBot.metadata.broadcaster" />  <!-- 取得結果： OBS -->
    <json var="__SUBAGENT_EXTBOT__.someBot.metadata.title" />        <!-- 取得結果： 午後のニュース -->

として、公開botからの戻り値、及び、metadataの情報を取得することができます。

| （デバッグ情報）
| 　公開Botからの受信データ形式が不正な場合には通信異常として扱いますが、受信データは、``__SUBAGENT_EXTBOT__.エイリアス名`` に格納します。


.. _subagent_nlu:

NLU通信インタフェース
----------------------------------------

| sraixの属性に、"nlu"を指定した場合、意図解釈エンジン（NLUサーバ）を呼び出します。
| 属性"nlu"の値には、:ref:`nlu_servers<storage_nlu_servers>` ファイルで登録したエイリアス名を指定し、sraixの内容をNLUサーバへの入力文(発話文)として送信します。
| （登録されていないエイリアス名を指定した場合には、AIML展開時に異常を検出し、該当のcategoryは無効になります。）
| NLUサーバからの戻ってくる内容は、:ref:`意図解釈エンジン仕様<nlu_json_example>` で規定されたJSON形式であり、sraixの戻り値に、JSON形式のデータを返します。

以降の説明では、NLUサーバから以下のJSON形式のデータが返却された例として説明します。

.. code:: json

    {
        "intents": [
            {"intent": "transportation", "score": 0.9}
        ], 
        "slots": [
            {"slot": "departure", "entity": "東京", "score": 0.85}
        ]
    }

また、nlu_serversの定義は以下のもの使用するものとします。

.. code:: yaml

  servers:
    someNlu:
      url: https://***.com/run
      apikey: test_key


送信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| NLU通信を利用する場合、sraixにはユーザ発話を設定する子要素はなく、sraixの内容がNLUに通知する発話文になります。
| sraixの内容が無い（含む、空文字）の場合、NLUへの通信は行われず、属性：defaultが未設定の場合のsraixの戻り値は空文字になります。
| 以下は、NLU呼び出し時の動作例です。

.. code:: xml

   <category>
       <pattern>nlu通信 *</pattern>
       <template>
           <sraix nlu="someNlu"><star /></sraix>
        </template>
   </category>

送信内容

.. code::

    POST /run HTTP/1.1
    Host: someNlu.com
    Content-Type: application/json;charset=UTF-8
    x-api-key: test_key

    {
        "utterance": "お出かけは"
    }

| Input: nlu通信 お出かけは
| Output: {"intents": [{"intent": "transportation", "score": 0.9 }], "slots": [{"slot": "departure", "entity": "東京", "score": 0.85}]}。

受信
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 送信時の例で示した通り、NLUサーバから受信したボディ(JSON形式)が、そのまま、sraixの戻り値として設定されます。
| 受信したボディがJSON形式でない場合はNLUとの通信として異常とし、属性：defaultが未設定の場合のsraixの戻り値は空文字になります。

| NLUからの受信内容は、ローカル変数(var) ``__SUBAGENT_NLU__.エイリアス名`` にJSON形式で展開され、jsonタグで取得することができます。
| 尚、該当変数はcategory単位に保持されるため、継続利用の場合は、グローバル変数(name/data)への代入が必要です。
| また、category内で、複数回、公開Botを呼び出した場合、``__SUBAGENT_NLU__`` は上書きされるため、必要なレスポンス内容は他の変数に代入してください。


.. code:: xml

   <category>
        <pattern>*</pattern>
        <template>
           <think>
               <sraix nlu="someNlu"><star /></sraix>
           </think>
           <json var="__SUBAGENT_NLU__.someNlu" />
        </template>
   </category>

| Input: お出かけは
| Output: {"intents": [{"intent": "transportation", "score": 0.9 }], "slots": [{"slot": "departure", "entity": "東京", "score": 0.85}]}。


getタグで取得する場合には、変数名のみを指定する必要があり、第一階層のキーとしてエイリアス名が設定されたJSON全体を取得することになります。
前述のjsonタグをgetタグに変更すると、以下の様になります。

.. code:: xml

    <get var="__SUBAGENT_NLU__" />

| Input: お出かけは
| Output: {"someNlu": {"intents": [{"intent": "transportation", "score": 0.9 }], "slots": [{"slot": "departure", "entity": "東京", "score": 0.85}]}}。

NLUからの受信ボディの内容はJSONのため、 :ref:`json<template_json>` タグでJSON内部のパラメータを取得できます。

.. code:: xml

    <json var="__SUBAGENT_NLU__.someNlu.intents" />             <!-- 取得結果： [{"intent": "transportation", "score": 0.9}] -->
    <json var="__SUBAGENT_NLU__.someNlu.intents" index="0" />   <!-- 取得結果： {"intent": "transportation", "score": 0.9} -->
    <json var="__SUBAGENT_NLU__.someNlu.slots" />               <!-- 取得結果： [{"slot": "departure", "entity": "東京", "score": 0.85}] -->
    <json var="__SUBAGENT_NLU__.someNlu.slots" index="0" />     <!-- 取得結果： {"slot": "departure", "entity": "東京", "score": 0.85} -->

| （デバッグ情報）
| 　NLUからの受信データ形式が不正な場合には通信異常として扱いますが、受信データは、``__SUBAGENT_NLU__.エイリアス名`` に格納します。

nluintent / nluslotでのデータ取得
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

NLUサーバからの受信したデータを、template要素の :ref:`nluintent<template_nluintent>` 、:ref:`nluslot<template_nluslot>` を利用して、
マッチ処理で取得したNLUデータ： ``__SYSTEM_NLUDATA__`` に対する処理と同様の操作を行うことができます。

以下に、NLUサーバから受信したデータに対して、nluintentで内容を取得する例を示します。

.. code:: xml

    <nluintent name="*" item="count" target="__SUBAGENT_NLU__.someNlu" />                 <!-- 取得結果： 1 -->
    <nluintent name="*" item="intent" index="0" target="__SUBAGENT_NLU__.someNlu" />      <!-- 取得結果： transportation -->
    <nluintent name="transportation" item="score" target="__SUBAGENT_NLU__.someNlu" />    <!-- 取得結果： 0.9 -->

同様に、nluslotで内容を取得する例は以下の様になります。

.. code:: xml

    <nluslot name="*" item="count" target="__SUBAGENT_NLU__.someNlu" />                 <!-- 取得結果： 1 -->
    <nluslot name="*" item="slot" index="0" target="__SUBAGENT_NLU__.someNlu" />        <!-- 取得結果： departure -->
    <nluslot name="departure" item="entity" target="__SUBAGENT_NLU__.someNlu" />        <!-- 取得結果： 東京 -->

尚、 :ref:`nluintent<template_nluintent>` 、:ref:`nluslot<template_nluslot>` で、JSON型の変数名（区切り文字：'.' を利用）を指定できるのは、
ローカル変数(var) の ``__SUBAGENT_NLU__.xxx`` の形式の場合のみです。


.. _subagent_custom:

カスタム外部サービス実装
----------------------------------------

属性に、"service"を指定した場合、カスタム実装を行なった処理を利用して、外部サービスを呼び出すことができます。
カスタム外部サービスは、利用するサービス(SubAgent)毎に実装が必要な呼び出し方法があるため、以下の基底クラスを継承して個別に実装します。

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
           

次に、Brainコンフィグレーションの :ref:`services<config_services>` に、次のようにカスタム外部サービスのエントリを追加することで、sraixのサービス名として利用できるようになります。

.. code:: yaml

           myService:
               classname: programy.services.myService.StatusCheck
               url: https://myService.com/api/statuscheck


AIMLで利用する場合には、以下の例のように、sraixの属性"service"に、カスタム外部サービスのエントリ名を指定します。
sraixのカスタム外部サービスの処理としては、カスタム外部サービスのエントリのclassnameで定義されたクラスをロードし、関数：ask_question()が呼び出します。
関数：ask_question()の戻り値が、sraixの結果になります。

.. code:: xml

   <category>
       <pattern>ステータスチェック *</pattern>
       <template>
           <star />のステータスは、<sraix service="myService"><star /></sraix>です。
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

| 以下の例はmyServiceという外部サービスを利用する例で、引数4つを空白区切りで設定する前提で記載されています。
| 尚、日本語の場合、sraix要素の内容が全て結合されて空白区切りにならない場合があるため、引数間に :ref:`space要素<template_space>` を指定しています。

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <set var="text">
                    <sraix service="myService">
                        <star />
                        <space />
                        <json var="__USER_METADATA__.arg1" />
                        <space />
                        <json var="__USER_METADATA__.arg2" />
                        <space />
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

| カスタム外部サービスの戻り値、つまり、個別実装のask_question()関数の戻り値は、ローカル変数(var) ``__SUBAGENT__.サービス名`` に展開されます。
| 該当変数はcategory単位に保持されるため、継続利用の場合は、グローバル変数(name/data)への代入が必要です。
| また、category内で、複数回、カスタム外部サービスを呼び出した場合、``__SUBAGENT__`` は上書きされるため、必要なレスポンス内容は他の変数に代入してください。

変数に格納される形式は、テキスト、または、JSON形式となり、カスタム実装でもバイナリは使用できません。

以下の例では、myServiceに対する処理の戻り値が ``__SUBAGENT__.myService`` に展開されていますが、その内容がJSON形式で、

.. code:: json

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

| カスタム外部サービスの戻り値がテキストの場合、``__SUBAGENT__.myService`` を変数名として、getタグで取得することになります。
| ※sraixの結果データの処理方式を統一する観点から、カスタム実装において、JSON形式で出力することを推奨します。

.. code:: xml

    <get var="__SUBAGENT__.myService />


関連項目: :doc:`metadata <Metadata>`, :doc:`対話API <../Api>`, :doc:`JSON <JSON>`
