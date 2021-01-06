==============================================================
COTOBA Agent dialog engine API
==============================================================

.. _coversation_api:

対話API
===============================
対話エンジンをREST APIクラス(programy.clients.restful.yadlan.sanic.client)を用いて立ち上げた場合に利用されるAPIです。
ユーザの発話文を含むリクエストをボットに送信し、ボットからの応答文を含むレスポンスを取得します。


.. csv-table::
    :header: "項目","内容"
    :widths: 20,80

    "プロトコル","HTTP"
    "メソッド","POST"

リクエスト
-------------------------------
対話APIリクエストに設定する内容です。

* リクエストヘッダ

.. csv-table::
    :header: "フィールド名","値","説明"
    :widths: 20,30,50

    "Content-Type","application/json;charset=UTF-8","コンテントタイプでJSON、文字コードはUTF8を指定します。"


* リクエストボディ

.. csv-table::
    :header: "項目名","","キー","型","必須","説明"
    :widths: 20,20,20,10,10,40

    "ロケール","","locale","string","No","言語指定。ISO-639 言語コードとISO-3166 国コードをハイフン繋いだ組み合わせを指定します。例:ja-JP, en-US, zh-CN。 未指定の場合、ボット側で規定した言語で動作します。"
    "時間情報","","time","string","No","リクエスト側時刻。ISO8601(RFC3339)形式で指定します。例:2018-07-01T12:18:45+09:00。 未設定の場合サーバ時刻で動作します。"
    "ユーザID","","userId","string","Yes","ユーザ毎のユニークなIDを指定します。"
    "トピックID","","topic","string","No","対話シナリオで記載したシナリオIDを指定します。未指定の場合、対話エンジンが持っているtopicのまま動作します。"
    "ユーザ発話","","utterance","string","Yes","ユーザの発話文。"
    "タスク変数削除","","deleteVariable","boolean","No","trueを設定するとAIML記述のset/getの属性 ``data`` で設定した変数を一括で削除します。name、varで設定した変数は削除されません。"
    "メタデータ","","metadata","string","No","JSON形式のメタデータ、もしくは文字列を設定できます。設定したデータを対話シナリオ内で利用する場合、対話シナリオ側で、このメタデータを使用するための記述が必要です。詳細は :doc:`対話APIデータの変数利用<rst/API_Variables>` を参照してください。"
    "コンフィグ","","config","","No","対話エンジン動作状態に関する設定。"
    "","ログレベル","logLevel","string","No","対話エンジンの対話ログの出力レベルを設定します。設定値は次のとおりです。none:対話ログを出力しない、error:エラーレベル以上の対話ログを出力する、warning:ワーニングレベル以上の対話ログを出力する、info:動作状態以上の対話ログを出力する、debug:デバッグ以上の対話ログを出力する。対話ログの出力量の大小関係は、none<error<warning<info<debugになります。未指定の場合、対話ログの出力レベルは変更されません。"

* リクエスト例

