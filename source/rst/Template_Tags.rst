==================================================
template要素
==================================================

本章ではtemplate要素内で記述可能なAIML要素について説明します。

対話プラットフォームでサポートしているtemplateの要素内に記述可能なAIML要素のリストは以下のとおりです。

-  `addtriple <#addtriple>`__
-  `authorise <#authorise>`__
-  `bot <#bot>`__
-  `button <#button>`__
-  `card <#card>`__
-  `carousel <#carousel>`__
-  `condition <#condition>`__

   -  :ref:`単一判定<condition_type1>`

   -  :ref:`条件分岐<condition_type2>`

   -  :ref:`複数条件での分岐 <condition_type3>`

   -  :ref:`ループ処理 <condition_looping>`

-  `date <#date>`__
-  `delay <#delay>`__
-  `deletetriple <#deletetriple>`__
-  `denormalize <#denormalize>`__
-  `eval <#eval>`__
-  `explode <#explode>`__
-  `first <#first>`__
-  `extension <#extension>`__
-  `formal <#formal>`__
-  `gender <#gender>`__
-  `get <#get>`__
-  `id <#id>`__
-  `image <#image>`__
-  `implode <#implode>`__
-  `input <#input>`__
-  `interval <#interval>`__
-  `json <#json>`__
-  `learn <#learn>`__
-  `learnf <#learnf>`__
-  `li <#li>`__
-  `link <#link>`__
-  `list <#list>`__
-  `log <#log>`__
-  `lowercase <#lowercase>`__
-  `map <#map>`__
-  `nluintent <#nluintent>`__
-  `nluslot <#nluslot>`__
-  `normalize <#normalize>`__
-  `oob <#oob>`__
-  `olist <#olist>`__
-  `person <#person>`__
-  `person2 <#person2>`__
-  `program <#program>`__
-  `random <#random>`__
-  `reply <#reply>`__
-  `request <#request>`__
-  `resetlearn <#resetlearn>`__
-  `resetlearnf <#resetlearnf>`__
-  `response <#response>`__
-  `rest <#rest>`__
-  `set <#set>`__
-  `select <#select>`__
-  `sentence <#sentence>`__
-  `size <#size>`__
-  `space <#space>`__
-  `split <#split>`__
-  `sr <#sr>`__
-  `srai <#srai>`__
-  `sraix <#sraix>`__
-  `star <#star>`__
-  `system <#system>`__
-  `that <#that>`__
-  `thatstar <#thatstar>`__
-  `think <#think>`__
-  `topicstar <#topicstar>`__
-  `uniq <#uniq>`__
-  `uppercase <#uppercase>`__
-  `video <#video>`__
-  `vocabulary <#vocabulary>`__
-  `word <#word>`__
-  `xml <#xml>`__

詳細
============
| このセクションでは、AIMLのtemplate要素内に記述する要素の説明を行います。
| ほとんどの要素はXMLの属性または子要素を追加して、データを利用します。
| 各要素の先頭の[...]は、対象の要素が最初に定義されたAIMLのバージョンを示しています。

addtriple 
---------------
[2.0]

addtriple要素により、RDFナレッジベースにエレメント(知識)を追加します。
エレメントの構成要素には、subject (主語)、predicate (述語)、object (目的語)の3つのアイテムがあります。
addtriple要素の詳細については、:doc:`RDFサポート<RDF_Support>` を参照してください。

下の使用例では、ユーザの発話文「私はカレーが好きだ」に対して、subject='私'、pred='好き', object='カレー' のアイテムで構成されるエレメント(知識)をRDFナレッジベースに登録します。

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

