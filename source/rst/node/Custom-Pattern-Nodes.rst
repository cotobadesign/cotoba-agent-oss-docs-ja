カスタムpattern要素
=============================

| パーサーは、pattern_nodes.confで定義されているAIMLタグをサポートします。
| 要素の実装を変更したり、独自の要素を追加することができます。
| ``要素の実装を変更すると、パーサが動作しなくなったりパフォーマンスや他の要素の動作に重大な影響を与える可能性があるため全体の動作を理解した上でご利用ください。``


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

| 各pattern要素は、``programy.parser.pattern.nodes.base.PatternNode`` を基底クラスとして継承します。
| pattern要素の基底クラスであり、XML要素をpatternクラスのノード属性にカプセル化します。また、pattern評価中に単語を要素の型に一致させる処理も含んでいます。

以下が、オーバーライドする主なメソッドになります。

-  ``def equivalent(self, other)`` - 処理要素が他の評価要素と等価かどうかをテストするメソッドです。
-  ``def equals(self, bot, client, words, word_no)`` - 単語が要素のルールにマッチするかどうかを判定するメソッドです。
-  ``def to_string(self, verbose=True)`` - デバッグやロギングの為に要素情報を文字列表現に変換するメソッドです。
-  ``def to_xml(self, bot, clientid)`` - 要素の内容をXML形式に変換します。XML形式の `Braintree` へ出力時に利用するメソッドです。
