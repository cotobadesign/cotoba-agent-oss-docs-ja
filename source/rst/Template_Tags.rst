==================================================
template要素内の子要素
==================================================

| 本章ではtemplate要素内で記述可能な子要素について説明します。
| 子要素の使用可否については、カスタマイズが可能です。詳細は :doc:`カスタムtemplate要素<node/Custom-Template-Nodes>` を参照してください。

| 対話プラットフォームでサポートしているtemplateの要素内に記述可能な子要素のリストは以下のとおりです。
| 公式の AIML 2.x仕様に対し、独自の要素セットを追加しています。

-  `addtriple <#addtriple>`__: RDFナレッジベースへのエレメント追加要素
-  `authorise <#authorise>`__: ユーザロールによりAIML要素の実行権の切り替え要素
-  `bot <#bot>`__: bot固有のプロパティの取得要素
-  `button <#button>`__: ボタン押下を促す要素
-  `card <#card>`__: 画像、ボタン、タイトル、サブタイトルを1つにまとめる要素
-  `carousel <#carousel>`__: カード要素をまとめる要素
-  `condition <#condition>`__: 条件分岐して処理を行う要素

   -  :ref:`単一判定<condition_type1>`

   -  :ref:`条件分岐<condition_type2>`

   -  :ref:`複数条件での分岐 <condition_type3>`

   -  :ref:`ループ処理 <condition_looping>`

