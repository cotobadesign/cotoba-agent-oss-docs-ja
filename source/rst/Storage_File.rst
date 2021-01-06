================================
ファイル管理
================================

| 対話プラットフォームで使用するファイルに関して説明します。
| 以降の説明では、主に、対話プラットフォームで利用するファイル管理方式として、ローカルファイルを指定しています。
| ファイル管理方式には、ローカルファイル以外にも、データベースでの管理もありますが、構成は同じになります。

| 定義ファイルの拡張子は、シナリオファイルがaiml、サブエージェント連携用の定義ファイルがyaml、その他の主な設定ファイルはtxtです。
| （その他、クラス定義ファイルのconfなども使用します。）

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
  ├── debug デバッグ情報ディレクトリ
  │   ├── duplicates.txt
  │   ├── errors.txt
  │   ├── errors_collection.txt
  │   └── *.log
  ├── learnf 動的生成シナリオファイルディレクトリ
  │   └── *.aiml
  ├── licences ライセンスファイルディレクトリ
  │   └── license.keys.txt
  ├── lookups 置換辞書ファイルディレクトリ
  │   ├── regex.txt
  │   ├── denormal.txt
  │   ├── gender.txt
  │   ├── normal.txt
  │   ├── person.txt
  │   └── person2.txt
  ├── maps mapリストファイルディレクトリ
  │   └── *.txt
  ├── nodes 要素処理クラス定義ファイルディレクトリ
  │   ├── pattern_nodes.conf
  │   └── template_nodes.conf
  ├── properties プロパティ定義ファイルディレクトリ
  │   ├── nlu_servers.yaml
  │   ├── botnames.yaml
  │   ├── rest_templates.yaml
  │   ├── defaults.txt
  │   ├── properties.txt
  │   └── json JSON形式プロパティ定義ファイルディレクトリ
  │       └── *.json
  ├── prosessing 文編集クラス定義ファイルディレクトリ
  │   ├── preprocessors.conf
  │   └── postprocessors.conf
  ├── rdfs RDFファイルディレクトリ
  │   └── *.txt
  ├── security セキュリティファイルディレクトリ
  │   └── usergroups.yaml
  ├── sets setリストファイルディレクトリ
  │   └── *.txt
  └── spelling スペルチェックファイルディレクトリ
      └── corpus.txt


.. _storage_entity:

エンティティ
================================

| 対話プラットフォームでは、格納するデータの種類によってエンティティを規定し、各々でファイル管理を行います。
| 利用するファイルの構成は、コンフィグファイル(config.yaml)で定義し、どのデータの種類を利用するかを、以下のエンティティを設定するか否かで指定します。
| 尚、エンティティには、共通的に使用するものと、特定要素のみで使用するものがありますが、定義や管理の方法は同じです。
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
    "errors_collection","登録エラー情報","Yes","Yes","(デバッグ用）辞書などの各種定義ファイル登録時のエラー情報ファイルを格納します。"
    "license_keys","ライセンスキー","Yes","No","外部接続等で必要になるライセンスキーファイルを格納します。"
    "pattern_nodes","クラス定義リスト","Yes","No","patternの各要素の処理クラス定義ファイルを格納します。"
    "postprocessors","クラス定義リスト","Yes","No","レスポンスの応答文に対して編集を行う場合の処理クラス定義ファイルを格納します。"
    "preprocessors","クラス定義リスト","Yes","No","リクエストのユーザ発話に対して編集を行う場合の処理クラス定義ファイルを格納します。"
    "properties","プロパティリスト","Yes","No","patternの :ref:`bot<pattern_bot>`、及び、templateの :ref:`bot<template_bot>` 要素で使用するプロパティ定義ファイルを格納します。"
    "properties_json","JSON形式プロパティ","No","No","主にtemplateの :ref:`bot<template_bot>` 要素で使用するJSON形式プロパティファイルを格納します。"
    "spelling_corpus","スペルチェック情報","No","No","スペルチェックを行う場合のもとになるコーパスファイルを格納します。"
    "template_nodes","クラス定義リスト","Yes","No","templateの各要素の処理クラス定義ファイルを格納します。"

-  Pattern要素用のエンティティ

