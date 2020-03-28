RDFサポート
===========================================

| RDF(Resource Descriptor Framework)はW3Cが勧告する、XMLをベースにメタコンテンツを記述するための規格です。
| RDFで基本となるデータ構造は"triple"と呼ばれる3つのデータの組で、主語(subject)、述語(predicate)、目的語(object)を組み合わせで表されています。
| 詳細は、 `RDF Concepts And Abstract <https://www.w3.org/TR/2004/REC-rdf-concepts-20040210/>`__ を参照してください。


| 関連するAIMLのタグは、addtriple、deletetriple、select、tuple、uniqです。



AIML tripleファイル
----------------------------------------

| "triple"は、Subject、Predicate、Objectをコロン '：'で区切った1行のテキストです。本プログラムではこの1行をエレメントと称します。
| RDF定義を飛行機の例で説明します。
| この定義には、"subject"が"AIRPLANE"として記載されており、多数の"predicate"でそれぞれの値("object")を説明しています。

::

   AIRPLANE:hasPurpose:to transport us through the air
   AIRPLANE:hasSize:9
   AIRPLANE:hasSpeed:12
   AIRPLANE:hasSyllables:1
   AIRPLANE:isa:Aircraft
   AIRPLANE:isa:Transportation
   AIRPLANE:lifeArea:Physical

AIML RDF タグ
----------------------------------------

| AIMLには、"addtriple", "deletetriple", "select", "uniq" といった作成、削除、検索を行うタグが定義されています。
| また、検索結果を処理するには、"set", "get", "first", "rest" 等のタグも使用できます。

エレメントロード
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| 本プログラムでは、事前に用意したファイルをロードする機能があります。
| configのrdf_storageに設定している条件で、RDFの定義ファイルを読み込みます。

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

本セクションの説明では、以下の内容を記述した定義ファイルを使用するものとします。

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

エレメント生成
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ローディングデータだけではなく、addtriple要素を利用し動的に新しい"triple"を追加できます。

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

ただし、addtripleで追加したデータは永続的ではありません。

エレメント削除
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

addtriple要素を用いて追加したデータは、deletetriple要素を用いて削除できます。
これは、addtripleで追加されたエレメントだけではなく、ファイルから読み込んだエレメントも対象になります。

.. code:: xml

   <deletetriple>
     <subj>cow</subj><pred>sound</pred><obj>moo</obj>
   </deletetriple>
   <deletetriple>
     <subj>ant</subj><pred>sound</pred><obj>scratch</obj>
   </deletetriple>

| 3つの要素（subject、predicate、object）を指定すると、全て合致したエレメントのみが削除されます。
| subjectとpredicateだけを指定した場合、objectの値に関係なく、合致するエレメントを削除します。
| subjectだけを指定した場合、そのsubjectに合致する全てのエレメントを削除します。

検索
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
select要素を用いてRDFの検索を行います。

単純検索
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

単純な検索の場合、<q>要素の内容として、subject、predicate、objectの3つを指定すると、合致した結果として登録されている内容をリスト型で返します。

.. code:: xml

   <select>
       <q><subj>dog</subj><pred>sound</pred><obj>woof</obj></q>
   </select>

| 該当する情報が存在する場合、以下の結果が返ってきます。
| [[[["subj", "DOG"], ["pred", "SOUND"], ["obj", "woof"]]]]

特定の１つの要素のみを取得する場合、以下の記述ができます。

.. code:: xml

   <select>
       <q><subj>dog</subj><pred>sound</pred><obj>?</obj></q>
   </select>

| この場合、"?"を指定した要素の内容を示す、以下の結果が返ってきます。
| [[["?", "woof"]]]

変数による検索
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 複数の要素を返す場合や、一致する要素のリストを受け取る場合は、変数を使用する必要があります。
| 変数はvarsタグの内容で定義し、変数名の接頭辞として "?" を付けます。
| クエリ<q>でtripleの要素のタグに変数を設定することができます。
| 以下の場合、変数：?x はsubject、変数：?y はpredicate、変数：?z はobjectの格納対象となります。

.. code:: xml

   <select>
       <vars>?x ?y ?z</vars>
       <q><subj>?x</subj><pred>?y</pred><obj>?z</obj></q>
   </select>

| 指定したデータに一致するすべてのtripleから、変数に該当するデータを取得することができます。
| 以下は、動物の足の本数(legs)を検索する例です。

.. code:: xml

   <select>
       <vars>?x ?y</vars>
       <q><subj>?x</subj><pred>legs</pred><obj>?y</obj></q>
   </select>

| 検索結果が合致した場合、以下のような結果が返ってきます。
| [[["?x", "ANT"], ["?y", "6"]], [["?x", "BAT"], ["?y", "2"]], [["?x", "BEAR"], ["?y", "4"]], [["?x", "BUFFALO"], ["?y", "4"]], [["?x", "CAT"], ["?y", "4"]], [["?x", "CHICKEN"], ["?y", "2"]], [["?x", "DOLPHIN"], ["?y", "0"]], [["?x", "FISH"], ["?y", "0"]]]

複合条件検索
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| より複雑な検索を行う必要がある場合は、複数のクエリを連鎖させることができ、それぞれが'and'クエリとして結合されます。
| 下記の2つのタイプのクエリは、<q>タグに一致する結果の中で、<notq>タグと一致しない結果を返します

.. code:: xml

   <select>
       <vars>?x ?y ?z</vars>
       <q><subj>?x</subj><pred>legs</pred><obj>?y</obj></q>
       <notq><subj>?z</subj><pred>legs</pred><obj>0</obj></notq>
   </select>




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
| [[["?x", "BAT"], ["?y", "eee"]], [["?x", "BEAR"], ["?y", "grrrrr"]], [["?x", "BUFFALO"], ["?y", "moo"]], [["?x", "CAT"], ["?y", "meow"]], [["?x", "CHICKEN"], ["?y", "cluck cluck"]], [["?x", "DOLPHIN"], ["?y", "meep meep"]], [["?x", "FISH"], ["?y", "bubble bubble"]], [["?x", "DOG"], ["?y", "woof"]]]

前述の'select'要素から生成されたデータを取得する場合、'tuple'要素を利用し'get'タグを子要素として取得します。

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

| この例の場合、getの'var'アトリビュートに、select要素で指定した変数 "?x" を指定しています。
| 次に、tupleタグで、select要素の結果を格納した"tuples"(リストオブジェクト)を指定することで、"tuples"の中から、変数 "?x" に該当するデータが取得できます。
| 結果として、変数 "?x" からは以下の内容が取得できます。
| BAT BEAR BUFFALO CAT CHICKEN DOLPHIN FISH DOG
| 同様に、変数 "?y" からは以下の内容が取得できます。
| eee grrrrr moo meow cluck cluck meep meep bubble bubble woof

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
| grrrrr moo meow cluck cluck meep meep bubble bubble woof
