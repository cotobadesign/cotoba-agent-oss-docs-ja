================================
ファイル管理
================================

| 対話プラットフォームで使用するファイルに関して説明します。
| 以降の説明では、主に、対話プラットフォームで利用するファイル管理方式として、ローカルファイルを指定しています。
| ファイル管理方式には、ローカルファイル以外にも、データベースでの管理もありますが、構成は同じになります。

定義ファイルの拡張子は、シナリオファイルの拡張子はaiml、シナリオ以外の設定ファイルの拡張子はtxtです。

ディレクトリ構成
================================

 シナリオファイルのディレクトリ構成は以下のとおりです。


.. code::

  storage 各種ファイルディレクトリ
  ├── braintree AIML展開ファイルディレクトリ
  ├── categories シナリオファイルディレクトリ
  │   └── *.aiml
  ├── conversations 対話履歴情報ディレクトリ
  │   └── *.conv
  ├── debug 対話履歴情報ディレクトリ
  │   ├── duplicates.txt
  │   ├── errors.txt
  │   └── *.log
  ├── lookups 置換処理用ファイルディレクトリ
  │   ├── regex.txt
  │   ├── denormal.txt
  │   ├── gender.txt
  │   ├── normal.txt
  │   ├── person.txt
  │   └── person2.txt
  ├── maps map要素利用ファイルディレクトリ
  │   └── *.txt
  ├── nodes map要素利用ファイルディレクトリ
  │   ├── pattern_nodes.conf
  │   └── template_nodes.conf
  ├── properties プロパティリスト用ファイルディレクトリ
  │   ├── nlu_servers.yaml
  │   ├── defaults.txt
  │   └── properties.txt
  ├── security セキュリティ用ファイルディレクトリ
  │   └── usergroups.yaml
  └── sets set利用ファイルディレクトリ
      └── *.txt


.. _storage_entity:

エンティティ
================================

| 対話プラットフォームでは、格納するデータの種類によってエンティティを規定し、各々でファイル管理を行います。
| 利用するファイルの構成は、コンフィグファイル(config.yaml)で定義し、どのデータの種類を利用するかを、以下のエンティティを設定するか否かで指定します。
| 尚、エンティティには、共通的に使用するものと、特定ノードのみで使用するものがありますが、定義や管理の方法は同じです。
| 以下のエンティティの中で、"自動生成"が”No"のデータは編集が可能で、起動時に展開して利用します。
| "単一ファイル"が"Yes"のデータは対象ファイルを1つだけ指定でき、"No"のファイルはディレクトリ指定を行い複数のファイルを指定します。

-  共通のエンティティ

.. csv-table::
    :header: "エンティティ","格納内容","単一ファイル","自動生成","説明"
    :widths: 18,40,10,5,65

    "binaries","ツリーデータ","No","Yes","AIMLを展開した検索用ツリーのバイナリデータを格納します。"
    "braintree","ツリーデータ","No","Yes","AIMLを展開した検索用ツリーのXML形式データを格納します。"
    "categories","対話用AIML","No","No","対話の制御に使用するAIMLファイル群を格納します。"
    "conversations","対話履歴情報","No","Yes","ユーザ毎の過去を含めた対話処理内容(変数値を含む)の履歴情報ファイルを格納します。"
    "defaults","プロパティリスト","Yes","No","初期起動時に展開するグローバル変数(name)の定義ファイルを格納します。"
    "duplicates","重複指定情報","Yes","Yes","(デバッグ用）AIMLファイル展開時のcategory重複情報ファイルを格納します。"
    "errors","エラー情報","Yes","Yes","(デバッグ用）AIMLファイル展開時のエラー情報ファイルを格納します。"
    "license_keys","ライセンスキー","Yes","No","外部接続等で必要になるライセンスキーファイルを格納します。"
    "pattern_nodes","クラス定義リスト","Yes","No","patternの各ノードの処理クラス定義ファイルを格納します。"
    "postprocessors","クラス定義リスト","Yes","No","レスポンスの応答文に対して編集を行う場合の処理クラス定義ファイルを格納します。"
    "preprocessors","クラス定義リスト","Yes","No","リクエストのユーザ発話に対して編集を行う場合の処理クラス定義ファイルを格納します。"
    "properties","プロパティリスト","Yes","No","patternの :ref:`bot<pattern_bot>`、及び、templateの :ref:`bot<template_bot>` ノードで使用するプロパティ定義ファイルを格納します。"
    "spelling_corpus","スペルチェック情報","No","No","スペルチェックを行う場合のもとになるコーパスファイルを格納します。"
    "template_nodes","クラス定義リスト","Yes","No","templateの各ノードの処理クラス定義ファイルを格納します。"