.. csv-table::
    :header: "エンティティ","格納内容","単一ファイル","自動生成","説明"
    :widths: 18,40,10,5,65

    "regex_templates","正規表現リスト","Yes","No",":ref:`regex<pattern_regex>` 要素のtemplate指定で使用する正規表現リストファイルを格納します。"
    "sets","対象単語リスト","No","No",":ref:`set<pattern_set>` 要素で使用する単語リストファイルを格納します。"

-  Template要素用のエンティティ

.. csv-table::
    :header: "エンティティ","格納内容","単一ファイル","自動生成","説明"
    :widths: 18,40,10,5,65

    "denormal","変換辞書","Yes","No",":ref:`denormalize<template_denormalize>` 要素での変換に使用する辞書ファイルを格納します。"
    "gender","変換辞書","Yes","No",":ref:`gender<template_gender>` 要素での変換に使用する辞書ファイルを格納します。"
    "learnf","categoryリスト","No","YES",":ref:`learnf<template_learnf>` 要素の処理で作成されるcategories情報を、ユーザ毎に格納します。"
    "logs","ログ情報","No","YES",":ref:`log<template_log>` 要素の処理で作成されたログ情報を、ユーザ毎に格納します。"
    "maps","プロパティリスト","No","No",":ref:`map<template_map>` 要素での変換に使用する辞書ファイルを格納します。"
    "normal","変換辞書","Yes","No",":ref:`normalize<template_normalize>` 要素での変換に使用する辞書ファイルを格納します。"
    "person","変換辞書","Yes","No",":ref:`person<template_person>` 要素での変換に使用する辞書ファイルを格納します。"
    "person2","変換辞書","Yes","No",":ref:`person2<template_person2>` 要素での変換に使用する辞書ファイルを格納します。"
    "rdf","RDFデータリスト","No","No",":doc:`RDF<RDF_Support>` 関連要素の処理対象となるRDFデータの定義ファイルを格納します。"
    "usergroups","セキュリティ情報","Yes","No",":ref:`authorise<template_authorise>` 要素で使用するロール定義ファイルを格納します。"

-  サブエージェント連携用のエンティティ

.. csv-table::
    :header: "エンティティ","格納内容","単一ファイル","自動生成","説明"
    :widths: 18,40,10,5,65

    "nlu_servers","利用NLU接続情報","Yes","No","NLUマッチング対象のNLUと、:ref:`sraix<template_sraix>` 要素で実施するNLU通信に使用するアクセス情報の定義ファイルを格納します。"
    "bot_names","公開Bot接続情報","Yes","No",":ref:`sraix<template_sraix>` 要素で実施する公開Botとの通信に使用するアクセス情報の定義ファイルを格納します。"
    "rest_templates","REST通信雛形情報","Yes","No",":ref:`sraix<template_sraix>` 要素で実施する汎用REST通信で使用する雛形の定義ファイルを格納します。"


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
             errors_collection: file
             license_keys: file
             pattern_nodes: file
             postprocessors: file
             preprocessors: file
             properties: file
             properties_json: file
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
             nlu_servers: file
             bot_names: file
             rest_templates: file
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
                   errors_collection_storage:
                     file: ./storage/debug/errors_collection.txt
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
                   properties_json_storage:
                     dirs: ./storage/properties/json
                     extension: json
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
                   rdf_storage:
                     dirs: ./storage/rdfs
                     subdirs: true
                     extension: txt
                   nlu_servers_storage:
                     dirs: ./storage/properties/nlu_servers.yaml
                   bot_names_storage:
                     dirs: ./storage/properties/botnames.yaml
                   rest_templates_storage:
                     dirs: ./storage/properties/rest_templates.yaml
                   usergroups_storage:
                     file: ./storage/security/usergroups.yaml

'config'サブセクションでの定義は、対象となるエンティティによって、単一ファイルや、複数ファイルの利用を示す記載方法をとります。

単一ファイルのエンティティの場合
------------------------------------------------

単一ファイルを指定するエンティティの場合、'file'属性でファイルパスを指定します。

.. code:: yaml

                   usergroups_storage:
                       file: ./storage/security/usergroups.yaml

複数ファイルの利用が可能なエンティテイの場合
------------------------------------------------------

複数ファイルが利用可能なエンティティの場合、以下の3つの属性を指定します。
ただし、自動生成対象のエンティティの場合、ディレクトリパスのみの指定となります。

