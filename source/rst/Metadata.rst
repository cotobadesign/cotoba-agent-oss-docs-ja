metadata
=======================================

概要
----------------------------------------
| 対話APIリクエストボディに設定された metadata情報は、シナリオの変数データ(テキスト、または、JSON)として用いることができます。
| また、対話APIレスポンスボディに設定する metadata情報も、シナリオの変数データとして設定することができます。

対話APIで設定されたmetadataの利用
----------------------------------------

| 対話APIリクエストでJSONの要素として指定された metadata情報を、対話シナリオで利用可能な変数に展開した状態で保持します。
| APIから渡されたJSON内の ``metadata`` 要素は、変数名 ``__USER_METADATA__`` に展開します。
| metadata要素がJSON形式の場合、 :ref:`json<template_json>` タグを用いることで、__USER_METADATA__内の要素を取得することができます。
| __USER_METADATA__の内容は、対話APIのレスポンスを返却するまでの間有効です。継続して__USER_METADATA__の内容を利用する場合、別途変数に代入してください。

メタデータ変数__USER_METADATA__はローカル変数（var）として扱いますが、ユーザ毎の管理情報であるため、特別に、レスポンスを返却するまでの間は、srai処理でも引き継いで利用することができます。

metadataの変数への展開方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 対話APIからmetadataとして渡されたデータを、変数__USER_METADATA__に展開します。
| metadataには、テキストデータと、JSONデータを設定することができ、各々のシナリオでの取り扱い方を説明します。

| 尚、以下の説明では、myServiceサブエージェントを利用して情報を取得し、以下の結果がJSONデータとして返却されることを前提として記載します。
| 返却されたデータは、変数 ``__SUBAGENT__.myService`` に展開されています。サブエージェント連携についての詳細は、 :doc:`SubAgent <SubAgent>` を参照してください。

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
            },
            "facility": ["鹿苑寺", "清水寺", "伏見稲荷大社"]
        }
    }



テキストデータとしての取り扱い方
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

テキストデータの場合、変数__USER_METADATA__に対する :ref:`get<template_get>` タグで内容を取得することができます。

対話APIコールのmetadataとして、以下のように文字列が与えられた場合を説明します。

..  code:: json

    {
        "locale": "ja-JP",
        "time": "2018-07-01T12:18:45+09:00",
        "topic": "*",
        "utterance": "subagent こんにちは",
        "metadata": "メタデータテスト"
    }

| シナリオには以下のように記載します。
| __USER_METADATA__の内容を :ref:`get<template_get>` タグで取得し、 :ref:`sraix<template_sraix>` タグの内容として設定することで、myService サブエージェントに引き渡します。
| サブエージェントの返却データは、 ``__SUBAGENT__.myService`` に保持されており( 詳細は :doc:`SubAgent <SubAgent>` に記載)、要素のキーを指定することでJSON内の値を取得しています。

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star /><space />
                        <get var="__USER_METADATA__" />
                    </sraix>
                    <set name="departure"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name="arrival"><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                <get name="departure"/>から<get name="arrival"/>までを検索します。
            </template>
        </category>
    </aiml>

ユーザ発話が「subagent こんにちは」の場合、以下のように展開されたデータが、サブエージェントに渡ります。

.. csv-table::
    :header: "引数番号","サブエージェントに渡された内容"
    :widths: 20,80

    "第1引数","こんにちは"
    "第2引数","メタデータテスト"

※ 引数を空白で分離する為に、space要素を使用しています。

JSONデータとしての取り扱い方
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

対話APIコールのmetadataとして、以下のようにJSONデータが与えられた場合を説明します。

..  code:: json

    {
        "locale": "ja-JP",
        "time": "2018-07-01T12:18:45+09:00",
        "topic": "*",
        "utterance": "subagent こんにちは",
        "metadata": {"arg1": "value1", "arg2": "value2", "arg3": "value3"}
    }

| metadataがJSONの場合、 :ref:`json<template_json>` タグを使用することで、JSON形式のデータとして取り扱うことができます。
| JSONの値を取得して個々に設定する場合には、 :ref:`json<template_json>` タグにキーを指定して対象となる値を取得し、その値を :ref:`sraixタグ<template_sraix>` の内容として設定することで、サブエージェントに引き渡すことができます。

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star /><space />
                        <json var="__USER_METADATA__.arg1" /><space />
                        <json var="__USER_METADATA__.arg2" /><space />
                        <json var="__USER_METADATA__.arg3" />
                    </sraix>
                    <set name="departure"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name="arrival"><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                <get name="departure"/>から<get name="arrival"/>までを検索します。
            </template>
        </category>
    </aiml>