-  Patternノード用のエンティティ

.. csv-table::
    :header: "エンティティ","格納内容","単一ファイル","自動生成","説明"
    :widths: 18,40,10,5,65

    "regex_templates","正規表現リスト","Yes","No",":ref:`regex<pattern_regex>` ノードのtemplate指定で使用する正規表現リストファイルを格納します。"
    "sets","対象単語リスト","No","No",":ref:`set<pattern_set>` ノードで使用する単語リストファイルを格納します。"

-  Templateノード用のエンティティ

.. csv-table::
    :header: "エンティティ","格納内容","単一ファイル","自動生成","説明"
    :widths: 18,40,10,5,65

    "denormal","変換辞書","Yes","No",":ref:`denormalize<template_denormalize>` ノードでの変換に使用する辞書ファイルを格納します。"
    "gender","変換辞書","Yes","No",":ref:`gender<template_gender>` ノードでの変換に使用する辞書ファイルを格納します。"
    "learnf","categoryリスト","No","YES",":ref:`learnf<template_learnf>` ノードの処理で作成されるcategories情報を、ユーザ毎に格納します。"
    "logs","ログ情報","No","YES",":ref:`log<template_log>` ノードの処理で作成されたログ情報を、ユーザ毎に格納します。"
    "maps","プロパティリスト","No","No",":ref:`map<template_map>` ノードでの変換に使用する辞書ファイルを格納します。"
    "normal","変換辞書","Yes","No",":ref:`normalize<template_normalize>` ノードでの変換に使用する辞書ファイルを格納します。"
    "person","変換辞書","Yes","No",":ref:`person<template_person>` ノードでの変換に使用する辞書ファイルを格納します。"
    "person2","変換辞書","Yes","No",":ref:`person2<template_person2>` ノードでの変換に使用する辞書ファイルを格納します。"
    "rdfs","RDFデータリスト","No","No",":doc:`RDF<RDF_Support>` 関連ノードの処理対象となるRDFデータの定義ファイルを格納します。"
    "rdf_updates","RDF更新情報","Yes","Yes","RDFデータの変更履歴情報を格納します。"
    "usergroups","セキュリティ情報","Yes","No",":ref:`authorise<template_authorise>` ノードで使用するロール定義ファイルを格納します。"



ローカルファイル利用時の定義例
================================

エンティティ定義
--------------------------------

| 以下の例は、``console`` という名前のクライアントを使用した場合のエンティティの定義例です。
| クライアントの設定では、 ``storage`` というセクションに、 ``entities`` というサブセクションがあります。
| ファイルの管理方法として、各エンティティ毎に入出力の制御を行う方式（ストア）の名称を指定します。
| ここでは、ストア方式名：``file`` を指定しています。

.. code:: yaml

   console:
     storage:
         entities:
             binaries: file
             braintree: file
             categories: file
             conversations: file
             defaults: file
             duplicates: file
             errors: file
             license_keys: file
             pattern_nodes: file
             postprocessors: file
             preprocessors: file
             properties: file
             spelling_corpus: file
             template_nodes: file
             regex_templates: file
             sets: file
             denormal: file
             gender: file
             learnf: file
             logs:   file
             maps: file
             normal: file
             person: file
             person2: file
             rdf: file
             rdf_updates: file
             usergroups: file

fileストレージエンジンの定義
--------------------------------------------