- dirs: 対象ファイルディレクトリパスを指定。
- subdirs: 対象ファイルディレクトリ配下のサブディレクトリをサーチするかどうかをtrue/falseで指定。
- extension: ロードするファイルタイプの拡張子を指定。

.. code:: yaml

                   categories_storage:
                     dirs: ./storage/categories
                     subdirs: true
                     extension: aiml

                   conversations_storage:
                     dirs: ./storage/conversations

尚、自動生成対象外で、複数ファイルを指定することが可能なストレージエンジンは、以下の5つになります。

- categories_storage
- sets_storage
- maps_storage
- properties_json_storage
- rdf_storage

※sets_storage/maps_storage/properties_json_storageは、ファイル名を識別子として利用するため、同一ファイル名の重複配置を防止する意味で、
'subdirs: false' を指定することを推奨します。


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
             binaries: redis
             braintree: redis
             categories: file
             ：

Redisでの入出力が可能なエンティティは、以下のものになります。

- binaries ： AIMLを展開した検索用ツリーのバイナリデータを格納。
- braintree ： AIMLを展開した検索用ツリーのXML形式データを格納。
- conversations ： ユーザ毎の過去を含めた対話処理内容(変数値を含む)の履歴情報を格納。
- duplicates ： (デバッグ用）AIMLファイル展開時のcategory重複情報を格納。
- errors ： (デバッグ用）AIMLファイル展開時のエラー情報を格納。
- errors_collection ： (デバッグ用）辞書などの各種定義ファイル登録時のエラー情報ファイルを格納。
- learnf ： :ref:`learnf<template_learnf>` 要素の処理で作成されるcategories情報を、ユーザ毎に格納。
- logs ： :ref:`log<template_log>` 要素の処理で作成されたログ情報を、ユーザ毎に格納。


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
                    db: 0
                    prefix: programy
                    drop_all_first: false
                    username: xxx
                    password: xxx
                    ssl: false
                    timeout: 1

尚、``username`` は、redis-server:V6.0以降の利用時に指定が可能です。

定義ファイルの記述方法
================================

編集可能なファイルで、AIML以外に、使用することの多い定義ファイルについて記述方法を説明します。

| 各定義ファイルは、シナリオ（AIML）の解析前に展開します。この為、定義ファイルに登録されていない名称等を使用したAIML記述は無効になります。
| :doc:`Brain コンフィグレーション<config/Config_Brain>` に以下の定義を行うことで、各定義ファイル展開時の異常情報をまとめて ``errors_collection`` エンティティで指定したファイルに出力することができます。

.. code:: yaml

    brain:
      debugfiles:
        save-errors_collection: true


プロパティリストファイル
--------------------------------

以下のエンティティで指定するファイルは、botのプロパティや変数への値設定を目的として ’名称: 値' の形式で記述します。

- defaults ： グローバル変数(name)の初期値を定義。
- properties : patternの :ref:`bot<pattern_bot>`、及び、templateの :ref:`bot<template_bot>` 要素で使用するbotのプロパティを定義。


.. _storage_file_defaults:

defaults
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``defaults`` エンティティでは、シナリオで使用するグローバル変数(name)の値設定を初期起動時に行うことができます。
| ただし、ユーザ毎の対話情報履歴が存在する場合は、履歴上の最新値が反映されるため、既に存在するの変数に対する変更は行わず、存在しない変数のみを追加します。
| 以下の例では、initial_variable(name変数)の値として、ユーザ毎の初回対話時に"初期値"を設定します。

.. code:: yaml

  initial_variable: 初期値


.. _storage_file_properties:

properties
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``properties`` エンティティは、templateの :ref:`bot<template_bot>` 要素での情報取得に使用するとともに、マッチングで指定する :ref:`bot<pattern_bot>` 要素でも使用できます。
| JSON形式の値を設定する場合には、``properties_json`` エンティティも使用できます。

.. csv-table::
    :header: ”名称","内容","説明"
    :widths: 5,10,50

    "name","ボット名",":ref:`bot<template_bot>` 要素のname属性に'name'を指定した際に取得できる値。"
    "birthdate","ボット作成日",":ref:`bot<template_bot>` 要素のname属性に'birthdate'を指定した際に取得できる値。"
    "grammar_version","グラマーバージョン",":ref:`bot<template_bot>` 要素のname属性に'grammar_version'を指定した際に取得できる値。"
    "app_version","アプリバージョン",":ref:`bot<template_bot>` 要素のname属性に'app_version'を指定した際に取得できる値。"
    "version","シナリオバージョン","他の項目と同様にname指定で取得するとともに、 :ref:`program<template_program>` 要素で取得するバージョン情報の値。"

| 以下の定義を指定すると、コンフィグレーションで定義された制御値を変更して、独自の動作を指定することができます。
| 記述として、名称のみを指定することで、制御値を空文字に変更する（制御を無効化する）こともできます。

.. csv-table::
    :header: "名称","内容","説明","対応config定義"
    :widths: 5,10,50,10

    "default-response","デフォルトレスポンス","マッチするpatternがなかった場合に返す応答文。",":ref:`Bot定義のdefault_response<config_bot_response>`"
    "exception-response","例外レスポンス","処理例外が発生した場合に返す応答文。",":ref:`Bot定義のexception_response<config_bot_response>`"
    "default-get","取得失敗文字列","未定義変数に対し、 :ref:`get<template_get>` 要素等でデータ取得を行なった場合に設定される文字列。",":ref:`Brain定義(defaults)のdefault-get<config_defaults>`"
    "default-property","property取得失敗文字列",":ref:`bot<template_bot>` 要素で未定義の変数名を指定した場合に設定される文字列。",":ref:`Brain定義(defaults)のdefault-property<config_defaults>`"
    "default-map","map変換失敗文字列",":ref:`map<template_map>` 要素で変換対象の文字列がなかった場合に設定される文字列。",":ref:`Brain定義(defaults)のdefault-map<config_defaults>`"
    "joiner_terminator","文終端文字","応答文の語尾句等を自動的に付与する文字列を指定します。指定なしの場合何も付与しません。",":ref:`Bot定義(joiner)のterminator<config_bot_joiner>`"
    "joiner_join_chars","文終端除外文字","joiner_terminatorの指定で文終端文字を自動付与する際に、joiner_terminator指定の文字を結合除外する文字列を指定します。 指定なしの場合、joiner_terminatorで指定した文字を付与します。",":ref:`Bot定義(joiner)のjoin_chars<config_bot_joiner>`"
    "splitter_split_chars","文分割文字","内部的に文章分割を行う文字を指定します。指定された文字列が文中に含まれていると、複数文として扱い、responseに複数の応答文を結合した文字列を返します。ただし、metadataは最終文で設定した内容のみが返ります。指定なしの場合、 発話文を1文として扱います。",":ref:`Bot定義(splitter)のsplit_chars<config_bot_splitter>`"
    "punctuation_chars","区切り文字","区切り文字扱いを行う文字を指定します。区切り文字はマッチング対象外として、発話文や、topic・thatの対象文から除外してマッチング処理を行います。",":ref:`Brain定義(tokenizer)のpunctuation_chars<config_tokenizer>`"
    "before_concatenation_rule","文字列連結時の空白挿入条件（前条件）","応答文生成等で、生成された複数の文字列を連結する時点で空白を挿入する場合の前文字列の形式を正規表現で指定します。",":ref:`Brain定義(tokenizer)のbefore_concatenation_rule<config_tokenizer>`"
    "after_concatenation_rule","文字列連結時の空白挿入条件（後条件）","応答文生成等で、生成された複数の文字列を連結する時点で空白を挿入する場合の後文字列の形式を正規表現で指定します。",":ref:`Brain定義(tokenizer)のafter_concatenation_rule<config_tokenizer>`"

``default-get`` の設定値は、:ref:`json<template_json>` や、RDFの検索 :ref:`select<template_select>` 等の要素での取得失敗時に設定されるとともに、``default-property`` 、 ``default-map`` が未定義の場合の値としても使用されます。

* 設定例

.. code:: 

  name:基本応答
  birthdate:March 01, 2019

  version: v0.0.1
  grammar_version:0.0.1
  app_version: 0.0.1

  default-response: すみません、意味がわかりませんでした。
  exception-response: 処理例外が発生しました。
  default-get: わかりません
  default-property: 定義されていません
  default-map: map登録されていません
  
  joiner_terminator: 。 
  joiner_join_chars: .?!。？！
  splitter_split_chars:  。
  punctuation_chars: ;'",!()[]：’”；、。！（）「」 
  before_concatenation_rule: .*[a-z]
  after_concatenation_rule: [a-z].*

JSON形式の定義をpropertyとして設定する場合、改行を含めて記入することで視覚的な確認を容易にする方法として、``properties_json`` エンティティを使用することができます。

| ``properties_json`` エンティティは、templateの :ref:`bot<template_bot>` 要素でのJSON形式の情報取得に使用するもので、マッチングで使用することは推奨できません。
| ``properties_json`` エンティティでは、その配下にJSONファイルを配置することで、ファイル名（拡張子を除く）をそのまま名称として使用することができます。（大文字/小文字・全角/半角を区別します。）
| 尚、propertyの値には、JSONファイルの内容そのものではなく、JSONデータとして変換された文字列が設定されます。


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

| joiner_join_chars（結合除外文字）は、joiner_terminator(文終端文字)を自動付与する際に、結合除外する文字列を指定します。
| joiner_join_chars未指定の場合、 応答文に"今日も元気に行きましょう。"、"いい気分ですね！"などの応答文を記載した場合に、joiner_terminator指定の文字を結合すると、
| "今日も元気に行きましょう。。"、"いい気分ですね！。"のように、応答文記載の文末文字に加えjoiner_terminatorで指定した句点を結合した応答文が返ります。
| joiner_join_charsを指定しておくと、"そうですね!"、"こんにちは。"と、joiner_terminatorを結合しない応答文を返します。

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


punctuation_chars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

入力文の区切り文字扱いを行う文字を指定します。区切り文字はマッチング対象外とし発話文から除外した形でマッチング処理を行います。
"こんにちは。"および"こんにちは"という入力がある場合、punctuation_charsに指定された文字は、無視され同一発話扱いになります。

* 設定例

.. code:: 

  punctuation_chars: ;'",!()[]：’”；、。！（）「」 

.. code:: xml

    <category>
        <pattern>こんにちは</pattern>
        <template>今日も元気に行きましょう</template>
    </category>

| Input: こんにちは。
| Output: 今日も元気に行きましょう。
| Input: こんにちは
| Output: 今日も元気に行きましょう。


punctuation_charsを未指定にすると、"。"もマッチ対象となるため、"こんにちは。"と"こんにちは"は別発話扱いとなります。

.. code::

  punctuation_chars:

| Input: こんにちは
| Output: 今日も元気に行きましょう。
| Input: こんにちは。
| Output: すみません、意味がわかりませんでした。


concatenation_rule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| template要素の展開処理では、子要素毎の結果文字列を結合して応答文を生成します。
| 英文であれば一律に単語間に空白を挿入して結合しますが、日英混合文では、前後の文字列（単語）の関係で制御する必要があり、本設定にて、空白を挿入する対象の文字列形式を正規表現で指定します。 

.. code:: 

  before_concatenation_rule: .*[ -~]  (前文字列の語尾が半角英数字または記号)
  after_concatenation_rule: [ -~].*   (後文字列の先頭が半角英数字または記号)

* 設定例　（前文字列の語尾が半角英字で、後文字列の先頭が半角英字の場合に、空白を挿入。）

.. code:: 

  before_concatenation_rule: .*[a-z]
  after_concatenation_rule: [a-z].*

.. code:: xml

    <category>
        <pattern>* and *</pattern>
        <template><star /><star index="2" /></template>
    </category>

| Input: sugar and milk
| Output: sugar milk。
| Input: sugar and ミルク
| Output: sugarミルク。
| Input: 砂糖 and milk
| Output: 砂糖milk。
| Input: 砂糖 and ミルク
| Output: 砂糖ミルク

全角英字を含めて対応する場合の指定は、以下の様になります。(正規表現では、大文字・小文字を区別しないため、便宜上、全角を大文字で指定しています。）

.. code:: 

  before_concatenation_rule: .*[a-zＡ-Ｚ]
  after_concatenation_rule: [a-zＡ-Ｚ].*

英数字と通貨記号の間に空白を挿入する場合には、以下の設定を行います。

.. code:: 

  before_concatenation_rule: .*[a-z0-9]
  after_concatenation_rule: [a-z0-9$¥].*


サブエージェント定義ファイル
--------------------------------

以下のエンティティで指定するファイルは、利用するサブエージェント毎の接続情報を定義するため、yaml形式で記述します。
yaml形式の場合、記号が意味を持つ場合があるため、記号を含む文字列を指定する場合には、間に空白を入れない、または、文字列全体を "'" で囲む必要があります。

- nlu_servers : 利用するNLUサーバに関するアクセス情報を定義します。
- bot_names : 利用する公開Botに関するアクセス情報を定義します。
- rest_templates : 利用するRESTサーバに関するアクセス情報を定義します。


.. _storage_nlu_servers:

nlu_servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``nlu_servers`` エンティティでは、次の３つの定義を行います。

- :ref:`sraix<template_sraix>` 要素で使用するNLUサーバのアクセス先情報を定義します。
- マッチ処理に使用するNLUサーバのアクセス先情報を定義します。
- マッチ処理時のNLUサーバ毎の通信時間の最大値を指定します。

| 'sraix'要素で使用するNLUサーバ情報には、エイリアス名毎に、アクセス先URL(エンドポイント)、APIキーを設定し、'sraix'の属性： ``nlu`` でエイリアス名を指定することで利用できます。 
| 尚、アクセス先URLの指定は必須で、エイリアス名は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で管理します。
| 以下の例は、2つのエイリアス名の定義を行った例です。1つ目はURLにAPIキー設定なし、2つ目はURLとともにAPIキーを設定しています。

.. code:: yaml

  servers:
    エイリアス名_1:
      url: http://localhost:5200/run
    エイリアス名_2:
      url: http://localhost:3000/run
      apikey: test_key


| マッチ処理に使用するNLUサーバ情報についても、アクセス先URL(エンドポイント)、APIキーを設定します。 （name指定を除き、アクセス先URLの指定は必須です。）
| 複数のNLUサーバを利用する場合があるため、yaml記述にはリスト（行頭記号：'-'）指定を行い、記述順序に従って、マッチ処理時のNLUサーバ通信を行います。
| （１つのNLUサーバのみを利用する場合は、行頭記号：'-'の指定は省略が可能です。）
| 尚、NLUマッチングでは、同一のURLに対する重複通信を抑止するため、同じURLが指定されている場合、２つ目以降の重複指定は無視されます。
| 以下の例は、1つ目はURL指定のAPIキー設定なし、2つ目はURL指定とともにAPIキーを設定、3つ目にはserversで定義したアクセス情報を使用することを指定しています。

.. code:: yaml

  nlu:
    - url: http://localhost:5201/run
    - url: http://localhost:3000/run
      apikey: test_key
    - name: エイリアス名_1

| NLUサーバ毎の通信時間の最大値の指定は秒単位で、各サーバに共通の値として以下の様に指定します。最大値の時間を超えて通信が行われた場合、該当サーバとの通信は失敗したものとして扱います。
| 尚、本指定はNLUを利用したマッチ処理を行う場合に有効で、省略時には、コンフィグレーション定義で指定された値（又は、初期値：10秒）が使用されます。

.. code:: yaml

  timeout: 1

| マッチ処理に使用するNLUサーバ情報を本ファイルで指定する場合には、コンフィグレーション定義に以下の指定が必要です。
| この指定がない場合や、本ファイルにマッチ処理に使用するNLUサーバ情報が指定されていない場合には、コンフィグレーション定義のNLUサーバの情報を使用します。
| （コンフィグレーション定義でもNLUサーバの情報が指定されていない場合には、マッチ処理にNLUは利用されません。）

.. code:: yaml

  brain:
    nlu:
      use_file: true


.. _storage_bot_names:

bot_names
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``bot_names`` エンティティでは、:ref:`sraix<template_sraix>` 要素で使用する公開Botのアクセス先情報として、エイリアス名毎に、主にアクセス先URL(エンドポイント)、APIキーを設定します。
| sraix要素では属性： ``botName`` でエイリアス名を指定することで公開Botを利用できます。 
| (指定する値は、Bot生成時に確定しますので、公開Botの提供者に確認する必要があります。)
| 尚、アクセス先URLの指定は必須で、エイリアス名は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で管理します。
| 以下の例は、2つのエイリアス名の設定を行った例です。1つ目はURLにAPIキー設定なし、2つ目はURLとともにAPIキーを設定しています。

.. code:: yaml

  bot:
    エイリアス名_1:
      url: http://localhost:5400/bots/botId_1/ask
    エイリアス名_2:
      url: http://localhost:5401/bots/botId_1/ask
      apikey: test_key

公開Botとの通信では、各種のパラメータが指定できるため、エイリアス名毎に、雛形情報として以下の指定も可能です。（url以外は省略が可能です。）
尚、通信時に使用されるパラメータの値には、:ref:`sraix<template_sraix>` 要素での子要素指定の内容が優先して設定されます。

.. code:: yaml

  bot:
    エイリアス名:
      url: http://localhost:5401/bots/botId_1/ask
      apikey: test_key
      metadata: Send Data
      locale: ja-JP
      time: 2018-07-01T12:18:45+09:00
      topic: test
      deleteVariable: false
      config: '("loglevel": "info"}'


.. _storage_rest_templates:

rest_templates
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``rest_templates`` エンティティでは、:ref:`sraix<template_sraix>` 要素で行う汎用REST通信で使用する雛形情報をテンプレート名毎に設定します。
| 'sraix'要素では属性： ``template`` でテンプレート名を指定することで汎用REST通信を行います。 
| 尚、アクセス先情報である 'host' の指定は必須で、テンプレート名は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で管理します。
| 設定項目としては、汎用REST通信で指定できる全パラメータの指定が可能ですが、通信時に使用されるパラメータの値には、:ref:`sraix<template_sraix>` 要素での子要素指定の内容が優先して設定されます。
| 以下の例は、2つのテンプレート名の設定を行った例です。1つ目は必須定義のhost設定のみ、2つ目では全パラメータを設定しています。

.. code:: yaml

  rest:
    テンプレート名_1:
      host: 'http://localhost:5300/rest'
    テンプレート名_2:
      host: 'http://localhost:5300/rest'
      method: POST
      query:  '"item":"1234"'
      header: '"Content-Type": "applicaton/json"'
      body: '{"key": "Send Data"}'


.. _storage_file_sets:

単語リストファイル
--------------------------------

以下のエンティティで指定するファイルでは、処理対象となる単語・文字列を列記します。

- sets ： :ref:`set<pattern_set>` 要素で使用するマッチ処理対象の単語リストを定義。

| ``sets`` エンティティでは、set要素での情報参照がファイル名（拡張子を除く）で行われるため、情報の種類毎にファイルを分けることが可能です。
| 識別名は、ファイル名（拡張子を除く）を、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した名称で管理します。
| マッチ処理も、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で行いますが、 :ref:`star<template_star>` 要素で取得される値はファイルに記述したものになります。
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

英文の場合でも、複数単語からなる文字列を指定することが可能で、マッチ処理では単語数が多いものが優先されます。
尚、英字リストと日本語リストとでは処理方式が異なるため、１つのファイル内に両者を混合させた場合の動作は保証されません。


.. _storage_regex_templates:

正規表現リストファイル
--------------------------------

以下のエンティティで指定するファイルでは、 ’正規表現名 : 正規表現文字列' の形式で記述します。

- regex_templates ： :ref:`regex<pattern_regex>` 要素のtemplate指定で使用する正規表現リストを定義。

| ``regex_templates`` エンティティでは、regex要素で行うマッチ処理に使用する正規表現文字列を、正規表現名毎で指定します。
| regex要素側では、template属性で正規表現名を指定します。尚、正規表現の記述は、基本的に単語ベースで指定する必要があります。
| 尚、正規表現名は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で管理します。
| 記述例は、以下の様になります。

.. code:: 

  konnichiwa : こんにち[は|わ]
  tomorrow : 明日|あす|あした
  today : 今日|きょう
    ：


変換辞書ファイル
--------------------------------
以下のエンティティで指定するファイルは、変換用のテーブルを作成することを目的として 変換対象文字列と変換後文字列の関係を列記します。

- maps ： :ref:`map<template_map>` 要素用の変換テーブルのリストを定義。
- normal ： :ref:`normalize<template_normalize>` 要素用の変換テーブルのリストを定義。
- denormal ： :ref:`denormalize<template_denormalize>` 要素用の変換テーブルのリストを定義。
- gender ： :ref:`gender<template_gender>` 要素用の変換テーブルのリストを定義。
- person ： :ref:`person<template_person>` 要素用の変換テーブルのリストを定義。
- person2 ： :ref:`person2<template_person2>` 要素用の変換テーブルのリストを定義。


.. _storage_file_maps:

maps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``maps`` エンティティでは、map要素の情報参照をファイル名（拡張子を除く）で行われるため、情報の種類毎にファイルを分けることが可能です。
| ファイル名（拡張子を除く）は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で管理します。
| ファイル毎の記述は、’変換対象文字列: 変換後文字列' の形式で列記します。
| 変換対象文字列の一致判定は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で行いますが、 変換結果には指定された変換後文字列が設定されます。

以下の例は、都道府県と県庁所在地の関係を列記したprefectural_office.txtの例です。

.. code::

  東京都:東京
  東京:東京
  神奈川県:横浜市
  神奈川:横浜市
  大阪府：大阪市
  大阪：大阪市
   ：


.. _storage_file_normal:

normal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``normal`` エンティティでは、’"変換対象文字列","変換後文字列"' の形式を列記することで、文字列内の記号等を独立した単語に変換します。
| 英字の場合、"変換対象文字列"の1文字目が' '(空白)でない場合、記号の変換を前提として、対象文字列内で一致するものすべてを変換します(文字置換)。
| 対して、1文字目が' '(空白)の場合には、単語を単位とした変換を行います(単語置換)。
| 英文に対するnormalizeでの変換処理は、文字変換、単語変換の順で行い、両者とも、変換後の文字列の前後には、空白が挿入されます。
| 2つの変換を組み合わせた例として、"."を"dot"、" Mr"を”mister"で指定することで、"Mr."を"mister dot"に変換することもできます。
| 尚、日本語の場合、単語を単位として変換を行います。

.. code:: 

  ".","dot"
  "/","slash"
  ":","colon"
  "*","_"
  " Mr","mister"
  " can t","can not"


.. _storage_file_denormal:

denormal
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``normal`` エンティティでの変換と対をなす、``denormal`` エンティティでも、’"変換対象文字列","変換後文字列"' の形式を列記し、単語から記号等の文字列に戻します。
| 英字の場合、"変換対象文字列"は単語であり、"変換後文字列"には前後の文字列と連結する場合の' '(空白)の要否を含めて指定します。空白が無い場合には、前後の文字列と連結されます。
| normalizeの逆の例として、"mister dot"を" Mr."に戻す場合、別の指定方法として、"dot"を"."、”mister"を" Mr"の2つに分けて指定することもできます。

.. code:: 

  "dot","."
  "slash","/"
  "colon",":"
  "_","*"
  "mister dot"," Mr."
  "can not"," can't "


.. _storage_file_gender:
  
gender,person,person2
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| ``gender``、``person``、``person2`` の各エンティティは、単語単位で変換を行うもので、’"変換対象文字列","変換後文字列"' の形式を列記します。
| 変換対象文字列、変換後文字列に空白を含む複数単語の文字列を指定することも可能です。
| 変換対象文字列の一致判定は、英字：半角大文字、数字・記号：半角、カタカナ：全角に変換した上で行いますが、 変換結果には指定された値が設定されます。

例として、genderでは以下の様に定義します。

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


AIML要素の定義ファイル
--------------------------------

以下のエンティティでは、対話エンジンで使用可能なAIML要素と処理クラスを関連づけるための定義ファイルを指定します。
詳細は :doc:`カスタム要素<node/Custom_Nodes>` を参照してください。

- pattern_nodes ： patternの各要素の処理クラス定義ファイルを指定。
- template_nodes ： templateの各要素の処理クラス定義ファイルを指定。


その他のエンティティ
--------------------------------

``rdf`` エンティティで指定するファイル形式は、:doc:`RDFサポート<RDF_Support>` を参照してください。

``usergroups`` エンティティで指定するファイル形式は、Securityの :ref:`ユーザグループファイル <security_usergroups>` を参照してください。

以下のエンティティについては、実装(クラス定義等)に依存する内容を含むため、記述方法の説明は省略します。

- license_keys ： 外部接続等で必要になるライセンスキーファイル。
- postprocessors ： レスポンスの応答文に対して編集を行う場合の処理クラス定義ファイル。
- preprocessors ： リクエストのユーザ発話に対して編集を行う場合の処理クラス定義ファイル。
- spelling_corpus ： スペルチェックを行い場合のもとになるコーパスファイル。
