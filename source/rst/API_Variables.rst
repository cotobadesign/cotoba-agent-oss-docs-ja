対話APIデータの変数利用
=======================================

概要
----------------------------------------
:ref:`対話API<coversation_api>` で指定されたデータの利用方法を説明します。

対話APIデータの変数
----------------------------------------

対話APIで与えられたデータは、対話シナリオで利用可能な変数に展開した状態で保持します。
保持期間はクライアントにレスポンスを返却するまでです。継続して内容を保持する場合、別途変数に代入してください。
対話APIリクエスト時に該当変数が未設定の場合、'None'が返ります。

対話APIデータを保持する変数名は下表のとおりです。

.. csv-table::
    :header: "項目名","変数名","型","説明"
    :widths: 30,20,10,40

    "ロケール","__USER_LOCALE__","string","対話API ``locale`` で指定した言語コード。"
    "時間情報","__USER_TIME__","string","対話API ``time`` で指定した時刻情報。"
    "ユーザID","__USER_USERID__","string","対話API ``userId`` で指定したユーザID。"
    "ユーザ発話","__USER_UTTERANCE__","string","対話API ``utterance`` で指定したユーザ発話内容。"
    "メタデータ","__USER_METADATA__","string","対話API ``metadata`` で指定したメタデータ。変数としては文字列で保持しています。 :doc:`json <JSON>` タグで取り扱う場合はJSONデータとして利用します。"

| 尚、各変数はローカル変数（var）として扱いますが、ユーザ毎の管理情報であるため、特別に、レスポンスを返却するまでの間は、srai処理でも引き継いで利用することができます。
| （各変数値をシナリオで変更して利用することは可能ですが、一部の要素処理での省略値としても使用するため、推奨はしません。） 

利用例
----------------------------------------

クライアントからの対話APIで以下のように、対話APIデータが与えられた場合、

..  code:: json

    {
        "locale": "ja-JP",
        "time": "2018-07-01T12:18:45+09:00",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "*",
        "utterance": "こんにちは",
        "metadata": {"arg1": "value1", "arg2": "value2"}
    }

シナリオでの扱い方は、以下のようになります。

.. code:: xml

    <aiml>
        <category>
            <pattern>こんにちは</pattern>
            <template>
                <get var="__USER_LOCALE__" /> ,
                <get var="__USER_TIME__" /> ,
                <get var="__USER_USERID__" /> ,
                <get var="__USER_UTTERANCE__" /> 
            </template>
        </category>
    </aiml>

| Input: こんにちは
| Output: ja-JP,2018-07-01T12:18:45+09:00,E8BDF659B007ADA2C4841EA364E8A70308E03A71,こんにちは

metadataについては、 :doc:`metadata <Metadata>` を参照してください。