| 同じ storageセクションのサブセクションstoresで、ストア内での実処理を行うストレージエンジンを指定します。
| ここでは、ストア名：fileに対して、”type: file”で、ローカルファイルの入出力を行うストレージエンジンを利用することを指定しています。
| ローカルファイル入出力（file指定）の場合、実処理を行うストレージエンジンの名称は、 ``エンティティ名+'_storage'`` になります。
| エンティティ毎のストレージエンジンの設定は、以下のように 'config'サブセクションで行います。

.. code:: yaml

         stores:
             file:
                 type: file
                 config:
                   binaries_storage:
                     file: ./storage/braintree/braintree.bin
                   braintree_storage:
                     file: ./storage/braintree/braintree.xml
                   categories_storage:
                     dirs: ./storage/categories
                     subdirs: true
                     extension: aiml
                   conversations_storage:
                     dirs: ./storage/conversations
                   defaults_storage:
                     file: ./storage/properties/defaults.txt
                   duplicates_storage:
                     file: ./storage/debug/duplicates.txt
                   errors_storage:
                     file: ./storage/debug/errors.txt
                   license_keys_storage:
                     file: ./storage/licenses/license.keys
                   pattern_nodes_storage:
                     file: ./storage/nodes/pattern_nodes.conf
                   postprocessors_storage:
                     file: ./storage/processing/postprocessors.conf
                   preprocessors_storage:
                     file: ./storage/processing/preprocessors.conf
                   properties_storage:
                     file: ./storage/properties/properties.txt
                   spelling_corpus_storage:
                     file: ./storage/spelling/corpus.txt
                   template_nodes_storage:
                     file: ./storage/nodes/template_nodes.conf
                   regex_templates_storage:
                     file: ./storage/lookups/regex.txt
                   sets_storage:
                     dirs: ./storage/sets
                     extension: txt
                   denormal_storage:
                     file: ./storage/lookups/denormal.txt
                   gender_storage:
                     file: ./storage/lookups/gender.txt
                   learnf_storage:
                     dirs: ./storage/learnf
                   logs_storage:
                     dirs: ./storage/debug
                   maps_storage:
                     dirs: ./storage/maps
                     extension: txt
                   normal_storage:
                     file: ./storage/lookups/normal.txt
                   person_storage:
                     file: ./storage/lookups/person.txt
                   person2_storage:
                     file: ./storage/lookups/person2.txt
                   rdfs_storage:
                     dirs: ./storage/rdfs
                     subdirs: true
                     extension: txt
                   rdf_updates_storage:
                     dirs: ./storage/rdf_updates
                   usergroups_storage:
                     file: ./storage/security/usergroups.yaml

'config'サブセクションでの定義は、対象となるエンティティによって、単一ファイルや、複数ファイルの利用を示す記載方法をとります。

単一ファイルのエンティティの場合
------------------------------------------------

単一ファイルを指定するエンティティの場合、'file'アトリビュートでファイルパスを指定します。

.. code:: yaml

                   usergroups_storage:
                       file: ./storage/security/usergroups.yaml

複数ファイルの利用が可能なエンティテイの場合
------------------------------------------------------

複数ファイルが利用可能なエンティティの場合、以下の3つのアトリビュートを指定します。
ただし、自動生成対象のエンティティの場合、ディレクトリパスのみの指定となります。

- dirs: 対象ファイルディレクトリパスを指定。
- subdirs: 対象ファイルディレクトリは、以下のサブディレクトリをサーチするかどうかをtrue/falseで設定。
- extension: ロードするファイルタイプの拡張子を指定。

.. code:: yaml

                   categories_storage:
                     dirs: ./storage/categories
                     subdirs: true
                     extension: aiml

                   conversations_storage:
                     dirs: ./storage/conversations

尚、自動生成対象外で、複数ファイルを指定することが可能なストレージエンジンは、以下の４つになります。

- categories_storage
- sets_storage
- maps_storage
- rdfs_storage


データベース利用時の定義例
================================

