RDFサポート
===========================================

| RDF(Resource Descriptor Framework)はW3Cが勧告する、XMLをベースにメタコンテンツを記述するための規格です。
| RDFで基本となるデータ構造は ``triple`` と呼ばれる3つのデータの組で、主語(subject)、述語(predicate)、目的語(object)の組み合わせで表されます。
| 詳細は、 `RDF Concepts And Abstract <https://www.w3.org/TR/2004/REC-rdf-concepts-20040210/>`__ を参照してください。

| tripleのデータは、subject、predicate、objectをコロン '：'で区切ったテキストで表記することができます。
| 以下は、飛行機に関する情報をtriple化して列記した例で、"AIRPLANE"という"subject"に対し、複数の"predicate"と、predicateに対応する"object"が定義できます。

::

   AIRPLANE:hasPurpose:to transport us through the air
   AIRPLANE:hasSize:9
   AIRPLANE:hasSpeed:12
   AIRPLANE:hasSyllables:1
   AIRPLANE:isa:Aircraft
   AIRPLANE:isa:Transportation
   AIRPLANE:lifeArea:Physical

| 本対話エンジンのRDFサポートは、W3C勧告にある外部リソースを利用したデータ処理ではなく、内部に保持するtripleデータ群（以降、’RDFデータ'と表記）を対象として処理を行います。
| また、検索機能についても、単純な検索に対応することを目的としており、検索エンジンが提供している複雑な検索処理には対応していません。


RDFデータ処理用のAIMLタグ
----------------------------------------

本対話エンジンでは、template要素でのRDFデータ処理用として以下のAIMLタグが使用できます。

- ``addtriple`` ： RDFデータの追加。
- ``deletetriple`` ： RDFデータの削除。
- ``uniq`` ： RDFデータに対する検索（単一結果出力用）。
- ``select`` ：  RDFデータに対する検索（複合結果出力用）。

また、検索結果を変数に出力した場合には、以下のAIMLタグが使用できます。

- ``get`` ： 検索結果データを取得。一部のデータのみを取得する場合、子要素：tuple を利用します。
- ``first`` ： 検索結果データから、先頭の検索結果を取得します。
- ``rest`` ： 検索結果データから、２番目以降の検索結果を取得します。
- ``set`` ： 検索結果データの他変数への代入。


RDFデータ管理
----------------------------------------

対話エンジンでは、subject、predicate、objectの組み合わせを一意のデータとして管理し、エレメントと称します。
RDFデータの各エレメントは、対話エンジン起動時の初期ロードとともに、シナリオでの操作により追加・削除が行えます。

初期ロード
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

対話エンジンの起動時に、事前に用意したファイル群から各エレメントを登録します。

対象ファイルの指定は、:ref:`ファイル管理のエンティティ<storage_entity>` の ``rdf`` で行い、該当する全ての定義ファイルを読み込んで、エレメントを展開します。

＊特定のフォルダ配下の '\*.txt' から展開する場合の例（サブフォルダも利用可能）。

.. code:: yaml

    client_type:
        storage:
            entities:
                rdf: file

            stores:
                file:
                    type: file
                    config:
                        rdf_storage:
                            dirs: ../storage/rdfs
                            subdirs: true
                            extension: txt


| 定義ファイルは、エレメント毎に、subject、predicate、objectをコロン '：'で区切って列記します。subject、predicate、objectのいずれかが空文字の場合は、無効になります。
| 同じ組み合わせのエレメントが複数指定されている場合でも、重複登録は行いません。
| なお、RDFデータに対する検索を効率的に行うため、登録されたデータは全て文字コードを変換して登録します（英字：半角大文字、数記号：半角、カタカナ：全角）。

以降では、次の記述をした定義ファイルを初期ロードしたものとして説明します。

::

   ant:legs:6
   ant:sound:scratch
   bat:legs:2
   bat:sound:eee
   bear:legs:4
   bear:sound:grrrrr
   buffalo:legs:4
   buffalo:sound:moo
   cat:legs:4
   cat:sound:meow
   chicken:legs:2
   chicken:sound:cluck cluck
   dolphin:legs:0
   dolphin:sound:meep meep
   fish:legs:0
   fish:sound:bubble bubble


.. _rdf_add_element:

エレメント追加
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

template要素の :ref:`addtriple<template_addtriple>` 要素を利用することで、動的に新しいエレメントを追加できます。記述形式は、以下になります。

.. code:: xml

   <addtriple>
     <subj>Subject</subj><pred>Predicate</pred><obj>Object</obj>
   </addtriple>

動物の特性を追加する例です。

.. code:: xml

   <addtriple>
       <subj>cow</subj><pred>sound</pred><obj>moo</obj>
   </addtriple>
   <addtriple>
       <subj>dog</subj><pred>sound</pred><obj>woof</obj>
   </addtriple>

初期ロードと同様に文字コード変換を行って登録するため、以下のRDFデータが登録されます。

::

    COW:SOUND:MOO
    DOG:SOUND:WOOF