-  `date <#date>`__: 日付、時刻取得要素
-  `delay <#delay>`__: 遅延要求設定要素
-  `deletetriple <#deletetriple>`__: RDFナレッジベースからのエレメント削除要素
-  `denormalize <#denormalize>`__: 単語から文字列への変換要素
-  `eval <#eval>`__: 変数値の文字列化要素(learn/learnfで利用）
-  `explode <#explode>`__: 文字分割要素
-  `first <#first>`__: 先頭単語取得要素
-  `extension <#extension>`__：拡張機能要素
-  `formal <#formal>`__: 先頭文字大文字化要素
-  `gender <#gender>`__: 三人称代名詞の性別変換要素
-  `get <#get>`__: 変数内容取得要素
-  `id <#id>`__: クライアント名取得要素
-  `image <#image>`__: 画像情報指定要素
-  `implode <#implode>`__: 文字列結合要素
-  `input <#input>`__: pattern文取得要素
-  `interval <#interval>`__: 時刻差分計算要素
-  `json <#json>`__: JSON要素
-  `learn <#learn>`__: ユーザ固有category登録要素
-  `learnf <#learnf>`__: ユーザ固有category登録要素
-  `li <#li>`__: 分岐条件記述要素
-  `link <#link>`__: URL情報指定要素
-  `list <#list>`__: リスト形式情報指定要素
-  `log <#log>`__: ログ出力要素
-  `lowercase <#lowercase>`__: 英文字小文字化要素
-  `map <#map>`__: 辞書利用の変換要素
-  `nluintent <#nluintent>`__: NLUインテント取得要素
-  `nluslot <#nluslot>`__: NLUスロット取得要素
-  `normalize <#normalize>`__: 文字列から単語への変換要素
-  `oob <#oob>`__: Out of Band要素
-  `olist <#olist>`__: オーダーリスト形式指定要素
-  `person <#person>`__: 一・二人称代名詞変換要素
-  `person2 <#person2>`__: 一・二人称代名詞と三人称代名詞の変換要素
-  `program <#program>`__: プログラムバージョン取得要素
-  `random <#random>`__: ランダム分岐指定要素
-  `reply <#reply>`__: ユーザへの提示内容指定要素
-  `request <#request>`__: 入力履歴取得要素
-  `resetlearn <#resetlearn>`__: learn情報リセット要素
-  `resetlearnf <#resetlearnf>`__: learnf情報リセット要素
-  `response <#response>`__: 出力履歴取得要素
-  `rest <#rest>`__: 先頭単語以外の取得要素
-  `set <#set>`__: 変数設定要素
-  `select <#select>`__: RDFナレッジベースの検索要素
-  `sentence <#sentence>`__: 英文整形要素
-  `size <#size>`__: カテゴリ数取得要素
-  `space <#space>`__: 半角スペース挿入要素
-  `split <#split>`__: 文章分割要素
-  `sr <#sr>`__: sraiとstarの省略要素
-  `srai <#srai>`__: パターンマッチ再実行要素
-  `sraix <#sraix>`__: REST API呼び出し要素
-  `star <#star>`__: ワイルドカード取得要素
-  `system <#system>`__: システムコール実行要素
-  `that <#that>`__: 過去応答文取得要素
-  `thatstar <#thatstar>`__: 過去応答文内ワイルドカード取得要素
-  `think <#think>`__: 内部処理記述要素
-  `topicstar <#topicstar>`__: topic内ワイルドカード取得要素
-  `uniq <#uniq>`__: RDFナレッジベースの検索要素
-  `uppercase <#uppercase>`__: 英文字大文字化要素
-  `video <#video>`__: ビデオ情報指定要素
-  `vocabulary <#vocabulary>`__: シナリオ単語数取得要素
-  `word <#word>`__: 単語ノードを示す定義
-  `xml <#xml>`__: 未定義XMLノードの定義

| 尚、templateの子要素は、配下に上記の子要素を記述することで複合的に各種の処理を行うことができます。（一部、配下に子要素を指定できないものもあります。）
| 本章の各子要素の説明で記述する指定可能な子要素の項目は、親となる子要素の配下で特有の処理を行うもののみを記述しています。

以下の要素は処理結果が空文字の為、単体で使用した場合にはアンマッチと同じ結果になります。他の要素や任意の文字列と併せて使用することを推奨します。

-  addtriple
-  deletetriple
-  json　（取得の場合を除く）
-  learn
-  learnf
-  resetlearn
-  resetlearnf
-  think

詳細
============
| このセクションでは、AIMLのtemplate要素内に記述する要素の説明を行います。
| ほとんどの要素はXMLの属性または子要素を追加して、データを利用します。
| 各要素の先頭の[...]は、対象の要素が最初に定義されたAIMLのバージョンを示しています。


.. _template_addtriple:

addtriple 
---------------
[2.0]

addtriple要素は、RDFナレッジベースにエレメント(知識)を追加します。
エレメントの構成要素には、subject (主語)、predicate (述語)、object (目的語)の3つのアイテムがあります。

| addtriple要素の結果文字列は、常に空文字になります。
| addtriple要素の詳細については、:doc:`RDFサポート<RDF_Support>` を参照してください。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "subj","string","Yes","subject (主語)の値"
    "pred","string","Yes","predicate (述語)の値"
    "obj","string","Yes","object (目的語)の値"

| 指定された値は検索を効率的に行う為、統一コード（英数記号：半角、カタカナ：全角）に変換して格納されます。
| 値に空文字（空白のみを含む）を指定した場合、実行時に例外が発生します。（シナリオ展開時に検出した場合、該当シナリオは無効になります。）

以下の使用例では、ユーザの発話文「私はカレーが好きだ」に対して、subject='私'、pred='好き', object='カレー' のアイテムで構成されるエレメント(知識)をRDFナレッジベースに登録します。

* 使用例

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>* は * が 好き #</pattern>
            <template>
                <addtriple>
                    <subj><star /></subj>
                    <pred>好き</pred>
                    <obj><star index="2"/></obj>
                </addtriple>
                好みを登録しました
            </template>
        </category>
    </aiml>

| Input: 私 は カレー が好きだ
| Output: 好みを登録しました

登録を行った結果の確認方法は、`uniq <#uniq>`__, `select <#select>`__ を参照してください。

関連項目: `deletetriple <#deletetriple>`__, `select <#select>`__, `uniq <#uniq>`__, :doc:`RDFサポート<RDF_Support>`


.. _template_authorise:

authorise 
---------------
[1.0]

| authorise要素を使うことにより、template要素内に記述されるAIML要素を実行するかどうかを、ユーザの権限によって切り替えることができます。
| ユーザとしてauthorise要素のrole属性で指定された権限が無い場合、authorise要素内に記述したAIML要素は実行されません。
| 詳細は Securityの :ref:`承認 <security_authorisation>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "role","string","Yes","ロール名。:ref:`ユーザグループファイル <security_usergroups>` で規定"
    "denied_srai","string","No","認証失敗時のsrai用発話文"

:ref:`ユーザグループファイル<security_usergroups>` で規定されていない"ロール名"を指定した場合、全ユーザが権限なしとして扱われます。

* 使用例

この使用例では、ユーザの権限に"root"がある場合のみ、vocabularyの内容を返せます。
ユーザに権限がない場合、コンフィグレーションで定義された、denied_srai（又は、denied_text）が実行されます。

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>ボキャブラリリスト数</pattern>
            <template>
                <authorise role="root">
                    <vocabulary />
                </authorise>
            </template>
        </category>
    </aiml>

| また、denied_srai属性を指定することで、ユーザに権限が無い場合のデフォルト動作を決めることができます。
| （denied_sraiの値をpattern要素に定義したシナリオが必要になります。）

.. code:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <aiml version="2.0">
        <category>
            <pattern>ボキャブラリリスト数</pattern>
                <template>
                    <authorise role="root" denied_srai="ACCESS_DENIED">
                        <vocabulary />
                    </authorise>
                </template>
        </category>
    </aiml>

関連項目: :ref:`承認 <security_authorisation>`


.. _template_bot:

bot
---------
[1.0]

bot要素は、bot固有のプロパティを取得します。この要素は読み込み専用です。
プロパティは、:ref:`propertiesファイル<storage_file_properties>` で規定することで、任意の情報を固定値として登録することができます。

* 属性　（子要素での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","string","Yes","propertiesファイルで規定した、プロパティ名を指定"

| プロパティ名は変数名と同じ位置づけになる為、大文字・小文字、全角・半角を区別します。
| プロパティには、任意の固有値以外に、コンフィグレーション定義を置換するパラメータとしても使用します。

* 使用例

プロパティとして、name、birthdate、app_version、grammar_versionが登録されている前提です。

.. code:: xml

    <category>
       <pattern>あなたは誰？</pattern>
       <template>
           私の名前は<bot name="name" />です。
           <bot name="birthdate" />生まれです。
           アプリケーションバージョンは<bot name="app_version" />です。
           グラマーバージョンは<bot name="grammar_version" />です。
       </template>
   </category>

name属性に、登録されてないプロパティ名を指定した場合、シナリオ展開時に異常を検出し、該当シナリオは無効になります。

botの子要素としてnameを利用することで、name属性と同じ内容を記載することができます。

.. code:: xml

   <category>
       <pattern>あなたは誰ですか？</pattern>
       <template>
           私の名前は<bot><name>name</name></bot>です。
           <bot><name>birthdate</name></bot>生まれです。
           アプリケーションバージョンは<bot><name>app_version</name></bot>です。
           グラマーバージョンは<bot><name>grammar_version</name></bot>です。
       </template>
   </category>

子要素のnameに、登録されてないプロパティ名を指定した場合、実行時に取得失敗となり、コンフィグレーション定義の :ref:`default-property<config_defaults>` の定義値が返ります。
（default-propertyが定義されていない場合、default-getの定義値が返ります。）

| 尚、:ref:`properties_jsonエンティティ<storage_file_properties>` を利用して、プロパティの値にJSON形式を指定した場合、取得されるデータはファイルの内容そのものではなく、JSONデータとして変換された文字列が設定されます。
| ``properties_jsonエンティティ`` に、以下のJSONファイルを 'test_json.json' として配置した場合、

.. code:: json

   {
     "key1": "value1",
     "key2":
       {
         "key2_1": "value2_1"
       }
   }

``<bot name="test_json" />`` で取得される値は、以下に様に編集された文字列になります。

.. code:: json

   {"key1": "value1", "key2": {"key2_1": "value2_1"}}

関連項目: :ref:`ファイル管理：properties<storage_entity>` 


button 
------------
[2.1]

button要素は、会話中にユーザにタップを促す用途で利用されるリッチメディア要素です。 
子要素として、buttonの表記に使用するテキスト、Botに対するpostback、ボタン押下時のURLを記載できます。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素　（属性での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "text","string","Yes","ボタンに表示するテキストを記載します。（空文字を許容します。）"
    "postback","string","No","ボタン押下時の動作を記載します。ユーザにはこのメッセージは見せず、Botに対するレスポンスやアプリケーションで処理を行う場合に利用します。"
    "url","string","No","ボタン押下時のURLを記載します。"

* 使用例

.. code:: xml

   <category>
       <pattern>乗り換え</pattern>
       <template>
            <button>
                <text>乗り換え検索しますか？</text>
                <postback>乗り換え案内</postback>
            </button>
       </template>
    </category>

   <category>
       <pattern>検索</pattern>
       <template>
            <button>
                <text>検索しますか？</text>
                <url>https://searchsite.com</url>
            </button> 
       </template>
    </category>


card 
----------
[2.1]

card要素はリッチメディア要素で、画像、ボタン、タイトル、サブタイトルなど、いくつかの他の要素を使用し1つのカードとします。
これらのリッチメディア要素すべてを含むメニューが表示されます。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素　（属性での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "title","string","Yes","カードのタイトルを記載します。（空文字を許容します。）"
    "subtitle","string","No","カードに対する追加情報を記載します。"
    "image","string","Yes","カード用の画像URL等を記載します。（空文字を許容します。）"
    "button","string","Yes","カード用のボタン情報を記載します。"

* 使用例

.. code:: xml

    <category>
        <pattern>検索</pattern>
        <template>
            <card>
                <title>カードメニュー</title>
                <subtitle>カードメニュー詳細情報</subtitle>
                <image>https://searchsite.com/image.png</image>
                <button>
                    <text>検索しますか？</text>
                    <url>https://searchsite.com</url>
                </button>
            </card>
        </template>
    </category>

関連項目: `button <#button>`__, `image <#image>`__


carousel 
--------------
[2.1]

carousel要素はリッチメディア要素で、カード要素を複数利用しタップスルーメニューを表示します。
これらのリッチメディア要素すべてを含むメニューが表示されます。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "card","string","Yes","複数のカードを指定します。一度にカードを1つ表示、タップスルーで別のカードを表示します。"

* 使用例

.. code:: xml

    <category>
        <pattern>レストラン検索</pattern>
        <template>
            <carousel>
                <card>
                    <title>イタリアン</title>
                    <subtitle>イタリア料理店の検索</subtitle>
                    <image>https://searchsite.com?q=italian</image>
                    <button>イタリアン検索</button>
                </card>
                <card>
                    <title>フレンチ</title>
                    <subtitle>フランス料理店の検索</subtitle>
                    <image>https://searchsite.com?q=french</image>
                    <button>フレンチ検索</button>
                </card>
            </carousel>
        </template>
    </category>

関連項目: `card <#card>`__, `button <#button>`__, `image <#image>`__


condition 
---------------
[1.0]

| condition要素は、template内での条件判断を行う際に使用し、switch-caseのような処理を記載できます。
| conditionの属性・子要素で指定した変数と判定値の組み合わせで条件判定を行い、``li`` 子要素を使用することで複数の条件を列記することができます。
| 対象変数には、get/setで定義したグローバル変数、ローカル変数とともに、Bot固有情報（プロパティ）も指定できます。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","変数名","No","分岐条件とするグローバル変数(name)名を指定します。"
    "var","変数名","No","分岐条件とするローカル変数(var)名を指定します。"
    "data","変数名","No","分岐条件とするグローバル変数(data)名を指定します。"
    "bot","プロパティ名","No","分岐条件とするBot固有情報（プロパティ）名を指定します。"
    "value","判定値","No","分岐条件となる値を指定します。（大文字・小文字、全角・半角を区別します）"
    "regex","判定値","No","分岐条件となる値を正規表現で指定します。判定は完全一致（大文字・小文字は同一視）で行います。"

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "li","string","No","指定した変数に対する分岐条件を記載します。"

　属性の各パラメータも子要素として指定できます。

condition要素には、変数と判定値の記載方法により、次の３つの処理方式があります。

  - 単一判定： ``li`` 子要素を使用せずに、１つの変数と判定値の組み合わせを指定します。
  - 条件分岐： condition として対象とする１つの変数を規定し、``li`` 子要素毎に判定値を指定することで、複数の条件に対応します。
  - 複数条件での分岐： ``li`` 子要素毎に変数と判定値の組み合わせを指定することで、複数の条件に対応します。

| この３つの処理方式とは異なる記載方法の場合には、シナリオ展開時に異常となり、該当シナリオは無効になります。
| （不正例：変数と判定値の組み合わせが成立しない場合や、'condition'要素に変数を指定した状態で、'li'要素にも変数を指定した場合等。）

以下に、各方式でのconditionの記載方法を説明します。


.. _condition_type1:

単一判定
~~~~~~~~~~~~~~~

| condition要素として、１つの変数と判定値の組み合わせを指定する方式です。
| この記載方法は、変数値の判定結果がtrueの場合に要素の文字列を返し、falseの場合は何も実行しません。
| 以下の使用例の様に4種類の記載方法があり、いずれも同じ動作を示しています。

* 使用例

.. code:: xml

   <condition name="ペット" value="犬">私も犬派です</condition>
   <condition name="ペット"><value>犬</value>私も犬派です</condition>
   <condition value="犬"><name>ペット</name>私も犬派です</condition>
   <condition><name>ペット</name><value>犬</value>私も犬派です</condition>

name変数：”ペット"の値が"犬"であった場合、"私も犬派です"を返します。


.. _condition_type2:

条件分岐
~~~~~~~~~~~~~~~~~~~~~~~~~~

| condition の属性・子要素で１つの変数を設定し、 ``li`` 子要素で対象となる値に対する分岐を記述します。分岐方法はswitch-caseに似ています。
| condition の変数の内容を ``li`` 子要素の判定値と比較し、trueになった条件の内容を返します。
| 分岐条件に合致しない場合、判定値が指定されていない ``li`` 子要素の内容を返します。判定値が指定されていない ``li`` 子要素がない場合は、何も返しません。

以下の使用例では、name変数：”ペット"の値を評価します。評価の優先順序は記載順になります。

* 使用例

.. code:: xml

   <condition name="ペット">
       <li value="犬">私も犬派です</li>
       <li value="猫">猫が一番好きです</li>
       <li>ペットは飼ってないです</li>
   </condition>

   <condition>
       <name>ペット</name>
       <li value="犬">私も犬派です</li>
       <li value="猫">猫が一番好きです</li>
       <li>ペットは飼ってないです</li>
   </condition>


.. _condition_type3:

複数条件での分岐
~~~~~~~~~~~~~~~~~~~~~~~~~

| ``li`` 子要素毎に条件分岐を指定する場合の記載方法で、分岐方法はif文の集合体に似ています。
| ``li`` 子要素で定義された各条件が順次チェックされます。各``li`` 子要素では異なる変数、判定値を持つことができます。
| 条件がtrueになるとその時点で評価が完了し、該当する ``li`` 子要素の内容を返します。

以下の使用例では、name変数：”ペット"と、name変数："飲み物"の値を評価します。評価の優先順序は記載順になります。

* 使用例

.. code:: xml

   <condition>
       <li name="ペット" value="犬">私も犬派です</li>
       <li value="猫"><name>ペット</name>猫が一番好きです</li>
       <li name="飲み物"><value>コーヒー</value>マンデリンがいいです</li>
       <li><name>飲み物</name><value>紅茶</value>アールグレイが好きです</li>
       <li>好きなものはありますか</li>
   </condition>


.. _condition_looping:

ループ処理
~~~~~~~~~~~~~~

| 条件によりcondition処理を繰り返す場合、``<loop />`` を ``li`` の子要素として記載します。
| 通常<li>で分岐した場合、処理内容を<template>として返しますが、'<loop />'がある場合、対象となる<li>に分岐し、<li>の処理を終えた後、<condition>の内容を再評価します。
| 尚、１つのcondition要素内で実施できるループ処理の回数は、コンフィグレーション定義の :ref:`制限値定義<config_bot_max>` の ``max_search_condition`` までとなります。
| 制限数を超えてループ処理を行った場合、処理例外が発生します。

以下の使用例では、変数"話題"を評価して返す内容を決定しますが、分岐条件に一致しなかった場合、"話題"に"雑談"を設定して<condition>の再評価を行い、"雑談"としてループを抜けます。

* 使用例

.. code:: xml

    <condition var="話題">
        <li value="花">花は何が好きですか</li>
        <li value="飲み物">コーヒーはどうですか</li>
        <li value="雑談">何かいいことありました？</li>
        <li><think><set var="話題">雑談</set></think><loop /></li>
    </condition>

関連項目: `li <#li>`__, `get <#get>`__, `set <#set>`__, `bot <#bot>`__


date 
----------
[1.0]

date要素は、日付と時刻の文字列を取得します。基準値はシステムの現在日時ですが、:ref:`対話API<coversation_api>` で、locale/timeが指定されている場合、返す内容は次の様に変化します。

- localeを指定：　国コードを元に、該当する地域の日時への換算を行います。（国コードに対して地域が確定できない場合は変換を行いません。）
- timeを指定：　指定された日時を基準値として処理します。

日付・時刻の出力形式の指定には、Pythonの日時文字列の書式を使用します。 
詳細は Pythonのマニュアル(`datetime <https://docs.python.jp/3.6/library/datetime.html>`__)を参照してください。


* 属性　（子要素での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "format","string","No","出力形式指定。未指定時は '%c'"

formatの指定が不正な場合には、結果にはformatで指定された文字列がそのまま返ります。

* 使用例

.. code:: xml

   <category>
       <pattern>今日は何日ですか</pattern>
       <template>
           今日は<date format="%Y/%m/%d" />です。
       </template>
   </category>

   <category>
       <pattern>今日は何日ですか</pattern>
       <template>
           今日は<date><format>%Y/%m/%d</format></date>です。
       </template>
   </category>

関連項目: `interval <#interval>`__


delay 
-----------
[2.1]

delay要素はリッチメディア要素で、遅延を行う要素です。
音声合成の再生中などの待ち時間の定義を行ったり、ユーザに対するBotの返答遅延を指定したりするために利用します。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素　（属性での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "seconds","string（数値）","Yes","遅延秒数を指定。"

secondsに数値以外を指定した場合、属性指定では該当シナリオが無効になり、子要素指定の場合は "0" が設定されます。

* 使用例

.. code:: xml

   <category>
       <pattern>* 秒待って</pattern>
       <template>
            <delay>
                <seconds><star/></seconds>
            </delay>
        </template>
    </category>


.. _template_deletetriple:

deletetriple
------------------
[2.0]

| deletetriple要素は、RDFナレッジベースからエレメントを削除します。
| 指定できるエレメントは起動時にRDFファイルから読み込まれたエレメント、もしくは `addtriple <#addtriple>`__ で追加されたエレメントです。
| deletetriple要素の結果文字列は、常に空文字になります。
| 詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "subj","string","Yes","削除対象のsubject (主語)の値"
    "pred","string","No","削除対象のpredicate (述語)の値"
    "obj","string","No","削除対象のobject (目的語)の値"

| 指定された値は統一コード（英数記号：半角、カタカナ：全角）に変換して、削除処理に使用します。
| obj指定を省略した場合、subjとpredが一致するものが削除され、pred/objの指定を省略した場合、subj以下の全てが削除されます。

* 使用例

.. code:: xml

   <category>
       <pattern>* は * を削除</pattern>
       <template>
           <deletetriple>
               <subj><star /></subj>
               <pred>は</pred>
               <obj><star index="2"/></obj>
           </deletetriple>
           削除しました
       </template>
   </category>

関連項目: `addtriple <#addtriple>`__, `select <#select>`__, `uniq <#uniq>`__, :doc:`RDFサポート<RDF_Support>` 


.. _template_denormalize:

denormalize 
-----------------
[1.0]

| normalize要素が対象文字列に含まれる記号や短縮形の文字列を単語に変換するのに対して、denormalize要素は逆の動作を行います。
| 変換内容は、:ref:`denormalファイル<storage_file_denormal>` で指定し、大文字・小文字、全角・半角を区別して変換を行います。
| 例えば、'www.***.com'に対して、normalizeで'.'を'dot'、'*'を'_'に変換した場合、'www dot _ _ _ dot com 'になりますが、
| denormalizeで'dot'を'.'、'_'を'*'に変換するように指定した場合、normalize/denormalizeで、'www.***.com'に復元されます。
| 英文の場合、normalizeの変換結果には必ず前後に空白が挿入されている為、denormalizeの指定では、前後に空白を挿入するか否かを含めて指定する必要があります。

* 使用例

.. code:: xml

    <category>
        <pattern>URLは * です。</pattern>
        <template>
            <think>
                <set var="url"><normalize><star /></normalize></set>
            </think>
            <denormalize><get var="url" /></denormalize>を復元します。
        </template>
    </category>

| Input: URLはwww.***.comです。
| Output: www.***.comを復元します。

<denormalize />は<denormalize><star /></denormalize>と同義です。

* 使用例

.. code:: xml

   <category>
       <pattern>URLは *です。</pattern>
       <template>
            <denormalize />に変換します。
       </template>
   </category>

| Input: URLは___です。
| Output: \***に変換します。

関連項目: :ref:`ファイル管理：denormal<storage_entity>`, `normalize <#normalize>`__


eval 
----------
[1.0]

eval要素は、`learn <#learn>`__、`learnf <#learnf>`__ 要素の一部として利用されます。
eval要素は、内容をテキスト化した値を返します。

eval要素の内容にget要素等を記載してその値の取得に失敗した場合、:ref:`default-get<config_defaults>` の値が設定されます。

次の例では、eval指定により、name型変数'name'の値：'マロン'と、name型変数'animal'の値：'犬'が文字列として設定されたcategoryが生成されます。
その後このlearnfノードに合致する、'マロンは誰ですか'という入力を行うと、'あなたのペットの犬です。'と返します。

* 使用例

.. code:: xml

    <category>
        <pattern>私のペットは * の * です。</pattern>
        <template>
            あなたのペットは、<star />の<star index="2" />ですね。
            <think>
                <set name="animal"><star /></set>
                <set name="name"><star index="2" /></set>
            </think>
            <learnf>
                <category>
                    <pattern>
                        <eval>
                            <get name="name"/>
                        </eval>
                        は誰ですか。
                    </pattern>
                    <template>
                        あなたのペットの
                        <eval>
                            <get name="animal"/>
                        </eval>
                        です。
                    </template>
                </category>
            </learnf>
        </template>
    </category>

| Input: 私のペットは犬のマロンです。
| Output: あなたのペットは犬のマロンですね。
| Input: マロンは誰ですか。
| Output: あなたのペットの犬です。

関連項目: `learn <#learn>`__, `learnf <#learnf>`__


explode 
-------------
[1.0]

explode要素は、対象文字列を1文字単位に分割し、半角スペースで区切ります。 
’coffee'と入力した場合、explodeを有効にすると、'c o f f e e'に変換します。

* 使用例

.. code:: xml

   <category>
       <pattern>EXPLODE *</pattern>
       <template>
           <explode><star /></explode>
       </template>
   </category>

<explode />は、<explode><star /></explode>と同義です。書き換えると以下の様になります。

.. code:: xml

   <category>
       <pattern>EXPLODE *</pattern>
       <template>
           <explode />
       </template>
   </category>

| Input: EXPLODE coffee
| Output: c o f f e e

関連項目: `implode <#implode>`__


image 
-----------
[2.1]

image要素はリッチメディア要素で、画像の情報を返すことができます。
内容に、画像URLやファイル名を指定します。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

.. code:: xml

    <category>
        <pattern>画像表示</pattern>
        <template>
            <image>https://url.for.image</image>
        </template>
    </category>


first 
-----------
[1.0]

| first要素は、複数単語からなる文字列に対して、先頭の単語を返します。単単語の場合は、そのまま返ります。
| (文字列の単語分割時には、 :ref:`punctuation_chars<storage_file_properties>` の指定による特定文字除外の処理を行います。)
| 取得に失敗した場合は、コンフィグレーション定義の :ref:`default-get<config_defaults>` の値が返ります。

* 使用例

.. code:: xml

   <category>
       <pattern>私の名前は * です </pattern>
       <template>
           あなたの名前は <first><star /></first> さんですね。
       </template>
   </category>

| Input: 私の名前は 山田 太郎 です
| Output: あなたの名前は山田さんですね

| RDFナレッジベースの検索結果に適用した場合、結果リスト内の先頭データを取得します。 詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。
| JSON形式のデータには対応していません。

関連項目: `rest <#rest>`__


extension 
---------------------
[custom]

| extension要素は、対話エンジンのカスタマイズを必要とする要素になります。
| extension要素では、Pythonの処理クラスを呼び出す機能を提供し、 ``programy.extensions.base.Extension`` インタフェースを実装するPythonモジュールクラスへのフルパスを指定します。
| extension要素の内容が処理クラスに対する引数（文字列）として、引き渡されます。

詳細は、 :doc:`Extensions<Extensions>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "path","string","Yes","extension処理クラスのパス名"

* 使用例

.. code:: xml

   <category>
       <pattern>
           GEOCODE *
       </pattern>
       <template>
            <extension path="programy.extensions.goecode.geocode.GeoCodeExtension">
                <star />
            </extension>
       </template>
   </category>

関連項目: :doc:`Extensions<Extensions>`


formal 
------------
[1.0]

| formal要素は、対象文字列内の単語の先頭文字を大文字に変換します。
| JSON形式やリスト形式のデータに適用した場合、各要素毎に処理が行われます。

* 使用例

.. code:: xml

   <category>
       <pattern>私の名前は * * </pattern>
       <template>
        <formal><star /></formal> <formal><star index="2"/></formal>さん、こんにちは
       </template>
   </category>

<formal />は<formal><star /></formal> と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>私の名前は * * </pattern>
       <template>
           <formal /><formal><star index="2"/></formal>さん、こんにちは
       </template>
   </category>


| Input: 私の名前は george washington
| Output: George Washington さん、こんにちは


.. _template_gender:

gender 
------------
[1.0]

| gender要素は、発話文に含まれる性別を表す人称代名詞等を対象に、逆の性別の単語に変換することを目的としています。
| 変換内容は、:ref:`genderファイル<storage_file_gender>` で指定します。
| 変換方法の指定には変換前と変換後のセットで記載し、genderのセット内に一致するものがある場合にのみ変換が行われます。複数単語に対する指定も可能です。
| 変換対象の判定は、英数記号：半角、カタカナ：全角に変換した上で行いますが、変換結果はgenderファイルの値になります。

* 使用例

.. code:: xml

   <category>  
       <pattern>* に会いましたか？</pattern>  
       <template>
           いえ、 <gender><star/></gender> に会いました。
       </template>  
   </category>

| Input: 彼に会いましたか？
| Output: いえ、彼女に会いました。

関連項目: :ref:`ファイル管理：gender<storage_entity>`


.. _template_get:

get 
---------
[1.0]

| get要素は、変数の値取得に用います。取得に失敗した場合、コンフィグレーション定義の :ref:`default-get<config_defaults>` の値が返ります。
| （:ref:`propertiesファイル<storage_file_properties>` での "default-get" 定義が優先されます。）
| getで取得できる値は、`set <#set>`__ を使って、対話処理実施時に値の設定を行います。
| 起動時に初期値を設定する場合、:ref:`defaultsファイル<storage_file_defaults>` に記載することで、グローバル変数(name)として利用することができます。
| 変数種別には3種類あり、ローカル変数と保持期間が異なるグローバル変数が2種類あります。

* ローカル変数(var)

| "var"属性を指定することで、ローカル変数扱いになります。
| ローカル変数は、set/getが記載されているcategoryの範囲のみ保持されます。したがって、sraiの参照先では別変数扱いになります。

* 継続保持グローバル変数(name)

| "name"属性を指定することで、グローバル変数扱いになります。グローバル変数は別categoryで設定した内容も参照することができます。
| また、グローバル変数の内容は継続的に保持しており、対話処理を繰り返し実施した場合でも内容を保持しています。

* 指定範囲保持グローバル変数(data)

| "data"属性を指定することで、グローバル変数扱いになります。nameとの差異は、対話APIのdeleteVariableにtrueが設定された時点でdataで定義した変数がクリアされる点です。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,65

    "name","変数名","No","グローバル変数(name)名を指定"
    "var","変数名","No","ローカル変数(var)名を指定"
    "data","変数名","No","グローバル変数(data)名を指定"

| AIMLの変数を値として指定する場合に属性では指定できないため、子要素としても指定できるようにしています。動作は属性と同じになります。
| また、子要素 ``tuple`` を指定することで、RDFナレッジベースのエレメントも取得できます。詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,65

    "name","変数名","No","グローバル変数(name)名を指定"
    "var","変数名","No","ローカル変数(var)名を指定"
    "data","変数名","No","グローバル変数(data)名を指定"
    "tuple","子要素","No","RDFナレッジベースのエレメントを処理する場合に指定"

| get要素として、属性・子要素で、``var``, ``name``, ``data``, ``tuple`` のいずれかが設定されている必要があります。
| 複数の属性、子要素を指定した場合には、不正指定としてシナリオは無効になります。
| 尚、変数名は、大文字・小文字、全角・半角を区別して管理します。

* 使用例

.. code:: xml

    <!-- Access Global Variable -->
    <category>
        <pattern>今日は * です</pattern>
        <template>
            <think><set name="weather"><star/></set></think>
             今日の天気は、<get name="weather" />です。
        </template>
    </category>

    <!-- Access Local Variable -->
    <category>
        <pattern>明日は * です</pattern>
        <template>
            <think><set var="weather"><star/></set></think>
             今日の天気は<get name="weather" />,明日の天気は<get var="weather"/>です。
        </template>
    </category>
    <category>
        <pattern>天気は？</pattern>
        <template>
             今日の天気は<get name="weather" />,明日の天気は<get var="weather"/>です。
        </template>
    </category>

| Input: 今日は晴れです。
| Output: 今日の天気は晴れです。
| Input: 明日は雨です。
| Output: 今日の天気は晴れ,明日の天気は雨です。
| Input: 天気は？
| Output: 今日の天気は晴れ,明日の天気はunknownです。

コンフィグレーションの :ref:`dynamic<config_dynamic>` で ``variables`` が定義されている場合、グローバル変数(name)名として ``dynamic`` の定義が優先されます。

関連項目: `set <#set>`__, :ref:`ファイル管理：properties<storage_entity>` 


id 
--------
[1.0]

| id要素は、クライアント名を返します。クライアント名はクライアント開発者が :ref:`コンフィグファイル<config_file>` のclient定義で指定します。
| id要素には、属性・子要素は指定できません。

* 使用例

.. code:: xml

   <category>
       <pattern>あなたの名前は？</pattern>
       <template>
           <id />
       </template>
   </category>

| Input: あなたの名前は？
| Output: console


implode 
-------------------
[custom]

| implode要素は、対象文字列内にある半角スペースを削除し、1単語に結合します。
| 'c o f f e e'と入力した場合、implodeを実施すると、’coffee'に変換します。

* 使用例

.. code:: xml

   <category>
       <pattern>Implode *</pattern>
       <template>
           <implode><star /></implode>
       </template>
   </category>

<implode />は、<implode><star /></implode>と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>Implode *</pattern>
       <template>
           <implode />
       </template>
   </category>

| Input: Implode c o f f e e
| Output: coffee

関連項目: `explode <#explode>`__


input 
-----------
[1.0]

| input要素は、入力された発話文に対してマッチ処理用に編集した文字列（対話処理の対象となる文章）を返します。
| 編集処理では、英数記号の半角化とカタカナの全角化、マッチ処理対象外文字の除去、及び、単語分解と再結合が行われ、発話文が再構成されます。
| input要素は、pattern内のワイルドカード ``<star/>`` とは異なり、発話文全体の範囲を返します。

* 使用例

.. code:: xml

   <category>
       <pattern>質問はなんですか？</pattern>
       <template>
           あなたの質問は、"<input />"です。
       </template>
   </category>

| Input: 質問はなんですか？
| Output: あなたの質問は、"質問はなんですか？"です。


interval 
--------------
[1.0]

| interval要素は、2つの日時の差分を計算します。（マイナスの差分にも対応します。）
| 日時の形式指定には、Pythonの日時文字列の書式を利用します。 詳細は `Pythonのマニュアル <https://docs.python.jp/3.6/library/datetime.html>`__ を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "format","string","No","日時データの書式。省略値は '%c'"
 
* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "format","string","No","日時データの書式。省略値は '%c'"
    "from","string","Yes","計算を行う始端日時をformatの書式で指定"
    "to","string","Yes","計算を行う終端開始日時をformatの書式で指定"
    "style","string","No","出力する単位を指定。省略値は 'days'"

``style`` で指定する形式には、以下の種類があります。

   - 日時の単位指定： 'years', 'months', 'weeks', 'days', 'hours', 'minutes', 'seconds', 'microseconds'
   - 複合単位指定： 'ymd', 'hms', 'ymdhms' 

| ``from``, ``to`` で指定されたデータが ``format`` の書式と異なる場合や、不正な形式の場合、実行時に例外が発生します。
| 又、``style`` が変数等で指定され、実行時に対象外の値が使用された場合、結果として空文字が返ります。

* 使用例

.. code:: xml

   <category>
       <pattern>あなたは何歳ですか？</pattern>   
       <template>
            <interval format="%B %d, %Y">
                <style>years</style>
                <from><bot name="birthdate"/></from>
                <to><date format="%B %d, %Y" /></to>
            </interval>
            歳です。
       </template>
   </category>

| Input: あなたは何歳ですか？
| Output: 5歳です。

関連項目: `date <#date>`__


.. _template_json:

json 
---------
[custom]

| json要素は、JSONをシナリオで利用するための機能で、データの取得と設定の両方の機能を持ちます。設定系の処理を行った場合、結果には空文字が返ります。
| :doc:`SubAgent<SubAgent>`、:doc:`metadata<Metadata>`、:doc:`NLU<NLU>` (高度意図解釈)などで使用するJSONデータを利用するために使用します。
| 詳細は、 :doc:`JSON <JSON>` を参照してください。

| 属性／子要素のname/var/dataで指定する変数名には、get/setで定義した変数名を使用します。
| 但し、設定を行う場合には、変数名とともに、JSONとしてのキーを付加して指定する必要があります。
| 　指定例： "変数名.キー”　（属性・子要素で”key”を指定した場合には、変数名のみでも処理されます。）
| 変数型 varはローカル変数、nameはグローバル変数、dataはグローバル変数です。
| また、メタデータ変数や、サブエージェントの戻り値等のシステム固定変数名もvar型変数として利用できます。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","指定値","必須","説明"
    :widths: 10,10,10,5,65

    "name","JSON変数名","","No","グローバル変数(name)名を指定します。キー名を付加することを推奨します。"
    "var","JSON変数名","","No","ローカル変数(var)名を指定します。キー名を付加することを推奨します。"
    "data","JSON変数名","","No","グローバル変数(data)名を指定します。キー名を付加することを推奨します。"
    "function","","len","No","対象のJSONプロパティが配列の場合、配列長を取得します。対象がJSONオブジェクトの場合、JSONオブジェクトの要素数を取得します。"
    "","","delete","No","対象プロパティを削除します。配列の場合でindexを指定していると対象となる要素を削除します。"
    "","","insert","No","JSON配列に対する値の追加を指定します。配列番号(index)とともに指定します。"
    "index","インデックス","","No","JSONデータを取得する場合のインデックスを指定します。対象が配列の場合、配列番号を指します。JSONオブジェクトではキーを先頭から順に数えたオブジェクトを指します。JSONデータを設定・変更する場合、配列のみに指定できます。"
    "item","","key","No","JSONデータからキーを取得する場合に使用します。この属性を指定すると値ではなくキーを取得します。"
    "key","キー指定","","No","JSONデータを操作するキーを指定します。"
    "type","","string","No","数値・論理値・null値を文字列として処理することを指定します。"

json要素として、``var``, ``name``, ``data`` のいずれかが設定されている必要があります。

* 子要素

| シナリオの変数を値として指定する場合に属性では指定できないため、子要素としても指定できるようにしています。
| 動作は属性と同じ動作になります。同じ属性名、子要素名を指定した場合には子要素の設定が優先されます。

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "function","関数名","No","JSONに対する処理オプションを指定します。内容については属性のfunctionを参照。"
    "index","インデックス","No","JSONデータを取得する場合、JSONオブジェクト、配列に対して指定でき、配列では配列番号を差し、JSONオブジェクトではキーを先頭から順に数えたオブジェクトを指します。JSONデータを設定・変更する場合、配列のみに指定できます。"
    "item","キー名取得","No","JSONデータからキーを取得する場合に使用します。この属性を指定すると値ではなくキーを取得します。"
    "key","キー指定","No","JSONデータを操作するキーを指定します。"

* 使用例

| "transit"というSubAgentからのレスポンスが返ってきた場合のJSONのデータを取得し、対話処理の応答文として利用する場合を説明します。
| 以下のjsonデータがSubAgentから返却された場合、"__SUBAGENT__.transit"がSubAgentからのレスポンスデータの格納変数名になります。
| JSONデータを取得する場合、属性に対象となるjson名を指定しますが、この場合"__SUBAGENT__.transit"が対象json名となります。
| JSONデータの子要素の取得を行う場合、json名に、要素毎のキー名を"."で繋げたプロパティを指定する方法と、``key`` で指定する方法があります。

.. code:: json

        {
            "transportation":{
                "station":{
                    "departure":"東京",
                    "arrival":"京都"
                },
                "time":{
                    "departure":"2018/11/1 11:00",
                    "arrival":"2018/11/1 13:30"
                }
            }
        }

上記例の様に、transportation.station.departureを返却する場合、

.. code:: xml

    <category>
        <pattern>東京から京都に行きたい。</pattern>
        <template>
            <json var="__SUBAGENT__.transit.transportation.station.departure"/>出発ですね。
        </template>
    </category>

| Input: 東京から京都に行きたい。
| Output: 東京出発ですね。

関連項目: :doc:`JSON <JSON>`, :doc:`SubAgent<SubAgent>`


learn 
-----------
[2.0]

| learn要素は、対話の条件によりユーザ固有の新たなcategoryを登録します。すでに登録していたcategoryの内容を置換することも可能です。
| 新たなcategoryはメモリ上に保持されており、contextが有効な期間、同一クライアントを利用する同一ユーザからのアクセス時のみ有効になります。
| 登録されたcategoryは、他のシナリオで `restlearn <#resetlearn>`__ 要素・ `resetlearnf <#resetlearnf>`__ 要素が実行されるまで保持されます。
| 初期ロードされるcategory（:ref:`categoriesエンティティ<storage_entity>` で展開)と同じPattern要素を新たに登録した場合、learn要素で指定したcategoryが優先されます。

