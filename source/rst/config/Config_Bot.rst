Bot コンフィグレーション
===========================

botコンフィグレーションは、対話エンジンの全体的な動作を制御するアイテムで構成されています。

-  `initial_question <#initial-question>`__
-  `default_response <#default-response>`__
-  `empty_string <#empty-string>`__
-  `max_search_depth <#max-search-depth>`__
-  `override_properties <#override-properties>`__
-  `exit_response <#exit-response>`__

また、botの文字列処理、付加機能や設定等は、次のサブセクションで構成されています。

-  :doc:`conversations <Config_Bot_Conversation>`
-  :doc:`splitter/joiner <Config_Bot_Sentence>`
-  :doc:`spelling <Config_Bot_Spelling>`
-  :doc:`transration <Config_Bot_Translation>`
-  :doc:`sentiment <Config_Bot_Sentiment>`

設定例

.. code:: yaml

    bot:
        version: v1.0

        brain: brain

        initial_question: おはよう。
        initial_question_srai: コンバンワ
        default_response: unknown
        default_response_srai: YEMPTY
        empty_string: YEMPTY
        exit_response: 終了します。
        exit_response_srai: YEXITRESPONSE

        override_properties: true

        max_question_recursion: 1000
        max_question_timeout: 60
        max_search_depth: 100
        max_search_timeout: 60
        max_search_condition: 20
        max_search_srai: 50
        max_categories: 20000

        spelling:
            load: false
            classname: programy.spelling.norvig.NorvigSpellingChecker
            corpus: file
            check_before: false
            check_and_retry: false

        conversations:
        save: true
        load: false

        joiner:
            classname: programy.dialog.joiner.joiner_jp.SentenceJoiner
            join_chars: .?!。？！
            terminator: 。

        splitter:
            classname: programy.dialog.splitter.splitter_jp.SentenceSplitter
            split_chars: 。



initial_question
---------------------------

.. csv-table:: 機能一覧
  :header: "設定値","内容"
  :widths: 40, 60

        "initial_question_srai","起動時発話文。client起動時に実施する発話。AIMLに記載してあるシナリオが動作し応答文を生成します。"
        "initial_question","起動時応答文。initial_question_sraiに対応するシナリオが記載されていない場合に返す応答文を指定します。"

default_response
---------------------------

.. csv-table:: 機能一覧
  :header: "設定値","内容"
  :widths: 40, 60

        "default_response_srai","マッチするpatternがなかった場合に実行する発話文。AIMLに記載してあるシナリオが動作し応答文を生成します。"
        "default_response","マッチするpatternがなかった場合に返す応答文。default_response_sraiに対応するシナリオが記載されていない場合に返す応答文を指定します。propertiesに記載する、 :ref:`default-response<storage_file_properties>` は、設定より優先度が高くpropertiesに記載の応答文が優先されます。"


empty_string
---------------------------

.. csv-table:: 機能一覧
  :header: "設定値","内容"
  :widths: 40, 60

        "empty_string","pre_processorの処理結果が無い場合に設定する発話文。pre_processorの処理結果が無い場合、設定値を発話文としてシナリオが動作し応答文を生成します。"

max_search_depth
---------------------------

.. csv-table:: 機能一覧
  :header: "設定値","内容"
  :widths: 40, 60

        "max_question_recursion","文探索最大回数。長文が入力され、分割文字で分割し、内部的に複数回対話シナリオを実行した場合の最大探索回数を指定します。最大回数に達すると :ref:`default-response<storage_file_properties>` を返します。"
        "max_question_timeout","文探索最大時間。長文が入力され、分割文字で分割し、内部的に複数回対話シナリオを実行した場合の最大処理時間を秒単位で指定します。最大処理時間を超過すると :ref:`default-response<storage_file_properties>` を返します。"
        "max_search_timeout","単語探索最大時間を秒単位で指定します。文探索中に長いpatternが記載されていた場合など、文探索中の単語探索での最大探索時間を秒単位で指定します。最大処理時間を超過すると :ref:`default-response<storage_file_properties>` を返します。"
        "max_search_depth","単語探索分岐最大数。 :ref:`ワイルドカード<aiml_pattern_matching>` 、 :ref:`set<pattern_set>` 等の指定で単語の探索が膨大になった場合の単語の探索最大回数を指定します。単語の探索回数が最大回数に達すると :ref:`default-response<storage_file_properties>` を返します。"
        "max_search_condition",":ref:`condition loop <condition_looping>` 最大回数。conditionのloop記載時にconditionの条件にマッチしなかった場合無限ループになるため、ループの最大回数を指定します。ループが最大回数に達すると :ref:`default-response<storage_file_properties>` を返します。"
        "max_search_srai",":ref:`srai <srai>` 最大探索回数。sraiの記述が再帰呼び出しになった場合の再帰呼び出し最大回数を指定します。最大回数に達すると :ref:`default-response<storage_file_properties>` を返します。"
        "max_categories","最大読み込みcategory数。AIMLの最大読み込みcategory数を指定します。AIMLで記載したcategoryが上限を越えると読み込みを行いません。読み込みを行わなかったcategoryは、 :ref:`errors<storage_entity>` のdescriptionに ``Max categories [n] exceeded`` として出力されます。"

override_properties
---------------------------

.. csv-table:: 機能一覧
  :header: "設定値","内容"
  :widths: 40, 60

        "override_properties","name変数の上書き許可フラグ。falseの場合、同名のname変数には上書きを行いません。"

exit_response
---------------------------

.. csv-table:: 機能一覧
  :header: "設定値","内容"
  :widths: 40, 60

        "exit_response_srai","終了時発話文。client終了時に実施する発話文。AIMLに記載してあるシナリオが動作し応答文を生成します。"
        "exit_response","終了時応答文。exit_response_sraiに対応するシナリオが記載されていない場合に返す応答文を指定します。"





