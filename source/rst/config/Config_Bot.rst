Bot コンフィグレーション
===========================

botコンフィグレーションには共通定義と機能別定義があり、共通定義では、対話エンジンの全体的な動作を制御する複数の項目があり、次の様に分類できます。

- :ref:`構成情報定義 <config_bot_base>`
- :ref:`応答文定義 <config_bot_response>`
- :ref:`制限値定義 <config_bot_max>`
- :ref:`起動終了文定義 <config_bot_console>`

また、機能別に、botの文字列処理や、付加機能に関する指定を行う為に、次のサブセクションを使用します。

-  `conversations <#conversations>`__ ： 対話履歴に関する項目を定義します。
-  `splitter <#splitter>`__ ： 発話文の分解に関する項目を定義します。
-  `joiner <#joiner>`__ ： 応答文生成時の終端文字に関する項目を定義します。
-  `spelling <#spelling>`__ ： スペルチェックに関する項目を定義します。
-  `translation <#translation>`__ ： 翻訳制御に関する項目を定義します。
-  `sentiment <#sentiment>`__ ： 感情判定に関する項目を定義します。

コンフィグレーションの記述方法は、yamlフォーマットを例に説明します。
jsonおよびxmlでも記載方法が異なるだけで名称は同じとなるため、jsonおよびxmlを用いる場合は読み替えてください。

設定例

.. code:: yaml

    bot:
        version: v1.0
        brain: brain

        default_response: unknown
        default_response_srai: YEMPTY
        exception_response: Exception
        empty_string: YEMPTY

        max_question_recursion: 1000
        max_question_timeout: 60
        max_search_depth: 100
        max_search_timeout: 60
        max_search_condition: 20
        max_search_srai: 50
        max_categories: 20000
        max_properties: 10000

        initial_question: おはよう。
        initial_question_srai: コンバンワ
        exit_response: 終了します。
        exit_response_srai: YEXITRESPONSE

        conversations:
            initial_topic: '*'
            max_histories: 100

        splitter:
            classname: programy.dialog.splitter.splitter_jp.SentenceSplitter
            split_chars: 。

        joiner:
            classname: programy.dialog.joiner.joiner_jp.SentenceJoiner
            join_chars: .?!。？！
            terminator: 。

        spelling:
            classname: programy.spelling.norvig.NorvigSpellingChecker
            alphabet: ABCDEFGHIJKLMNOPQRSTUVWXYZ
            check_before: false
            check_and_retry: false

        translation:
            classname: programy.translate.textblob_translator.TextBlobTranslator
            from: EN
            to: JP

        sentiment:
            classname: programy.sentiment.textblob_sentiment.TextBlobSentimentAnalyse
            scores: programy.sentiment.scores.SentimentScores


共通定義項目
--------------------------------

.. _config_bot_base:

構成情報定義
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

botの基本情報を定義します。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "version","templateの :ref:`program <template_program>` 要素で出力するバージョン名。 propertiesに記載する :ref:`version<storage_file_properties>` が、本設定よりも優先して使用されます。","(空文字)"
        "brain","botが利用するbrain定義の名称。（’brain’固定で、変更しても無効、）","brain"
        "brain_selector","複数のbrainが存在する場合のセレクターの処理クラス。（指定は無効）","(なし)"
        "tab_parse_output","パターンマッチ時のログ出力で、深度表現用のタブ挿入を制御する指定。","true"


.. _config_bot_response:

応答文定義
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

応答文生成時のデフォルト処理の内容を定義します。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "default_response_srai","マッチするpatternがなかった場合に実行する発話文。発話文に対応するシナリオがある場合に応答文を再生成します。","(空文字)"
        "default_response","マッチするpatternがなかった場合に返す応答文。default_response_sraiに対応するシナリオが記載されていない場合にも返します。propertiesに記載する :ref:`default-response<storage_file_properties>` が、本設定よりも優先して使用されます。","(空文字)"
        "exception_response","処理例外が発生した場合に返す応答文。propertiesに記載する :ref:`exception-response<storage_file_properties>` が、本設定よりも優先して使用されます。","(空文字)"
        "empty_string","pre_processorの処理結果が無い場合に設定する発話文。pre_processorの処理結果が無い場合、設定値を発話文としてシナリオが動作し応答文を生成します。","(空文字)"


.. _config_bot_max:

制限値定義
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

