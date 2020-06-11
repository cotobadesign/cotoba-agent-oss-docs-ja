JSON
============================

JSONをAIMLで利用するための機能です。
SubAgent,metadata,意図解釈結果などのJSONデータをAIMLで利用するために使用します。

name/var/dataで指定する変数名には、get/setで定義した変数名を使用します。
get/set varはローカル変数、get/set nameはグローバル変数、get/set dataはグローバル変数でAPIからのdeleteVariableがtrueまで保持する変数として作用します。
また、メタデータ、サブエージェントの戻り値等のシステム固定変数名もnameとして利用できます。


* 属性

.. csv-table::
    :header: "パラメータ","設定値","タイプ","必須","説明"
    :widths: 10,10,10,5,65

    "name","","JSON名","Yes","パースを行うJSONを指定します。var,name,dataのいずれかが設定されている必要があります。"
    "var","","JSON名","Yes","パースを行うJSONを指定します。var,name,dataのいずれかが設定されている必要があります。"
    "data","","JSON名","Yes","パースを行うJSONを指定します。var,name,dataのいずれかが設定されている必要があります。"
    "key","","キー指定","No","JSONデータを操作するキーを指定します。"
    "item","","キー名取得","No","JSONデータからキーを取得する場合に使用します。この属性を指定すると値ではなくキーを取得します。"
    "function","","関数名","No","JSONに対する処理を記述します。"
    "","len","関数名","No","対象のJSONプロパティが配列の場合、配列長を取得します。対象がJSONオブジェクトの場合、JSONオブジェクトの要素数を取得します。"
    "","delete","関数名","No","対象プロパティを削除します。配列の場合でindexを指定していると対象となる要素を削除します。"
    "","insert","関数名","No","JSON配列に対する値の追加を指定します。"
    "index","","インデックス","No","JSONデータを取得する場合のインデックスを指定します。対象が配列の場合、配列番号を指します。JSONオブジェクトではキーを先頭から順に数えたオブジェクトを指します。JSONデータを設定・変更する場合、配列のみに指定できます。"
    "type","","設定タイプ","No","数値、真偽値、nullを文字列として利用するかどうかを指定します。"


* 子要素

AIMLの変数を値として指定する場合に属性では指定できないため、子要素としても指定できるようにしています。
動作は属性と同じ動作になります。同じ属性名、子要素名を指定した場合子要素の設定が優先されます。
ただし、keyを子要素で利用する場合のみ、nameとvarの区別はしており別扱いとなります。

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "function","関数名","No","JSONに対する処理を記述します。ここのfunctionについては属性を参照。"
    "index","インデックス","No","JSONデータを取得する場合のインデックスを指定します。対象が配列の場合、配列番号を指します。JSONオブジェクトではキーを先頭から順に数えたオブジェクトを指します。JSONデータを設定・変更する場合、配列のみに指定できます。"
    "item","キー名取得","No","JSONデータからキーを取得する場合に使用します。この属性を指定すると値ではなくキーを取得します。"
    "key","キー指定","No","JSONデータを操作するキーを指定します。"


基本利用方法
-----------------------------

JSON要素にname、data、もしくは、var属性で、対象となるjsonデータを指定します。
JSON要素に内容を設定した場合、対象となるJSONデータに値を設定、更新します。
JSON要素に内容を設定しなかった場合、削除の場合を除き、対象となるキーの値を取得します。取得に失敗した場合、 :ref:`get<template_get>` と同様に、Config等で設定された"default-get"の値が返ります。
以下に、JSONを対象とした場合のデータの取得方法、設定方法を説明します。
サブエージェントの戻り値を利用する例で説明します。サブエージェントの戻り値は、変数 ``__SUBAGENT__.family`` に保持されている前提です。

