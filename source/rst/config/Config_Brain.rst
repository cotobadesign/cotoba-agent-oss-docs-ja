Brain コンフィグレーション
===============================

| brainのコンフィグレーションでは、brainが行う対話処理に関する設定や、各要素に依存する個別設定等を行います。
| 機能別に定義を行う為に、次のサブセクションで構成します。

-  `overrides <#overrides>`__ ： 機能拡張に関する項目を定義します。
-  `defaults <#defaults>`__ ： 取得失敗時等のdefault文字列を定義します。
-  `binaries <#binaries>`__ ： シナリオのバイナリデータ利用に関する項目を定義します。
-  `braintree <#braintree>`__ ： シナリオのダンプ出力に関する項目を定義します。
-  `services <#services>`__ ： SubAgent連携機能に関する項目を定義します。
-  `nlu <#nlu>`__ ： NLU通信に関する項目を定義します。
-  `security <#security>`__ ： 利用制限に関する項目を定義します。
-  `oob <#oob>`__ ： OOB(Out of Band)処理に関する項目を定義します。
-  `dynamic <#dynamic>`__ ： 動的データ処理に関する項目を定義します。
-  `tokenizer <#tokenizer>`__ ： 単語分解や文字列結合に関する項目を定義します。
-  `debugfiles <#debugfiles>`__ ： 異常情報の出力に関する項目を定義します。

コンフィグレーションの記述方法は、yamlフォーマットを例に説明します。
jsonおよびxmlでも記載方法が異なるだけで名称は同じとなるため、jsonおよびxmlを用いる場合は読み替えてください。


設定例

.. code:: yaml

   brain:

      overrides:
        allow_system_aiml: true

      defaults:
        default-get: unknown
        default-property: unknown
        default-map: unknown

      binaries:
        save_binary: true
        load_binary: true
        load_aiml_on_binary_fail: true

      braintree:
        create: true

      services:
        REST:
            classname: programy.services.rest.GenericRESTService
            method: GET
            host: 0.0.0.0
        __PublishedREST__:
            classname: programy.services.publishedrest.PublishedRestService
        __PublishedBot__:
            classname: programy.services.publishedbot.PublishedBotService

      nlu:
          classname: programy.services.rest.GenericRESTService
          url: http://localhost:3000/run
          apikey: test_key
          use_file: false
          max_utterance_length: 300

      security:
          authorisation:
            classname: programy.security.authorise.usergroupsauthorisor.BasicUserGroupAuthorisationService
            denied_srai: AUTHORISATION_FAILED
            denied_text: parmission error

      oob:
        default:
          classname: programy.oob.defaults.default.DefaultOutOfBandProcessor
        alarm:
          classname: programy.oob.defaults.alarm.AlarmOutOfBandProcessor
        camera:
          classname: programy.oob.defaults.camera.CameraOutOfBandProcessor
        clear:
          classname: programy.oob.defaults.clear.ClearOutOfBandProcessor
        dial:
          classname: programy.oob.defaults.dial.DialOutOfBandProcessor
        dialog:
          classname: programy.oob.defaults.dialog.DialogOutOfBandProcessor
        email:
          classname: programy.oob.defaults.email.EmailOutOfBandProcessor
        geomap:
          classname: programy.oob.defaults.map.MapOutOfBandProcessor
        schedule:
          classname: programy.oob.defaults.schedule.ScheduleOutOfBandProcessor
        search:
          classname: programy.oob.defaults.search.SearchOutOfBandProcessor
        sms:
          classname: programy.oob.defaults.sms.SMSOutOfBandProcessor
        url:
          classname: programy.oob.defaults.url.URLOutOfBandProcessor
        wifi:
          classname: programy.oob.defaults.wifi.WifiOutOfBandProcessor

      dynamic:
          sets:
              numeric: programy.dynamic.sets.numeric.IsNumeric
              roman:   programy.dynamic.sets.roman.IsRomanNumeral
          maps:
              romantodec: programy.dynamic.maps.roman.MapRomanToDecimal
              dectoroman: programy.dynamic.maps.roman.MapDecimalToRoman
          variables:
              gettime: programy.dynamic.variables.datetime.GetTime

      tokenizer:
        classname: programy.dialog.tokenizer.tokenizer_jp.TokenizerJP
        punctuation_chars: ;'",!()[]：’”；、。！（）「」
        before_concatenation_rule: '.*[ -~]'
        after_concatenation_rule: '[ -~].*'

      debugfiles:
        save-errors: true
        save-duplicates: true
        save-errors_collection: true


.. _config_overrides:

overrides
--------------------------------

機能毎の処理制限等を制御する定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "allow_system_aiml","templateの :ref:`system <template_system>` 要素でのシステムコマンド（OS依存）の実行可否。","false"


