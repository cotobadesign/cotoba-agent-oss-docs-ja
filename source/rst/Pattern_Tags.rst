=========================
pattern要素
=========================

| 本章ではpattern要素の内容として記述可能な子要素について説明します。

| cAIMLでサポートしているパターンマッチング要素の一覧は以下のとおりです。
| 公式の AIML 2.x仕様に対し、独自の要素セットを追加しています。


-  `bot <#bot>`__
-  `iset <#iset>`__
-  `nlu <#nlu>`__
-  `oneormore <#oneormore>`__
-  `priority <#priority>`__
-  `regex <#regex>`__
-  `set <#set>`__
-  `topic <#topic>`__
-  `that <#that>`__
-  `word <#word>`__
-  `zeroormore <#zeroormore>`__

| ここで説明する要素の大半は、属性の指定と、内容としての子要素の記述により対話エンジンがアクセス可能な各種のデータを利用できます。
| pattern要素の記述によるパターンマッチ方法の詳細については :doc:`パターンマッチング <AIML-Pattern-Matching>` を参照してください。
| 各要素の先頭の[...]は、対象の要素が最初に定義されたAIMLのバージョンを示しています。

.. _pattern_bot:

bot
---------
[1.0]

| bot要素は、カスタムボットプロパティを呼び出すために使用されます。 これらの変数には、ボットにアクセスするすべてのユーザがアクセスできます。
| カスタムボットプロパティは、properties.txtファイルで設定します。

* 属性

.. list-table::
    :widths: 20 20 5 55
    :header-rows: 1

    *
      + パラメータ
      + タイプ
      + 必須
      + 説明
    *
      + name
      + 文字列
      + yes
      + ボットプロパティ名を指定します。

* 使用例

以下の使用例ではボットの名称を返します。(properties.txt で 'name:ボット' と設定している前提)

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>あなたは<bot name="name" />ですか？</pattern>
            <template>私の名前は<bot name="name" />です。</template>
        </category>
    </aiml>

| Input: あなたはボットですか？
| Output: 私の名前はボットです。

関連項目: :ref:`ファイル管理：properties<storage_entity>`

iset
----------
[1.0]

iset要素は、setsファイルを使用せず、少数のマッチ対象単語を記述する場合に使用します。

* 属性

.. list-table::
    :widths: 20 20 5 55
    :header-rows: 1

    *
      + パラメータ
      + タイプ
      + 必須
      + 説明
    *
      + words
      + 文字列
      + yes
      + カンマ区切りでマッチ対象単語を記載します。

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>私は<iset words="東京, 神奈川, 千葉, 群馬, 埼玉, 栃木" />に住んでいます。</pattern>
            <template>
                私も関東に住んでいます。
            </template>
        </category>
    </aiml>


| Input: 私は東京に住んでいます。
| Output: 私も関東に住んでいます。

関連項目: `set <#set>`__

.. _pattarn_nlu :

nlu
----------
[custom]

| nlu要素は、高度意図解釈エンジンによるユーザ発話文の意図解釈結果を用いて対話処理を行う場合に使用します。
| 属性、子要素の設定方法、高度意図解釈エンジンの利用方法などの詳細は、:doc:`NLU <NLU>` を参照してください。

* 属性

.. list-table::
    :widths: 20 20 5 55
    :header-rows: 1

    *
      + パラメータ
      + タイプ
      + 必須
      + 説明
    *
      + intent
      + 文字列
      + yes
      + マッチさせるインテント名を指定します。
    *
      + scoreGt
      + 文字列
      + No
      + マッチさせる信頼度を指定します。対象インテントの信頼度が指定した値より大きい場合にマッチします。
    *
      + scoreGe
      + 文字列
      + No
      + マッチさせる信頼度を指定します。対象インテントの信頼度が指定した値以上の場合にマッチします。
    *
      + score
      + 文字列
      + No
      + マッチさせる信頼度を指定します。対象インテントの信頼度が指定した値の時にマッチします。
    *
      + scoreLe
      + 文字列
      + No
      + マッチさせる信頼度を指定します。対象インテントの信頼度が指定した値以下の場合にマッチします。
    *
      + scoreLt
      + 文字列
      + No
      + マッチさせる信頼度を指定します。対象インテントの信頼度が指定した値より小さい場合にマッチします。
    *
      + maxLikelihood
      + 文字列
      + No
      + ``true`` 、 ``false`` を指定します。対象インテントの信頼度が最大尤度かどうかを指定します。 ``true`` の場合、対象インテントが最尤時のみマッチします。 ``false`` 場合、対象インテントがの信頼度が最尤候補でなくてもマッチします。未指定時は ``true`` として処理します。

