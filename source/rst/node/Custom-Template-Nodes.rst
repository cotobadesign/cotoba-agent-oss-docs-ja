カスタムtemplate要素
============================

| 対話エンジンは template要素として、template_nodes.confを含むシステムで定義された全てのAIMLタグをサポートします。
| template_nodes.conf を変更することで、要素に対応する処理クラスを変更したり、独自の要素を追加することができます。
| ``要素の実装を変更すると、パーサが動作しなくなったりパフォーマンスや他の要素の動作に重大な影響を与える可能性があるため全体の動作を理解した上でご利用ください。``

要素毎に以下の記述を列記します。

.. code:: ini

   AIMLタグ名 = python処理クラスのパス

＊記述例

.. code:: ini

   word = programy.parser.template.nodes.word.TemplateWordNode
   authorise = programy.parser.template.nodes.authorise.TemplateAuthoriseNode
   random = programy.parser.template.nodes.rand.TemplateRandomNode
   condition = programy.parser.template.nodes.condition.TemplateConditionNode
   srai = programy.parser.template.nodes.srai.TemplateSRAINode
   sraix = programy.parser.template.nodes.sraix.TemplateSRAIXNode
   get = programy.parser.template.nodes.get.TemplateGetNode
   set = programy.parser.template.nodes.set.TemplateSetNode
   map = programy.parser.template.nodes.map.TemplateMapNode
   bot = programy.parser.template.nodes.bot.TemplateBotNode
   think = programy.parser.template.nodes.think.TemplateThinkNode
   normalize = programy.parser.template.nodes.normalise.TemplateNormalizeNode
   denormalize = programy.parser.template.nodes.denormalise.TemplateDenormalizeNode
   person = programy.parser.template.nodes.person.TemplatePersonNode
   person2 = programy.parser.template.nodes.person2.TemplatePerson2Node
   gender = programy.parser.template.nodes.gender.TemplateGenderNode
   sr = programy.parser.template.nodes.sr.TemplateSrNode
   id = programy.parser.template.nodes.id.TemplateIdNode
   size = programy.parser.template.nodes.size.TemplateSizeNode
   vocabulary = programy.parser.template.nodes.vocabulary.TemplateVocabularyNode
   eval = programy.parser.template.nodes.eval.TemplateEvalNode
   explode = programy.parser.template.nodes.explode.TemplateExplodeNode
   implode = programy.parser.template.nodes.implode.TemplateImplodeNode
   program = programy.parser.template.nodes.program.TemplateProgramNode
   lowercase = programy.parser.template.nodes.lowercase.TemplateLowercaseNode
   uppercase = programy.parser.template.nodes.uppercase.TemplateUppercaseNode
   sentence = programy.parser.template.nodes.sentence.TemplateSentenceNode
   formal = programy.parser.template.nodes.formal.TemplateFormalNode
   that = programy.parser.template.nodes.that.TemplateThatNode
   thatstar = programy.parser.template.nodes.thatstar.TemplateThatStarNode
   topicstar = programy.parser.template.nodes.topicstar.TemplateTopicStarNode
   star = programy.parser.template.nodes.star.TemplateStarNode
   input = programy.parser.template.nodes.input.TemplateInputNode
   request = programy.parser.template.nodes.request.TemplateRequestNode
   response = programy.parser.template.nodes.response.TemplateResponseNode
   date = programy.parser.template.nodes.date.TemplateDateNode
   interval = programy.parser.template.nodes.interval.TemplateIntervalNode
   system = programy.parser.template.nodes.system.TemplateSystemNode
   extension = programy.parser.template.nodes.extension.TemplateExtensionNode
   learn = programy.parser.template.nodes.learn.TemplateLearnNode
   learnf = programy.parser.template.nodes.learnf.TemplateLearnfNode
   first = programy.parser.template.nodes.first.TemplateFirstNode
   rest = programy.parser.template.nodes.rest.TemplateRestNode
   log = programy.parser.template.nodes.log.TemplateLogNode
   oob = programy.parser.template.nodes.oob.TemplateOOBNode
   xml = programy.parser.template.nodes.xml.TemplateXMLNode
   addtriple = programy.parser.template.nodes.addtriple.TemplateAddTripleNode
   deletetriple = programy.parser.template.nodes.deletetriple.TemplateDeleteTripleNode
   select = programy.parser.template.nodes.select.TemplateSelectNode
   uniq = programy.parser.template.nodes.uniq.TemplateUniqNode
   search = programy.parser.template.nodes.search.TemplateSearchNode

| 新たなtemplate要素を作成する場合、``programy.parser.template.nodes.base.TemplateNode`` を基底クラスとして継承します。
| 本基底クラスを継承することで、AIMLのXML要素をtemplateクラスのノード属性にカプセル化します。また、template展開中に子要素も含めた結果のテキスト化も行います。

オーバーライドする主要なメソッドには、以下のものがあります。

-  ``def parse_expression(self, graph, expression)``

   xmlの要素を解析して展開します。AIMLパーサの環境が graph で、該当タグのxml要素が expression で引き渡されます。展開できない場合は必要に応じて適切な例外をスローします。

-  ``def resolve_to_string(self, client_context)``

   要素の内容を文字列(応答文)に展開します。後述の resolve() によって呼び出されます。

-  ``def resolve(self, client_context)``

   個々のtemplate要素を文字列に展開するために、brainによって呼び出されます。文字列を作成するために子要素を辿り、下位から順に resolve() を実行して結果文字列を生成（結合）します。

-  ``def to_string(self)``

   デバッグやロギングの出力の為に、要素情報を文字列表現に変換します。

-  ``def to_xml(self, client_context)``

   要素の内容をXML形式に変換します。これは、:ref:`learnf/learnf<template_learnf>` の結果としてaimlファイルを作成する際に利用したり、XML形式の `Braintree` を出力する場合に利用します。