authorise要素を使うことにより、template要素内に記述されるAIML要素を実行するかどうかを、ユーザのロールによって切り替えることがができます。
ユーザのロールがauthorise要素のroot属性で指定されたロールと異なる場合、authorise要素内に記述したAIML要素は実行されません。
詳細は :doc:`Security <Security>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "role","文字列","Yes","ロール名"
    "denied_srai","文字列","No","認証失敗時のsrai先"

* 使用例

この使用例では、ユーザのロールが"root"である場合のみ、vocabularyの内容を返せます。

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

また、denied_srai属性を指定することで、ユーザのロールが指定したロールと異なる場合のデフォルト動作を決めることができます。

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

関連項目: :doc:`Security <Security>`

.. _template_bot:

bot
---------
[1.0]

bot固有のプロパティを取得します。この要素は読み込み専用です。
該当のプロパティは、properties.txtで指定し、起動時に読み込むことでbot固有情報として取得できます。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","文字列","Yes","基本的に、name,birthdate,app_version,grammar_versionのいずれかを記載(properties.txtで変更可能)。"

* 使用例

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

関連項目: :ref:`ファイル管理：properties<storage_entity>` 

button 
------------
[2.1]

button要素は、会話中にユーザにタップを促す用途で利用されるリッチメディア要素です。 
子要素として、buttonの表記に使用するテキスト、Botに対するpostback、ボタン押下時のurlを記載できます。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "text","文字列","Yes","ボタンへの表示テキストを記載します。"
    "postback","文字列","No","ボタン押下時の動作を記載します。ユーザにはこのメッセージは見せずBotに対するレスポンスやアプリケーションで処理を行う場合に利用します。"
    "url","文字列","No","ボタン押下時のURLを記載します。"

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

カードは、画像、ボタン、タイトル、サブタイトルなど、いくつかの他の要素を使用し1つのカードとします。
これらのリッチメディア要素すべてを含むメニューが表示されます。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "title","文字列","Yes","カードのタイトルを記載します。"
    "subtitle","文字列","No","カードに対する追加情報を記載します。"
    "image","文字列","Yes","カード用の画像URL等を記載します。"
    "button","文字列","Yes","カード用のボタン情報を記載します。"


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

カルーセルは、カード要素を複数利用しタップスルーメニューを表示します。
これらのリッチメディア要素すべてを含むメニューが表示されます。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "card","文字列","Yes","複数のカードを指定します。一度にカードを1つ表示、タップスルーで別のカードを表示します。"


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

| template内で条件判断を記述する際に使用し、switch-caseのような処理を記載できます。
| conditionの属性で指定した変数を、liの属性で判断することで分岐を記載します。
| get/setで定義した変数、及び、Bot固有情報を条件名として使用します。
| 変数型の varはローカル変数、nameはグローバル変数、dataはグローバル変数で、APIからのdeleteVariableでtrueが指定されるまで保持する変数として作用します。
| 以下に、conditionの記載方法を説明します。


* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","変数名","No","分岐条件とする変数を指定します。"
    "var","変数名","No","分岐条件とする変数を指定します。"
    "data","変数名","No","分岐条件とする変数を指定します。"
    "bot","プロパティ名","No","分岐条件とするBot固有情報を指定します。"
    "value","判定値","No","分岐条件となる値を指定します。"

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "li","文字列","No","指定した変数に対する分岐条件を記載します。"

※属性の各パラメータも子要素として指定できます。


.. _condition_type1:

単一判定
~~~~~~~~~~~~~~~

| この記載方法は、変数値の判定結果がtrueの場合に要素の文字列を<template>として返し、falseの場合は何も実行しません。
| 以下の使用例の様に4種類の記載方法があり、いずれも同じ動作を示しています。

* 使用例

.. code:: xml

   <condition name="ペット" value="犬">私も犬派です</condition>
   <condition name="ペット"><value>犬</value>私も犬派です</condition>
   <condition value="犬"><name>ペット</name>私も犬派です</condition>
   <condition><name>ペット</name><value>犬</value>私も犬派です</condition>

変数ペットが"犬"であった場合、"私も犬派です"を返します。

.. _condition_type2:

条件分岐
~~~~~~~~~~~~~~~~~~~~~~~~~~

| <condition>の属性に変数を設定し、対象となる値に対する分岐を記述します。分岐方法はswitch-caseに似ています。
| <condition>の変数の内容を<li>のvalueの値と比較し、trueになった条件の内容を返します。
| 分岐条件に合致しない場合、value未指定の<li>の内容を返します。value未指定の<li>がない場合は、何も返しません。

以下の使用例では、変数"ペット"の内容を評価します。評価の優先順序は記載順になります。

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

| <li>毎に条件分岐を指定する場合の記載方法を説明します。分岐方法はif文の集合体に似ています。
| <li>で定義された各条件が順次チェックされます。各ステートメントでは異なる変数、評価値を持つことができます。
| 条件がtrueになるとその時点で評価が完了(break)します。

以下の使用例では、変数"ペット"と、変数"飲み物"を評価します。評価の優先順序は記載順になります。

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
~~~~~~~

| <loop>は<li>の子要素の1つとして記載します。
| 通常<li>で分岐した場合、処理内容を<template>として返しますが<loop>がある場合、対象となる<li>に分岐し、<li>の処理を終えた後、<condition>の内容を再評価します。

以下の使用例では、変数"話題"を評価して返す内容を決定しますが、分岐条件に一致しなかった場合、"話題"に"雑談"を設定して<condition>の再評価を行い、"雑談"としてループを抜けます。

* 使用例

.. code:: xml

    <condition var="話題">
        <li value="花">花は何が好きですか</li>
        <li value="飲み物">コーヒーはどうですか</li>
        <li value="雑談">何かいいことありました？</li>
        <li><think><set var="話題">雑談</set></think><loop/></li>
    </condition>

関連項目: `li <#li>`__, `get <#get>`__, `set <#set>`__


date 
----------
[1.0]

| 日付と時刻の文字列を取得します。 APIでのlocale/time指定で、返す内容は変化します。
| format属性は、Pythonの日時文字列の書式設定をサポートします。 詳細は `Pythonのマニュアル <https://docs.python.jp/3.6/library/datetime.html>`__ を参照してください。

 詳細は Pythonのマニュアル(`datetime <https://docs.python.jp/3.6/library/datetime.html>`__)を参照してください。


* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "format","文字列","No","出力形式指定。未指定時は%c"

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "format","文字列","No","出力形式指定。未指定時は%c"

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