.. _config_defaults:

defaults
--------------------------------

取得処理を行う要素での取得失敗時の設定文字列を定義します。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "default-get","未定義変数に対し、 :ref:`get<template_get>` 要素等でデータ取得を行なった場合に設定される文字列。propertiesに記載する :ref:`default-get<storage_file_properties>` が、本設定よりも優先して使用されます。","unknown"
    "default-property",":ref:`bot<template_bot>` 要素で未定義の変数名を指定した場合に設定される文字列。propertiesに記載する :ref:`default-property<storage_file_properties>` が、本設定よりも優先して使用されます。","unknown"
    "default-map",":ref:`map<template_map>` 要素で変換対象の文字列がなかった場合に設定される文字列。propertiesに記載する :ref:`default-map<storage_file_properties>` が、本設定よりも優先して使用されます。","unknown"

``default-get`` の設定値は、:ref:`json<template_json>` や、RDFの検索 :ref:`select<template_select>` 等の要素での取得失敗時に設定されるとともに、``default-property`` 、 ``default-map`` が未定義の場合の値としても使用されます。

binaries
--------------------------------

シナリオを展開したバイナリデータの利用に関する定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "load_binary","Bot起動時のバイナリデータ読み込みの実施。","false"
    "save_binary","シナリオ展開時のバイナリデータ保存の実施。","false"
    "load_aiml_on_binary_fail","バイナリデータ読み込み失敗時のシナリオ再展開の実施。’false’の場合、起動時の処理例外発生により終了します。","false"


braintree
--------------------------------

シナリオの展開結果のダンプ出力に関する定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "create","シナリオの展開結果のファイル生成を実施。","false"


.. _config_services:

services
--------------------------------

外部サービスと連携する為に、SubAgent連携機能で使用する処理クラスや、URLの定義を、サービス名を単位として定義します。

以下は、サービス名毎に設定可能な項目ですが、使用される項目は処理クラスに依存します。（サービス名毎の処理クラスの指定は必須です。）
サービス毎の処理クラスの作成については、:ref:`カスタム外部サービス実装<subagent_custom>` を参照してください。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "classname","外部サービスと通信する処理クラス。","(なし)"
    "url","外部サービスの接続URL。","(なし)"
    "host","外部サービスを提供するサーバのhost名、または、IPアドレス。","(なし)"
    "port","外部サービスを提供するサーバのport番号。","(なし)"
    "method","HTTP通信のmethod名。","(なし)"
    "denied_srai","通信に失敗した場合に実行する発話文。シナリオ中に該当する発話文がない場合は空文字が返ります。","(なし)"

設定例にある次のサービス定義は、SubAgent機能を提供するための定義であり、常に指定が必要です。

- ``__PublishedREST__`` ： :ref:`汎用REST通信<subagent_rest>` を行う処理クラスです。
- ``__PublishedBot__`` ： :ref:`公開Bot連携<subagent_cotoba_design_pf>` を行う処理クラスです。

nlu
--------------------------------

| マッチ処理に使用するNLUサーバに関する定義を行います。
| NLUサーバの接続先情報として有効な定義がない場合には、NLUを利用したマッチ処理は行いません。
| NLUに関する詳細は、:doc:`NLU <../NLU>` を参照してください。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "classname","NLUサーバと通信する処理クラス。","programy.nlu.nlu.NluRequest"
    "url","NLUサーバの接続URL。","http://localhost:3000/run"
    "apikey","NLUサーバとの通信時に付加するapi-key。HTTPヘッダ：'x-api-key'で送信します。","(空文字)"
    "timeout","NLUサーバ毎の通信時間の最大値（秒単位）を共通値として指定。最大値の時間を超えた場合、該当サーバとの通信は失敗したものとして扱います。","10"
    "use_file","NLUサーバの接続定義に :ref:`nlu_serversファイル<storage_nlu_servers>` を利用する指定。","false"
    "max_utterance_length","NLUサーバに送信する発話文の最大長。制限長を超える発話文が指定された場合には、通信を行いません。","-1(制限なし)"

| ``classname`` には、'programy.nlu.cotobadesignNlu.CotobadesignNlu’ を指定します。
| 本処理クラスは、sraix要素で直接NLUからの結果を取得する :ref:`NLU通信<subagent_nlu>` 処理でも利用します。

尚、複数のNLUサーバを利用する場合には、``use_file: true`` を指定して、nlu_serversファイルで定義を行ってください。

.. _config_security:

security
--------------------------------

| ユーザ毎でのシナリオの利用制限等の定義を行います。
| securityの制御には、次の３つの種類があります。

- authentication： brain処理での利用制限を行います。（使用していません。）
- account_linker： 外部サービスを連携して利用制限を行います。（使用していません。）
- authorisation： templateの :ref:`authorize<template_authorise>` 要素で利用を制限します。