| 登録可能なcategory数は、コンフィグレーション定義の :ref:`制限値定義<config_bot_max>` の ``max_categories`` で制限されており、learn要素での登録も制限されます。
| 制限数に達した状態で新規登録を行った場合には、処理例外が発生します。（同じpattern要素に対する再登録を行うことは可能です。）

learnfはファイル保持なので、bot再起動でも状態を保持しますが、learnはbot再起動時に初期化されます。

| 子要素には、category要素を記述し、登録時の変数の値を処理条件に含める場合には `eval <#eval>`__ を利用します。
| 一度に複数のcategoryを登録することも可能ですが、topic要素でグループ化することはできません。topicを指定する場合には、category要素配下にtopic要素を記述する必要があります。
| 尚、learn要素の結果は、常に空文字になります。

* 使用例

.. code:: xml

   <category>
        <pattern>私のペットは * の * です。</pattern>
        <template>
            あなたのペットは、<star />の<star index="2" />ですね。
            <think>
                <set name="animal"><star /></set>
                <set name="name"><star index="2" /></set>
            </think>
            <learn>
                <category>
                    <pattern>
                        <eval>
                            <get name="name"/>
                        </eval>
                        は誰ですか。
                    </pattern>
                    <template>
                        あなたのペットの
                        <eval>
                            <get name="animal"/>
                        </eval>
                        です。
                    </template>
                </category>
            </learn>
        </template>
    </category>