| 指定可能なインテント名は、高度意図解釈エンジンが使用している意図解釈モデルを作成する学習データ内に記述したインテント名の範囲内に限定されます。
| scoreXxは1つしか指定できません。複数記載した場合、scoreGt、scoreGe、score、scoreLe、scoreLtの順で採用します。(scoreGtとscoreが記載されていると、scoreGtが採用されます。)

* 使用例

意図解釈モデルで周辺検索のインテント名が 'aroundsearch' と設定されている場合の例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>
                <nlu intent="aroundsearch" />
            </pattern>
            <template>
                周辺検索を行います。
            </template>
        </category>
    </aiml>


| Input: この周辺を探して。
| Output: 周辺検索を行います。

関連項目: :doc:`NLU <NLU>`

oneormore
---------------
[1.0]

この要素は、ワイルドカードの1つで、少なくとも1個の任意の単語とマッチします。
このワイルドカードが pattern要素内の記述の最後にある場合は、ユーザの発話文の終端までマッチ処理を行います。
また、このワイルドカードが pattern要素内の記述の他のAIMLパターンマッチング要素の間にある場合は、ワイルドカードの次のパターンマッチング要素のマッチ処理が行われるまでマッチ処理を行います。

| このワイルドカードによるマッチ処理と他のAIMLパターンマッチング要素のマッチ処理の間にはマッチ処理が適用される優先順位があります。
| ワイルドカード "_"は、 AIMLパターンマッチング要素 ``set``、``iset``、``regex``、``bot`` よりも先にマッチ処理が行われます。ワイルドカード "*"は、これらのAIMLパターンマッチング要素よりも後にマッチ処理が行われます。
| パターンマッチング処理の詳細は、 :doc:`パターンマッチング <AIML-Pattern-Matching>` を参照してください。

次の2つの使用例では、「こんにちは」1単語とその後に続く1個以上の単語とのマッチを評価します。

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>こんにちは _</pattern>
            <template>
                こんにちは
            </template>
       </category>
    </aiml>

| Input: こんにちは いい天気ですね
| Output: こんにちは

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
       <category>
           <pattern>こんにちは *</pattern>
           <template>
               ご機嫌いかがですか？
           </template>
       </category>
    </aiml>

| Input: こんにちは いい天気ですね
| Output: ご機嫌いかがですか？

関連項目: `zeroormore <#zeroormore>`__ 、 :doc:`パターンマッチング <AIML-Pattern-Matching>`

priority
--------------
[1.0]

| ``ワイルドカード``, ``set``、``iset``、``regex``、``bot`` 等にマッピングした単語がマッチするようにパターンを記述します。
| AIMLでは、pattern要素内のマッチ対象単語の先頭に "$" を記述することで、他のAIMLパターンマッチング要素のマッチ処理よりも、当該単語のマッチ処理を優先します。
| 以下の使用例では、"こんにちは * "、"こんにちは * ありがとう"等のワイルドカードを含む pattern要素が記述されている対話ルールがあっても、"$" を記述した単語 "今日" を優先してマッチします。

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>こんにちは $今日もいい天気ですね</pattern>
            <template>
                そうですね
            </template>
        </category>

        <category>
            <pattern>こんにちは *</pattern>
	        <template>
	            こんにちは
	        </template>
	    </category>

	    <category>
	        <pattern>こんにちは * ありがとう</pattern>
	        <template>
	            どういたしまして
	        </template>
	    </category>
    </aiml>

| Input: こんにちは 今日もいい天気ですね
| Output: そうですね
| Input: こんにちは 今日の天気はいまいちですね
| Output: こんにちは
| Input: こんにちは 今日は ありがとう
| Output: どういたしまして

関連項目: `word <#word>`__、 :doc:`パターンマッチング <AIML-Pattern-Matching>`

.. _pattern_regex:

regex
-----------------
[custom]

