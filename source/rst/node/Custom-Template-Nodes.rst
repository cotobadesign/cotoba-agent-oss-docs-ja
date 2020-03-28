カスタムtemplate要素
============================

| パーサーは、template_nodes.confを含むシステムで定義された全てのAIMLタグをサポートします。
| template_nodes.confは、要素の実装を変更したり、独自の要素を追加することもできます。
| ``要素の実装を変更すると、パーサが動作しなくなったりパフォーマンスや他の要素の動作に重大な影響を与える可能性があるため全体の動作を理解した上でご利用ください。``


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

| 各template要素は、``programy.parser.template.nodes.base.TemplateNode`` を基底クラスとして継承します。
| programy.parser.template.nodes.base.TemplateNode はtemplate要素の基底クラスであり、XML要素をtemplateクラスのノード属性にカプセル化します。また、template評価中に子要素も含めたテキスト化の評価にも用います。

オーバーライドする主要なメソッドには、以下があります。

-  ``def parse_expression(self, graph, expression)`` - xmlの要素に解析します。展開できない場合は必要に応じて適切な例外をスローします。
-  ``def resolve_to_string(self, bot, clientid)`` - 要素を文字列(応答文)に展開します。resolve()によって呼び出されます。
-  ``def resolve(self, bot, clientid)`` - 個々のtemplate要素を文字列に展開するためにbrainによって呼び出されます。文字列を作成するために子要素を辿り順次処理し文字列を結合します。
-  ``def to_string(self)`` - デバッグやロギングの為に要素情報を文字列表現に変換するメソッドです。
-  ``def to_xml(self, bot, clientid)`` - 要素の内容をXML形式に変換します。これは、:ref:`learnf<template_learnf>` の一部としてaimlファイルを作成する際に利用したり、XML形式の `Braintree` へ出力時に利用するメソッドです。