| Input: 私のペットは犬のマロンです。
| Output: あなたのペットは犬のマロンですね。
| Input: マロンは誰ですか。
| Output: あなたのペットの犬です

関連項目; `eval <#eval>`__, `learnf <#learnf>`__, `restlearn <#resetlearn>`__, `resetlearnf <#resetlearnf>`__


.. _template_learnf:

learnf 
------------
[2.0]

| learnf要素は、対話の条件によりユーザ固有の新たなcategoryを登録します。すでに登録していたcategoryの内容を置換することも可能です。
| 新たなcategoryはメモリ上とともに、ファイル上にも保持されており、他のシナリオで `restlearn <#resetlearn>`__ 要素・ `resetlearnf <#resetlearnf>`__ 要素が実行されるまで保持し続けます。
| 登録されたcategoryは、同一クライアントを利用する同一ユーザからのアクセス時のみ有効になります。
| 初期ロードされるcategory（:ref:`categoriesエンティティ<storage_entity>` で展開)と同じPattern要素を新たに登録した場合、learnf要素で指定したcategoryが優先されます。

| 登録可能なcategory数は、コンフィグレーション定義の :ref:`制限値定義<config_bot_max>` の ``max_categories`` で制限されており、learn要素での登録も制限されます。
| 制限数に達した状態で新規登録を行った場合には、処理例外が発生します。（同じpattern要素に対する再登録を行うことは可能です。）

learnfはファイル保持なので、botの再起動時に再ロードされます。

| 子要素には、category要素を記述し、登録時の変数の値を処理条件に含める場合には `eval <#eval>`__ を利用します。
| 一度に複数のcategoryを登録することも可能ですが、topic要素でグループ化することはできません。topicを指定する場合には、category要素配下にtopic要素を記述する必要があります。
| 尚、learnf要素の結果は、常に空文字になります。

* 使用例

.. code:: xml

   <category>
        <pattern>私のペットは * の * です</pattern>
        <template>
            あなたのペットは、<star />の<star index="2" />ですね。
            <think>
                <set name="animal"><star /></set>
                <set name="name"><star index="2" /></set>
            </think>
            <learnf>
                <category>
                    <pattern>
                        <eval>
                            <get name="name"/>
                        </eval>
                        は誰ですか。
                    </pattern>
                    <template>
                            あなたのペットの
                        <eval>
                            <get name="animal"/>
                        </eval>
                        です。
                    </template>
                </category>
            </learnf>
        </template>
    </category>

| Input: 私のペットは犬のマロンです。
| Output: あなたのペットは犬のマロンですね。
| Input: マロンは誰ですか。
| Output: あなたのペットの犬です。

関連項目: `eval <#eval>`__, `learn <#learn>`__, `restlearn <#resetlearn>`__, `resetlearnf <#resetlearnf>`__


li
---------------
[1.0]

li要素は、condition要素での分岐条件の指定と、random要素での選択要素を指定する子要素として使用します。

condition要素の子要素として使用する場合には、以下の子要素（属性）が指定できます。
詳細な利用方法は、`condition <#condition>`__ を参照してください。

* 子要素（'loop'以外は属性でも指定可能。）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","変数名","No","分岐条件とするグローバル変数(name)名を指定します。"
    "var","変数名","No","分岐条件とするローカル変数(var)名を指定します。"
    "data","変数名","No","分岐条件とするグローバル変数(data)名を指定します。"
    "bot","プロパティ名","No","分岐条件とするBot固有情報（プロパティ）名を指定します。"
    "value","判定値","No","分岐条件となる値を指定します。（大文字・小文字、全角・半角を区別します）"
    "regex","判定値","No","分岐条件となる値を正規表現で指定します。判定は完全一致（大文字・小文字は同一視）で行います。"
    "loop","string","No","condition要素内でのループ処理を指定します。"

random要素の子要素として使用する場合には、属性や子要素の規定はありません。
詳細な利用方法は、`random <#random>`__ を参照してください。

関連項目: `condition <#condition>`__, :ref:`loop <condition_looping>`,  `random <#random>`__


link 
----------
[2.1]

link要素は、会話中にユーザに表示するURLなどの用途で利用されるリッチメディア要素です。 
子要素として、表示や読み上げに使用するテキスト、遷移先のurlを記載できます。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素　（属性での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "text","string","Yes","ボタンへの表示テキストを記載します。（空文字を許容します。）"
    "url","string","No","ボタン押下時のURLを記載します。"

.. code:: xml

    <category>
        <pattern>検索</pattern>
        <template>
            <link>
                <text>検索サイト</text>
                <url>searchsite.com</url>
            </link>
        </template>
    </category>


list 
----------
[2.1]

list要素は、itemに記載した要素をリスト形式で返すリッチメディア要素です。 
子要素のitemにリストの内容を記載することができます。また、itemにlistを記載し入れ子にすることもできます。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "item","string","Yes","リストの内容を記載します。"

``item`` には、テキスト以外に、`button <#button>`__ や、`image <#image>`__ 等のリッチメディア要素を配置することができます。 

.. code:: xml

    <category>
        <pattern>リスト</pattern>
        <template>
            <list>
                <item>
                    <list>
                        <item>リストアイテム 1.1</item>
                        <item>リストアイテム 1.2</item>
                    </list>
                </item>
                <item>リストアイテム 2.1</item>
                <item>リストアイテム 3.1</item>
            </list>
        </template>
    </category>

関連項目: `card <#card>`__, `button <#button>`__, `image <#image>`__


.. _template_log:

log 
---------------
[custom]

| log要素は開発者用の要素で、この要素を記載すると、botのログファイルに出力されます。
| ロギングレベルは、level属性で指定します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "level","変数名","No","'error', 'warning', 'debug', 'info' のいずれかを指定します。省略時は'info'で出力されます。"

詳細は、 :doc:`ログ設定 <config/Config_Logging>` を参照してください。

| 尚、コンフィグレーションのストレージ定義で :ref:`logs<storage_entity>` を有効にすると、直近の対話での情報がユーザ毎のログファイルに出力されます。
| ユーザ毎のログファイルの内容は、:ref:`デバッグAPI<debug_api>` でも取得できます。

* 使用例

.. code:: xml

    <category>
        <pattern>こんにちは</pattern>
        <template>
            こんにちは
            <log>挨拶</log>
        </template>
    </category>

    <category>
        <pattern>さよなら</pattern>
        <template>
            さよなら
            <log level="error">挨拶</log>
        </template>
    </category>

| Input: こんにちは
| Output: こんにちは        ※ログには、infoレベルで"挨拶"と出力されている
| Input: さよなら
| Output: さよなら          ※ログには、errorレベルで"挨拶"と出力されている

関連項目: :doc:`ログ設定 <config/Config_Logging>`


lowercase
---------------
[1.0]