delay要素は遅延を行う要素です。
音声合成の再生中などの待ち時間の定義を行ったり、ユーザに対するBotの返答遅延を指定したりするために利用します。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "seconds","文字列","Yes","遅延秒数を指定。"

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


deletetriple
------------------
[2.0]

| RDFナレッジベースからエレメントを削除します。
| 指定できるエレメントは起動時にRDFファイルから読み込まれたエレメント、もしくは `addtriple <#addtriple>`__ で追加されたエレメントです。
| 詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。

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
       </template>
   </category>

関連項目: `addtriple <#addtriple>`__, `select <#select>`__, `uniq <#uniq>`__, :doc:`RDFサポート<RDF_Support>` 

.. _template_denormalize:

denormalize 
-----------------
[1.0]

| normalizeが対象文字列に含まれる記号や短縮形の文字列を単語に変換するのに対して、denormalizeは逆の動作を行います。変換内容は、denormal.txtで指定します。
| 例えば、'www.***.com'に対して、normalizeで'.'を'dot'、'*'を'_'に変換した場合、'www dot _ _ _ dot com 'になりますが、
| denormalizeで'dot'を'.'、'_'を'*'に変換するように指定した場合、normalize/denormalizeで、'www.***.com'に復元されます。

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
       <pattern>URLは *</pattern>
       <template>
            <denormalize />を復元します。
       </template>
   </category>

関連項目: :ref:`ファイル管理：denormal<storage_entity>`, `normalize <#normalize>`__

eval 
----------
[1.0]

evalは通常、`learn <#learn>`__、`learnf <#learnf>`__ 要素の一部として利用されます。
evalはテキスト化されたコンテンツを返す要素を評価します。

次の例では、変数'name'が'マロン'に設定され、変数'animal'に'犬'が設定されます。その後このlearnfノードに合致する入力、'マロンは誰ですか'という入力を行うと、'あなたのペットの犬です。'と返します。

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

1文字単位に分割し、半角スペースで区切ります。 
’coffee'と入力した場合、explodeを有効にすると、'c o f f e e'に変換します。

* 使用例

.. code:: xml

   <category>
       <pattern>EXPLODE *</pattern>
       <template>
           <explode><star /></explode>
       </template>
   </category>

<explode />は、<explode><star /></explode>と同義です。

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

image要素を使用すると、画像の情報を返すことができます。
画像URLやファイル名を指定することができます。

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

| 複数単語からなる文字列に対して、先頭の単語を返します。単語の場合は、そのまま返ります。
| 取得に失敗した場合、 `get <#get>`__ と同様に、Config等で設定された"default-get"の値が返ります。
| RDFナレッジベースの検索結果に適用した場合、結果リスト内の先頭データを取得します。 詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。

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


関連項目: `rest <#rest>`__

extension 
---------------------
[custom]

エンジンのカスタマイズを必要とする要素になります。
extensionはPythonのクラスを呼び出す機能を提供します。
extensionは、 ``programy.extensions.Extension`` インタフェースを実装するクラスへのフルPythonモジュールパスを記載します。
詳細は、 :doc:`Extensions<Extensions>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "path","文字列","Yes","利用extension名。"

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

formal要素の内容の単語の先頭文字を大文字に変換します。

* 使用例

.. code:: xml

   <category>
       <pattern>私の名前は * * </pattern>
       <template>
        <formal><star /></formal> <formal><star index="2"/></formal>さん、こんにちは
       </template>
   </category>

<formal />は<formal><star /></formal> と同義です。

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

| gender要素は、発話文に含まれる性別を表す人称代名詞等を対象に、逆の性別の単語に変換します。変換には gender.txtの内容を使用します。
| 変換方法の指定には変換前と変換後のセットで記載し、genderのセット内に一致するものがある場合にのみ変換が行われます。


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



| get要素は変数の値取得に用います。取得に失敗した場合、Configの"default-get"で設定した値が返ります。
| （Botのプロパティ情報:properties.txtで、"default-get"の定義を行った場合、Configの定義よりも優先されます。）
| getで取得できる値は、`set <#set>`__ を使って、対話処理実施時に値の設定を行います。
| 起動時に値を設定する場合、defaults.txtに記載することで、グローバル変数(name)として利用することができます。
| 変数種別には3種類あり、ローカル変数とグローバル変数で保持期間が異なります。
| また、子要素<tuples>を指定することで、RDFナレッジベースのエレメントも取得できます。詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。

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
    :header: "パラメータ","","タイプ","必須","説明"
    :widths: 10,10,10,5,65

    "name","","変数名","Yes","var,name,dataのいずれかが設定されている必要があります。"
    "var","","変数名","Yes","var,name,dataのいずれかが設定されている必要があります。"
    "data","","変数名","Yes","var,name,dataのいずれかが設定されている必要があります。"


| AIMLの変数を値として指定する場合にアトリビュートでは指定できないため、子要素としても指定できるようにしています。
| 動作はアトリビュートと同じ動作になります。同じアトリビュート名、子要素名を指定した場合子要素の設定が優先されます。

* 子要素