regex要素の使用によりユーザ発話文に対する正規表現によるパターンマッチングができます。
単語単位の正規表現への対応、文字列に関する正規表現にも対応します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "pattern","文字列","No","正規表現で単語を記述"
    "template","文字列","No","regex.txtファイルで定義した単語単位の正規表現を利用"
    "form","文字列","No","複数の単語を含めた文字列を対象とした正規表現を記述"

regex要素を記述する際には、pattern,template,formのいずれかの属性を指定することが必須になります。

* 使用例

| regex要素の属性は、３つの方法で正規表現を指定します。
| 1つ目は、patternに直接正規表現を記述する方法(単語単位)です。
| 「こんにちは」「こんにちわ」はいずれも分かち書き処理によって1単語として扱われる文字列なので、次の使用例でマッチします。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern><regex pattern="こんにち[は|わ]" /></pattern>
            <template>
                こんにちは
            </template>
        </category>
    </aiml>

| ２つ目は、templateを利用する方法です。テンプレートファイルに対応するテンプレート名を指定します。
| テンプレートファイルは、'テンプレート名.txt'で定義します。テンプレートファイルに記述した正規表現は複数の対話ルールで参照することができます。
| templateの記述内容は、patternと同じく単語単位です。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern><regex template="konnichiwa" /></pattern>
            <template>
                こんにちは
            </template>
        </category>
    </aiml>

| 3つ目は、formを利用する方法です。文字列を対象とした正規表現を指定します。
| マッチ対象のユーザ発話文を文字列として扱うため、発話文が分かち書き処理によって1単語として扱われることはありません。
| 日本語文字列が分かち書き処理によりどのような単語に分割されるかに左右されずに正規表現でマッチするための機能です。
| formに指定する正規表現は、以下の組み合わせになります。

.. csv-table::
    :header: "表記","意味"
    :widths: 10,70

    "'[...]'","'[...]'内のいずれかの文字にマッチすればOK。"
    "'A|B'","'|'の左右の文字列のいずれかにマッチすればOK。"
    "'(X)'","正規表現Xのサブパターン化。XにマッチすればOK。"
    "'()?'","'?'の直前のサブパターンにマッチしてもしなくてもOK。"

次の使用例では、以下の文のいずれにもマッチします。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>
                <regex form="今[はわ]何時(ですか|です)?" />
            </pattern>
            <template>
                <date format="%H時%M分%S秒" />
            </template>
        </category>
    </aiml>

「今は何時」「今は何時ですか」「今は何時です」
「今わ何時」「今わ何時ですか」「今わ何時です」

関連項目: :ref:`ファイル管理：regex_templates<storage_entity>`


.. _pattern_set:

set
---------
[1.0]

| 単語セットとのマッチ処理を指定します。
| マッチ対象の単語は、setsファイルにリスト形式で列記し、configで指定したディレクトリ配下に格納します。
| setの属性nameには、setsファイルのファイル名から拡張子を除いた文字列を設定します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","文字列","Yes","setsファイル名から拡張子を除いた文字列"

* 使用例

以下の例では、prefecture.txtに日本の都道府県名が記載されていることを想定しています。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>私は<set name="prefecture" />に住んでいます。</pattern>
            <template>
                私は東京に住んでいます。
            </template>
        </category>
   </aiml>

| Input: 私は千葉に住んでいます。
| Output: 私は東京に住んでいます。

関連項目: `iset <#iset>`__ 、 :ref:`ファイル管理：sets<storage_entity>`

.. _pattern_topic:

topic
----------
[1.0]

topic要素を使用すると、システムの予約変数topicの値がnameに指定した値と一致することを条件に追加することができます。
topicは、次のようにtemplate要素のsetを用いて値を指定することができます。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern><!-- pattern description goes here --></pattern>
            <template>
                <think><set name="topic">FISHING</set></think>
		<!-- response sentence goes here-->
            </template>
        </category>
    </aiml>

topic要素で指定した条件は、patternのマッチ処理よりも優先して評価されます。例えば、現在設定されているtopicの値による条件分岐でpatternのマッチ処理を場合分けして、応答文を変化させることができます。

