NLU
============================

| pattern要素で、意図解釈処理を行うための定義です。
| patternの子要素として ``nlu`` を定義すると、高度意図解釈のインテントとのマッチングの評価を行います。
| 対話制御では、シナリオの記述に従いルールベースの意図解釈を行って、patternマッチングで評価した結果に応じて応答を返しますが、マッチするpatternがなかった場合、高度意図解釈のインテントの結果を用いた対話制御を行います。
| これは意図解釈の結果より、シナリオ作成者が記述する内容を優先させるためです。
| 例外として、patternにワイルドカードのみが記述されたcategoryが存在する場合、シナリオ記述のマッチングと、意図解釈のマッチングとの両方でマッチしなかった後に、マッチ処理を行います。
| 子要素のnluを定義した場合でも、pattarn要素の内容を記載すると通常のパターン評価が行われます。
| nlu要素の属性は、:ref:`nlu<pattarn_nlu>` を参照してください。

.. _nlu_json_example:

基本利用方法
-----------------------------

高度意図解釈から以下のフォーマットで、インテント、スロットの情報が返却された例で説明します。

.. code:: json

    {
        "intents": [
            {"intent": "transportation", "score": 0.9 },
            {"intent": "aroundsearch", "score": 0.8 }
        ], 
        "slots": [
            {"slot": "departure", "entity": "東京", "score": 0.85, "startOffset": 3, "endOffset": 5 },
            {"slot": "arrival", "entity": "京都", "score": 0.86, "startOffset": 8, "endOffset": 10 },
            {"slot": "departure_time", "entity": "2018/11/1 19:00", "score": 0.87, "startOffset": 12, "endOffset": 14 },
            {"slot": "arrival_time", "entity": "2018/11/1 11:00", "score": 0.88, "startOffset": 13, "endOffset": 18 }
        ]
    }

インテントでのマッチング
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| AIMLのpattarnの子要素としてnluを記載します。
| 下記の例では、高度意図解釈から返却されたインテントが 'transportation' の場合にマッチし、応答文が返ります。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="transportation" />
        </pattern>
        <template>
            乗り換え案内ですね？
        </template>
    </category>

| Input: 東京から京都に行きたい。
| Output: 乗り換え案内ですね？


インテント候補にはあるがマッチしないパターン
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 次の例は、高度意図解釈のインテントが 'aroundsearch' の場合です。
| 'aroundsearch' はインテントの候補にありますが、最尤候補ではないためマッチしません。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="aroundsearch" />
        </pattern>
        <template>
            周辺検索ですね？
        </template>
    </category>

| Input: 東京から京都に行きたい。
| Output: NO_MATCH


最尤候補でない場合のマッチング
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| インテント 'aroundsearch' が最尤候補でなくても含まれる場合にマッチングさせたい場合、属性 ``maxLikelihood`` に ``false`` を設定します。
| ``maxLikelihood`` が未指定の場合、``true`` を指定した場合と同じ動作になります。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="aroundsearch" maxLikelihood="false" />
        </pattern>
        <template>
            周辺検索ですね？
        </template>
    </category>

| Input: 東京から京都に行きたい。
| Output: 周辺検索ですね？

score指定でのマッチ
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| インテントのscore値によるマッチング条件について説明します。
| 属性として、 scoreGt、scoreGe、score、scoreLe、scoreLtの5種類の指定が可能となり、設定内容は以下になります。
| また、この属性を指定した場合、信頼度での比較マッチングを行うため ``maxLikelihood`` は ``false`` 扱いになります。

.. csv-table::
    :header: "パラメータ名","意味","説明"
    :widths: 10,10,75

    "scoreGt",">","対象インテントの信頼度が指定した値より大きい場合にマッチします。"
    "scoreGe",">=","対象インテントの信頼度が指定した値以上の場合にマッチします。"
    "score","=","対象インテントの信頼度が指定した値の時にマッチします。"
    "scoreLe","<=","対象インテントの信頼度が指定した値以下の場合にマッチします。"
    "scoreLt","<","対象インテントの信頼度が指定した値より小さい場合にマッチします。"

scoreXx指定時の動作は下記のマッチングになります。

.. code:: xml

    <nlu intent="transportation" scoreGt="0.9"/>  transportationにマッチングしません。
    <nlu intent="transportation" scoreGe="0.9"/>  transportationにマッチングします。
    <nlu intent="transportation" score="0.9"/>    transportationにマッチングします。
    <nlu intent="aroundsearch" scoreLe="0.8"/>  aroundsearchにマッチングします。
    <nlu intent="aroundsearch" scoreLt="0.8"/>  aroundsearchにマッチングしません。

| 尚、下記例のように、高度意図解釈の結果によって複数の条件が成立つ記述が可能ですが、AIMLファイル内のcategoryの記載順序が早いものから適用されます。
| 複数のAIMLファイルを利用する場合、AIMLの展開処理をデイレクトリ名・ファイル名の昇順で行うため、その順序を意識して配置する必要があります。
| （サブディレクトリを使用する場合、上位ディレクトリ内のファイルを処理した後、サブディレクトリ配下のファイル処理に移ります。）

.. code:: xml

    <category>
        <pattern><nlu intent="transportation" scoreGe="0.8"/></pattern>
        <template>乗り換え案内ですね？</template>
    </category>

    <category>
        <pattern><nlu intent="aroundsearch" scoreGe="0.8"/></pattern>
        <template>周辺検索ですね？</template>
    </category>


インテントマッチとワイルドカード
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