.. csv-table::
    :header: "パラメータ","","タイプ","必須","説明"
    :widths: 10,10,10,5,65

    "name","","変数名","Yes","var,name,dataのいずれかが設定されている必要があります。"
    "var","","変数名","Yes","var,name,dataのいずれかが設定されている必要があります。"
    "data","","変数名","Yes","var,name,dataのいずれかが設定されている必要があります。"



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


関連項目: `set <#set>`__, :ref:`ファイル管理：properties<storage_entity>` 


id 
--------
[1.0]

クライアント名を返します。クライアント名はクライアント開発者がConfigで指定します。

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

半角スペースで区切られた入力の半角スペースを削除し、1単語に結合します。
'c o f f e e'と入力した場合、implodeを有効にすると、’coffee'に変換します。

* 使用例

.. code:: xml

   <category>
       <pattern>Implode *</pattern>
       <template>
           <implode><star /></implode>
       </template>
   </category>

<implode />は、<implode><star /></implode>と同義です。

* 使用例

.. code:: xml

   <category>
       <pattern>Implode the acronym *</pattern>
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

pattern文全体を返します。
これはpattern内のワイルドカード ``<star/>`` とは異なり、pattern文全体を返します。

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

| 2つの時刻の差分を計算します。
| format属性は、Pythonの日時文字列の書式設定をサポートします。 詳細は `Pythonのマニュアル <https://docs.python.jp/3.6/library/datetime.html>`__ を参照してください。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "from","文字列","Yes","計算を行う始端時刻を記載。"
    "to","文字列","Yes","計算を行う終端開始時刻を記載。"
    "style","文字列","Yes","intervalで返す単位を記載。 years,months,days,secondsのいずれか。"


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

| JSONをAIMLで利用するための機能です。
| :doc:`SubAgent<SubAgent>`、:doc:`Metadata<Metadata>`、:doc:`NLU<NLU>` (高度意図解釈)などで使用するJSONデータを利用するために使用します。
| 詳細は、 :doc:`JSON <JSON>` を参照してください。

| 属性／子要素のname/var/dataで指定する変数名には、get/setで定義した変数名を使用します。
| 変数型 varはローカル変数、nameはグローバル変数、dataはグローバル変数で、APIからのdeleteVariableでtrueが指定されるまで保持する変数として作用します。
| また、メタデータ変数や、サブエージェン戻り値等のシステム固定変数名も利用できます。

* 属性

.. csv-table::
    :header: "パラメータ","設定値","タイプ","必須","説明"
    :widths: 10,10,10,5,65

    "name","","JSON名","Yes","パースを行うJSONを指定します。var,name,dataのいずれかが設定されている必要があります。"
    "var","","JSON名","Yes","パースを行うJSONを指定します。var,name,dataのいずれかが設定されている必要があります。"
    "data","","JSON名","Yes","パースを行うJSONを指定します。var,name,dataのいずれかが設定されている必要があります。"
    "function","","関数名","No","JSONに対する処理を記述します。"
    "","len","関数名","No","対象のJSONプロパティが配列の場合、'配列長を取得します。対象がJSONオブジェクトの場合、JSONオブジェクトの要素数を取得します。"
    "","delete","関数名","No","対象プロパティを削除します。配列の場合でindexを指定していると対象となる要素を削除します。"
    "","insert","関数名","No","JSON配列に対する値の追加を指定します。配列番号(index)とともに指定します。"
    "index","","インデックス","No","JSONデータを取得する場合のインデックスを指定します。対象が配列の場合、配列番号を指します。JSONオブジェクトではキーを先頭から順に数えたオブジェクトを指します。JSONデータを設定・変更する場合、配列のみに指定できます。"
    "item","","キー名取得","No","JSONデータからキーを取得する場合に使用します。この属性を指定すると値ではなくキーを取得します。"
    "key","","キー指定","No","JSONデータを操作するキーを指定します。"

* 子要素

| AIMLの変数を値として指定する場合に属性では指定できないため、子要素としても指定できるようにしています。
| 動作は属性と同じ動作になります。同じ属性名、子要素名を指定した場合子要素の設定が優先されます。

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "function","関数名","No","JSONに対する処理を記述します。内容については属性のfunctionを参照。"
    "index","インデックス","No","JSONデータを取得する場合、JSONオブジェクト、配列に対して指定でき、配列では配列番号を差し、JSONオブジェクトではキーを先頭から順に数えたオブジェクトを指します。JSONデータを設定・変更する場合、配列のみに指定できます。"
    "item","キー名取得","No","JSONデータからキーを取得する場合に使用します。この属性を指定すると値ではなくキーを取得します。"
    "key","キー指定","No","JSONデータを操作するキーを指定します。"


* 使用例

| "transit"というSubAgentからの、レスポンスが返ってきた場合のJSONのデータを取得し、レスポンスとして利用する場合を説明します。
| 下記のjsonデータがSubAgentから返却された場合、"__SUBAGENT__.transit"がSubAgentからのレスポンスデータの格納変数名になります。
| JSONデータを取得する場合、属性に対象となるjson名を指定しますが、この場合"__SUBAGENT__.transit"が対象json名となります。
| JSONデータの子要素の取得を行う場合、json名に、要素毎のキー名を"."で繋げたプロパティを指定します。

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