なお、addtripleで追加したデータは永続的ではなく、対話エンジンを再起動した時点で初期状態に戻ります。


エレメント削除
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

template要素の :ref:`deletetriple<template_deletetriple>` 要素を利用することで、addtriple要素で追加したエレメントだけではなく、初期ロードしたエレメントも削除できます。記述形式は、以下になります。

.. code:: xml

   <deletetriple>
     <subj>Subject</subj><pred>Predicate</pred><obj>Object</obj>
   </deletetriple>

| 3つの要素（subject、predicate、object）を指定すると、全て合致したエレメントのみが削除されます。
| subjectとpredicateだけを指定した場合、objectの値に関係なく、合致するエレメントを削除します。
| subjectだけを指定した場合、そのsubjectに合致する全てのエレメントを削除します。

addtripleで追加したエレメントと、初期ロードしたエレメントを削除する例です。

.. code:: xml

   <deletetriple>
     <subj>cow</subj><pred>sound</pred><obj>moo</obj>
   </deletetriple>
   <deletetriple>
     <subj>ant</subj><pred>sound</pred><obj>scratch</obj>
   </deletetriple>

なお、deletetripleも永続的ではなく、対話エンジンを再起動した時点で初期状態に戻ります。


RDFデータの検索
----------------------------------------

RDFデータの検索要素には２種類がありますが、それぞれの仕様は以下のように異なります。

- ``uniq要素``

   １つの検索条件を指定し、検索結果に対して重複した文字列を除外し、空白区切りの結合した文字列を返します。
   
- ``select要素``

   複数の検索条件の指定が可能で、検索結果をエレメントを単位とした複数候補をリスト化した文字列で返します。変数を使用することで関連づけた検索が可能です。


uniqによる検索
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| template要素の :ref:`uniq<template_uniq>` 要素を用いて、RDFデータの検索を行います。
| uniqでの検索では、取得したい項目に対して '?' を指定します。

特定の情報を取得する場合、以下の様に記述します。

.. code:: xml

   <uniq>
       <subj>ant</subj>
       <pred>sound</pred>
       <obj>?</obj>
   </uniq>

| 結果として、subject、predicateが一致するエレメントのobjectの値が取得できます。
| SCRATCH

検索時にエレメント要素の指定を省略することが可能で、subjectの一覧を取得する場合、以下の様に記述します。

.. code:: xml

   <uniq>
       <subj>?</subj>
   </uniq>

| 結果として、subjectの一覧が空白区切りで出力されます。uniqでは重複する文字列は除外される為、一覧として参照できます。
| ANT BAT BEAR BUFFALO CAT CHICKEN DOLPHIN FISH

特定の条件を指定する例として、predicate=”legs”の条件で、objectの一覧を取得します。

.. code:: xml

   <uniq>
       <pred>legs</pred>
       <obj>?</obj>
   </uniq>

| 結果として、重複したものは除外される為、以下の様になります。
| 6 2 4 0


