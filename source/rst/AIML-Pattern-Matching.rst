.. _aiml_pattern_matching:

パターンマッチング
=======================

入力文とpattern要素とのマッチングは、それぞれに対してコード変換を行った上で実施します。
マッチングで使用する文字は、英数記号は半角文字、カタカナは全角文字になります。

入力文に対しては、コンフィグレーション定義によりマッチングで使用しない文字を除去する制御も行います。
コンフィグレーション定義は、:ref:`tokenizer定義<config_tokenizer>` のpunctuation_charsで指定します。

AIMLのパターンマッチングでは、ユーザ入力に対してワイルドカードを利用してマッチングを行うことができます。
ワイルドカードの指定には ``*``, ``^`` , ``_``, ``#`` があり、各々利用方法が異なります。

ただし、ワイルドカードを連続して記述したpattern要素を指定する場合には、:ref:`oneormore<pattern_oneormore>` と
:ref:`zerooremore<pattern_zeroormore>` の混合は避けてください。誤動作が発生する場合があります。

``*`` ワイルドカード
-------------------------

``*`` を用いることで、ユーザ入力の単語を抽出することができ、1つ以上の合致を判定します。（oneormoreワイルドカード）

.. code:: xml

    <pattern>こんにちは *</pattern>

この例では、

::

    こんにちは！
    こんにちは山田さん
    こんにちは誰ですか？

にマッチします。
しかし、この例では"こんにちは"に続く単語を期待しているため、"こんにちは"という入力のみにはマッチしません。


``^`` ワイルドカード
-------------------------
``^`` を用いることで、0以上の単語マッチングを指定できます。（zeroormoreワイルドカード）
すなわち、ワイルドカードに相当する単語が無くても、指定した単語の表記のみがマッチングした場合でも評価結果が成り立ちます。

.. code:: xml

    <pattern>こんにちは ^</pattern>

この例では、``*`` と異なり、

::

    こんにちは
    こんにちは！
    こんにちは山田さん
    こんにちは誰ですか？

と、"こんにちは"単体入力にもマッチします。

``_`` と ``#`` ワイルドカード
-------------------------------------
``*`` と ``^`` は、単語のマッチングより優先順位が低い評価を行います。
すなわち、

.. code:: xml

    <pattern>こんにちは いい天気ですね</pattern>
    <pattern>こんにちは ^</pattern>
    <pattern>こんにちは *</pattern>

という定義がされていた場合、"こんにちは いい天気ですね"という入力の場合一番上のpatternにマッチします。

これに対し、 ``_`` と ``#`` は単語のマッチングより高い優先順位で評価されます。
``_`` は、 ``*`` 同様、1以上のマッチングで合致判定を行います。（oneormoreワイルドカード）
``#`` は、 ``^`` 同様、0以上のマッチングで合致判定を行います。（zeroormoreワイルドカード）

.. code:: xml

    <pattern>こんにちは いい天気ですね</pattern>
    <pattern>こんにちは _</pattern>
    <pattern>こんにちは #</pattern>

という定義がされていた場合、"こんにちは いい天気ですね"という入力の場合 ``#`` にマッチします。


優先単語指定
-------------------------
``$`` 指定した単語は、 ``_`` , ``#`` より優先して評価されます。

.. code:: xml

    <pattern>こんにちは $いい天気ですね</pattern>
    <pattern>こんにちは _</pattern>
    <pattern>こんにちは #</pattern>

という定義がされていた場合、"こんにちは いい天気ですね"という入力の場合一番上のpatternにマッチします。


判定優先順位
-------------------------
patternでのワイルドカード指定時の優先順位は以下のようになります。


-  ``$(単語)``
-  ``#`` ( 0以上 )
-  ``_`` ( 1以上 )
-  ``(単語)``
-  ``^`` ( 0以上 )
-  ``*`` ( 1以上 )