| learn要素は、対話の条件により新たなcategoryを有効にします。
| この新たなcategoryはメモリ上に保持されており、contextが有効な期間、同一クライアントからのアクセス時のみ有効になります。

learnfはファイル保持なので、bot再起動でも状態を保持しますが、learnはbot再起動時に初期化されます。

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

関連項目; `eval <#eval>`__, `learnf <#learnf>`__

.. _template_learnf:

learnf 
------------
[2.0]

| learnf要素は、対話の条件により新たなcategoryを有効にします。
| この新たなcategoryはファイル上に保持されており、有効化されると内容は保持し続けます。また、同一クライアントからのアクセス時のみ有効になります。

learnfはファイル保持なので、botの再起動時に再ロードされます。


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

関連項目: `eval <#eval>`__, `learn <#learn>`__

li
---------------
[1.0]

li要素では、<condition>で指定する分岐条件を記載します。
詳細な利用方法は、`condition <#condition>`__ を参照してください。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "think","文字列","No","動作に作用しない定義を記載。"
    "set","文字列","No","変数の設定を行います。"
    "get","文字列","No","変数の値を取得します。"
    "loop","文字列","No","<condition>に対するループを指定します。"
    "star","文字列","No","入力のワイルドカードを再利用します。"

関連項目: `condition <#condition>`__, :ref:`loop <condition_looping>`,  `think <#think>`__, `set <#set>`__, `get <#get>`__, `star <#star>`__


link 
----------
[2.1]

link要素は、会話中にユーザに表示するURLなどの用途で利用されるリッチメディア要素です。 
子要素として、表示や読み上げに使用するテキスト、遷移先のurlを記載できます。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "text","文字列","Yes","ボタンへの表示テキストを記載します。"
    "url","文字列","No","ボタン押下時のURLを記載します。"

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

link要素は、itemに記載した要素をリスト形式で返すリッチメディア要素です。 
子要素のitemにリストの内容を記載することができます。また、itemにlistを記載し入れ子にすることもできます。

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "item","文字列","Yes","リストの内容を記載します。"

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

.. _template_log:

log 
---------------
[custom]

| 開発者用の要素で、この要素を記載すると、botのログファイルに出力されます。
| ロギングレベルは、level属性で指定し、`Python Logging <https://docs.python.jp/3.6/library/logging.html>`__ に記載のレベルを指定できます。


* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "level","変数名","No","error,warning,debug,infoを指定します。省略時はinfoで出力されます。"

詳細は、 :ref:`ログ設定 <config_logging>` を参照してください。

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

関連項目: :ref:`ログ設定 <config_logging>`

lowercase
---------------
[1.0]

半角英字を小文字にします。

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは * です</pattern>
       <template>
           こんにちは <lowercase><star /></lowercase>さん
       </template>
   </category>


<lowercase />は、<lowercase><star /></lowercase>と同義です。

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは * です</pattern>
       <template>
           こんにちは <lowercase><star /></lowercase>さん
       </template>
   </category>

| Input: こんにちは GEORGE WASHINGTON です
| Output: こんにちは george washingtonさん

関連項目: `uppercase <#uppercase>`__

.. _template_map:

map 
---------
[1.0]

| 起動時に、'key:value’を列記したmapファイルを参照し、keyにマッチしたvalueを返します。keyにマッチしない場合、Configの"default-get"で設定した値が返ります。
| mapファイルとして、configで指定したディレクトリに格納されたファイルを参照します。


* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "name","変数名","Yes","mapファイル名を指定します。"


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

関連項目: :ref:`ファイル管理：maps<storage_entity>`


.. _template_nluintent:

nluintent
---------
[custom]

