Extensions
=======================================

概要
----------------------------------------

Extensionsとは、拡張機能を個別に実装するための機能で、extension要素のpathにフルPythonパスを記載することで追加機能を呼び出すことができる仕組みを提供します。


カスタムエクステンションの実装
----------------------------------------

エクステンションは、以下の基底クラスを継承する必要があります。

.. code:: python

       programy.extensions.Extension

次に実装例を示します。extension要素の内容を実行するメソッドはexecuteで、bot,clientid,dataを引数としたexecuteメソッドを実装します。
dataはextension要素の内容が半角スペース区切りで設定されています。

.. code:: python

    from programy.extensions.base import Extension
    
    class GeoCodeExtension(Extension):
        __metaclass__ = ABCMeta

        @abstractmethod
        def execute(self, context, data):
            latlng = getGeoCode(data)
            if latlng is not None:
                return "%s,%s"%(
                    latlng.latitude, latlng.longitude
                )

            return None

AIMLでextensionを用いてカスタムエクステンションを利用する場合、以下の例のようにextension要素を記載します。
AIMLのextensionタグのノード処理としては、属性pathで定義されたクラスをロードし、execute()が呼び出されます。
execute()の戻り値がtemplateの結果になります。

.. code:: xml

   <category>
       <pattern>
           * の座標は？
       </pattern>
       <template>
            緯度、経度は、
            <extension path="programy.extensions.geocode.geocode.GeoCodeExtension">
                <star />
            </extension>
            です。
       </template>
   </category>


| Input 東京駅の座標は？
| Output: 緯度、経度は、35.681236,139.767125です。