..  code:: http

    POST /v1.0/ask HTTP/1.1
    Host: www.***.com
    Accept: */*
    Content-Type: application/json;charset=UTF-8

    {
        "locale": "ja-JP",
        "time": "2018-07-01T12:18:45+09:00",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "greeting",
        "utterance": "こんにちは",
        "deleteVariable": false,
        "metadata": {"arg1":"value1","arg2":"value2"},
        "config": {"logLevel":"debug"}
    }

レスポンス
-------------------------------
対話APIリクエストに対するレスポンスボディはJSON形式です。
対話APIリクエストに対するレスポンスコードの一覧は以下のとおりです。
対話エンジン内の処理以外に起因するレスポンスコード(以下一覧以外のHTTPステータスコード)が返されることもあります。
その場合のレスポンスボディの内容は不定です。

* レスポンスコード

.. csv-table::
    :header: "コード","説明"
    :widths: 20,80

    "200","リクエスト正常終了。"
    "400","パラメータエラー。リクエストの内容の見直しが必要です。"

* レスポンスヘッダ

..  csv-table::
    :header: "フィールド名","値","説明"
    :widths: 20,50,30

    "Content-Type","application/json;charset=UTF-8","コンテントタイプでJSON、文字コードはUTF8を指定します。"

* レスポンスボディ

.. csv-table::
    :header: "項目名","キー","型","必須","説明"
    :widths: 20,20,10,10,40

    "ユーザ発話","utterance","string","Yes","対話エンジン内部で処理を行ったユーザ発話文。英数の半角化、半角カナの全角化等の内部処理を行った結果を返します。"
    "ユーザID","userId","string","Yes","ユーザ毎のユニークなIDを指定します。リクエストのuserIdと同じ。"
    "応答文","response","string","Yes","対話エンジンから応答文。UTF8の文字列を返します。"
    "トピック名","topic","string","Yes","現在のトピック名。"
    "レイテンシ","latency","number","Yes","エンジン内処理時間。リクエストを受けてからレスポンスを返すまでの処理時間で単位は秒。シナリオに登録されているパターンマッチ処理、意図解釈処理、SubAgentの処理を含んだ処理時間になります。"
    "メタデータ","metadata","string","No","JSON形式のメタデータ、もしくは文字列が設定されます。メタデータの内容は対話シナリオ内の記述により指定されます。"

* レスポンス例

..  code:: http

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "response": "こんにちは、今日もいい天気ですね",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "greeting",
        "latency":0.0317230224609375,
        "utterance": "こんにちは"
    }


音楽再生に対応した対話シナリオで"次の曲を再生"と発話し、metadataに再生指示の情報を設定するように対話シナリオで記述した場合の例。

..  code:: http

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "response": "次の曲を再生しますね",
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308E03A71",
        "topic": "music_play",
        "latency":0.0317230224609375,
        "utterance": "こんにちは",
        "metadata": {"play":"next"}
    }

.. _debug_api:

デバッグAPI
================================
デバッグAPIは、アップロードされたzipアーカイブの対話シナリオファイルを対話エンジンに登録する際に発生したエラー情報や、対話の履歴情報を取得するためのAPIです。
過去の対話を含めた対話状態を取得することができます。また、対話中に使用するグローバル変数の値を設定(変更)することもできます。

.. csv-table::
    :header: "項目","内容"
    :widths: 20,80

    "プロトコル","HTTP"
    "メソッド","POST"

リクエスト
-------------------------------
デバッグAPIリクエストに設定する内容です。
デバッグAPIエンドポイントには事前に登録したユーザのみがアクセスできます。

* リクエストヘッダ

..  csv-table::
    :header: "フィールド名","値","説明"
    :widths: 20,50,30

    "x-dev-key","yyyyyyyyyyyyyyyyy","`user-information <#user-information>`__ のx-dev-keyで取得したAPIキーを指定します。"
    "Content-Type","application/json;charset=UTF-8","コンテントタイプでJSON、文字コードはUTF8を指定します。"

* リクエストボディ

.. csv-table::
    :header: "項目名","","キー","型","必須","説明"
    :widths: 20,20,20,10,10,40

    "ユーザID","","userId","string","No","ユーザ毎のユニークなIDを指定します。未指定や存在しないユーザの場合、conversation,current_conversations,logs情報は取得されません。"
    "変数リスト","","variables","","No","値を設定する変数の情報をリスト形式で指定します。ユーザIDが未指定の場合、変数リストの指定は無効になります。存在しないユーザの場合、更新した変数情報を含むconversation情報は取得できますが、対話履歴の無い状態になります。"
    "","変数タイプ","type","string","No","変数タイプを指定します。指定できるタイプは 'name'もしくは 'data' になります。(key，valueとともに指定します。）"
    "","変数名","key","string","No","値を設定する変数名を指定します。(type，valueとともに指定します。）"
    "","値","value","string","No","変更する値を記載します。(type、keyとともに指定します。）"

* リクエスト例

..  code:: http

    POST / HTTP/1.1
    Host: www.***.com
    Accept: */*
    x-dev-key: yyyyyyyyyyyyyyyyy

    Content-Type: application/json;charset=UTF-8

    {
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308000000",
        "variables": [
            {
                "type": "name",
                "key": "name_variable",
                "value": "0"
            },
            {
                "type": "data",
                "key": "data_variable",
                "value": "1"
            },
            ：
            }
        ]
    }

レスポンス
-------------------------------
デバッグAPIリクエストに対するボディはJSON形式です。
デバッグAPIリクエストに対するレスポンスコードの一覧は以下のとおりです。
対話エンジン内の処理以外に起因するレスポンスコード(以下一覧以外のHTTPステータスコード)が返されることもあります。
その場合のレスポンスボディの内容は不定です。

なお、送信時に、変数リスト：variablesを指定した場合、受信データには変数設定が反映された情報が返ります。

