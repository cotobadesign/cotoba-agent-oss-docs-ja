Extensions
=======================================

概要
----------------------------------------

Extensionsとは、拡張機能を個別に実装するための機能で、templateのextension要素に、path属性としてPythonパスを記載することで追加機能を呼び出すことができる仕組みを提供します。


カスタムエクステンションの実装
----------------------------------------

カスタムエクステンションの実装クラスでは、以下の基底クラスを継承する必要があります。

.. code:: python

       programy.extensions.base.Extension

| 次に実装例を示します。
| エクステンションの処理を実行するメソッドは ``execute()`` で、エクステンションのクラスには、``context`` と ``data`` を引数としたexecuteメソッドを実装します。
| ``context`` には、クライアント・ユーザ毎の管理情報を格納したClientContextクラス（programy.context.ClientContext）が引き渡され、ユーザID等の情報を取得することができます。
| ``data`` には、extensionタグの内容が文字列で設定されます。

以下は、数値演算を行うカスタムエクステンションの例です。

.. code:: python

    # programy/extensions/calc.py

    from programy.extensions.base import Extension

    class CalcExtension(Extension):

        def execute(self, context, data):
            try:
                result = str(eval(data))
                return result
            except Exception:
                return None

| AIMLでカスタムエクステンションを利用する場合、以下の様にextension要素を記載します。
| AIMLのextension要素の処理としては、path属性で指定されたクラスをロードし、execute() を呼び出します。
| execute() の戻り値が、extension要素の結果になりますので、戻り値には文字列を設定する必要があります。（Noneの場合は、空文字扱いになります。）

なお、path属性で指定されたクラスのロード失敗や、execute() 処理に異常がある場合、対話エンジンは処理中の例外発生として扱います。
(応答文定義の :ref:`exception_response<config_bot_response>` が指定さている場合、指定の文字列が返ります。)

.. code:: xml

   <category>
       <pattern>
           演算 *
       </pattern>
       <template>
            結果は、
            <extension path="programy.extensions.calc.CalcExtension">
                <star />
            </extension>
            です。
       </template>
   </category>


| Input: 演算 1 + 2 * 3 / 4 - 5
| Output: 結果は、-2.5です。

この場合、execute() のdata引数には <star /> の展開文字列として、'1 + 2 * 3 / 4 - 5' が引き渡されます。


シナリオ変数の利用
----------------------------------------

| カスタムエクステンション実行時に引数として通知されるクライアント・ユーザ毎の管理情報：ClientContextクラスを利用することで、シナリオ内で設定された変数の情報も操作できます。
| 各変数は対話内容により変動する為、クライアント・ユーザ毎の対話情報（conversation）で管理されています。対話情報の取得は、以下の様に行います。

.. code::

    def execute(self, context, data):

        conversation = context.bot.get_conversation(context)

対話情報を取得することで、変数内容の参照や、設定を行うことが可能になります。変数の種別については、template要素の :ref:`get<template_get>` の記述を参照してください。

なお、各変数の値は文字列形式で格納されています。JSON形式のデータを利用する場合は、以下の操作で形式変換を行って使用してください。

- 取得時： <JSON変数：辞書型> = json.loads(<シナリオ変数:文字列型>)
- 格納時： <シナリオ変数:文字列型> = json.dumps(<JSON変数：辞書型>, ensure_ascii=False)


継続保持グローバル変数(name)の利用
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

name変数は、対話情報内の ``properties`` に辞書データとして格納されています。操作関数には以下を使用します。

.. code::

    name_list = conversation.properties                   # 変数（辞書）リストの取得
    get_value = conversation.proprty('変数名')             # 任意変数値の取得
    set_value = conversation.set_property('変数名', '値')  # 任意変数の追加・変更
    remove = conversation.set_property('変数名', ’’)       # 任意変数の削除（値に空文字を指定）


指定範囲保持グローバル変数(data)の利用
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

data変数は、対話情報内の ``data_properties`` に辞書データとして格納されています。操作関数には以下を使用します。

.. code::

    data_list = conversation.data_properties                   # 変数（辞書）リストの取得
    get_value = conversation.data_proprty('変数名')             # 任意変数値の取得
    set_value = conversation.set_data_property('変数名', '値')  # 任意変数の追加・変更
    remove = conversation.set_data_property('変数名', ’’)       # 任意変数の削除（値に空文字を指定）


ローカル変数(var)の利用
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

var変数は category単位の処理内でのみ有効な変数のため、対話情報においてcategory毎の情報を管理するquestionクラスの ``_properties`` に辞書データとして格納されています。
操作を行うためには、以下の様に、現在処理中の questionクラスを取得した上で操作関数を使用します。

.. code::

    question = conversation.current_question()

    var_list = question._properties                   # 変数（辞書）リストの取得
    get_value = question.proprty('変数名')             # 任意変数値の取得
    set_value = question.set_property('変数名', '値')  # 任意変数の追加・変更
    remove = question.set_property('変数名', ’’)       # 任意変数の削除（値に空文字を指定）


bot固有プロパティの取得
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

bot固有のプロパティ（固定値）の取得には、以下の操作関数を使用します。

.. code::

    bot_property = conversation.bot.brain.properties.property('プロパティ名')
