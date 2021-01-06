カスタムpattern要素
=============================

| 対話エンジンは pattern要素として、pattern_nodes.conf で定義されているAIMLタグをサポートします。
| pattern_nodes.conf を変更することで、要素に対応する処理クラスを変更したり、独自の要素を追加することができます。
| ``要素の実装を変更すると、パーサが動作しなくなったりパフォーマンスや他の要素の動作に重大な影響を与える可能性があるため全体の動作を理解した上でご利用ください。``

要素毎に以下の記述を列記します。

.. code:: ini

   AIMLタグ名 = python処理クラスのパス

＊記述例

.. code:: ini

   #AIML 1.0
   root = programy.parser.pattern.nodes.root.PatternRootNode
   word = programy.parser.pattern.nodes.word.PatternWordNode
   priority = programy.parser.pattern.nodes.priority.PatternPriorityWordNode
   oneormore = programy.parser.pattern.nodes.oneormore.PatternOneOrMoreWildCardNode
   topic = programy.parser.pattern.nodes.topic.PatternTopicNode
   that = programy.parser.pattern.nodes.that.PatternThatNode
   template = programy.parser.pattern.nodes.template.PatternTemplateNode

   #AIML 2.0
   zeroormore = programy.parser.pattern.nodes.zeroormore.PatternZeroOrMoreWildCardNode
   set = programy.parser.pattern.nodes.set.PatternSetNode
   bot = programy.parser.pattern.nodes.bot.PatternBotNode

   #Custom
   iset = programy.parser.pattern.nodes.iset.PatternISetNode
   regex = programy.parser.pattern.nodes.regex.PatternRegexNode 

| 新たなtemplate要素を作成する場合、``programy.parser.pattern.nodes.base.PatternNode`` を基底クラスとして継承します。
| 本基底クラスを継承することで、AIMLのXML要素をpatternクラスのノード属性にカプセル化します。また、patternの評価中に対象文字列を要素の型に変換させる処理も行います。

以下が、オーバーライドする主なメソッドになります。

-  ``__init__(self, attribs, text, userid='*', element=None, brain=None)``

   xmlの要素を解析して展開します。xmlの属性記述が attribs にリストで、内容が text で引き渡されます。展開できない場合は必要に応じて適切な例外をスローします。 

-  ``def equivalent(self, other)``

   処理要素が他の評価要素と等価かどうかを判定します。

-  ``def equals(self, client_context, words, word_no)``

   対象文字列が要素のルールにマッチするかどうかを判定します。words で単語単位の分割リスト、word_no で評価開始位置が引き渡されます。
   判定結果は EqualsMatchクラス（programy.parser.pattern.matcher.EqualsMatch）で返し、EqualsMatchクラス生成の第一引数で True/False を設定します。 

-  ``def to_string(self, verbose=True)``

   デバッグやロギングの出力の為に、要素情報を文字列表現に変換します。

-  ``def to_xml(self, client_context, include_user=False)``

   要素の内容をXML形式に変換します。XML形式の `Braintree` を出力する場合に利用します。