* レスポンスコード

.. csv-table::
    :header: "コード","説明"
    :widths: 20,80

    "200","リクエスト正常終了。"
    "400","パラメータエラー。リクエストの内容の見直しが必要です。"

* レスポンスヘッダ

..  csv-table::
    :header: "フィールド名","値","説明"
    :widths: 20,50,30

    "Content-Type","application/json;charset=UTF-8","コンテントタイプでJSON、文字コードはUTF8を指定します。"

* レスポンスボディ

.. csv-table::
    :header: "項目名","キー","型","必須","説明"
    :widths: 20,20,10,10,40

    "対話履歴情報","conversations","json","Yes","指定したユーザの対話履歴を取得します。"
    "直近対話情報","current_conversations","json","Yes","直近の対話処理での変数内容の変更状況を取得します。"
    "シナリオエラー情報","errors","json","Yes","対話シナリオ登録時のエラー内容を取得します。"
    "シナリオ重複情報","duplicates","json","Yes","対話シナリオ登録時のpatternの重複を取得します。"
    "設定ファイルエラー情報","errors_collection","json","Yes","各種設定ファイルの登録時のエラー内容を取得します。"
    "ログ情報","logs","json","Yes","直近の対話処理の中で、templateタグ内のlogタグで出力した対話ログ内容を取得します。"

* レスポンス例