.. code:: json

    {
        "family": {
            "名前": "一郎",
            "誕生日": "19700101",
            "年齢": 48,
            "性別": "male",
            "自宅": { "住所": "東京都港区","座標": "x,y" },
            "家族": {
                "父": "太郎",
                "母": "花子",
                "兄弟": [ "二郎", "菊子" ],
                "子": [ "三郎", "桃子" ]
            },
            "親戚": {
                "祖父": [ "士郎", "五郎" ],
                "祖母": [ "久里子", "梅子" ],
                "従姉妹": [ "史郎", "咲子" ],
                "孫": [ "紗希子" ]
            },
            "友人": [ "和宏", "京子" ],
            "趣味": [ "ゴルフ", "野球" ],
            "病歴": [ "高血圧", "花粉症" ],
            "アレルギー": [ "牛乳", "そば" ]
        }
    }

JSONデータの取得時の属性/子要素指定方法
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

属性および子要素の指定方法を説明します。
記載方法は異なりますが、処理結果は同じ結果になります。

キー指定の値取得
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

"父"の値を取得する場合、以下の記述を行います。
属性の場合、 ``.`` 区切りで取得したいキーを記載します。
子要素の場合、``<key>`` の内容に取得したいキーを記載します。

.. code:: xml

    <json var="__USER_METADATA__.family.家族.父" />
    <!-- <json var="__USER_METADATA__.family"><key>家族.父</key></json>  上記内容と同動作-->


配列の取得
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

配列になっている、"兄弟"の値を取得する場合、
値の取得同様、 ``.`` 区切りで取得したいキーを記載、もしくは子要素 ``<key>`` をに記述することで指定した配列を取得します。
実行結果として、 ``["二郎", "菊子"]`` を取得します。

.. code:: xml

    <json var="__USER_METADATA__.family.家族.兄弟" />
    <!-- <json var="__USER_METADATA__.family.家族"><key>兄弟</key></json>  上記内容と同動作-->


配列長の取得
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

functionに"len"を指定すると配列長を取得します。
"兄弟"の場合、 ``2`` を取得します。

.. code:: xml

    <json var="__USER_METADATA__.family.家族.兄弟" function="len"/>
    <!-- <json var="__USER_METADATA__.family.家族.兄弟"><function>len</function></json>   上記内容と同動作-->

配列値の取得
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

配列の内容を取得する場合、配列のインデックスを記載します。
"兄弟"の0番目の値、 ``二郎`` を取得します。

.. code:: xml

    <json var="__USER_METADATA__.family.家族.兄弟" index="0"/>
    <!-- <json var="__USER_METADATA__.family.家族.兄弟"><index>0</index></json>  上記内容と同動作-->



JSONオブジェクトの要素数取得
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

JSONオブジェクトの要素数を取得する場合、functionに ``len`` を指定します。
familyを指定した場合の要素数は ``11`` になります。

.. code:: xml

    <json var="__USER_METADATA__.family" function="len"/>
    <!-- <json var="__USER_METADATA__.family"><function>len</function></json>  上記内容と同動作-->


JSONオブジェクトのキーを取得する場合
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

JSONオブジェクトのキーを取得する場合、itemに ``key`` を指定します。
familyの5番目を指定すると、 ``家族`` を取得します。

.. code:: xml

    <json var="__USER_METADATA__.family" item="key" index="5"/>
    <!-- <json var="__USER_METADATA__.family"><item>key</item><index>5</index></json>  上記内容と同動作-->



JSONデータの更新
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

JSONデータに既にあるキーの値を変更する場合、JSON要素の内容に値を記載します。
内容を記載することで、"住所"を"新横浜"に更新します。
空の内容にする場合、 ``""`` を指定してください。

.. code:: xml

    <json var="__USER_METADATA__.family.自宅.住所">新横浜</json>
    <json var="__USER_METADATA__.family.自宅.座標">""</json>
    <!-- <json var="__USER_METADATA__.family.自宅"><key>住所</key>新横浜</json>
        <json var="__USER_METADATA__.family.自宅"><key>座標</key>""</json>  上記内容と同動作-->


更新前

.. code:: json

    "自宅": {"住所": "東京都港区", "座標": "x,y"},

更新後