データベースで管理する場合の例として、Redisを利用した例を以下に示します。

エンティティ定義
--------------------------------

Redisで管理するエンティティに対して、storageのentitiesサブセクションで、ストア方式名：``redis`` を指定しています。

.. code:: yaml

   console:
     storage:
         entities:
             binaries: file
             braintree: file
             categories: redis
             ：

Redisでの入出力が可能なエンティティは、以下のものになります。

- binaries ： AIMLを展開した検索用ツリーのバイナリデータを格納。
- braintree ： AIMLを展開した検索用ツリーのXML形式データを格納。
- conversations ： ユーザ毎の過去を含めた対話処理内容(変数値を含む)の履歴情報を格納。
- duplicates ： (デバッグ用）AIMLファイル展開時のcategory重複情報を格納。
- errors ： (デバッグ用）AIMLファイル展開時のエラー情報を格納。
- learnf ： :ref:`learnf<template_learnf>` ノードの処理で作成されるcategories情報を、ユーザ毎に格納。
- logs ： :ref:`log<template_log>` ノードの処理で作成されたログ情報を、ユーザ毎に格納。


Redisストレージエンジンの定義例
--------------------------------------------

| storageセクションのstoresのサブセクションで、ストア名：redisに対して、”type: redis”で、Redisに対する入出力を行うことを指定しています。
| 'config'サブセクションでは、Redisの利用に必要な共通パラメータを指定し、実際の入出力は、エンティティ毎のストレージエンジンでキー設定を含めて行います。

.. code:: yaml

         stores:
            redis:
                type: redis
                config:
                    host: localhost
                    port: 6379
                    password: xxxx
                    db: 0
                    prefix: programy
                    drop_all_first: false

定義ファイルの記述方法
================================

編集可能なファイルで、AIML以外に、使用することの多い定義ファイルについて記述方法を説明します。

プロパティリストファイル
--------------------------------

以下のエンティティで指定するファイルは、変数等への値設定を目的として ’名称: 値' の形式で記述します。

- defaults ： グローバル変数(name)の初期値を定義。
- properties : patternの :ref:`bot<pattern_bot>`、及び、templateの :ref:`bot<template_bot>` ノードで使用するbotのプロパティを定義。
- maps ： :ref:`map<template_map>` ノードで使用するキー／バリューの関係を定義。
- nlu_servers : NLUサーバに対する設定を行います。

defaults
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``defaults`` エンティティでは、シナリオで使用するグローバル変数(name)の値設定を初期起動時に行うことができます。
| ただし、ユーザ毎の対話情報履歴が存在する場合は、履歴上の最新値が反映されるため、本設定は無効になります。つまり、新規のユーザの利用時のみ有効です。
| 以下の例では、initial_variable(name変数)の値を"初期値"に指定します。

.. code:: 

  initial_variable:初期値


.. _storage_file_properties:

properties
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``properties`` エンティティは、botノードの情報を参照するとともに、botのデフォルトの動作をパラメータに設定します。

.. csv-table::
    :header: "エンティティ","内容","説明","デフォルト値"
    :widths: 10,30,30,30

    "name","ボット名",":ref:`bot<template_bot>` アトリビュートnameにnameを指定した際に取得できる値。","(未指定の場合default-getの値)"
    "birthdate","ボット作成日",":ref:`bot<template_bot>` アトリビュートnameにbirthdateを指定した際に取得できる値。","(未指定の場合default-getの値)"
    "grammar_version","グラマーバージョン",":ref:`bot<template_bot>` アトリビュートnameにgrammar_versionを指定した際に取得できる値。","(未指定の場合default-getの値)"
    "app_version","アプリバージョン",":ref:`bot<template_bot>` アトリビュートnameにapp_versionを指定した際に取得できる値。","(未指定の場合default-getの値)"
    "default-response","デフォルトレスポンス","マッチするpatternがなかった場合に返す応答文。","unknown"
    "default-get","デフォルトget","未定義変数に対し、getを行なった場合に取得できる文字列。","unknown"
    "joiner_terminator","文終端文字","応答文の語尾句等を自動的に付与する文字列を指定します。指定なしの場合何も付与しません。","。"
    "joiner_join_chars","文終端除外文字","joiner_terminatorの指定で文終端文字を自動付与する際に、joiner_terminator指定の文字を結合除外する文字列を指定します。 指定なしの場合、joiner_terminatorで指定した文字を付与します。",".?!。？！"
    "splitter_split_chars","文分割文字","内部的に文章分割を行う文字を指定します。指定された文字列が文中に含まれていると、複数文として扱い、responseに複数の応答文を結合した文字列を返します。ただし、metadataは最終文で設定した内容のみが返ります。指定なしの場合、 発話文を1文として扱います。","。"
    "punctation_chars","区切り文字","区切り文字扱いを行う文字を指定します。区切り文字はマッチング対象外とし発話文、応答文から除外した形でマッチング処理を行います。","(無し)"

* 設定例

.. code:: 

  name:基本応答
  birthdate:March 01, 2019

  grammar_version:0.0.1
  app_version: 0.0.1

  default-response: すみません、意味がわかりませんでした。
  default-get: わかりません
  version: v0.0.1

  joiner_terminator: 。 
  joiner_join_chars: .?!。？！
  splitter_split_chars:  。
  punctation_chars: ;'",!()[]：’”；、。！（）「」 


joiner_terminator
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

応答文の語尾句等を自動的に付与する文字列を指定します。
設定例に、"こんにちは"を指定した場合、

* 設定例

.. code:: 

  joiner_terminator: 。 

.. code:: xml

    <category>
        <pattern>こんにちは</pattern>
        <template>今日も元気に行きましょう</template>
    </category>


| Input: こんにちは
| Output: 今日も元気に行きましょう。


対話APIのレスポンスの応答文：responseの文末に自動的に付与される句点「。」を抑止する場合には、以下の定義を行ってください。
（":"の後ろに何も指定しないことで、無効化することができます。）

.. code:: 

  joiner_terminator: 

未指定にすると、応答文に句点が付与されません。

| Input: こんにちは
| Output: 今日も元気に行きましょう


joiner_join_chars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| joiner_terminatorの指定で文終端文字を自動付与する際に、joiner_terminator指定の文字を結合除外する文字列を指定します。
| joiner_join_chars未指定の場合、 応答文に"今日も元気に行きましょう。"、"いい気分ですね！"などの応答文を記載した場合に、joiner_terminator指定の文字を結合すると、
| "今日も元気に行きましょう。。"、"いい気分ですね！。"のように、応答文記載の文末文字に加えjoiner_terminatorで指定した木を結合した応答文が返ります。
| joiner_join_charsに結合除外文字を指定しておくと、"そうですね!"、"こんにちは。"と、joiner_terminatorを結合しない応答文を返します。

* 設定例

.. code:: 

  joiner_terminator: 。
  joiner_join_chars: .?!。？！

.. code:: xml

    <category>
        <pattern>こんにちは</pattern>
        <template>今日も元気に行きましょう。</template>
    </category>
    <category>
        <pattern>今日もいい天気ですね</pattern>
        <template>いい気分ですね！</template>
    </category>

| Input: こんにちは
| Output: 今日も元気に行きましょう。
| Input: 今日もいい天気ですね
| Output: いい気分ですね！


joiner_join_charsを未指定にすると、joiner_terminatorで指定した文字が必ず結合されます。

.. code::

  joiner_terminator: 。
  joiner_join_chars:

| Input: こんにちは
| Output: 今日も元気に行きましょう。。
| Input: 今日もいい天気ですね
| Output: いい気分ですね！。


splitter_split_chars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

内部的に文章分割を行う文字を指定します。
指定された文字列が文中に含まれていると、複数文として扱い、responseに複数の応答文を結合した文字列を返します。
splitter_split_charsに"。"を指定した場合、発話文が"こんにちは。今日もいい天気ですね。"のような1文が、
分割処理され "こんにちは"と"今日もいい天気ですね"の2文になります。

* 設定例

.. code::

  joiner_terminator: 。
  splitter_split_chars: 。

.. code:: xml

    <category>
        <pattern>こんにちは</pattern>
        <template>今日も元気に行きましょう</template>
    </category>
    <category>
        <pattern>今日もいい天気ですね</pattern>
        <template>いい気分ですね</template>
    </category>

| Input: こんにちは。今日もいい天気ですね。
| Output: 今日も元気に行きましょう。いい気分ですね。

splitter_split_charsを未指定にすると、発話文が分割されないため、"こんにちは。今日もいい天気ですね。"を一文としたマッチングを行い、前述のAIMLではマッチする発話がないため応答なしになります。

.. code:: 

  splitter_split_chars: 


| Input: こんにちは。今日もいい天気ですね。
| Output: すみません、意味がわかりませんでした。


punctation_chars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

入力文の区切り文字扱いを行う文字を指定します。区切り文字はマッチング対象外とし発話文から除外した形でマッチング処理を行います。
"こんにちは。"および"こんにちは"という入力がある場合、punctation_charsに指定された文字は、無視され同一発話扱いになります。

* 設定例

.. code:: 

  punctation_chars: ;'",!()[]：’”；、。！（）「」 

.. code:: xml

    <category>
        <pattern>こんにちは</pattern>
        <template>今日も元気に行きましょう</template>
    </category>

| Input: こんにちは。
| Output: 今日も元気に行きましょう。
| Input: こんにちは
| Output: 今日も元気に行きましょう。


punctation_charsを未指定にすると、"。"もマッチ対象となるため、"こんにちは。"と"こんにちは"は別発話扱いとなります。

.. code::

  punctation_chars:

| Input: こんにちは
| Output: 今日も元気に行きましょう。
| Input: こんにちは。
| Output: すみません、意味がわかりませんでした。



maps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``maps`` エンティティでは、mapノードの情報参照をファイル名（拡張子を除く）で行われるため、情報の種類毎にファイルを分けることが可能です。
| 以下の例は、都道府県と県庁所在地の関係を列記したprefectural_office.txtの例です。

.. code::

  東京都:東京
  東京:東京
  神奈川県:横浜市
  神奈川:横浜市
  大阪府：大阪市
  大阪：大阪市
   ：


nlu_servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``nlu_servers`` エンティティでは、エンティティでは、nluノードでのアクセス先URL(エンドポイント)とAPIキーを設定します。
| nluノードで利用するアクセス先URL(エンドポイント)とAPIキーについては、COTOBA DESIGNにお問い合わせください。(https://www.cotoba.net)
| 以下の例は、2つのURLの設定を行った例です。1つ目のURLはAPIキー設定なし、2つ目のURLはAPIキーを設定しています。

.. code:: yaml

  nlu:
    - url: http://localhost:5201/run
    - url: http://localhost:3000/run
      apikey: test_key


単語リストファイル
--------------------------------

以下のエンティティで指定するファイルでは、処理対象となる単語・文字列を列記します。

- sets ： :ref:`set<pattern_set>` ノードで使用するマッチ処理対象の単語リストを定義。

| ``sets`` エンティティでは、setノードでの情報参照がファイル名（拡張子を除く）で行われるため、情報の種類毎にファイルを分けることが可能です。
| 尚、日本語の場合、マッチ処理時に行う単語分割の結果によって一致しない場合が発生するため、単語ではなく文字列としてのマッチ処理を行います。
| 以下の例は、都道府県名を列記したprefecture.txtの例です。

.. code:: 

  東京都
  東京
  神奈川県
  神奈川
  大阪府
  大阪
   ：

正規表現リストファイル
--------------------------------

以下のエンティティで指定するファイルでは、 ’正規表現名 : 正規表現文字列' の形式で記述します。

- regex_templates ： ref:`regex<pattern_regex>` ノードのtemplate指定で使用する正規表現リストを定義。

| ``regex_templates`` エンティティでは、regexノードで行うマッチ処理に使用する正規表現文字列を、共通的にファイルで定義するために使用します。
| regexノード側では、templateアトリビュートで正規表現名を指定します。尚、正規表現の記述は、基本的に単語ベースで指定する必要があります。
| 記述例は、以下の様になります。

.. code:: 

  konnichiwa : こんにち[は|わ]
  tomorrow : 明日|あす|あした
  today : 今日|きょう
    ：

変換辞書ファイル
--------------------------------
以下のエンティティで指定するファイルは、変換用のテーブルを作成することを目的として ’"変換対象値","変換後値"' の形式で記述します。

- normal ： :ref:`normalize<template_normalize>` ノード用の変換テーブルのリストを定義。
- denormal ： :ref:`denormalize<template_denormalize>` ノード用の変換テーブルのリストを定義。
- gender ： :ref:`gender<template_gender>` ノード用の変換テーブルのリストを定義。
- person ： :ref:`person<template_person>` ノード用の変換テーブルのリストを定義。
- person2 ： :ref:`person2<template_person2>` ノード用の変換テーブルのリストを定義。

normal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``normal`` エンティティでは、以下の様に定義することで、文字列内の記号等を独立した単語に変換します。
| 英字の場合、"変換対象値"の1文字目が' '(空白)でない場合、記号の変換を前提として、対象文字列内で一致するものすべてを変換します(文字置換)。
| 対して、1文字目が' '(空白)の場合には、単語を単位とした変換を行います(単語置換)。
| normalizeの変換処理は、文字変換、単語変換の順で行い、両者とも、変換後の文字列の前後には、空白が挿入されます。
| 2つの変換を組み合わせた例として、"."を"dot"、" Mr"を”mister"で指定することで、"Mr."を"mister dot"に変換することもできます。
| 尚、日本語の場合、単語を単位として変換を行います。

.. code:: 

  ".","dot"
  "/","slash"
  ":","colon"
  "*","_"
  " Mr","mister"
  " can t","can not"
  
denormal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``normal`` エンティティでの変換と対をなす、``denormal`` エンティティでは、以下の様に定義し、単語から記号等の文字列に戻します。
| 英字の場合、"変換対象値"は単語であり、"変換後値"には前後の文字列との連結する場合の' '(空白)の要否を含めて指定します。空白が無い場合には、前後の文字列と連結されます。
| normalizeの逆の例として、"mister dot"を" Mr."に戻す場合、別の指定方法として、"dot"を"."、”mister"を" Mr"の2つに分けて指定することもできます。

.. code:: 

  "dot","."
  "slash","/"
  "colon",":"
  "_","*"
  "mister dot"," Mr."
  "can not"," can't "
  
gender,person,person2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``gender``、``person``、``person2`` の各エンティティの指定は単語単位で変換を行うもので、例として、genderでは以下の様に定義します。

.. code:: 

  "he","she"
  "his","her"
  "him","her"
  "her","him"
  "she","he"
  "かれ","彼女"
  "かのじょ","彼"
  "かれし","彼女"
  "彼","彼女"
  "彼女","彼"
  "彼氏","彼女"


その他の定義ファイル
--------------------------------

``rdfs`` エンティティで指定するファイル形式は、:doc:`RDFサポート<RDF_Support>` を参照してください。

``usergroups`` エンティティで指定するファイル形式は、:doc:`Security <Security>` を参照してください。

以下のエンティティについては、実装(クラス定義等)に依存する内容を含むため、記述方法の説明は省略します。

- license_keys ： 外部接続等で必要になるライセンスキーファイル。
- pattern_nodes ： patternの各ノードの処理クラス定義ファイルを格納します。
- postprocessors ： レスポンスの応答文に対して編集を行う場合の処理クラス定義ファイル。
- preprocessors ： リクエストのユーザ発話に対して編集を行う場合の処理クラス定義ファイル。
- spelling_corpus ： スペルチェックを行い場合のもとになるコーパスファイル。
- template_nodes ： templateの各ノードの処理クラス定義ファイル。
