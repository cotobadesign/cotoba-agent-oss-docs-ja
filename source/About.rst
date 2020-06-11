========================
COTOBA AIMLについて
========================
COTOBA AIMLとは、COTOBA DESIGN, Inc.の対話プラットフォームであるCOTOBA Agentで、対話シナリオを記載する為に利用する対話言語です。
AIML(Artificial Intelligence Markup Language)をベースにCOTOBA DESIGN, Inc.が独自拡張を行なっており、JSONの取り扱いやAPIからのメタデータの処理、REST APIの呼び出しおよび戻り値の参照による対話制御等、COTOBA Agent以外の情報を用いた対話制御ができるような拡張を行なっています。


COTOBA AIML記述方法
---------------------
対話シナリオの記述方法について説明します。
ここでは利用者の発話内容に対する、対話プラットフォームの応答を記載するという、基本的な対話シナリオの例を説明します。

変数の保持、JSONの利用方法、REST APIの呼び出し方法等はCOTOBA AIMLリファレンスの内容を組み合わせることで、様々な対話シナリオを作成することができます。


基本対話シナリオ
---------------------
利用者の発話の内容に応じ、対話プラットフォームからの応答を定義し、対話シナリオを記述します。

category要素は、AIMLの対話ルールの基本単位です。 category要素内に、 pattern と template といった1つの対話ルールを記載します。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">

        <category>
            <pattern>おはよう</pattern>
            <template>おはようございます、今日も1日頑張りましょう。</template>
        </category>

    </aiml>

| Input: おはよう
| Output: おはようございます、今日も1日頑張りましょう


応答の分岐
---------------------
対話シナリオを作成する場合、利用者の1つ前の発話に応じて応答文を変更する必要が多々あります。

例えば、対話プラットフォームからの問い合わせに対し、"はい/いいえ"で応答を返すシーンがあります。"はい/いいえ"をpatternに記載しますが"はい/いいえ"といった、一般的な言い回しは他の対話でも利用され区別をつける必要があります。

この場合の応答分岐の例としては、thatを利用します。

以下例では、"はい"、"いいえ"の結果により応答を変えています。ただし、"はい"、"いいえ"のユーザ発話はこの例以外でも発生しますが、 この応答はBotの応答が、"コーヒーに砂糖とミルクを入れますか？"の時だけにマッチするように、thatでBotの前発話をマッチ条件にしています。

.. code:: xml

    <category>
        <pattern>私はコーヒーが好きです</pattern>
        <template>コーヒーに砂糖とミルクを入れますか</template>
    </category>

    <category>
        <pattern>はい</pattern>
        <that>コーヒーに砂糖とミルクを入れますか</that>
        <template>わかりました</template>
    </category>

    <category>
        <pattern>いいえ</pattern>
        <that>コーヒーに砂糖とミルクを入れますか</that>
        <template>ブラックですね</template>
    </category>

| Input: 私はコーヒーが好きです
| Output: コーヒーに砂糖とミルクを入れますか
| Input: はい
| Output: わかりました


対話内容の抽出、変数利用
-------------------------
利用者の発話の内容を抽出するには"*"とstarを利用します。また、利用者の発話した内容を保持する為、getとsetを利用します。

1つ目の発話でペットの種類を保持します。その際利用者の発話patternのペットの種類に相当する部分をワイルドカード"*"で指定します。ワイルドカードの部分をtemplateで利用する場合<star/>を用います。starにはpatternで定義したワイルドカード指定した範囲の文字列が抽出されます。

変数の利用はset/getを使用します。setの属性で指定した名称petcategoryにsetの内容を保持します。

次の発話では、利用者のペットの名前を保持します。同様にsetを用い保持しますが、別の変数名petnameを利用します。

次の発話では、保持した内容を利用し対話プラットフォームからの応答の選択および返答内容に変数の内容を含めます。

変数の内容による分岐は、conditionを利用します。conditionは対象となる変数との文字列比較を行う要素で、switch-caseのような処理を記載することができます。