lowercase要素は、対象となる文字列内の半角英字を小文字にします。
英字以外に、大文字・小文字が存在するギリシャ文字等にも対応します。

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは * です</pattern>
       <template>
           こんにちは <lowercase><star /></lowercase>さん
       </template>
   </category>

<lowercase />は、<lowercase><star /></lowercase>と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは * です</pattern>
       <template>
           こんにちは <lowercase />さん
       </template>
   </category>

| Input: こんにちは GEORGE WASHINGTON です
| Output: こんにちは george washingtonさん

関連項目: `uppercase <#uppercase>`__


.. _template_map:

map 
---------
[1.0]

| map要素では、外部ファイルで指定した変換辞書により、対象文字列の変換を行います。
| 対象文字列の定義は、、:ref:`mapsファイル<storage_file_maps>` でファイル毎に、’変換対象文字列: 変換後文字列' の形式で列記します。
| 変換対象文字列の一致判定は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で行いますが、 変換結果には指定された変換後文字列が設定されます。
| 変換対象文字列、変換後文字列に、複数単語からなる文字列を指定することもできますが、空白の数を含めて一致する必要があります。

変換対象文字列のリストに一致するものが無い場合、コンフィグレーション定義の :ref:`default-map<config_defaults>` の定義値が返ります。
（default-mapが定義されていない場合、空文字が返ります。）

* 属性　（子要素での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","map名","Yes","mapsファイル名から拡張子を除いた文字列を指定します。"

| 属性でnameを指定した場合、登録されてないファイル名を指定すると、シナリオ展開時に異常を検出し、該当シナリオは無効になります。
| 子要素でnameを指定した場合、登録されてないファイル名を指定すると、実行時に例外が発生します。

* 使用例

.. code:: xml

   <category>
       <pattern>* の県庁所在地は？</pattern>
       <template>
          <map name="prefectural_office"><star/></map>です。
       </template>
   </category>

| Input: 神奈川県の県庁所在地は？
| Output: 横浜市です。

コンフィグレーションの :ref:`dynamic<config_dynamic>` で ``maps`` が定義されている場合、mapsファイル名として ``dynamic`` の定義が優先されます。

関連項目: :ref:`ファイル管理：maps<storage_entity>`


.. _template_nluintent:

nluintent
---------
[custom]

| nluintent要素は、NLU結果のintent情報を取得するための機能です。
| NLU結果がある場合のみ値が返ります。従って、基本的に以下の変数に対して使用します。

- patternに :ref:`nluタグ<pattern_nlu>` を指定したcategoryにマッチした場合に、ローカル変数(var): ``__SYSTEM_NLUDATA__`` を対象に処理します。
- templateの :ref:`sraixタグ<template_sraix>` でNLU通信を行った場合に、ローカル変数(var): ``__SUBAGENT_NLU__.エイリアス名`` を対象に処理します。
- NLU結果を代入した変数を対象に処理します。

詳細は、 :doc:`NLU <NLU>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,30,5,55

    "name","インテント名","Yes","取得するインテント名を指定します。 ``*`` でワイルドカード扱いになります。ワイルドカード指定時はindexで取得対象を指定します。"
    "item","取得アイテム名","Yes","指定したインテントの情報を取得します。``intent`` 、 ``score`` および ``count`` を指定できます。
    intent指定時はインテント名を取得することができます。score指定時は確信度(0.0〜1.0)を取得します。countはインテント名の数を返します。"
    "index","インデックス","No","取得するインテントのインデックス番号を指定します。nameで指定したインテント名がマッチしたリスト中のインデックス番号を指定します。"
    "target","対象変数名","No","任意の変数を対象とする場合に、その変数名を指定します（省略値は、 ``__SYSTEM_NLUDATA__`` ）。sraixでのNLU通信結果を対象とする場合には、``__SUBAGENT_NLU__.エイリアス名`` を指定します。"
    "type","変数種別","No","``target`` の指定時に有効で、変数の種別：'name', 'data', 'var' のいずれかを指定します（省略値は、'var'）。"

| AIMLの変数を値として指定する場合に属性では指定できないため、子要素としても指定できるようにしています。
| 動作は属性と同じ動作になります。同じ属性名、子要素名を指定した場合子要素の設定が優先されます。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,30,5,55

    "name","インテント名","Yes","取得するインテント名を指定します。 内容については属性のnameを参照。"
    "item","取得アイテム名","Yes","指定したインテントの情報を取得します。内容については属性のitemを参照。"
    "index","インデックス","No","取得するインテントのインデックス番号を指定します。内容については属性のindexを参照。"

* 使用例

NLUの処理結果のインテント情報を取得します。
以下例のNLU処理結果からインテントを取得する場合を説明します。

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

NLUで処理したインテントを取得する場合、以下のように記述します。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <nluintent name="restaurantsearch" item="score" />
        </template>
    </category>

| Input: イタリアンかフレンチか中華を探して
| Output: 0.9。

| :ref:`sraixタグ<template_sraix>` でNLU通信を行った結果からインテントを取得する場合、以下のように記述します。
| 尚、この場合の :ref:`nlu_servers<storage_nlu_servers>` ファイルでは、serversのエイリアス名として、'someNlu' が登録されているものとします。


.. code:: xml

    <category>
        <pattern>
            NLU通信 *
        </pattern>
        <template>
            <think>
                <sraix nlu="sameNlu"><star /></sraix>
            <thimk>
            <nluintent name="restaurantsearch" item="score" target="__SUBAGENT_NLU__.someNlu" />
        </template>
    </category>

| Input: NLU通信 イタリアンかフレンチか中華を探して
| Output: 0.9。

関連項目: :doc:`NLU <NLU>` 、 :ref:`NLUインテントの取得<nlu_intent_example>`


.. _template_nluslot:

nluslot
---------
[custom]

| nluslot要素は、NLU結果のslot情報を取得するための機能です。
| NLU結果がある場合のみ値が返ります。従って、基本的に以下の変数に対して使用します。

- patternに :ref:`nluタグ<pattern_nlu>` を指定したcategoryにマッチした場合に、ローカル変数(var): ``__SYSTEM_NLUDATA__`` を対象に処理します。
- templateの :ref:`sraixタグ<template_sraix>` でNLU通信を行った場合に、ローカル変数(var): ``__SUBAGENT_NLU__.エイリアス名`` を対象に処理します。
- NLU結果を代入した変数を対象に処理します。

詳細は、 :doc:`NLU <NLU>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,30,5,55

    "name","スロット名","Yes","取得するスロット名を指定します。 ``*`` でワイルドカード扱いになります。ワイルドカード指定時はindexで取得対象を指定します。"
    "item","取得アイテム名","Yes","指定したスロットの情報を取得します。``slot`` 、 ``entity`` 、 ``score`` 、``startOffset`` 、``endOffset`` および ``count`` を指定できます。
    slot指定時はスロット名を取得することができます。entity指定時はスロットの抽出文字列、score指定時は確信度(0.0〜1.0)、startOffset指定時は抽出文字列の開始文字位置、endOffset指定時は抽出文字列の終端文字位置を取得します。
    countは同一スロット名の数を返します。"
    "index","インデックス","No","取得するスロットのインデックス番号を指定します。nameで指定したスロット名がマッチしたリスト中のインデックス番号を指定します。"
    "target","対象変数名","No","任意の変数を対象とする場合に、その変数名を指定します（省略値は、 ``__SYSTEM_NLUDATA__`` ）。sraixでのNLU通信結果を対象とする場合には、``__SUBAGENT_NLU__.エイリアス名`` を指定します。"
    "type","変数種別","No","``target`` の指定時に有効で、変数の種別：'name', 'data', 'var' のいずれかを指定します（省略値は、'var'）。"

AIMLの変数を値として指定する場合に属性では指定できないため、子要素としても指定できるようにしています。
動作は属性と同じ動作になります。同じ属性名、子要素名を指定した場合子要素の設定が優先されます。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,30,5,55

    "name","スロット名","Yes","取得するスロット名を指定します。 内容については属性のnameを参照。"
    "item","取得アイテム名","Yes","指定したスロットの情報を取得します。内容については属性のitemを参照。"
    "index","インデックス","No","取得するスロットのインデックス番号を指定します。内容については属性のindexを参照。"

* 使用例

NLUの処理結果のスロット情報を取得します。
以下例のNLU処理結果からスロットを取得する場合を説明します。

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

NLUで処理したスロットを取得する場合、以下のように記述します。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <nluslot name="genre" item="count" />
            <nluslot name="genre" item="entity" index="0" />
            <nluslot name="genre" item="entity" index="1" />
            <nluslot name="genre" item="entity" index="2" />
        </template>
    </category>

| Input: イタリアンかフレンチか中華を探して
| Output: 3 イタリアン フレンチ 中華。

| :ref:`sraixタグ<template_sraix>` でNLU通信を行った結果からスロットを取得する場合、以下のように記述します。
| 尚、この場合の :ref:`nlu_servers<storage_nlu_servers>` ファイルでは、serversのエイリアス名として、'someNlu' が登録されているものとします。

.. code:: xml

    <category>
        <pattern>
            NLU通信 *
        </pattern>
        <template>
            <think>
                <sraix nlu="sameNlu"><star /></sraix>
            <think>
            <nluslot name="genre" item="count" target="__SUBAGENT_NLU__.someNlu" />
            <nluslot name="genre" item="entity" index="0" target="__SUBAGENT_NLU__.someNlu" />
            <nluslot name="genre" item="entity" index="1" target="__SUBAGENT_NLU__.someNlu" />
            <nluslot name="genre" item="entity" index="2" target="__SUBAGENT_NLU__.someNlu" />
        </template>
    </category>

| Input: NLU通信 イタリアンかフレンチか中華を探して
| Output: 3 イタリアン フレンチ 中華。

関連項目: :doc:`NLU <NLU>` 、 :ref:`NLUスロットの取得<nlu_slot_example>`


.. _template_normalize:

normalize 
---------------
[1.0]

| normalize要素は、対象となる文字列に含まれる記号や、短縮形の文字列を、指定された単語に変換します。
| 変換内容は、:ref:`normalファイル<storage_file_normal>` で指定し、大文字・小文字、全角・半角を区別して変換を行います。
| 英単語に対する変換の場合、文字を単位とする変換（文字変換）と、単語を単位とする変換（単語変換）を、文字変換・単語変換の順で行い、両者とも、変換後の文字列の前後には、空白が挿入されます。
| （日本語の場合、単語変換のみを行います。）
| 例えば、'.'を'dot'、'*'を'_'に変換する場合、'www.***.com'は、'www dot _ _ _ dot com'に変換されます。

* 使用例

.. code:: xml

   <category>
       <pattern>URLは *</pattern>
       <template>
           <normalize><star /></normalize>を表示します。
       </template>
   </category>

<normalize />は、<normalize><star /></normalize>と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>URLは *</pattern>
       <template>
            <normalize />を表示します。
       </template>
   </category>

| Input: URLはwww.***.com
| Output: www dot _ _ _ dot com を表示します。

関連項目: :ref:`ファイル管理：normal<storage_entity>` , `denormalize <#denormalize>`__


olist 
-----------
[2.1]

olist（ordered list）要素は、子要素の `item <#item>`__ に記載した要素をリスト形式で返すリッチメディア要素です。 
子要素のitemにはリストの内容を記載することができます。また、itemにlistを記載し入れ子にすることもできます。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "item","string","Yes","リストの内容を記載します。"

* 使用例

.. code:: xml

   <category>
       <pattern>リストを表示して</pattern>
       <template>
            <olist>
               <item>
                    <card>
                        <image>https://searchsite.com/image0.png</image>
                        <title>Image No.1</title>
                        <subtitle>Tag olist No.1</subtitle>
                        <button>
                            <text>Yes</text>
                            <url>https://searchsite.com:?q=yes</url>
                        </button>
                    </card>
                </item>
                <item>
                    <card>
                        <image>https://searchsite.com/image1.png</image>
                        <title>Image No.2</title>
                        <subtitle>Tag olist No.2</subtitle>
                        <button>
                            <text>No</text>
                            <url>https://searchsite.com:?q=no</url>
                        </button>
                    </card>
                </item>
            </olist>
       </template>
    </category>

関連項目: `item <#item>`__ , `card <#card>`__


oob
---------
[1.0]

| OOBは "Out of Band" の略で、oob要素が評価されると対応する内部モジュールが処理を行い、処理結果をクライアントに返します。
| 内部モジュールでの処理はOS上での機器操作を想定しており、組み込み機器などで利用することを想定した機能です。
| OOBを処理する内部モジュールは、システム開発者が設計、実装を行います。詳細は :doc:`OOB <OOB>` を参照してください。