selectによる検索
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| template要素の :ref:`select<template_select>` 要素を用いて、RDFデータの検索を行います。
| selectの検索結果は、["要素名", "値”] を最小単位として、エレメント毎に格納される為、以下の様なリスト形式の文字列になります。
| [[["subj", "ANT"], ["pred", "LEGS"], ["obj", "6"]], [["subj", "ANT"], ["pred", "SOUND"], ...], ...]

selectで使用可能なクエリ用の子要素には <q>とともに、<notq>がありますが、<notq>は、子要素で指定された条件に合致しない全てのエレメントを対象としますので大量のデータを出力することになります。


単純検索
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

単純な検索の場合、<q>要素の内容として、subject、predicate、objectの3つを指定すると、合致した結果として登録されている内容をリスト型で返します。

.. code:: xml

   <select>
       <q><subj>dog</subj><pred>sound</pred><obj>woof</obj></q>
   </select>

| 初期ロード状態では結果が空文字になる為、取得失敗時の応答文が返りますが、:ref:`エレメント追加<rdf_add_element>` を実施すると、以下の結果が返ってきます。
| [[["subj", "DOG"], ["pred", "SOUND"], ["obj", "WOOF"]]]

特定の１つの要素のみを取得する場合、以下の記述ができます。

.. code:: xml

   <select>
       <q><subj>cat</subj><pred>sound</pred><obj>?</obj></q>
   </select>

| この場合、"?"を指定した要素の内容を示す、以下の結果が返ってきます。
| [[["?", "MEOW"]]]


変数による検索
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 複数の要素を返す場合や、一致する要素のリストを受け取る場合は、変数を使用する必要があります。
| 変数はvarsタグの内容で定義し、変数名の接頭辞として "?" を付けます。
| クエリ<q>でエレメントの要素に変数を設定することができます。
| 以下の場合、変数：?x はsubject、変数：?y はpredicate、変数：?z はobjectの格納対象となります。

.. code:: xml

   <select>
       <vars>?x ?y ?z</vars>
       <q><subj>?x</subj><pred>?y</pred><obj>?z</obj></q>
   </select>

| 指定したデータに一致するすべてのエレメントから、変数に該当するデータを取得することができます。
| 以下は、動物の足の本数(legs)を検索する例です。

.. code:: xml

   <select>
       <vars>?x ?y</vars>
       <q><subj>?x</subj><pred>legs</pred><obj>?y</obj></q>
   </select>

| 検索結果が合致した場合、以下の様に変数に該当する情報のみの結果が返ってきます。
| [[["?x", "ANT"], ["?y", "6"]], [["?x", "BAT"], ["?y", "2"]], [["?x", "BEAR"], ["?y", "4"]], [["?x", "BUFFALO"], ["?y", "4"]], [["?x", "CAT"], ["?y", "4"]], [["?x", "CHICKEN"], ["?y", "2"]], [["?x", "DOLPHIN"], ["?y", "0"]], [["?x", "FISH"], ["?y", "0"]]]


複合条件検索
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 変数を使用した場合には、複数のクエリを使用することで条件を絞り込むことができます。
| 以下の例では、１つ目のクエリで取得したsubjectのリストと、２つ目のクエリで取得したsubjectのリスト中で、共通するものが変数：?xとして、出力されます。

.. code:: xml

   <select>
       <vars>?x</vars>
       <q><subj>?x</subj><pred>legs</pred><obj>0</obj></q>
       <q><subj>?x</subj><pred>sound</pred></q>   
   </select>

| 結果として、predicate="legs"でobject="0"が定義されたsubjectの中で、predicate="sound"が定義されているsubjectのリストが返ってきます。
| [[["?x", "DOLPHIN"]], [["?x", "FISH"]]]

| さらに変数を使用することで、該当するsubjectの中から特定の要素の情報を取得することができます。以下の例では、objectを変数：?yで出力します。

.. code:: xml

   <select>
       <vars>?x ?y</vars>
       <q><subj>?x</subj><pred>legs</pred><obj>0</obj></q>
       <q><subj>?x</subj><pred>sound</pred><obj>?y</obj></q>   
   </select>

| 結果は次の様になります。
| [[["?x", "DOLPHIN"], ["?y", "MEEP MEEP"]], [["?x", "FISH"], ["?y", "BUBBLE BUBBLE"]]]


データ取得
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| select要素は、SQLのSELECT文のようにデータセットを作成するために使用されます。
| 以下の例では、select要素の取得結果を'set'タグを用いて、tuplesに格納しています。

.. code:: xml

   <set var="tuples">
       <select>
           <vars>?x ?y</vars>
           <q><subj>?x</subj><pred>sound</pred><obj>?y</obj></q>
       </select>
   </set>

| この場合に、tuplesに以下の内容が設定されています。
| [[["?x", "BAT"], ["?y", "EEE"]], [["?x", "BEAR"], ["?y", "GRRRRR"]], [["?x", "BUFFALO"], ["?y", "MOO"]], [["?x", "CAT"], ["?y", "MEOW"]], [["?x", "CHICKEN"], ["?y", "CLUCK CLUCK"]], [["?x", "DOLPHIN"], ["?y", "MEEP MEEP"]], [["?x", "FISH"], ["?y", "BUBBLE BUBBLE"]], [["?x", "DOG"], ["?y", "WOOF"]]]

前述の'select'要素から生成されたデータを取得する場合、'get'タグの子要素'tuple'を利用して取得します。

.. code:: xml

   <get var="?x">
       <tuple>
           <get var="tuples" />
       </tuple>
   </get>
   <get var="?y">
       <tuple>
           <get var="tuples" />
       </tuple>
   </get>

| この例の場合、getの'var'属性に、select要素で指定した変数 "?x" を指定しています。
| 次に、tupleタグで、select要素の結果を格納した"tuples"(リストオブジェクト)を指定することで、"tuples"の中から、変数 "?x" に該当するデータが取得できます。
| 結果として、変数 "?x" からは以下の内容が取得できます。
| BAT BEAR BUFFALO CAT CHICKEN DOLPHIN FISH DOG
| 同様に、変数 "?y" からは以下の内容が取得できます。
| EEE GRRRRR MOO MEOW CLUCK CLUCK MEEP MEEP BUBBLE BUBBLE WOOF

また、"tuples"に対して、firstタグ、restタグを利用することで、以下の様に部分的な結果を取得することもできます。

.. code:: xml

   <get var="?x">
       <tuple>
           <first><get var="tuples" /></first>
       </tuple>
   </get>
   <get var="?y">
       <tuple>
           <rest><get var="tuples" /></rest>
       </tuple>
   </get>

| 結果として、firstタグ(先頭データ取得)で、変数 "?x" から取得した値は以下になります。
| BAT

| 同様に、restタグ(先頭以外のデータ取得)で、変数 "?y" から取得した値は以下になります。
| GRRRRR MOO MEOW CLUCK CLUCK MEEP MEEP BUBBLE BUBBLE WOOF