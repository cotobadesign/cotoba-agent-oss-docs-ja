=====================
COTOBA AIML: 基本要素
=====================

| この章では、COTOBA AIML (cAIML) でサポートしているAIMLの基本となる要素について説明します。
| セクション冒頭の[...]は、その要素が最初に定義されたAIMLのバージョンを示しています。
| [custom]は、cAIML 独自仕様拡張の要素です。

cAIMLのパターンマッチング優先順位ルールの詳細については「:doc:`パターンマッチング <./rst/AIML-Pattern-Matching>`」を参照してください。

-  `aiml <#aiml>`__
-  `category <#category>`__
-  `pattern <#pattern>`__
-  `template <#template>`__
-  `topic <#topic>`__

aiml
=====================

[1.0]

| aiml要素は、cAIMLのルート要素です。他の全てのcAIMLの要素はaiml要素の内容として記述する必要があります。

* 属性

.. list-table::
    :widths: 20 20 5 55
    :header-rows: 1

    *
      + パタメータ
      + タイプ
      + 必須
      + 説明

    *
      + version
      + string
      + Yes
      + 記述されているAIMLのバージョンを指定します。


* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <!-- cAIML element should be described here -->
    </aiml>


category
=====================

[1.0]

| category要素は、cAIMLの対話ルールの基本単位に相当します。
| category要素の内容として、ユーザの発話文とのマッチングパターンを指定する `pattern <#pattern>`__ 要素とシステムの応答文を指定する `template <#template>`__ 要素を記述して1つの対話ルールを構成します。
| aiml要素の内容に、複数のcategory要素のブロックを含んだ形で記述することができます。
| `aiml <#aiml>`__ 要素と `topic <#topic>`__ 要素を除く、すべてのcAIMLの要素は、category要素のブロック内に含まれている必要があります。

尚、登録可能なcategory数は、コンフィグレーション定義の :ref:`制限値定義<config_bot_max>` の ``max_categories`` までとなります。

* 属性

なし

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>こんにちは</pattern>
            <template>今日もいい天気ですね</template>
        </category>
	    <category>
	        <pattern>さようなら</pattern>
	        <template>明日また会いましょう</template>
	    </category>
    </aiml>

| Input: こんにちは
| Output: 今日もいい天気ですね
| Input: さようなら
| Output: 明日また会いましょう

関連項目: `aiml <#aiml>`__, `topic <#topic>`__ , :doc:`pattern <./rst/Pattern_Tags>`, :doc:`template <./rst/Template_Tags>`

pattern
=====================

[1.0]

| pattern要素は、category要素の内容として記述し、pattern要素の内容としてユーザの発話文とのパターンマッチングを行うパターンを記述します。
| pattern要素内に記述された文字列とユーザ発話文の文字列が一致した場合、当該のpattern要素を含む対話ルール(category要素のブロック内の処理)が実行されます。

* 属性

なし

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>こんにちは</pattern>
            <template>今日もいい天気ですね。</template>
        </category>
    </aiml>

| pattern要素の内容には文字列以外のcAIML要素を含む記述を行うこともできます。
| それにより、複雑なパターンマッチ処理を行うことができます。
| pattern要素の内容として記述可能なcAIML要素の詳細については、:doc:`pattern要素 <./rst/Pattern_Tags>` を御覧ください。

template
=====================

[1.0]

| template要素は、category要素の内容として記述し、template要素の内容としてシステムの応答文を記述します。
| 対話ルール(category要素のブロック)が実行された場合、当該category要素のブロック内のtemplate要素の内容に記述された文字列が、システムの応答文として返されます。

* 属性

なし

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>こんにちは</pattern>
            <template>今日もいい天気ですね。</template>
        </category>
    </aiml>

| template要素の内容には文字列以外のcAIMLタグを含む記述を行うことができます。
| それにより、複雑な応答文生成処理を行うことができます。
| template要素の内容に記述可能なcAIML要素の詳細については、:doc:`template要素 <./rst/Template_Tags>` を御覧ください。

topic
=====================

[1.0]

| topic要素のブロック内に、複数の対話ルール `category <#category>`__ 要素を記述することで対話ルールをコンテキスト化することができます。
| 対話ルールがコンテキスト化されると、対話エンジンが保持する変数 topic の値が、topic要素のname属性で指定した属性値と一致する時だけ、対話ルールが評価されます。
| topic要素のブロック内に含まれない対話ルール(コンテキスト化されない対話ルール) `category <#category>`__ 要素は、name属性の属性値がワイルドカード"*"と指定されているのと同じ扱いになり、対話エンジンが保持する変数 topic の値に関係なくその対話ルールは評価されます。
| ただし、topic要素でコンテキスト化された対話ルールが優先して評価され、そのコンテキスト化された対話ルールが実行されなかった場合にのみ、コンテキスト化されない対話ルールが評価されます。

"topic"は予約語となるためユーザが定義する変数名としては利用できません。

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
      + string
      + Yes
      + topic名を指定します。

| topic要素を利用すると、以下の使用例のように同じ `pattern <#./rst/Pattern_Tags>`__ のマッチング動作をコンテキスト(topicの値)に応じて使い分けることができます。
| この使用例では、ユーザの「私は何も入れません」という発話に対して、その発話より前に設定されたtopicの値に応じて、評価される対話ルールを切り替えることで応答を変えています。

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>*について話しましょう</pattern>
            <template>
                私も<set name="topic"><star /></set>が好きです。
            </template>
        </category>

        <topic name="コーヒー">
            <category>
                <pattern>私は何も入れません</pattern>
                <template>私はクリームと砂糖を入れます</template>
            </category>
        </topic>

        <topic name="紅茶">
            <category>
                <pattern>私は何も入れません</pattern>
                <template>私はレモンティーが好きです</template>
            </category>
        </topic>
    </aiml>


| Input: コーヒーについて話しましょう
| Output: 私もコーヒーが好きです
| Input: 私は何も入れません
| Output: 私はクリームと砂糖を入れます
| Input: 紅茶について話しましょう
| Output: 私も紅茶が好きです
| Input: 私は何も入れません
| Output: 私はレモンティーが好きです

関連項目: :ref:`that<pattern_that>`, :ref:`set<template_set>`, :ref:`think<template_think>`
