========================
用語集
========================

本章ではドキュメント中で用いる用語を定義します。

対話処理にまつわる用語
========================

本セクションでは、ドキュメント中で用いる対話処理にまつわる用語を定義します。

..  list-table::
    :widths: 40 60
    :header-rows: 1

    *
      + 用語
      + 定義
    *
      + 対話シナリオ
      + 対話処理の実行に関わる処理プログラムやデータを記述したファイル群。対話処理のプログラムは AIML (Artificial Inteligence Markup Language) で記述する。
    *
      + 開発者
      + 対話処理のプログラムやデータを記述し対話アプリケーションを開発する者。
    *
      + 対話エンジン
      + 対話シナリオを登録して対話処理を実行する実行環境。
    *
      + ボット
      + 対話エンジンに対話シナリオが登録されて対話処理を実行可能な状態にある対話処理プロセス。
    *
      + ユーザ
      + ボットに対して対話処理を要求する者。
    *
      + システム
      + ボットの別称。ユーザと対比する文脈で用いることがある。
    *
      + リクエスト
      + ユーザがボットに対して行う対話処理要求、あるいは、その内容。
    *
      + レスポンス
      + ボットが対話処理を行ってユーザに返す対話処理応答、あるいは、その内容。
    *
      + 発話文
      + リクエストに含まれ、ユーザがボットに送信する文字列。
    *
      + 応答文
      + レスポンスに含まれ、ボットがユーザに返信する文字列。
    *
      + 高度意図解釈
      + 機械学習モデルにより発話文の意図(インテント)を推論し、意図に付随するキーワード(スロット)を発話文から抽出する機能、あるいは、その機能を実行するモジュール。
    *
      + 対話ログ
      + 対話エンジン(ボット)が実行した対話処理の動作過程を記録したログ。
    *
      + サブエージェント
      + あるボットが、特定の対話処理やその他の情報処理を、他のボット、あるいは、情報サービスに行わせるために呼び出す先のボットや情報サービスをさす。SA (SubAgent) とも記す。
    *
      + メインエージェント
      + サブエージェントを呼び出す側のボット。MA (MainAgent) とも記す。

cAIML
========================

本セクションでは、ドキュメント中で用いる cAIML (COTOBA AIML) にまつわる用語を定義します。

.. list-table::
    :widths: 50 200
    :header-rows: 1

    *
      + 用語
      + 定義
    *
      + AIML
      + Artificial Inteligence Markup Language の略称。対話処理プログラムの記述言語の1つ。AIML は XML 形式で記述する。
    *
      + cAIML
      + COTOBA AIML の略称。COTOBA DESIGN による独自仕様拡張を含む AIML。
    *
      + タグ
      + XML の基本構造を規定するタグ(開始タグ '<...>'、終了タグ '</...>')のうち cAIML で定義されたもの。
    *
      + 要素
      + タグを構成する主構成要素。開始タグ '<要素>'、終了タグ '</要素>' の記法で記述される。
    *
      + 属性
      + タグを構成するサブ構成要素。開始タグ '<要素 属性="値">' の記法で記述される。属性は複数あっても良いが、同じ属性は複数指定できない。どの属性が使えるかは要素毎に規定される。要素を関数だと考えると、属性は引数と解釈出来る。
    *
      + 内容
      + 開始タグと終了タグの間に記述される文字列。'<要素 属性="値">内容</要素>' の記法で記述される。内容には文字列や他の要素を複数記述することが出来る。これにより cAIML は XML 同様、入れ子構造の記述形式となる。
    *
      + 要素名
      + 特定の要素を指定するための名称。
    *
      + 属性名
      + 特定の属性を指定するための名称。
    *
      + 属性値
      + 属性に設定する値。cAIML で属性値として記述する場合ダブルクオーテーションでくくる決まりである。
    *
      + ブロック
      + 内容を含んだある要素全体(対応する開始タグから終了タグの間の記述内容)を指す。
    *
      + ノード
      + cAIML の XML 形式のデータ構造を木構造と見た時の要素に相当する木構造の構成要素。
    *
      + NLU
      + cAIML から呼び出す高度意図解釈機能を指す。