| NLU結果のintent情報を取得するための機能です。
| NLU結果がある場合のみ値が返ります。従って、基本的にpatternに :ref:`nluタグ<pattarn_nlu>` を指定したcategoryにマッチした場合のtemplateで利用します。
| 詳細は、 :doc:`NLU <NLU>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,30,5,55

    "name","インテント名","Yes","取得するインテント名を指定します。 ``*`` でワイルドカード扱いになります。ワイルドカード指定時はindexで取得対象を指定します。"
    "item","取得アイテム名","Yes","指定したインテントの情報を取得します。``intent`` 、 ``score`` および ``count`` を指定できます。
    intent指定時はインテント名を取得することができます。score指定時は確信度(0.0〜1.0)を取得します。countはインテント名の数を返します。"
    "index","インデックス","No","取得するインテントのインデックス番号を指定します。nameで指定したインテント名がマッチしたリスト中のインデックス番号を指定します。"

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
下記例のNLU処理結果からインテントを取得する場合を説明します。

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

NLUで処理したインテントを取得する場合下記のように記述します。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <nluintent name="restaurantsearch" item="score"/>
        </template>
    </category>

| Input: イタリアンかフレンチか中華を探して。
| Output: 0.9

関連項目: :doc:`NLU <NLU>` 、 :ref:`NLUインテントの取得<nlu_intent_example>`

.. _template_nluslot:

nluslot
---------
[custom]

| NLU結果のslot情報を取得するための機能です。
| NLU結果がある場合のみ値が返ります。従って、基本的にpatternに :ref:`nluタグ<pattarn_nlu>` を指定したcategoryにマッチした場合のtemplateで利用します。
| 詳細は、 :doc:`NLU <NLU>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,30,5,55

    "name","スロット名","Yes","取得するスロット名を指定します。 ``*`` でワイルドカード扱いになります。ワイルドカード指定時はindexで取得対象を指定します。"
    "item","取得アイテム名","Yes","指定したスロットの情報を取得します。``slot`` 、 ``entity`` 、 ``score`` 、``startOffset`` 、``endOffset`` および ``count`` を指定できます。
    slot指定時はスロット名を取得することができます。entity指定時はスロットの抽出文字列、score指定時は確信度(0.0〜1.0)、startOffset指定時は抽出文字列の開始文字位置、endOffset指定時は抽出文字列の終端文字位置を取得します。
    countは同一スロット名の数を返します。"
    "index","インデックス","No","取得するスロットのインデックス番号を指定します。nameで指定したスロット名がマッチしたリスト中のインデックス番号を指定します。"

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
下記例のNLU処理結果からスロットを取得する場合を説明します。

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

NLUで処理したスロットを取得する場合下記のように記述します。

.. code:: xml

    <category>
        <pattern>
            <nlu intent="restaurantsearch"/>
        </pattern>
        <template>
            <nluslot name="genre" item="count" />
            <nluslot name="genre" item="entity" index="0"/>
            <nluslot name="genre" item="entity" index="1"/>
            <nluslot name="genre" item="entity" index="2"/>
        </template>
    </category>

| Input: イタリアンかフレンチか中華を探して。
| Output: 3 イタリアン フレンチ 中華

関連項目: :doc:`NLU <NLU>` 、 :ref:`NLUスロットの取得<nlu_slot_example>`

.. _template_normalize:

normalize 
---------------
[1.0]

対象となる文字列に含まれる記号や、短縮形の文字列を、指定された単語に変換します。変換内容は、normal.txtで指定します。
例えば、'.'を'dot'、'*'を'_'に変換する場合、'www.***.com'は、'www dot _ _ _ dot com'に変換されます。

* 使用例

.. code:: xml

   <category>
       <pattern>URLは *</pattern>
       <template>
           <normalize><star /></normalize>を表示します。
       </template>
   </category>

<normalize />は、<normalize><star /></normalize>と同義です。

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

olist(ordered list)要素は、itemに記載した要素をリスト形式で返すリッチメディア要素です。 
子要素のitemにリストの内容を記載することができます。また、itemにlistを記載し入れ子にすることもできます。


.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "item","文字列","Yes","リストの内容を記載します。"

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

関連項目: `card <#card>`__


oob
---------
[1.0]

OOBは"Out of Band"の略で、oob要素が評価されると対応する内部モジュールが処理を行い、処理結果をクライアントに返します。
内部モジュールでの処理は実際に機器操作を想定しており、組み込み機器などで利用することを想定した機能です。
OOBを処理する内部モジュールは、システム開発者が設計、実装を行います。詳細は :doc:`OOB <OOB>` を参照してください。

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

| person要素は、発話文に含まれる一人称の代名詞と二人称の代名詞の間の変換を行います。変換には person.txtの内容を使用します。
| 変換方法の指定には変換前と変換後のセットで記載し、personのセット内に一致するものがある場合にのみ変換が行われます。

* 使用例

.. code:: xml

   <category>
       <pattern>私は * を待っています。</pattern>  
       <template>
           あなたは <person><star /></person> を待っているんですね。
       </template>  
   </category>

<person />は、<person><star /></person>と同義です。

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

| person2要素は、発話文に含まれる一人称の代名詞と三人称の代名詞の間の変換を行います。変換には person2.txtの内容を使用します。
| 変換方法の指定には変換前と変換後のセットで記載し、person2のセット内に一致するものがある場合にのみ変換が行われます。

* 使用例

.. code:: xml

   <category>  
       <pattern>* に * を教えてください。</pattern>  
       <template>
           <person2><star/></person2> の <star index="2" /> はこれです。
       </template>  
   </category>

<person2 />は、<person2><star /></person2>と同義です。

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

program 
-------------
[1.0]

Configで規定されたBotのプログラムバージョンを返します。

* 使用例

.. code:: xml

   <category>
       <pattern>version</pattern>
       <template>
           <program />
       </template>
   </category>

| Input: version
| Output: AIML bot version X


random 
------------
[1.0]

<condition>で使用する<li>子要素をランダムに選びます。

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

関連項目: `li <#li>`__、`condition <#condition>`__

reply 
-----------
[2.1]

reply要素は、リッチメディア要素でbutton要素に似ています。
子要素として、読み上げに使用するてtext、Botに対するpostbackを記載します。
replyとbuttonの違いは、GUIを利用せず音声対話などで利用することを想定しています。

