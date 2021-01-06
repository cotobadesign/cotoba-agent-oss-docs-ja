OOB
============================

``OOB(Out of Band)`` は、動的にロードされるクラスとAIMLのoob要素で指定するOOB機能の名称の組み合わせで実現します。
使用するOOB機能毎に、Pythonクラスを実装し、コンフィグレーションの :doc:`OOB設定<config/Config_Brain_OOB>` で、以下の様に機能名と関連付けを行う必要があります。

.. csv-table::
    :header: "パラメータ名","説明"
    :widths: 20,20,70

    "OOBの名称","","この項目に設定した名称が、AIMLで指定するOOB名になります。例では'email'がOOB名になります。"
    "","classname","実装を行うOOBのpythonのクラスパスを定義します。OOB内を実装する子要素は実装クラスで定義しており、configで指定する必要はありません。"

OOBの実装基底クラスは次のように定義されています。

.. code:: python

   # programy/oob/defaults/oob.py

   import xml.etree.ElementTree as ET

   class OutOfBandProcessor(object):

       def __init__(self):
           self._xml = None

       # Override this method to extract the data for your command
       # See actual implementations for details of how to do this
       def parse_oob_xml(self, oob: ET.Element):
           self._xml = oob
           return True

       # Override this method in your own class to do something
       # useful with the command data
       def execute_oob_command(self, client_context):
           raise NotImplementedError()

       def process_out_of_bounds(self, client_context, oob: ET.Element):
           if self.parse_oob_xml(oob) is True:
               return self.execute_oob_command(client_context)
           return ""

電子メールを送信するOOB機能がある場合、OOBを利用するAIMLは次のように記載します。

.. code:: xml

   <oob>
       <email>
           <to>宛先</to>
           <subject>件名</subject>
           <body>本文</body>
       </email>
   </oob>

この場合、OOBの処理クラスに渡される引数は以下のXMLになります。

.. code:: xml

   <oob>
       <to>宛先</to>
       <subject>件名</subject>
       <body>本文</body>
   </oob>


実装クラスは次のようになります。
parse_oob_xml()メソッドでAIMLのoob要素で指定された内容（XML形式）から引数値を取得し、execute_oob_command()メソッドで実際にメールを送信する処理を実装します。

.. code:: python

   # programy/oob/email/email.py

   from programy.oob.defaults.oob import OutOfBandProcessor

   class EmailOutOfBandProcessor(OutOfBandProcessor):

       def __init__(self):
           OutOfBandProcessor.__init__(self)
           self._to = None
           self._subject = None
           self._body = None

       def parse_oob_xml(self, oob: ET.Element):
           for child in oob:
               if child.tag == 'to':
                   self._to = child.text
               elif child.tag == 'subject':
                   self._subject = child.text
               elif child.tag == 'body':
                   self._body = child.text
               else:
                   logging.error ("Unknown child element [%s] in email oob"%(child.tag))

               if self._to is not None and \
                   self._subject is not None and \
                   self.body is not None:
                   return True

               logging.error("Invalid email oob command")
               return False

       def execute_oob_command(self, client_context):
           logging.info("EmailOutOfBandProcessor: Emailing=%s", self._to)
           return ""



OOB機能を利用するための定義として、コンフィグレーションで以下の設定を行います。

.. code:: yaml

    oob:
        email:
            classname: programy.oob.email.email.EmailOutOfBandProcessor


OOBの詳細設定方法は、 :doc:`OOBの設定<config/Config_Brain_OOB>` を参照してください。