..  code::

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "conversations": {
            "categories": 1251,                      /* 登録されているCategory数 */
            "user_categories": 0,                    /* learnfで登録されたユーザ固有のCategory数 */
            "exception": null,                       /* 例外発生時のメッセージ */
            "client_context": {                      /* クライアント・ユーザ情報 */
                "botid": "bot",
                "brainid": "brain",
                "clientid": "yadlan",
                "depth": 0,
                "userid": "E8BDF659B007ADA2C4841EA364E8A70308E03A71"
            },
            "properties": {                          /* 現在のグローバル変数（name）値リスト */
                "topic": "daytime",
                "name_variable": "0"
            },
            "data_properties": {                     /* 現在のグローバル変数（data）値リスト */
                "data_variable": "1"
            },
            "max_histories": 100,                    /* 最大対話履歴管理数） */
            "questions": [                           /* 対話履歴（古いものから順に、max_histories分を格納） */
                {
                    "exception": null, 
                    "name_properties": {                 /* 処理完了時のグローバル変数（name）値リスト */
                        "topic": "daytime"
                    },
                    "data_properties": {},               /* 処理完了時のグローバル変数（data）値リスト */
                    "var_properties": {                  /* 処理完了時のローカル変数（var）値リスト */
                        "__USER_LOCALE__": "None",
                        "__USER_METADATA__": "None"
                        :
                    },
                   "sentences": [                        /* 発話文毎の処理情報 */
                        {
                            "question": "test",              /* 発話文 */
                            "matched_node": {                /* マッチしたシナリオ情報 */
                                "file_name": "../storage/categories/basic.aiml",
                                "start_line": "78",
                                "end_line": "92"
                            },
                            "response": "response OK"        /* 応答文 */
                        }
                    :
                :
            ]
        },
        "current_conversation": [
            {
                "before_variables": {                /* 変更があった変数の処理開始時の値リスト */
                    "name_properties": {
                        "topic": "*",
                        "name_variable": null
                    },
                    "data_properties": {
                        "data_variable": null
                    }
                },
                "after_variables": {                 /* 変更があった変数の処理完了時の値リスト */
                    "name_properties": {
                        "topic": "daytime",
                        "name_variable": "0"
                    },
                    "data_properties": {
                        "data_variable": "1"
                    }
                },
                "question": "test",                  /* 発話文 */
                "that": "*",                         /* マッチ対象となる直近の応答文(初期値："*") */
                "topic": "*"                         /* マッチ対象となる現在のTopic値 */
                "matched_node": {                    /* マッチしたシナリオ情報 */
                    "file_name": "../storage/categories/basic.aiml",
                    "start_line": "78",
                    "end_line": "92"
                },
                "processing_result": "response OK",  /* brainの通常処理で生成された応答文（denied_srai実施時には空文字） */
                "response": "response OK"            /* 応答文 */
                "srai_histories": [                  /* srai処理の実施履歴 */
                    {
                        "before_variables": {            /* 変更があった変数のsrai処理開始時の値リスト */
                            "name_properties": {
                                "topic": "*"
                            },
                            "var_properties": {
                                "var_variable": null
                            }
                        },
                        "after_variables": {            /* 変更があった変数のsrai処理完了時のリスト */
                            "name_properties": {
                                "topic": "daytime"
                            },
                            "var_properties": {
                                "var_variable": "srai_var"
                            }
                        },
                        "question": "test srai",         /* srai対象の発話文 */
                        "that": "*",
                        "topic": "*",
                        "matched_node": {
                            "file_name": "../storage/categories/basic.aiml",
                            "start_line": "4",
                            "end_line": "11"
                        },
                        "processing_result": "srai OK",
                        "response": "srai OK"
                    }
                ]
            }
        ],
        "errors": [
            {                                        /* シナリオ内の不正情報をファイル・Category単位で出力 */
                "category": {
                    "end": "None",
                    "start": "None"
                },
                "description": "Failed to load contents of AIML file : XML-Parser Exception [mismatched tag: line 238, column 25]",
                "file": "../storage/categories/ng.aiml",
                "node": {
                    "column": "0",
                    "raw": "0"
                },
                "node_name": null
            }
        ],
        "duplicates": [
            {                                        /* シナリオ内での重複Categoryの情報を出力 */
                "category": {
                    "end": "35",
                    "start": "21"
                },
                "description": "Dupicate grammar tree found [おはよう]",
                "file": "../storage/categories/basic.aiml",
                "node": {
                    "column": "9",
                    "raw": "22"
                }
            }
        ],
        "errors_collection": {
            "denormals": [                           /* denormal.txtの不正情報を出力 */
                {
                    "description": "illegal format [\"word\"]",
                    "file": "../storage/lookups/denormal.txt",
                    "line": 1
                }
            ],
            "normals": [],                           /* normal.txtの不正情報を出力 */
            "genders": [],                           /* gender.txtの不正情報を出力 */
            :
        },
        "logs": [
            {
                "info": "(templete log-node) log message"
            }
        ]
    }


errors/duplicates/errors_collectionの内容を参照することで、シナリオや各種設定ファイルで登録に失敗した項目を確認することができます。

| conversations/current_conversation/logsの内容からは対話処理の状況が確認できます。（例外発生時の要因は、conversationsのexceptionで提示されます。）
| conversationsでは、使用されている全変数の情報が出力されますが、current_conversationでは、開始時と終了時の間で変更があった変数のみを出力します。
| レスポンス例のcurrent_conversationからは、発話文 "test" に対して、"test srai" に対するsrai処理が実施され、srai処理の中でtopic変数が変更されたことが確認できます。

ユーザ固有情報のクリア指定
-------------------------------
| デバッグAPIのリクエストには、ユーザ毎に保持する固有情報をクリアする専用の指定もあります。
| 本指定を行った場合には、他の指定よりも優先される為、レスポンスボディにデバッグ情報は格納されません。
| クリア処理を行う対象は、以下の２種類です。

- 対話履歴（``conversation`` 指定）： ユーザ毎の対話履歴情報をクリアします。履歴情報と共にユーザ毎に保持する変数情報も初期化されます。
- learn登録情報（``learn`` 指定）： Templateの :ref:`learnf<template_learnf>` ノードで登録された、ユーザ固有のCategory情報をクリアします。

* リクエストボディ

.. csv-table::
    :header: "項目名","キー","型","必須","説明"
    :widths: 20,20,10,10,40

    "ユーザID","userId","string","Yes","ユーザ毎のユニークなIDを指定します。存在しないユーザの場合、処理は失敗します。"
    "クリア対象","reset","string","Yes","'conversation'、'learn' のいずれか、または、両方を意味する 'all' を指定します。"

* リクエスト例

..  code:: http

    POST / HTTP/1.1
    Host: www.***.com
    Accept: */*
    x-dev-key: yyyyyyyyyyyyyyyyy

    Content-Type: application/json;charset=UTF-8

    {
        "userId": "E8BDF659B007ADA2C4841EA364E8A70308000000",
        "reset": "all"
    }


* レスポンスボディ

以下のどちらかのJSON形式が設定されます。

 - 成功時： {"reset": "Succeeded"}
 - 失敗時： {"reset": "Failed"}

* レスポンス例

..  code::

    HTTP/1.1 200 Ok
    Content-Type: application/json;charset=UTF-8

    {
        "reset": "Succeeded"
    }