以下の使用例では、"なぜそれを知っていますか？"というユーザ発話に対して、それより前の対話でtopicの値が"FISHING"か"COOKING"のどちらに設定されているかで、応答文が変ります。

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>なぜそれを知っていますか？</pattern>
            <topic>FISHING</topic>
            <template>
                子供の頃、父が教えてくれました。
            </template>
        </category>

        <category>
            <pattern>なぜそれを知っていますか？</pattern>
            <topic>COOKING</topic>
            <template>
                子供の頃、母が教えてくれました。
            </template>
        </category>
    </aiml>

関連項目: `that <#that>`__, :ref:`set(template要素)<template_set>`, :ref:`think<template_think>`

.. _pattern_that:

that
----------
[1.0]

that要素を用いることで、1つ前の対話におけるシステムの応答文が指定した文字列とマッチすることを条件に追加することができます。
category要素内にpattern要素とthat要素が含まれている場合、システムの1つ前の応答の最後の応答文がthat要素で指定した文字列と一致する場合にのみ、pattern要素のマッチ処理が行われます。
that要素のこの働きにより、対話内容の流れを考慮した対話ルール記述が可能になります。
例えば、"はい"、"いいえ" といった、汎用的なユーザ発話文に対するシステム応答文を、直前の対話内容によって場合分けすることができます。


* 使用例

以下の使用例では、直前の対話のシステム応答文が「コーヒーに砂糖とミルクを入れますか」か「紅茶にレモンを入れますか」かで、ユーザ発話文の「はい」、「いいえ」にマッチする対話ルールを場合分けして、直前の対話内容に整合するシステム応答文が返されるようにしています。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>私はコーヒーが好きです</pattern>
            <template>コーヒーに砂糖とミルクを入れますか</template>
        </category>

	    <category>
            <pattern>私は紅茶が好きです</pattern>
            <template>紅茶にレモンを入れますか</template>
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

        <category>
            <pattern>はい</pattern>
            <that>紅茶にレモンを入れますか</that>
            <template>わかりました</template>
        </category>

        <category>
            <pattern>いいえ</pattern>
            <that>紅茶にレモンを入れますか</that>
            <template>ストレートティーですね</template>
        </category>
    </aiml>

関連項目: `topic <#topic>`__

word
----------
[1.0]

AIMLの最も基本的なパターンマッチング要素です。
word要素は、単語(分かち書きされた各文字列の単位)を表しており、対話エンジン内部で利用する要素でシナリオでの記述はできません。
英単語においては大文字と小文字を区別せずマッチ処理を行います。
また、全角文字・半角文字については、英数字は半角、カナ文字は全角でマッチ処理を行います。

以下の使用例ではHELLO, hello, Hello, HeLlOのどれでもマッチします。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>HELLO</pattern>
            <template>
                こんにちは
            </template>
        </category>
    </aiml>

関連項目: `priority <#priority>`__

zeroormore
----------------
[1.0]

この要素は、ワイルドカードの1つで、少なくとも0個の任意の単語とマッチします。
このワイルドカードが pattern要素内の記述の最後にある場合は、ユーザの発話文の終端までマッチ処理を行います。
また、このワイルドカードが pattern要素内の記述の他のAIMLパターンマッチング要素の間にある場合は、ワイルドカードの次のパターンマッチング要素のマッチ処理が行われるまでマッチ処理を行います。

| このワイルドカードによるマッチ処理と他のAIMLパターンマッチング要素のマッチ処理の間にはマッチ処理が適用される優先順位があります。
| ワイルドカード "^"は、 AIMLパターンマッチング要素 ``set``、``iset``、``regex``、``bot`` よりも先にマッチ処理が行われます。ワイルドカード "#"は、これらのAIMLパターンマッチング要素よりも後にマッチ処理が行われます。


次の使用例では、「こんにちは」のみ、あるいは、「こんにちは」で始まり1つ以上の単語が続く文にマッチします。

詳細は、:doc:`パターンマッチング <AIML-Pattern-Matching>` を参照してください。

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>こんにちは ^</pattern>
            <template>
                こんにちは
            </template>
        </category>

        <category>
            <pattern>こんにちは #</pattern>
            <template>
                ご機嫌いかがですか？
            </template>
        </category>
    </aiml>

関連項目: `oneormore <#oneormore>`__ 、 :doc:`パターンマッチング <AIML-Pattern-Matching>`