* 子要素

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "text","文字列","Yes","読み上げテキストを記載します。"
    "postback","文字列","No","動作を記載します。ユーザにはこのメッセージは見せずBotに対するレスポンスやアプリケーションで処理を行う場合に利用します。"

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


request 
-------------
[1.0]

入力履歴を返します。index属性で履歴番号を指定します。
0が現在の入力で、数値が大きくなるほど過去の履歴になります。
入力単位のインデックスで、1入力で複数文ある場合も複数文を一度に返します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","文字列","No","入力番号。0が現在の入力、未指定時はindex='1'と同義。"

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


<request />は、<request index="1" />と同義です。

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

``<learn>`` ``<learnf>`` 要素で有効にしたcategoryを全て削除します。

* 使用例

.. code:: xml

   <category>
       <pattern>私の言ったことを忘れて。</pattern>
       <template>
            <think><resetlearn /></think>
            わかりました。残念ですが忘れます。
       </template>
   </category>

resetlearnf 
-----------------
[2.x]

``<learn>`` ``<learnf>`` 要素で有効にしたcategoryを全て削除します。
resetlearnとの差は、learnf要素で作成したファイルを削除する点です。

* 使用例

.. code:: xml

   <category>
       <pattern>私の言ったことを忘れて。</pattern>
       <template>
            <think><resetlearnf /></think>
            わかりました。残念ですが忘れます。
       </template>
   </category>

response 
--------------
[1.0]

出力履歴を返します。index属性で履歴番号を指定します。
数値が大きくなるほど過去の履歴になります。
入力単位のインデックスで、1入力が複数文になった場合、複数文の出力を一度に返します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","文字列","No","入力番号。未指定時はindex='1'と同義。"

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

<response />は、<response index="1" />と同義です。

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

| 複数単語からなる文字列に対して、最初以外の単語列を返します。firstの逆の動作です。取得に失敗した場合、 `get <#get>`__ と同様に、Config等で設定された"default-get"の値が返ります。
| 例えば、"山田 太郎"の場合、"太郎"が返ります。
| RDFナレッジベースの検索結果に適用した場合、結果リスト内の先頭以外のデータを取得します。 詳細は、:doc:`RDFサポート<RDF_Support>` を参照してください。

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


関連項目: `first <#first>`__


.. _template_set:

set 
---------
[1.0]

template内のset要素は、グローバル変数とローカル変数の設定を行うことができます。
変数型：name/var/dataの差異は、 `get <#get>`__ を参照してください。


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

select 
------------
[2.0]

| 起動時に参照するRDFファイルの内容と、addtripleで追加されたRDFナレッジベースを検索し、該当する情報を取得します。
| RDFファイルとして、configで指定したディレクトリに格納されたファイルを参照します。
| 詳細は :doc:`RDFサポート<RDF_Support>` を参照してください。

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

関連項目: `addtriple <#addtriple>`__, `deletetriple <#deletetriple>`__, `uniq <#uniq>`__, :doc:`RDFサポート<RDF_Support>`, :ref:`ファイル管理：rdfs<storage_entity>`

sentence 
--------------
[1.0]

文章の最初の単語を大文字にし、他のすべての単語を小文字に設定します

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

<sentence />は<sentence><star /></sentence>と同義です。

* 使用例

.. code:: xml

   <category>
       <pattern>CORRECT THIS *</pattern>
       <template>
           <sentence />
       </template>
   </category>

| Input: CORRECT THIS PleAse tEll Us The WeSthEr ToDay.
| Output: Please tell us the weather today.


size 
----------
[1.0]

Botのカテゴリ数を返します。

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

space
-----------
[custom]

space要素は、文生成時に半角スペースを挿入します。

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


split 
-----------
[2.1]

split要素は、Botの応答を複数の部分に分割するために利用します。 
分割されたメッセージは別々のメッセージとして扱われます。

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
| Output: 今日はいい天気ですね。
| Output: 明日も晴れるといいですね。


sr 
--------
[1.0]

``<srai><star/></srai>`` の省略形です。

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

.. _srai:

srai 
----------
[1.0]

srai要素は、srai要素で括った文章でパターンマッチを再実行します。
"srai"自体には正式な意味を持ちませんが、symbolic reduction、もしくは、symbolic recursionという意味になります。

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

関連項目: `star <#star>`__, `sr <#sr>`__, `sraix <#sraix>`__


.. _template_sraix:

sraix 
-----------
[2.0]

外部とのREST通信用APIを呼び出します。サブエージェント呼び出しに利用します。
sraixの利用方法の詳細は :doc:`SubAgent<SubAgent>` を参照してください。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "service","文字列","No","カスタム外部サービスのサービス名。"
    "botid","文字列","No","対話プラットフォームで公開されているbot名。"

* 使用例

.. code:: xml

   <category>
       <pattern>*から*までの乗り換え案内</pattern>
       <template>
           <sraix service="myService">
               <star/>
               <star index="2"/>
           </sraix>
       </template>
   </category>

| Input: 東京から大阪までの乗り換え案内
| Output: 10:00発の、のぞみが一番早く着きます。