ユーザ発話が「subagent こんにちは」の場合、以下のように展開されたデータがサブエージェントに渡ります。

.. csv-table::
    :header: "引数番号","サブエージェントに渡された内容"
    :widths: 20,80

    "第1引数","こんにちは"
    "第2引数","value1"
    "第3引数","value2"
    "第4引数","value3"

※ 引数を空白で分離する為に、space要素を使用しています。

サブエージェントにmetadata全てを引き渡す方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 対話APIからmetadataとして渡されたJSONデータを、サブエージェントにそのままJSONとして渡すことができます。
| :ref:`json<template_json>` タグの属性に__USER_METADATA__を指定することで、metadataに設定されたデータ全てを取得し、サブエージェントに引き渡します。

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star /><space />
                        <json var="__USER_METADATA__" /> 
                    </sraix>
                    <set name=departure><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                    <set name=arrival><json var="__SUBAGENT__.myService.transportation.station.arrival" /></set>
                </think>
                <get name='departure'>から<get name='arrival'>までを検索します。
            </template>
        </category>
    </aiml>


ユーザ発話が「subagent こんにちは」の場合、myServiceサブエージェントに対する第2引数で指定されたJSONがそのまま渡ります。

.. csv-table::
    :header: "引数番号","サブエージェントに渡された内容"
    :widths: 20,80

    "第1引数","こんにちは"
    "第2引数","{'arg1': 'value1', 'arg2': 'value2', 'arg3': 'value3'}"

※ 引数を空白で分離する為に、space要素を使用しています。

対話APIに返すmetadataの設定
----------------------------------------

| 対話APIのレスポンスに設定するmetadata要素の指定は、シナリオの返却用メタデータ変数__SYSTEM_METADATA__にデータを設定することで行います。
| レスポンスのmetadata要素には、テキストデータ、または、JSONデータを設定することができ、各々のシナリオでの取り扱い方を説明します。

メタデータ変数__SYSTEM_METADATA__はローカル変数（var）として扱いますが、ユーザ毎の管理情報であるため、特別に、レスポンスを返却するまでの間は、srai処理でも引き継いで利用することができます。

テキストデータとしての取り扱い方
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 以下の例では、myServiceサブエージェントから取得したデータの中の"出発地"のテキストを、返却用メタデータ変数に設定しています。
| サブエージェントから返却されたJSONから、出発地："station.departure"の要素(テキスト)を取得し、__SYSTEM_METADATA__に設定します。
| これによって、対話APIのレスポンスのmetadata要素として、テキストデータが返却されます。

.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star />
                    </sraix>
                    <set var="__SYSTEM_METADATA__"><json var="__SUBAGENT__.myService.transportation.station.departure" /></set>
                </think>
                メタデータに出発地を設定しました。
            </template>
        </category>
    </aiml>

| Input: subagent 東京
| Output: メタデータに出発地を設定しました。
| metadataの内容: "東京"



JSONデータとしての取り扱い方
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 以下の例では、myServiceサブエージェントから取得したJSONデータを、返却用メタデータ変数に設定しています。
| :ref:`json<template_json>` タグで、サブエージェントから返却されたJSONデータ全体を__SYSTEM_METADATA__に設定します。
| これによって、対話APIのレスポンスのmetadata要素として、JSONデータが返却されます。


.. code:: xml

    <aiml>
        <!-- sub agent execute -->
        <category>
            <pattern>subagent *</pattern>
            <template>
                <think>
                    <sraix service="myService">
                        <star />
                    </sraix>
                    <set var="__SYSTEM_METADATA__"><json var="__SUBAGENT__.myService" /></set>
                </think>
                メタデータにサブエージェントの処理結果を設定しました。
            </template>
        </category>
    </aiml>

| Input: subagent 東京
| Output: メタデータにサブエージェントの処理結果を設定しました。
| metadataの内容: 
        {"transportation": 
            {"station": 
                {"departure": "東京",
                 "arrival": "京都"},
            "time": 
                {"departure": "2018/11/1 11:00",
                 "arrival": "2018/11/1 13:30"},
            "facility: ["鹿苑寺", "清水寺", "伏見稲荷大社"]}}

関連項目: :doc:`対話API <../Api>`、 :doc:`対話APIデータの変数利用 <API_Variables>`、 :doc:`JSON <JSON>`、 :doc:`SubAgent <SubAgent>`