.. code:: json

    "自宅": {"住所": "新横浜", "座標": ""},


JSONデータへの追加
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

新しいキーを追加する場合、新しいキーに値を指定することで新たなキーを追加します。

.. code:: xml

    <json var="__USER_METADATA__.family.郵便番号">222-0033</json>
    <!-- <json var="__USER_METADATA__.family"><key>郵便番号</key>222-0033</json>  上記内容と同動作-->

更新前

.. code:: json

    {
        "family":{
            "病歴": ["高血圧", "花粉症"],
            "アレルギー": ["牛乳", "そば"]
        }
    }

更新後

.. code:: json

    {
        "family":{
            "病歴": ["高血圧", "花粉症"],
            "アレルギー": ["牛乳", "そば"],
            "郵便番号": "222-0033"
        }
    }

配列の内容の変更
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

配列の内容を変更する場合、配列のインデックスを記載します。
"趣味"の0番目の値を変更する場合、以下のように取得します。

.. code:: xml

    <json var="__USER_METADATA__.family.趣味" index="0">サッカー</json>
    <!-- <json var="__USER_METADATA__.family"><key>趣味</key><index>0</index>サッカー</json>  上記内容と同動作-->

更新前

.. code:: json

    {
        "family":{
            "趣味": ["ゴルフ", "野球"]
        }
    }

更新後

.. code:: json

    {
        "family":{
            "趣味": ["サッカー", "野球"]
        }
    }


配列の変更
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

配列になっている"趣味"の要素を全て変更する場合、
個々の要素をダブルクォートで囲み、カンマで区切ります。

.. code:: xml

    <json var="__USER_METADATA__.family.趣味">"サッカー","釣り","映画鑑賞"</json>
    <!-- <json var="__USER_METADATA__.family"><key>趣味</key>"サッカー","釣り","映画鑑賞"</json>  上記内容と同動作-->

を指定すると、

更新前

.. code:: json

            "趣味": ["ゴルフ", "野球"],

更新後

.. code:: json

            "趣味": ["サッカー","釣り","映画鑑賞"],

配列への要素追加
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

配列への要素追加はfunctionにinsertを指定し、indexで挿入箇所を設定します。
先頭に値を追加する場合、indexに0を指定します。
マイナスインデックスは後ろからのインデックス値を表し、indexに-1を指定すると配列の最後に値を追加します。
個々の要素をダブルクォートで囲み、カンマで区切ります。

以下の例では、配列になっている"趣味"に対しindex="0"を指定し、配列の先頭に値を追加しています。

.. code:: xml

    <json var="__USER_METADATA__.family.趣味" function="insert" index="0">"サッカー","釣り","映画鑑賞","旅行(海外,国内)"</json>
    <!-- <json var="__USER_METADATA__.family"><key>趣味</key><function>insert</function><index>0</index>"サッカー","釣り","映画鑑賞","旅行(海外,国内)"</json>   上記内容と同動作-->


更新前

.. code:: json

            "趣味": ["ゴルフ", "野球"],

更新後

.. code:: json

            "趣味": ["サッカー", "釣り", "映画鑑賞", "旅行(海外,国内)", "ゴルフ", "野球"],


以下の例では、配列になっている"趣味"に対し配列要素数のindex="2"を指定することで、配列の最後に値を追加しています。

.. code:: xml

    <json var="__USER_METADATA__.family.趣味" function="insert" index="2">"サッカー","釣り","映画鑑賞","旅行(海外,国内)"</json>
    <!-- <json var="__USER_METADATA__.family"><key>趣味</key><function>insert</function><index>2</index>"サッカー","釣り","映画鑑賞","旅行(海外,国内)"</json>  上記内容と同動作-->

更新前

.. code:: json

            "趣味": ["ゴルフ", "野球"],

更新後

.. code:: json

            "趣味": ["ゴルフ", "野球", "サッカー", "釣り", "映画鑑賞", "旅行(海外,国内)"],

また、index="-1"でも同様に、配列の最後に値を追加しています。