| oob要素の子要素の名称に、利用するOOB機能の識別名を指定し、その内容として引数を指定します。引数として記述した内容はXML化されてOOBの処理モジュールに引き渡されます。
| 引数を複数指定する場合、要素名にはAIMLで使用する要素名と重ならない名称を使用してください。
| コンフィグレーションで登録していないOOBの識別名を指定した場合には、実行時に例外が発生します。

| (重要）
| 　OOBの処理は、他の要素とは異なり、template内に記述された他の要素の処理を全て実施した後に実行されます。
| 　又、実施できるOOB機能はcategory内で１つのみで、複数のOOBの指定を行っても、実施されるのは最初の１機能のみです。
　
* 使用例

.. code:: xml

   <category>
       <pattern>DIAL *</pattern>
       <template>
            <oob><dial><star /></dial></oob>
       </template>
   </category>

| Input: DIAL 0123-456-7890
| Output: (DIAL) (返却内容は内部モジュールの実装次第)   

関連項目: `xml <#xml>`__ 、 :doc:`OOB <OOB>`


.. _template_person:

person 
------------
[1.0]

| person要素は、発話文に含まれる一人称の代名詞と二人称の代名詞の間の変換を行うことを目的としています。
| 変換内容は、:ref:`personファイル<storage_file_gender>` で指定します。
| 変換方法の指定には変換前と変換後のセットで記載し、personのセット内に一致するものがある場合にのみ変換が行われます。複数単語での指定も可能です。
| 変換対象の判定は、英数記号：半角、カタカナ：全角に変換した上で行いますが、変換結果はpersonファイルの値になります。

* 使用例

.. code:: xml

   <category>
       <pattern>私は * を待っています。</pattern>  
       <template>
           あなたは <person><star /></person> を待っているんですね。
       </template>  
   </category>

<person />は、<person><star /></person>と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>私は * を待っています。</pattern>  
       <template>
           あなたは <person /> を待っているんですね。
       </template>  
   </category>

| Input: 私はあなたを待っています。
| Output: あなたは私を待っているんですね。

関連項目: :ref:`ファイル管理：person<storage_entity>` , `person2 <#person2>`__


.. _template_person2:

person2 
-------------
[1.0]

| person2要素は、発話文に含まれる一人称の代名詞と三人称の代名詞の間の変換を行うことを目的としています。
| 変換内容は、:ref:`person2ファイル<storage_file_gender>` で指定します。
| 変換方法の指定には変換前と変換後のセットで記載し、person2のセット内に一致するものがある場合にのみ変換が行われます。複数単語での指定も可能です。
| 変換対象の判定は、英数記号：半角、カタカナ：全角に変換した上で行いますが、変換結果はperson2ファイルの値になります。

* 使用例

.. code:: xml

   <category>  
       <pattern>* に * を教えてください。</pattern>  
       <template>
           <person2><star/></person2> の <star index="2" /> はこれです。
       </template>  
   </category>

<person2 />は、<person2><star /></person2>と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>  
       <pattern>* に * を教えてください。</pattern>  
       <template>
           <person2 /> の <star index="2" /> はこれです。
       </template>  
   </category>

| Input: 私に行き方を教えてください。
| Output: あなた方の行き方はこれです。

関連項目: :ref:`ファイル管理：person2<storage_entity>` , `person <#person>`__


.. _template_program:

program 
-------------
[1.0]

| program要素は、Botのバージョン情報を 'ボット名 version バージョン名' の形式で返します。(対話エンジンのバージョンを示すものではありません。)
| ボット名は、default:'AIML bot' で、 :ref:`propertiesファイル<storage_file_properties>` で 'fullname' を指定することで変更できます。
| バージョン名は、コンフィグレーションの :ref:`version<config_bot_base>` をdefaultとし、 :ref:`propertiesファイル<storage_file_properties>` の 'version' 指定で変更できます。

propertiesファイルのversionに 'v1,0' を指定した場合には、以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>version</pattern>
       <template>
           <program />
       </template>
   </category>

| Input: version
| Output: AIML bot version v1.０

関連項目: :ref:`ファイル管理：properties<storage_entity>`


random 
------------
[1.0]

random要素は、列記された `li <#li>`__ 子要素の中からランダムに１個を選び、その結果を返します。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "li","string","Yes","ランダムで出力する結果を指定します。"

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは</pattern>
       <template>
           <random>
                <li>こんにちは</li>
                <li>今日の調子はどうですか？</li>
                <li>今日の予定を調べましょうか？</li>
           </random>
       </template>
   </category>

| Input: こんにちは
| Output: 今日の予定を調べましょうか？
| Input: こんにちは
| Output: 今日の調子はどうですか？

関連項目: `li <#li>`__


reply 
-----------
[2.1]

| reply要素は、リッチメディア要素でbutton要素に似ています。子要素として、読み上げに使用するtext、Botに対するpostbackを記載します。
| replyとbuttonの違いは、GUIを利用せず音声対話などで利用することを想定しています。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 子要素　（属性での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "text","string","Yes","読み上げテキストを記載します。（空文字を許容します。）"
    "postback","string","No","動作を記載します。ユーザにはこのメッセージは見せずBotに対するレスポンスやアプリケーションで処理を行う場合に利用します。"

* 使用例

.. code:: xml

   <category>
       <pattern>乗り換え</pattern>
       <template>
            <reply>
                <text>乗り換え検索しますか？</text>
                <postback>乗り換え案内</postback>
            </reply>
       </template>
    </category>

関連項目: `button <#button>`__


request 
-------------
[1.0]

| request要素は、履歴番号を指定することで対話履歴の中から該当する入力（発話文）履歴を返します。
| 入力文は、英数記号の半角化とカタカナの全角化、マッチ処理対象外文字の除去、及び、単語分解と再結合が行われ、発話文が再構成されたものになります。
| 履歴番号は、'0'が現在で、数値が大きくなるほど過去の履歴になります。'-1'を指定すると最古の入力文が取得できます。
| 該当する履歴がない履歴番号を指定した場合、取得失敗として空文字を返します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","string","No","履歴番号を整数値で指定。未指定時は '1'。"

* 使用例

.. code:: xml

   <category>
       <pattern>なんて言ったっけ？</pattern>
       <template>
             <request index="2" />、
             <request index="1" />、
             <request index="0" />、
             と言いました。
       </template>
   </category>

| Input: こんにちは
| Output: こんにちは
| Input: もう夜だね
| Output: こんばんは
| Input: なんて言ったっけ？
| Output: こんにちは、もう夜だね、なんて言ったっけ？、と言いました。

<request />は、<request index="1" />と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>なんて言ったっけ？</pattern>
       <template>
             <request />、と言いました。
       </template>
   </category>

| Input: こんにちは
| Output: こんにちは
| Input: なんて言ったっけ？
| Output: こんにちは、と言いました。

関連項目: `response <#response>`__


resetlearn 
----------------
[2.x]

| resetlearn要素は、メモリ上の `learn <#learn>`__  要素. `learnf <#learnf>`__ 要素で登録されたユーザ固有のcategoryを全て削除します。
| resetlearnでは、learnf要素で作成したファイルを削除しない為、再起動時にlearnf要素で登録したcategoryが復活します。
| resetlearnを実行することで、使用中のcategory数は初期ロードしたcategory数に戻ります。また、Pattern要素が同じでユーザ固有定義が優先されて使用できなかった初期ロードのcategoryも使用できる様になります。
| 尚、resetlearn要素の結果は、常に空文字になります。

* 使用例

.. code:: xml

   <category>
       <pattern>私の言ったことを忘れて。</pattern>
       <template>
            <think><resetlearn /></think>
            わかりました。残念ですが忘れます。
       </template>
   </category>

関連項目: `learn <#learn>`__, `learnf <#learnf>`__, `resetlearnf <#resetlearnf>`__


resetlearnf 
-----------------
[2.x]

| resetlearnf要素は、`learn <#learn>`__  要素. `learnf <#learnf>`__ 要素で登録されたユーザ固有のcategoryを全て削除します。
| resetlearnとの差は、learnf要素で作成したファイルを含めて削除する点です。
| resetlearnfを実行することで、使用中のcategory数は初期ロードしたcategory数に戻ります。また、Pattern要素が同じでユーザ固有定義が優先されて使用できなかった初期ロードのcategoryも使用できる様になります。
| 尚、resetlearnf要素の結果は、常に空文字になります。

* 使用例

.. code:: xml

   <category>
       <pattern>私の言ったことを忘れて。</pattern>
       <template>
            <think><resetlearnf /></think>
            わかりました。残念ですが忘れます。
       </template>
   </category>

関連項目: `learn <#learn>`__, `learnf <#learnf>`__, `resetlearn <#resetlearn>`__


response 
--------------
[1.0]

| response要素は、履歴番号を指定することで対話履歴の中から該当する出力（応答文）履歴を返します。
| 履歴番号は、'0'が現在（未確定の為、指定不可）で、数値が大きくなるほど過去の履歴になります。'-1'を指定すると最古の出力文が取得できます。
| 該当する履歴がない履歴番号を指定した場合、取得失敗として空文字を返します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","string","No","履歴番号を '0' 以外の整数値で指定。未指定時は '1'。"

* 使用例

.. code:: xml

   <category>
       <pattern>君はなんて言ったっけ？</pattern>
       <template>
             <response index="2" />、
             <response index="1" />、
             と言いました。
       </template>
   </category>

| Input: こんにちは
| Output: こんにちは
| Input: もう夜だね
| Output: こんばんは
| Input: 君はなんて言ったっけ？
| Output: こんにちは、こんばんは、と言いました。

<response />は、<response index="1" />と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>君はなんて言ったっけ？</pattern>
       <template>
             <response /> 、と言いました。
       </template>
   </category>

| Input: こんにちは
| Output: こんにちは
| Input: 君はなんて言ったっけ？
| Output: こんにちは、と言いました。

関連項目: `request <#request>`__


rest 
----------
[2.0]

| rest要素は、複数単語からなる文字列に対して、最初以外の単語列を返します。first要素の逆の動作です。
| (文字列の単語分割時には、 :ref:`punctuation_chars<storage_file_properties>` の指定による特定文字除外の処理を行います。)
| 取得に失敗した場合は、コンフィグレーション定義の :ref:`default-get<config_defaults>` の値が返ります。
| 例えば、"山田 太郎"の場合、"太郎"が返ります。
| 尚、結果の単語列は、日本語の場合でも空白区切りで返ります。

* 使用例

.. code:: xml

   <category>
       <pattern>私の名前は * です</pattern>
       <template>
           あなたの名前は<rest><star /></rest>さんですね
       </template>
   </category>

| Input: 私の名前は 山田 太郎 です
| Output: あなたの名前は太郎さんですね

| RDFナレッジベースの検索結果に適用した場合、結果リスト内の先頭以外のデータを取得します。 詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。
| JSON形式のデータには対応していません。

| first要素とrest要素を、condition要素内で使用することで、単語毎の処理を繰り返して実行することができます（取得失敗を意味する "unknown" の取得で終了します）。
| （但し、本処理は単語分解結果に依存します。又、conditionでのloop処理には回数制限がありますので、大規模なデータに対する処理では例外が発生します。）

* 使用例

.. code:: xml

   <category>
       <pattern>単語展開 *</pattern>
       <template>
           <think>
               <set var="WORDS"><star /></set>
           </think>
           <condition var="WORDS">
               <li value="unknown" />
               <li>
                   "<first><get var="WORDS" /></first>"
                   <think>
                       <set var="WORDS"><rest><get var="WORDS" /></rest></set>
                   </think>
                   <loop />
               </li>
           </condition>
       </template>
   </category>

| Input: 単語展開 apple orange grape
| Output: "apple" "orange" "grape"。

関連項目: `first <#first>`__


.. _template_set:

set 
---------
[1.0]

| template内のset要素では、グローバル変数とローカル変数の設定を行うことができます。変数型：name/var/dataの差異は、 `get <#get>`__ を参照してください。
| JSON形式やリスト形式のデータも文字列として指定することで、set要素で設定することができます。
| set要素の結果には、設定した値が返ります。応答文に反映されない様にする場合は、`think <#think>`__ を利用してください。

| グローバル変数(name/data)については、利用可能な変数の総数がコンフィグレーション定義の :ref:`制限値定義<config_bot_max>` の ``max_properties`` で制限されます。
| 制限数に達した状態で新規登録を行った場合には、処理例外が発生します。（登録済みの変数は使用できます。）
| :ref:`defaultsファイル<storage_file_defaults>` で初期設定するグローバル変数(name)については全て登録されますが、初期登録段階で制限値に達している場合、新たな変数は使用できません。
| 変数を削除するには、set要素の設定値に空文字を指定します。（グローバル変数(data)については、:ref:`対話API<coversation_api>` の ``deleteVariable`` 指定で削除することもできます。）
| 但し、グローバル変数(name)の 'topic' は削除できない変数として、空文字を指定しても '*' が設定されます。

* 属性　（子要素での指定も可能）

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,65

    "name","変数名","No","グローバル変数(name)名を指定"
    "var","変数名","No","ローカル変数(var)名を指定"
    "data","変数名","No","グローバル変数(data)名を指定"

| set要素として、属性・子要素で、``var``, ``name``, ``data`` のいずれかが設定されている必要があります。
| 複数の属性、子要素を指定した場合には、不正指定としてシナリオは無効になります。
| 尚、変数名・設定値とも、大文字・小文字、全角・半角を区別して管理します。

* 使用例

.. code:: xml

   <!-- グローバル変数 -->
   <category>
       <pattern>MY NAME IS *</pattern>
       <template>
           <set name="myname"><star /></set>
       </template>
   </category>

   <!-- ローカル変数 -->
   <category>
       <pattern>MY NAME IS *</pattern>
       <template>
           <set var="myname"><star /></set>
       </template>
   </category>

関連項目: `get <#get>`__ 


.. _template_select:

select 
------------
[2.0]

| select要素はRDFの検索に使用する要素で、起動時に展開するRDFファイルの内容と、`addtriple <#addtriple>`__ で追加されたRDFナレッジベースを対象に検索を行い、該当する情報を取得します。
| select要素での検索は、肯定検索を行うクエリと否定検索を行うクエリが指定でき、条件に合わせてそれぞれを列記することで複合的な検索が行えます。但し、クエリを列記した場合、それぞれの結果に対してAND条件で合致する結果を返します。
| 又、検索用変数を利用することで、クエリ間で共通する値を検索対象とすることもできます。検索用変数名の先頭文字には '?' を付与します。
| (クエリを列記した場合で、先行するクエリの結果が後続のクエリの結果より少ない場合には、全ての検索対象が取得できない場合があります。)
| 詳細は :doc:`RDFサポート<RDF_Support>` を参照してください。

* 子要素

.. csv-table::
    :header: "パラメータ","","タイプ","必須","説明"
    :widths: 10,10,10,5,75

    "vars","","変数名リスト","No","利用する検索用変数名を空白区切りで列記"
    "q","","","No","肯定検索をするクエリの指定"
    "","subj","string","No","検索対象のsubject (主語)の値、又は、検索用変数名を指定"
    "","pred","string","No","検索対象のpredicate (述語)の値、又は、検索用変数名を指定"
    "","obj","string","No","検索対象のobject (目的語)の値、又は、検索用変数名を指定"
    "notq","","","No","否定検索をするクエリの指定。子要素指定に対して AND条件で処理します。"
    "","subj","string","No","検索対象外のsubject (主語)の値、又は、検索用変数名を指定"
    "","pred","string","No","検索対象外のpredicate (述語)の値、又は、検索用変数名を指定"
    "","obj","string","No","検索対象外のobject (目的語)の値、又は、検索用変数名を指定"

| select要素には、子要素として、``q``、又は、``notq`` が１つは必要です。又、クエリ間で変数を共有する場合、``vars`` 子要素も必須になります。
| 'q'、'notq' の子要素についても、``subj``、``pred``、``obj`` のいずれか１つが最低限必要です。同一子要素が複数指定された場合、最後の指定が有効になります。
| クエリ内で、'subj'、'pred'、'obj'の指定を省略した場合、省略された項目の全データが合致するものとして扱います。

| 以下の例では、RDFナレッジベースに対して、predicate="legs", object="4" を指定した検索を行って、subject(変数名:'?name') の一覧を取得しています。
| 検索結果は変数名と値のリスト形式で設定され、基本的にRDFデータ毎にまとめられます。
| 検索結果がなかった場合には、コンフィグレーション定義の :ref:`default-get<config_defaults>` の値が返ります。

* 使用例

.. code:: xml

   <category>
       <pattern>* 本足の動物は？</pattern>
       <template>
           <select>
                <vars>?name</vars>
                <q><subj>?name</subj><pred>legs</pred><obj><star/></obj></q>
           </select>
       </template>
   </category>

| Input: 4 本足の動物は？
| Output: [[["?name", "ZEBRA"]], [["?name", "LION"]], [["?name", "ELEPHANT"]]]

| 次の例では、RDFナレッジベースに対して、predicate="legs" を指定した検索を行って、subject(変数名:'?name') と object(変数名:'?number') の一覧を取得しています。
| 出力結果は、RDFデータ毎に 'subject' と 'object' がまとめられた形でリストに格納されています。

.. code:: xml

   <category>
       <pattern>動物の足は？</pattern>
       <template>
        <select>
            <vars>?name ?number</vars>
            <q><subj>?name</subj><pred>legs</pred><obj>?number</obj></q>
        </select>
       </template>
   </category>

| Input: 動物の足は？
| Output: [[["?name", "ANT"], ["?number", "6"]], [["?name", "BAT"], ["?number", "2"]], [["?name", "LION"], ["?number", "4"]], [["?name", "PIG"], ["?number", "4"]], [["?name", "ELEPHANT"], ["?number", "4"]], [["?name", "PERSON"], ["?number", "2"]], [["?name", "BEE"], ["?number", ""]], [["?name", "BUFFALO"], ["?number", "4"]], [["?name", "ANIMAL"], ["?number", "Legs"]], [["?name", "FROG"], ["?number", "4"]], [["?name", "PENGUIN"], ["?number", "2"]], [["?name", "DUCK"], ["?number", "2"]], [["?name", "BIRD"], ["?number", "2"]], [["?name", "MONKEY"], ["?number", "4"]], [["?name", "GOOSE"], ["?number", "2"]], [["?name", "FOX"], ["?number", "4"]], [["?name", "KANGAROO"], ["?number", "2"]], [["?name", "DOG"], ["?number", "4"]], [["?name", "COW"], ["?number", "4"]], [["?name", "SHEEP"], ["?number", "4"]], [["?name", "FISH"], ["?number", "0"]], [["?name", "OX"], ["?number", "4"]], [["?name", "DOLPHIN"], ["?number", "0"]], [["?name", "BEAR"], ["?number", "4"]], [["?name", "WOLF"], ["?number", "4"]], [["?name", "ZEBRA"], ["?number", "4"]], [["?name", "CAT"], ["?number", "4"]], [["?name", "WHALE"], ["?number", "0"]], [["?name", "CHICKEN"], ["?number", "2"]], [["?name", "TIGER"], ["?number", "4"]], [["?name", "HORSE"], ["?number", "4"]], [["?name", "OWL"], ["?number", "2"]], [["?name", "GOAT"], ["?number", "4"]], [["?name", "RABBIT"], ["?number", "4"]]]

任意の変数名を利用せずに検索対象項目に '?' のみを指定した場合、変数名として、"?subj", "?pred", "?obj" が出力されます。

関連項目: `addtriple <#addtriple>`__, `deletetriple <#deletetriple>`__, `uniq <#uniq>`__, :doc:`RDFサポート<RDF_Support>`


sentence 
--------------
[1.0]

sentence要素は、英文に対する整形を目的として、文章の最初の単語を大文字にし、他のすべての単語を小文字に設定します。
英字以外に、大文字・小文字が存在するギリシャ文字等にも対応します。

* 使用例

.. code:: xml

   <category>
       <pattern>Create a sentence with the word *</pattern>
       <template>
           <sentence>HAVE you Heard ABouT <star/></sentence>
       </template>
   </category>

| Input: Create a sentence with the word AnImAl
| Output: Have you heard about animal

<sentence />は<sentence><star /></sentence>と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>CORRECT THIS *</pattern>
       <template>
           <sentence />
       </template>
   </category>

| Input: CORRECT THIS PleAse tEll Us The WeAthEr ToDay.
| Output: Please tell us the weather today.


size 
----------
[1.0]

size要素は、利用可能なカテゴリ数を返します。
カテゴリ数には、初期登録されたcategoryに加え、learn/learnfで登録されたcategoryを含む為、ユーザ毎で数値が変わる場合があります。

* 使用例

.. code:: xml

   <category>
       <pattern>理解できるカテゴリ数は？</pattern>
       <template>
           <size />です。
       </template>
   </category>

| Input: 理解できるカテゴリ数は？
| Output: 5000です。


.. _template_space:

space
-----------
[custom]

| space要素は、要素毎の結果文字列の間に半角スペースを挿入します。
| 英文の場合には単語間に空白が入りますが、日本語の場合には基本的に空白が入らない為、本要素で空白を挿入します。`sraix <#sraix>`__ 要素でのSubAgent連携で引数を空白区切りで指定する場合などにも使用します。
| 尚、文字列結合の処理で重複空白は１つの空白に置換される為、英文に対して本要素を指定した場合でも空白が重複することはありません。

.. code:: xml

   <category>
       <pattern>おはようございます。</pattern>
       <template>
            <think>
                <set var="french">フレンチ</set>
                <set var="italian">イタリアン</set>
                <set var="chinese">中華</set>
            </think>
            <get var="french"/><get var="italian"/><get var="chinese"/>を検索。
            <get var="french"/><space/><get var="italian"/><space/><get var="chinese"/>を検索。
       </template>
   </category>

| Input: おはようございます。
| Output: フレンチイタリアン中華を検索。フレンチ イタリアン 中華を検索。

関連項目: `sraix <#sraix>`__


split 
-----------
[2.1]

split要素はリッチメディア要素で、XML内の文章を分割するために使用します。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

.. code:: xml

   <category>
       <pattern>おはようございます。</pattern>
       <template>
            今日はいい天気ですね。
            <split/>
            明日も晴れるといいですね。
       </template>
   </category>

| Input: おはようございます。
| Output: 今日はいい天気ですね。<split/>明日も晴れるといいですね。


sr 
--------
[1.0]

| sr要素は、入力文に対するsrai要素処理を簡易に行うもので、``<srai><star /></srai>`` の省略形です。
| 従って、`srai <#srai>`__ 要素と同様に、コンフィグレーション定義の :ref:`制限値定義<config_bot_max>` の ``max_search_srai`` の制限対象です。

* 使用例

.. code:: xml

   <category>
       <pattern>私の質問は * です</pattern>
       <template>
           <sr />
       </template>
   </category>

<sr />は、<srai><star/></srai>と同義です。

.. code:: xml

   <category>
       <pattern>私の質問は * です</pattern>
       <template>
           <srai><star/></srai>
       </template>
   </category>

関連項目: `star <#star>`__, `srai <#srai>`__


.. _template_srai:

srai 
----------
[1.0]

| srai要素は、内容で括られた文章でパターンマッチを実行し、該当するcategoryに対する処理を実施して結果文字列を返します。該当するcategoryが無い場合には、空文字が返ります。
| "srai"自体には正式な意味を持ちませんが、symbolic reduction、もしくは、symbolic recursionという意味になります。
| 共通的な処理を行うcategoryを登録しておき、srai要素に対応する発話文を指定することで、シナリオの記述を整理することができます。
| srai処理はパターンマッチを複数回行うため負荷が高く、実施回数をコンフィグレーション定義の :ref:`制限値定義<config_bot_max>` の ``max_search_srai`` で制限しています。
| 制限回数を超えてsrai処理を行った場合、処理例外が発生します。

以下の例は、”HI”に対応したcategoryで共通する処理を定義し、その他の入力文に対応したcategoryで利用しています。

* 使用例

.. code:: xml

    <category>
        <pattern>こんにちは</pattern>
        <template><srai>HI</srai></template>
    </category>

    <category>
        <pattern>Hello</pattern>
        <template><srai>HI</srai></template>
    </category>

    <category>
        <pattern>Hola</pattern>
        <template><srai>HI</srai></template>
    </category>

    <category>
        <pattern>HI</pattern>
        <template>こんにちは</template>
    </category>      

| Input: Hola
| Output: こんにちは

関連項目: `star <#star>`__, `sr <#sr>`__


.. _template_sraix:

sraix 
-----------
[2.0]

| sraix要素は、sraiの拡張機能ではなく、サブエージェント連携を行う別の機能で、外部サービスとのREST通信を行って結果を返します。
| 外部との通信を行うため、相手サービスとの通信内容の確認を行った上で利用する必要があります。
| 又、通信異常が発生した場合の考慮も必要で、異常時の文字列の指定の機能とともに、REST通信時のHTTPステータスコードを取得する機能もあります。
| 通信時間についても、タイムアウト時間の指定とともに、送受信の時間情報を取得することもできます。

サブエージェントが提供するサービスと連携する方法には、以下の４種類があり、それぞれで設定される結果や受信データの取得方法が異なります。

- :ref:`汎用RESTインタフェース<subagent_rest>`
- :ref:`対話プラットフォームで、公開されているbot呼び出し<subagent_cotoba_design_pf>`
- :ref:`NLU通信インタフェース<subagent_nlu>`
- :ref:`カスタム外部サービス実装<subagent_custom>`

sraix要素で指定する属性・子要素は以下になります。詳細は :doc:`SubAgent<SubAgent>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "service","string","No","カスタム外部サービスのサービス名。"
    "botName","string","No","botサービス連携のbotエイリアス名。"
    "nlu","string","No","NLUサービス連携のNLUエイリアス名。"
    "template","string","No","汎用RESTサービス連携用の雛形名。"
    "default","string","No","通信異常発生時の結果文字列。"
    "timeout","string","No","通信タイムアウト時間（秒単位）を '1' 以上の整数で指定。未指定時は '10'。"

* 子要素

| サブエージェントの連携方法によって指定する子要素が異なります。:doc:`SubAgent<SubAgent>` を参照してください。
| 尚、子要素の内容に不正があった場合、処理例外が発生することがあります。

| 以下は、カスタム外部サービスを利用した例です。
| 尚、日本語の場合、sraix要素の内容が全て結合されて空白区切りにならない場合があるため、引数間に `space <#space>`__ を指定しています。
　
* 使用例

.. code:: xml

   <category>
       <pattern>*から*までの乗り換え案内</pattern>
       <template>
           <sraix service="myService">
               <star/>
               <space />
               <star index="2"/>
           </sraix>
       </template>
   </category>

| Input: 東京から大阪までの乗り換え案内
| Output: 10:00発の、のぞみが一番早く着きます。

関連項目: `star <#star>`__, `space <#space>`__, :doc:`SubAgent<SubAgent>`


.. _template_star:

star 
----------
[1.0]

| star要素は、入力文に対して、pattern要素（ワイルドカードを含む）に該当した文字列を取得するために使用し、先頭から順に識別番号(1〜)が振られます。
| ワイルドカードとは、1つ以上の文字列を指定する ``*``, ``_``、もしくは、0以上の文字列を指定する ``^``, ``#`` に該当する文字列を意味し、pattern要素の ``set``、``iset``、``regex``、``bot`` 要素に該当する文字列も対象になります。
| 文字列取得には識別番号を指定しますが、識別番号に対応した文字列が無かった場合には空文字が返ります。

star要素で取得する結果はパターンマッチング時の値が対象となるため、以下の様になります。

- ワイルドカードの結果： 英数記号：半角、カタカナ：全角に変換されたマッチ文字列。
- set要素の結果： マッチした :ref:`単語リスト（sets）ファイル<storage_file_sets>` の定義文字列。
- iset要素の結果： マッチした :ref:`isetの<pattern_iset>` のwords定義文字列。
- regex要素の結果： 正規表現としてマッチした、英数記号：半角、カタカナ：全角に変換された文字列。
- bot要素の結果： マッチした :ref:`propertiesファイル<storage_file_properties>` の定義文字列。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","string","No","star識別番号を '1' 以上の整数で指定。未指定時は '1'。"

* 使用例

.. code:: xml

   <category>
       <pattern>私は * と * が好きです。</pattern>
       <template>
           あなたは、 <star /> と <star index="2" /> が好きなのですね。
       </template>
   </category>

| Input: 私は花と猫が好きです。
| Output: あなたは、花と猫が好きなのですね。

関連項目: `sr <#sr>`__, `srai <#srai>`__


.. _template_system:

system 
------------
[1.0]

| system要素を使用すると、システムコールを利用することができます。
| ユーザがオペレーティングシステムを知っていてシェルスクリプトを注入できれば、基本システムへ自由にアクセスできるようになる為、セキュリティ対策として。デフォルト環境では使用できません。
| 利用が必要な場合は、コンフィグレーションの :ref:`overrides定義<config_overrides>` で、``allow_system_aiml`` に 'true' を設定する必要があります。
| system要素の結果はシステム環境に依存しますが、システムコールが利用できない状態での結果は空文字になります。

* 使用例

.. code:: xml

   <category>
       <pattern>LIST ALL AIML FILES</pattern>
       <template>
           <system>ls -l *.aiml</system>
       </template>
   </category>


.. _template_that:

that
----------
[1.0]

| that要素は、``category`` の子要素としても定義があり、直前の応答文との一致判定に利用されますが、templateの要素としての ``that`` は、過去の応答文を取得する要素として使用します。
| 履歴の指定には履歴番号を使用し、'0'が現在（未確定の為、指定不可）で、数値が大きくなるほど過去の履歴を指定することになります。'-1'を指定すると最古の応答文が取得できます。
| 該当する履歴がない履歴番号を指定した場合、取得失敗として空文字を返します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","string","No","履歴番号を '0' 以外の整数値で指定。未指定時は '1'。"

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちわ</pattern>
       <template>
            こんにちは
       </template>
   </category>

   <category>
       <pattern>すみません</pattern>
       <template>
            <that />ですね
       </template>
   </category>

| Input: こんにちわ
| Output: こんにちは
| Input: すみません
| Output: こんにちはですね

関連項目: :ref:`that(pattern)<pattern_that>`, `thatstar <#thatstar>`__


thatstar 
--------------
[1.0]

| thatstar要素は、``that`` で指定したワイルドカードに対応する文字列を取得します。
| patternに対する `star <#star>`__ と同じ方法でアクセスされますが、patternのワイルドカードではなく、thatに指定されたワイルドカードを利用する際に使用します。
| 文字列取得には識別番号を指定しますが、識別番号に対応した文字列が無かった場合には空文字が返ります。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","string","No","star識別番号を '1' 以上の整数で指定。未指定時は '1'。"

* 使用例

.. code:: xml

   <category>
       <pattern>...</pattern>
       <template>
            コーヒーが好きですか？
       </template>
   </category>

   <category>
       <pattern>はい</pattern>
       <that> * が好きですか？</that>
       <template>
           私も<thatstar />が好きです。
       </template>
   </category>

| Input: ...
| Output: コーヒーが好きですか？
| Input: はい
| Output: 私もコーヒーが好きです。

関連項目: :ref:`that(pattern)<pattern_that>`, `that <#that>`__, `star <#star>`__, `topicstar <#topicstar>`__


.. _template_think:

think
-----------
[1.0]

think要素は、内容の処理結果を応答文に反映せずに、Bot内での処理を実施する要素です。結果は常に空文字になります。

* 使用例

.. code:: xml

   <category>
       <pattern>私の名前は * です</pattern>
       <template>
          <think>
              <set name="name"><star /></set>
          </think>
          あなたの名前を覚えました。
       </template>
   </category>


topicstar 
---------------
[1.0]

| topicstar要素は、``topic`` で指定したワイルドカードに対応する文字列を取得します。
| patternに対する `star <#star>`__ と同じ方法でアクセスされますが、patternのワイルドカードではなく、topicに指定されたワイルドカードを利用する際に使用します。
| 文字列取得には識別番号を指定しますが、識別番号に対応した文字列が無かった場合には空文字が返ります。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","string","No","star識別番号を '1' 以上の整数で指定。未指定時は '1'。"
    
* 使用例

.. code:: xml

    <category>
        <pattern>私はコーヒーが好きです。</pattern>
        <template>
            <think><set name="topic">beverages コーヒー</set></think>
            わかりました。
        </template>
    </category>

    <topic name="beverages *">
        <category>
            <pattern>私の好きな飲み物は？</pattern>
            <template><topicstar/>です。</template>
        </category>
    </topic>

| Input: 私はコーヒーが好きです。
| Output: わかりました。
| Input: 私の好きな飲み物は？
| Output: コーヒーです。

関連項目: :ref:`topic(pattern)<pattern_topic>`, `star <#star>`__, `thatstar <#thatstar>`__


.. _template_uniq:

uniq 
----------
[2.0]

| uniq要素は `select <#select>`__ と同様に、起動時に展開するRDFファイルの内容と、`addtriple <#addtriple>`__ で追加されたRDFナレッジベースを対象に検索を行い、該当する情報を取得します。
| selectとの違いは、selectは検索結果の複数候補をリスト形式で全てを返すのに対し、uniqは重複した候補を除外した結果を空白区切りで返す点です。 
| 詳細は :doc:`RDFサポート<RDF_Support>` を参照してください。

| uniq要素での検索では、取得したい項目に対して '?' を指定します。
| 検索結果がなかった場合には、コンフィグレーション定義の :ref:`default-get<config_defaults>` の値が返ります。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "subj","string","No","検索対象のsubject (主語)の値、又は、'?'"
    "pred","string","No","検索対象のpredicate (述語)の値、又は、'?'"
    "obj","string","No","検索対象のobject (目的語)の値、又は、'?'"

| uniq要素には、子要素として、``subj``、``pred``、``obj`` のいずれか１つが最低限必要です。同一子要素が複数指定された場合、最後の指定が有効になります。
| 'subj'、'pred'、'obj'の指定を省略した場合、省略された項目の全データが合致するものとして扱います。

| subjectの一覧を取得する場合、以下の様に記述します。
| 結果として、subjectの一覧が空白区切りで出力されますが、重複したものは除外されます。（空白区切りのため、RDFとして空白を含むデータが登録されていた場合には注意が必要です。）

.. code:: xml

    <category>
        <pattern>subject list</pattern>
        <template>
            <uniq>
                <subj>?</subj>
            </uniq>
        </template>
    </category>


以下の例では、結果的に、RDFナレッジベースに対して、predicate="は", object="バラ科" を指定した検索を行って、'subject'の一覧を取得しています。

* 使用例

.. code:: xml

    <category>
        <pattern>* は *</pattern>
        <template>
            <addtriple>
                <subj><star /></subj>
                <pred>は</pred>
                <obj><star index="2"/></obj>
            </addtriple>
            登録しました
        </template>
    </category>

    <category>
        <pattern>探して * * *</pattern>
        <template>
            <uniq>
                <subj><star /></subj>
                <pred><star index="2"/></pred>
                <obj><star index="3"/></obj>
            </uniq>
        </template>
    </category>

| Input: 桜 は バラ科
| Output: 登録しました
| Input: 苺 は バラ科
| Output: 登録しました
| Input: 探して 桜 は ?
| Output: バラ科
| Input: 探して ? は バラ科 
| Output: 桜 苺

関連項目: `addtriple <#addtriple>`__, `deletetriple <#deletetriple>`__, `select <#select>`__, :doc:`RDFサポート<RDF_Support>`


uppercase 
---------------
[1.0]

uppercase要素は、対象となる文字列内の半角英字を大文字にします。英字以外に、大文字・小文字が存在するギリシャ文字等にも対応します。

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは * です</pattern>
       <template>
           こんにちは<uppercase><star /></uppercase>さん
       </template>
   </category>

<uppercase />は、<uppercase><star /></uppercase>と同義です。書き換えると以下の様になります。

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは * さん</pattern>
       <template>
           こんにちは<uppercase />さん
       </template>
   </category>


| Input: こんにちは george washington さん
| Output: こんにちはGEORGE WASHINGTONさん

関連項目: `lowercase <#lowercase>`__


vocabulary
----------------
[1.0]

vocabulary要素は、Botの単語数として起動時に展開された以下の項目の合計数を返します。

- :doc:`pattern要素<Pattern_Tags>` で記述された単語の総数（ワイルドカードや、iset等の子要素内の単語を除く）。
- 全ての :ref:`単語リスト（sets）ファイル<storage_file_sets>` に記載されたリスト数の合計値。

| 日本語の場合、単語分割した上で積算します。
| 単語の重複は精査していないため、累計値になります。

* 使用例

.. code:: xml

   <category>
       <pattern>知っている単語数は？</pattern>
       <template>
           <vocabulary />です。
       </template>
   </category>

| Input: 知っている単語数は？
| Output: 10000です。

関連項目: `size <#size>`__


video 
-----------
[2.1]

video要素は、リッチメディア要素で、ビデオの情報を返すことができます。
ビデオのURLやファイル名を指定することができます。

対話エンジンでは、リッチメディア要素に対して、XML形式の結果を返します。
実際の画面描画等は、コンフィグレーション定義のclientの :ref:`renderer<config_file>` で指定した処理クラスの制御に依存します。

* 使用例

.. code:: xml

    <category>
        <pattern>ビデオ表示</pattern>
        <template>
            <video>https://url.for.video</video>
        </template>
    </category>


word 
----------
[1.0]

word要素は内部処理で生成する要素で、template内に記述されているテキスト部分（子要素内を含む）が展開されます。

* 使用例

.. code:: xml

   <category>
       <pattern>HELLO</pattern>
       <template>Hi there!</template>
   </category>

この使用例の場合、'Hi there!' がword要素として展開されます。


xml 
---------
[1.0]

xmlという要素は存在しませんが、対話エンジンで使用可能な要素以外がXML形式で指定されている場合、XML形式のままで結果に反映します。
これにより、templateの応答の一部として、XML形式の要素を記載することができます。

* 使用例

.. code:: xml

   <category>
       <pattern> * をボールド表示</pattern>
       <template>
           <bold><star /></bold>
       </template>
   </category>

| Input: 対象範囲ををボールド表示
| Output: <bold>対象範囲</bold>。