下記は、ルールベース、意図解釈結果にマッチしなかった場合、雑談サブエージェントを呼び出す例になります。
patternとしてワイルドカードのみが記述されたcategoryが存在する場合、シナリオ記述のマッチングと、意図解釈のマッチングとの両方でマッチしなかった後に、マッチ処理を行います。

.. code:: xml

    <aiml>
        <category>
            <pattern>こんにちは</pattern>
            <template>こんにちは</template>
        </category>

        <category>
            <pattern><nlu intent="aroundsearch" /></pattern>
            <template>
                周辺を検索します
            </template>
        </category>

        <category>
            <pattern>
                *
            </pattern>
            <template>
                <sraix service="雑談"><get var="__USER_UTTERANCE__" /></sraix>
            </template>
        </category>
    </aiml>

| Input: こんにちは
| Output: こんにちは
| Input: この辺りのコンビニ
| Output: 周辺検索をします
| Input: 雑談対話
| Output: 雑談対話の結果です


NLUデータの取得
-----------------------------

NLUのデータは、変数 ``__SYSTEM_NLUDATA__`` に展開されます。
JSON要素を用い :ref:`意図解釈結果例<nlu_json_example>` のデータを取得する例を示します。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="transportation" />
        </pattern>
        <template>
            <think>
                    <set var="slot"><json var="__SYSTEM_NLUDATA__.slots"><index>1</index></json></set>
                    <set var="entity"><json var="slot.entity" /></set>
                    <set var="score"><json var="slot.score" /></set>
            </think>
            <get var="entity"/>のscoreは<get var="score" />です。
        </template>
    </category>

| Input: 東京から京都に行きたい。
| Output: 東京のscoreは0.85です。

関連項目: :ref:`nlu<pattarn_nlu>`、 :doc:`JSON要素 <JSON>`


.. _nlu_intent_example:

NLUインテントの取得
-----------------------------

templateでインテントの内容を取得する場合、 :ref:`nluintent<template_nluintent>` を使用します。
NLU処理結果は以下の結果を取得している前提でインテント情報の取得方法を説明します。

.. code:: json

    {
        "intents": [
            {"intent": "restaurantsearch", "score": 0.9 },
            {"intent": "aroundsearch", "score": 0.4 }
        ], 
        "slots": [
            {"slot": "genre", "entity": "イタリアン", "score": 0.95, "startOffset": 0, "endOffset": 5 },
            {"slot": "genre", "entity": "フレンチ", "score": 0.86, "startOffset": 7, "endOffset": 10 },
            {"slot": "genre", "entity": "中華", "score": 0.75, "startOffset": 12, "endOffset": 14 }
        ]
    }

NLUで処理したインテント情報を取得する例です。mapには数値をカウントアップすることを定義していることが前提です。
intentCountにインテント数を保持しconditionの条件としてintentCount数になるまで、各スロットのintent名、scoreを取得します。


.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <think>
              <set var="count">0</set>
              <set var="intentCount"><nluintent name="*" item="count" /></set>
            </think>
            <condition>
                <li var="count"><value><get var="intentCount" /></value></li>
                <li>
                    intent:<nluintent name="*" item="intent"><index><get var="count" /></index></nluintent>
                    score:<nluintent name="*" item="score"><index><get var="count" /></index></nluintent>
                    <think>
                        <set var="count"><map name="upcount"><get var="count" /></map></set>
                    </think>
                    <loop/>
                </li>
            </condition>
        </template>
    </category>

| Input: イタリアンかフレンチか中華を探して。
| Output: intent:restaurantsearch score:0.9 intent:aroundsearch score:0.4


関連項目: :ref:`nluintent<template_nluintent>`


.. _nlu_slot_example:

NLUスロットの取得
-----------------------------

templateでNLU処理結果のスロットの内容を取得する場合、 :ref:`nluslot<template_nluslot>` を使用します。
NLU処理結果は以下の結果を取得していることを前提としてスロット情報の取得方法を説明します。

.. code:: json

    {
        "intents": [
            {"intent": "restaurantsearch", "score": 0.9 },
            {"intent": "aroundsearch", "score": 0.4 }
        ], 
        "slots": [
            {"slot": "genre", "entity": "イタリアン", "score": 0.95, "startOffset": 0, "endOffset": 5 },
            {"slot": "genre", "entity": "フレンチ", "score": 0.86, "startOffset": 7, "endOffset": 10 },
            {"slot": "genre", "entity": "中華", "score": 0.75, "startOffset": 12, "endOffset": 14 }
        ]
    }

NLUで処理したスロット情報を取得する例です。mapには数値をカウントアップすることを定義していることが前提です。
slotCountにスロット数を保持しconditionの条件としてslotCount数になるまで、各スロットのslot名、entity、scoreを取得します。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch" />
        </pattern>
        <template>
            <think>
              <set var="count">0</set>
              <set var="slotCount"><nluslot name="*" item="count" /></set>
            </think>
            <condition>
                <li var="count"><value><get var="slotCount" /></value></li>
                <li>
                    slot:<nluslot name="*" item="slot"><index><get var="count" /></index></nluslot>
                    entity:<nluslot name="*" item="entity"><index><get var="count" /></index></nluslot>
                    score:<nluslot name="*" item="score"><index><get var="count" /></index></nluslot>
                    <think>
                        <set var="count"><map name="upcount"><get var="count" /></map></set>
                    </think>
                    <loop/>
                </li>
            </condition>
        </template>
    </category>

| Input: イタリアンかフレンチか中華を探して。
| Output: slot:genre entity:イタリアン score:0.95 slot:genre entity:フレンチ score:0.86 slot:genre entity:中華 score:0.75 

関連項目: :ref:`nluslot<template_nluslot>`