.. code:: xml

    <json var="__USER_METADATA__.family.趣味" function="insert" index="-1">"サッカー","釣り","映画鑑賞","旅行(海外,国内)"</json>
    <!-- <json var="__USER_METADATA__.family"><key>趣味</key><function>insert</function><index>-1</index>"サッカー","釣り","映画鑑賞","旅行(海外,国内)"</json>  上記内容と同動作-->

更新前

.. code:: json

            "趣味": ["ゴルフ", "野球"],

更新後

.. code:: json

            "趣味": ["ゴルフ", "野球", "サッカー", "釣り", "映画鑑賞", "旅行(海外,国内)"],

配列の作成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

カンマ区切りの要素を設定するか、functionにinsertを指定しindexを0か-1を設定した場合に配列を作成します。(indexに0もしくは-1以外を指定した場合作成されません)

以下の例では、新たな配列要素として"学歴"を作成しています。


.. code:: xml

    <json var="__USER_METADATA__.family.学歴" >"A小学校","B中学校","C高校","D大学"</json>
    <!-- <json var="__USER_METADATA__.family.学歴" function="insert" index="0">"A小学校","B中学校","C高校","D大学"</json>  上記内容と同動作-->
    <!-- <json var="__USER_METADATA__.family.学歴" function="insert" index="-1">"A小学校","B中学校","C高校","D大学"</json>  上記内容と同動作-->
    <!-- <json var="__USER_METADATA__.family"><key>学歴</key><function>insert</function><index>0</index>"A小学校","B中学校","C高校","D大学"</json>  上記内容と同動作-->

作成後

.. code:: json

            "学歴": ["A小学校","B中学校","C高校","D大学"]

1要素の場合は、functionにinsert未指定でJSONオブジェクトを作成することができるが、insertを指定して配列に変更することはできません。
1要素でも要素が増える場合は、配列要素として作成する必要があります。

.. code:: xml

    <!-- <json var="__USER_METADATA__.family.学歴" function="insert" index="0">"D大学"</json>  上記内容と同動作-->
    <!-- <json var="__USER_METADATA__.family.学歴" function="insert" index="-1">"D大学"</json>  上記内容と同動作-->
    <!-- <json var="__USER_METADATA__.family"><key>学歴</key><function>insert</function><index>0</index>"D大学"</json>  上記内容と同動作-->

更新後、1要素の配列が作成されます。

.. code:: json

            "学歴": ["D大学"]


functionにinsert未指定の場合、

.. code:: xml

    <json var="__USER_METADATA__.family.学歴" >"D大学"</json>

更新後はJSONオブジェクトが作成されます。

.. code:: json

            "学歴": "D大学"


JSONデータの削除
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


配列の要素削除
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

配列の要素を削除するには、functionにdelete、indexに削除する要素の番号を設定します。
指定したindexの値を削除します。
マイナスインデックスは後ろからのインデックス値を表し、indexに-1を指定すると配列の最後の値を削除します。

.. code:: XML

    <json var="__USER_METADATA__.family.趣味" index="0" function="delete" />
    <!-- <json var="__USER_METADATA__.family.趣味"><index>0</index><function>delete</function></json>  上記内容と同動作-->
    <json var="__USER_METADATA__.family.趣味" index="-1" function="delete" />
    <!-- <json var="__USER_METADATA__.family.趣味"><index>-1</index><function>delete</function></json>  上記内容と同動作-->

更新前

.. code:: json

            "趣味": ["ゴルフ", "野球","読書"],

更新後

.. code:: json

            "趣味": ["野球"],


キーの削除
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

キーを削除する場合は、functionにdeleteを設定します。
指定されたキーおよび値が削除されます。

"function"に"delete"を指定することで"趣味"キーと値を削除します。

.. code:: xml

    <json var="__USER_METADATA__.family.趣味" function="delete" />
    <!-- <json var="__USER_METADATA__.family.趣味"><function>delete</function></json>   上記内容と同動作-->