対話処理における、時間・数量に関する制限値を定義します。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "max_question_recursion","文探索最大回数。長文が入力され、分割文字で分割し、内部的に複数回対話シナリオを実行した場合の最大探索回数を指定します。最大回数に達すると :ref:`exception-response<storage_file_properties>` を返します。","100"
        "max_question_timeout","文探索最大時間。長文が入力され、分割文字で分割し、内部的に複数回対話シナリオを実行した場合の最大処理時間を秒単位で指定します。最大処理時間を超過すると :ref:`exception_response<storage_file_properties>` を返します。","-1(制限なし)"
        "max_search_timeout","単語探索最大時間を秒単位で指定します。文探索中に長いpatternが記載されていた場合など、文探索中の単語探索での最大探索時間を秒単位で指定します。最大処理時間を超過すると :ref:`exception-response<storage_file_properties>` を返します。","-1(制限なし)"
        "max_search_depth","単語探索分岐最大数。 :ref:`ワイルドカード<aiml_pattern_matching>` 、 :ref:`set<pattern_set>` 等の指定で単語の探索が膨大になった場合の単語の探索最大深度を指定します。単語の探索回数が最大深度を超過すると :ref:`exception-response<storage_file_properties>` を返します。","100"
        "max_search_condition",":ref:`condition要素でのloop <condition_looping>` の最大回数。conditionのloop記載時にconditionの条件にマッチしなかった場合無限ループになるため、ループの最大回数を指定します。ループが最大回数を超過すると :ref:`exception-response<storage_file_properties>` を返します。","100"
        "max_search_srai",":ref:`srai <template_srai>` の最大探索回数。sraiの記述が再帰呼び出しになった場合の再帰呼び出しの最大回数を指定します。最大回数を超過すると :ref:`exception-response<storage_file_properties>` の設定値を返します。","50"
        "max_categories","最大読み込みcategory数。AIMLの最大読み込みcategory数を指定します。AIMLで記載したcategoryが上限を越えると読み込みを行いません。読み込みを行わなかったcategoryは、 :ref:`errors<storage_entity>` のdescriptionに ``Max categories [n] exceeded`` として出力されます。","5000"
        "max_properties","利用可能なグローバル変数の最大数。name型・data型の変数の最大利用数を指定します。該当変数の数が上限を越えると新たな変数を登録することはできず、 :ref:`exception-response<storage_file_properties>` を返します。","2500"


.. _config_bot_console:

起動終了文定義
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

コンソール等での処理の場合の、bot起動・終了時の応答文に関する内容を定義します。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "initial_question_srai","起動時発話文。client起動時に処理する発話文。発話文に対応するシナリオがある場合に応答文が生成されます。","(空文字)"
        "initial_question","起動時応答文。initial_question_sraiに対応するシナリオが記載されていない場合に返す応答文を指定します。","Hello"
        "exit_response_srai","終了時発話文。client終了時に処理する発話文。発話文に対応するシナリオがある場合に応答文が生成されます。","(空文字)"
        "exit_response","終了時応答文。exit_response_sraiに対応するシナリオが記載されていない場合にも返します。","Bye!"


conversations
--------------------------------

対話履歴の管理に関する制御項目を定義します。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "initial_topic","対話履歴の初回生成時に設定するTopicの初期値。","\*"
        "max_histories","対話履歴の最大保持数。","100"


.. _config_bot_splitter:

splitter
--------------------------------

入力された発話文を複数文に分割するための定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 50

        "classname","利用するSplitterの処理クラス。","programy.dialog.splitter.regex.RegexSentenceSplitter"
        "split_chars","propertiesに記載する :ref:`splitter_split_chars<storage_file_properties>` が、本設定よりも優先して使用されます。発話文の分割に使用する文字群。","[:;,.?!]"

日本語での対話を行う場合には、``classname`` に、'programy.dialog.splitter.splitter_jp.SentenceSplitter’ を指定します。

.. _config_bot_joiner:

joiner
--------------------------------

最終的な応答文を生成する際に、複数文を結合させるための定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 50

        "classname","利用するJoinerの処理クラス。","programy.dialog.joiner.joiner.SentenceJoiner"
        "terminator","応答文の末尾に付加する文字。propertiesに記載する :ref:`joiner_terminator<storage_file_properties>` が、本設定よりも優先して使用されます。","\."
        "join_chars","terminaterの付加を抑止する、応答文の末尾文字群。propertiesに記載する :ref:`joiner_join_chars<storage_file_properties>` が、本設定よりも優先して使用されます。",".?!"

日本語での対話を行う場合には、``classname`` に、'programy.dialog.joiner.joiner_jp.SentenceJoiner’ を指定します。

spelling
--------------------------------

スペルチェックを行う場合の定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "classname","利用するスペルチェッカーの処理クラス。","(なし)"
        "alphabet","対象となるアルファベット文字群。","(空文字)"
        "check_before","事前チェックの実施指定。","false"
        "check_and_retry","チェック異常時の再処理指定","false"


translation
--------------------------------

応答文を翻訳して返す場合の定義を行います。

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "classname","利用するTranslatorの処理クラス。","(なし)"
        "from","翻訳の対象となる言語種別。ISO-639 言語コードで指定します。","(空文字)"
        "to","翻訳後の言語種別。ISO-639 言語コードで指定します","(空文字)"


sentiment
--------------------------------

感情を判定する場合の定義を行います。（本機能は、使用していません）

.. csv-table:: 設定項目一覧
  :header: "設定値","内容","初期値"
  :widths: 40, 60, 10

        "classname","感情判定を行う処理クラス。","(なし)"
        "scores","感情を数値化する処理クラス。","(なし)"