以下の例では、ペットの種類petcategoryが"犬"か"猫"かをswitch-case文のcaseに当たるliで分岐します。どちらでもなかった場合未評価結果を返します。

また、返答の内容にpetnameで保持した内容を返しています。

.. code:: xml

    <category>
        <pattern>私のペットは*です</pattern>
        <template>
            <think><set name="petcategory"><star/></set></think>
            <star/>が好きなんですね
        </template>
    </category>

    <category>
        <pattern>ペットの名前は*です</pattern>
        <template>
            <think><set name="petname"><star/></set></think>
            いい名前ですね。
        </template>
    </category>

    <category>
        <pattern>私のペット覚えてる？</pattern>
        <template>
            <condition name="petcategory">
                <li value="犬">あなたのペットは犬の<get name="petname"/>ですよね</li>
                <li value="猫">あなたのペットは猫の<get name="petname"/>ですよね</li>
                <li>ペットは飼っていなかったよね</li>
            </condition>
        </template>
    </category>

| Input: 私のペットは犬です。
| Output: 犬が好きなんですね。
| Input: ペットの名前はマロンです。
| Output: いい名前ですね。
| Input: 私のペット覚えてる？
| Output: あなたのペットは犬のマロンですよね。


BOT連携
---------------------
複数BOTを作成し各々の結果を連携し動作させることができます。連携にはsraix要素の外部REST API呼び出しを利用します。
以下のように、既に作成したボットIDをホスト名の呼び出し先に指定し、bodyに必要な情報を設定します。

BOTからの戻り値は、var:__SUBAGENT_BODY__に含まれており、json要素で取り出しを行うことができます。


.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>

    <aiml version="2.0">
        <category>
            <pattern>サブエージェント*</pattern>
            <template>
                <think>
                    <json var="body.utterance"><star/></json>
                    <json var="body.userId"><get var="__USER_USERID__"/></json>
                    <set var="__SYSTEM_METADATA__"><json var="body"/></set>
                    <sraix>
                        <host>https://HOSTNAME/bots/BOT_ID/ask</host>
                    
                        <method>POST</method>
                        <header>"Content-Type":"application/json;charset=UTF-8"</header>
                        <body><json var="body"/></body>
                    </sraix>
                    
                </think>
                <json var="__SUBAGENT_BODY__.response"/>
            </template>
        </category>
    </aiml>


意図解釈エンジン連携
---------------------
意図解釈エンジンで作成したモデルを用いる場合、推論エンドポイントをボット作成時に設定します。

NLU要素を利用し意図解釈エンジンのインテントによるpattern分岐シナリオを作成します。その際の意図解釈エンジン利用時のインテント、スロットは、nluintent,nluslot要素で取得することができます。

また、対話プラットフォームはシナリオの記述に従いルールベースの意図解釈を行って、patternマッチングで評価した結果に応じて応答を返しますが、マッチするpatternがなかった場合、高度意図解釈のインテントの結果を用いた対話制御を行います。これは意図解釈の結果より、シナリオ作成者が記述する内容を優先させるためです。例外として、patternとしてワイルドカードのみが記述されたcategoryが存在する場合、シナリオ記述のマッチングと、意図解釈のマッチングとの両方でマッチしなかった後に、マッチ処理を行います。子要素のnluを定義した場合でも、pattern要素の内容を記載すると通常のパターン評価が行われます。nlu要素の属性は、:ref:`nlu<pattern_nlu>` を参照してください。

以下の例では、意図解釈エンジンの処理結果が"レストラン検索"だった場合、patternにマッチし、意図解釈エンジンのインテントリスト、スロットリストを返すサンプルです。

.. code:: xml

    <aiml version="2.0">
        <category>
            <pattern>
                <nlu intent="レストラン検索"/>
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
                        <!-- score:<nluslot name="*" item="score"><index><get var="count" /></index></nluslot> -->
                        <think>
                            <set var="count"><map name="upcount"><get var="count" /></map></set>
                        </think>
                        <loop/>
                    </li>
                </condition>
            </template>
        </category>
    </aiml>
