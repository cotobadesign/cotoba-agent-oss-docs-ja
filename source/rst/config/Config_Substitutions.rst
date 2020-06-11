コンフィグの置換
===========================

ライセンスキーの置換
-------------------------

- license.keysの設定値を置換する機能
 ``LICENSE_KEY:VALUE``  をコンフィグの構成アイテムに指定した場合、license.keysの値が置換される機能です。
 

設定例

| 以下のように、コンフィグファイルに ``LICENSE_KEY:VALUE`` と記載しておきます。
| ``LICENSE_KEY:`` は固定値で、 ``VALUE`` はlicense.keysに記載するキー名を指定します。

.. code:: yaml

   client:
     email:
       username: LICENSE_KEY:EMAIL_USERNAME
       password: LICENSE_KEY:EMAIL_PASSWORD

license.keysに以下のように記載すると、実行時に上記のLICENSE_KEY:VALUEをlicense.keysの内容で置換し動作します。

.. code:: text

   EMAIL_USERNAME=sample@somewere.com
   EMAIL_PASSWORD=qwerty

gitを利用する場合、license.keysを.gitignoreに設定することで意図せず登録してしまうことが防げます。


.. _config_subsitutions:

コマンドラインオプション置換
--------------------------------

| substitutions.txtファイルをコマンドライン引数にし、対話エンジンを起動すると、起動時の引数として置換するパラメータを指定することが出来ます。
| コマンドラインオプション --subs で指定したファイルで置換を行います。

設定例

| 以下のように、コンフィグファイルに $ +VALUE と記載しておきます。
| VALUEはsubstitutions.txtに記載するキー名を指定します。

.. code:: yaml

   client:
     email:
       host: $EMAIL_HOST
       port: $EMAIL_PORT

substitutions.txtを以下のように記載すると、実行時に上記の$VALUEをsubstitutions.txtの内容で置換し、動作します。

.. code:: text

   $EMAIL_HOST:prod_server.com
   $EMAIL_PORT:9999

これにより、 ``--subs substitutions.txt`` と起動時に置換リストを設定するだけで、configの内容は編集せず実行時に設定内容を変更することができます。