関連項目: `star <#star>`__, `sr <#sr>`__, :doc:`SubAgent<SubAgent>`

star 
----------
[1.0]

| star要素はワイルドカードで取得したユーザ入力を利用するための記述です。
| ワイルドカードは、1つ以上の文字列を指定する ``*``, ``_``、もしくは0以上の文字列を指定する ``^``, ``#`` に該当する文字列を意味し、先頭から順にIndex(1〜)が振られます。
| また、pattern要素の ``set``、``iset``、``regex``、``bot`` 要素に該当する文字列もIndexの対象になります。
| 該当する情報がなかった場合、 空文字列が返ります。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","文字列","No","入力番号。未指定時はindex='1'と同義。"


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

system 
------------
[1.0]

system要素を使用すると、システムコールを行うことができます。
この要素はセキュリティ上の懸念があり、ユーザがオペレーティングシステムを知っていてシェルスクリプトを注入できれば、基本システムへ自由にアクセスできるようになります。
デフォルトではAIMLとしての記述は無効になっています。設定オプション ``allow_system_aiml`` をtrueに設定すると有効になります。

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

that要素は、``category`` の子要素としても定義があり、botの以前の応答との一致判定に利用されますが、 
templateの要素として指定された場合の ``that`` は、Botからの過去の応答文を取得する要素として作用します。

* 属性

.. csv-table::
    :header: "パラメータ","タイプ","必須","説明"
    :widths: 10,10,5,75

    "index","文字列","No","入力番号。未指定時はindex='1'と同義。"


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

関連項目: :ref:`that(pattern)<pattern_that>`, `thatstar <#thatstar>`__, `topicstar <#topicstar>`__


thatstar 
--------------
[1.0]

| thatstarは、thatに対するワイルドカード指定として利用します。
| patternに対する ``<star />`` と同じ方法でアクセスされますが、 ``pattern`` のワイルドカードではなく ``that`` に含んだワイルドカードを利用する際に ``<thatstar />`` を使用します。
| 取得に失敗した場合、 空文字列が返ります。

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

関連項目: :ref:`that(pattern)<pattern_that>`, `that <#that>`__, `topicstar <#topicstar>`__

.. _template_think:

think
-----------
[1.0]

think要素は、内容をユーザに表示せずに、Bot内での処理を記載する要素です。

* 使用例

.. code:: xml

   <category>
       <pattern>私の名前は * です</pattern>
       <template>
          <think><set name="name"><star /></set></think>
          あなたの名前を覚えました。
       </template>
   </category>

topicstar 
---------------
[1.0]

| topicstar要素は、``topic`` に対するワイルドカードとして利用します。
| patternに対する ``<star />`` と同じ方法でアクセスされますが、patternのワイルドカードではなくtopicに含んだワイルドカードを利用する際に ``<topicstar />`` 使用します。
| 属性に、"index"の指定も可能です。尚、取得に失敗した場合、 空文字列が返ります。

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



関連項目: :ref:`topic(pattern)<pattern_topic>`, `thatstar <#thatstar>`__

uniq 
----------
[2.0]

| uniqは `select <#select>`__ と同様に、起動時に参照するRDFファイルの内容と、addtripleで追加されたRDFナレッジベースを検索し、該当する情報を取得します。
| RDFファイルとして、configで指定したディレクトリに格納されたファイルを参照します。
| selectとの違いは、selectは検索結果の複数候補がそのまま返るのに対し、uniqは重複した候補を除外した結果を返す点です。 
| 詳細は :doc:`RDFサポート<RDF_Support>` を参照してください。

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
| Input: 探して バラ科 は? 
| Output: 桜 苺

関連項目: `addtriple <#addtriple>`__, `deletetriple <#deletetriple>`__, `select <#select>`__, :doc:`RDFサポート<RDF_Support>`, :ref:`ファイル管理：rdfs<storage_entity>`

uppercase 
---------------
[1.0]

半角英字を大文字にします。

* 使用例

.. code:: xml

   <category>
       <pattern>こんにちは * です</pattern>
       <template>
           こんにちは<uppercase><star /></uppercase>さん
       </template>
   </category>

<uppercase />は、<uppercase><star /></uppercase>と同義です。

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

Botの単語数を返します。

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

video要素を使用すると、ビデオの情報を返すことができます。
ビデオのURLやファイル名を指定することができます。

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

``<word>`` は内部要素で、AIMLとしては記述できませんが、文中の個々の単語はwordノードに展開され処理されます。

* 使用例

.. code:: xml

   <category>
       <pattern>HELLO</pattern>
       <template>Hi there!</template>
   </category>

この使用例の場合、HELLOがwordノードに展開されます。

xml 
---------
[1.0]

``<XML>`` という、名称の要素は存在しませんが、XML形式の記述でAIMLとして未定義の要素は未処理になります。
templateの応答の一部として、XML形式の要素を記載することができます。

* 使用例

.. code:: xml

   <category>
       <pattern> * をボールド表示</pattern>
       <template>
           <bold><star /></bold>
       </template>
   </category>