従って、``authorisation`` のみの定義が有効です。詳細については、:doc:`Security <../Security>` を参照してください。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "classname","利用制限制御を行う処理クラス。","(なし)"
    "denied_srai","認証に失敗した場合に実行する発話文。発話文に対応するシナリオがある場合に応答文を再生成します。該当するシナリオが無い場合は空文字になります。","(なし)"
    "denied_txt","認証に失敗した場合に返す文字列。","(なし)" 

``denied_srai`` と ``denied_txt`` の両方が指定されている場合、 denied_sraiの結果が空文字の場合に、denied_txtの文字列が使用されます。


oob
--------------------------------

:doc:`OOBの設定<Config_Brain_OOB>` を参照してください。


.. _config_dynamic:

dynamic
--------------------------------

| データの取得・変換を値を定義する形式ではなく、処理クラスを利用して行う場合の定義を行います。
| dynamicが利用できる項目には、次の３つの種類があります。

- sets： patternの :ref:`set<pattern_set>` 要素に対するマッチ処理を指定された処理クラスで実施します。
- maps： templateの :ref:`map<template_map>` 要素に対する変換処理を指定された処理クラスで実施します。
- variables: グローバル変数（name）の :ref:`get<template_get>` に対して処理クラスの結果を返します。

それぞれで、次の定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "エントリ名","エントリ名に該当する要素の処理クラス。","(なし)"

設定例にある各処理クラスでは、以下の処理を行います。

.. csv-table:: 設定項目一覧
  :header: "種類","エントリ名","処理クラス","処理内容"
  :widths: 10, 20, 40, 60

    "sets","numeric","programy.dynamic.sets.numeric.IsNumeric","set要素のマッチ処理で該当文字列が数値の場合にマッチします。"
    "","roman","programy.dynamic.sets.roman.IsRomanNumeral",”setのマッチ処理で該当文字列が英数字の場合にマッチします。"
    "maps","romantodec","programy.dynamic.maps.roman.MapRomanToDecimal",”mapの処理として、アラビア数字表記をローマ数字表記に変換します。"
    "","dectoroman","programy.dynamic.maps.roman.MapDecimalToRoman","mapの処理として、ローマ数字表記をアラビア数字表記に変換します。"
    "variables","gettime","programy.dynamic.variables.datetime.GetTime","name変数のgetに対して、現在日時の情報を返します。"

尚、これらの定義が有効な場合、各要素の処理として優先されるため、ファイル定義：’sets’, 'maps', 'defaults' での同一名称の指定は無効になります。

.. _config_tokenizer:

tokenizer
--------------------------------

発話文の単語分解や、応答文生成時に行う文字列結合（templateの要素単位に実施）に使用するクラスに関する定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "classname","利用するTokenizerの処理クラス。","programy.dialog.tokenizer.tokenizer.Tokenizer"
    "punctuation_chars","区切り文字扱いを行う文字を指定します。区切り文字はマッチング対象外として、発話文や、topic・thatの対象文から除外してマッチング処理を行います。propertiesに記載する :ref:`punctuation_chars<storage_file_properties>` が、本設定よりも優先して使用されます。",”(なし)"
    "before_concatenation_rule","応答文生成等で、生成された複数の文字列を連結する時点で空白を挿入する場合の前文字列の形式を正規表現で指定します。propertiesに記載する :ref:`before_concatenation_rule<storage_file_properties>` が、本設定よりも優先して使用されます。",".*[ -~]"
    "after_concatenation_rule","応答文生成等で、生成された複数の文字列を連結する時点で空白を挿入する場合の後文字列の形式を正規表現で指定します。propertiesに記載する :ref:`after_concatenation_rule<storage_file_properties>` が、本設定よりも優先して使用されます。","[ -~].*"

| 日本語での対話を行う場合には、``classname`` に、'programy.dialog.tokenizer.tokenizer_jp.TokenizerJP’ を指定します。
| ``before_concatenation_rule`` , ``after_concatenation_rule`` は、日本語用の設定項目です。


debugfiles
--------------------------------

シナリオや設定ファイルの展開における異常情報の出力に関する定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

    "save-errors","シナリオ展開時の異常記述や、整合性の不正に関する情報の出力指定。'true'の場合 :ref:`errosエンティティ<storage_entity>` に対する出力処理が行われます。","false"
    "save-duplicates","シナリオ展開時の定義重複情報の出力指定。'true'の場合 :ref:`duplicatesエンティティ<storage_entity>` に対する出力処理が行われます。","false"
    "save-errors_collection","各種設定ファイルの異常に関する情報の出力指定。'true'の場合 :ref:`erros_collectionエンティティ<storage_entity>` に対する出力処理が行われます。。","false"