更新前

.. code:: json

            "友人": ["和宏", "京子"],
            "趣味": ["ゴルフ", "野球"],
            "病歴": ["高血圧", "花粉症"],

更新後

.. code:: json

            "友人": ["和宏", "京子"],
            "病歴": ["高血圧", "花粉症"],


JSON形式データの指定
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

要素として、JSON形式のデータを設定する場合、波括弧：{}で囲んだJSONの文字列形式を指定します。
配列の操作でも指定できますが、JSON文字列形式の記述をリストで指定することはできないため、１要素ずつ指定する必要があります。

.. code:: XML

    <json var="__USER_METADATA__.family.家族.父">{"名前": "太郎", "年齢": 80}</json>
    <!-- <json var="__USER_METADATA__.family.家族"><key>父</key>{"名前": "太郎", "年齢": 80}</json>  上記内容と同動作-->

更新前

.. code:: json

            "父": "太郎",

更新後

.. code:: json

            "父": {
                   "名前": "太郎",
                   "年齢": 80
                   },


尚、本機能はJSONのキーに対して内容を設定するものであり、以下の様に、変数に直接JSON形式のデータを設定することはできません。
変数にJSON形式の内容を設定する場合には、:ref:`set<template_set>` 要素を使用してください。

.. code:: XML

    <!-- 設定不可 --> <json var="__USER_METADATA__">{"family": {"家族": {"父": {"名前": "太郎", "年齢": 80}}}}</json>

    <!-- 設定可能 --> <set var="__USER_METADATA__">{"family": {"家族": {"父": {"名前": "太郎", "年齢": 80}}}}</set>


関連項目: :doc:`SubAgent<SubAgent>` 、:doc:`metadata<Metadata>` 、:doc:`意図解釈<NLU>` 


数値、真偽値、nullの取り扱い
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

AIMLでは文字列としての扱いしかなく、JSONの数値、真偽値、nullを直接取り扱う事は出来ません。
これらの内容をJSON要素に設定および取得する場合の動作を説明します。


設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 設定時に、数値のみを指定すると内部では数値として取り扱います。
| 数値以外の文字列が含まれると文字列扱いになります。
| 真偽値を示す、"true","false"を設定した場合、真偽値としてJSONに登録します。
| 値にnullを設定した場合、JSONにはnullを設定します。
| 数値、true,false,nullを文字列として設定する場合、typeに ``string`` を指定します。

例:

.. code:: xml

    <json var="__USER_METADATA__.family.年齢">30</json>
    <json var="__USER_METADATA__.family.満年齢">31歳</json>
    <json var="__USER_METADATA__.family.誕生日" type="string">19700101</json>
    <json var="__USER_METADATA__.family.自己紹介">null</json>
    <json var="__USER_METADATA__.family.電話番号認証">true</json>
    <json var="__USER_METADATA__.family.メール認証" type="string">false</json>

例の設定結果は以下のJSONになります。

.. code:: json

    {
        "family": {
            "年齢": 30,
            "満年齢": "31歳",
            "誕生日": "19700101",
            "自己紹介": null,
            "電話番号認証": true,
            "メール認証": "false"
        }
    }

取得
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

JSON要素で数値、真偽値、nullを取得する場合、これらは文字列として取得されます。
数値の場合、数値文字列、真偽値の場合"true","false"の文字列、nullの場合"null"の文字列として取得します。

例:

.. code:: XML

    <json var="__USER_METADATA__.family.年齢"/>
    <json var="__USER_METADATA__.family.満年齢"/>
    <json var="__USER_METADATA__.family.誕生日"/>
    <json var="__USER_METADATA__.family.電話番号認証"/>
    <json var="__USER_METADATA__.family.メール認証"/>
    <json var="__USER_METADATA__.family.自己紹介"/>


取得値は、以下のとおり各々が文字列で取得されるため、シナリオ設計者が取得元のデータ型を意識しておく必要があります。

.. code::

    30
    31歳
    19700101
    true
    false
    null
