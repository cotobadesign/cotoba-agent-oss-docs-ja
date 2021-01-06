Security
============================

認証と承認の2段階のセキュリティ定義があります。

認証(Authentication)
----------------------------

| 認証機能は要件に応じたユーザ管理等の個別の実装が必要になる機能のため、使用していません。
| また、外部サービスを利用して認証を行う、``account_linker`` については、対話処理とは別に実施すべき機能のため使用できません。
| 別途、必要な認証手続きを上位のアプリケーション等で行った上、対話処理を実施される事を推奨します。

| 以下に、認証に関する基本的な制御構造について説明します。
| 認証はコアコードのbrain内で実施され、ユーザからの発話文毎に処理される ask_question() 内で呼び出されます。
| コンフィグレーションで指定された認証サービスを利用して、認証されたユーザのみが対話処理を実施できる様に制御します。

.. code:: python

   class Brain(object):

       def __init__(self, bot, configuration: BrainConfiguration):
          self._security = SecurityManager(configuration.security)

       def authenticate_user(self, client_context):
           return self._security.authenticate_user(client_context)

       def ask_question(self, bot, clientid, sentence):

           authenticated = self._security.authenticate_user(client_context)
           if authenticated is not None:
               return authenticated

| 認証サービスは、コンフィグレーションの定義に従って、SecurityManagerによってロードされるクラスで、その基底クラスは次のように定義されています。
| SecurityManagerの authenticate_user() を実施することで、認証サービス内の authenticate() が呼び出されます。

.. code:: python

   class Authenticator(object):

       def __init__(self, configuration: BrainSecurityConfiguration):
           self._configuration = configuration

       @property
       def configuration(self):
           return self._configuration

       def get_default_denied_srai(self):
           return self.configuration.denied_srai

       def authenticate(self, clientid: str):
           return False

以下の認証サービスの例では、対話処理のアプリケーションの識別名となる'clientid'と、個々のユーザの識別名'userid'で制御しています。
この実装では、clientid：'console'でアクセスされたユーザの'userid'を'authorised'リストで管理して、認証を行うシンプルなものです。

.. code:: python

   class ClientIdAuthenticationService(Authenticator):

       def __init__(self, configuration: BrainSecurityConfiguration):
           Authenticator.__init__(self, configuration)
           self.authorised = [
               "console"
           ]

       def user_auth_service(self, client_context):
           return False

       def _auth_clientid(self, client_context):
           authorised = self.user_auth_service(client_context)
           if authorised is True:
               self.authorised.append(client_context.userid)
           return authorised

       def authenticate(self, client_context):
           try:
               if client_context.userid in self.authorised:
                   return True
               else:
                   if self._auth_clientid(client_context) is True:
                       return True

                   return False
           except Exception as excep:
               YLogger.error(client_context, str(excep))
               return False

上記機能を使用するには、Brainコンフィグレーションの :ref:`security<config_security>` で、以下の項目を定義する必要があります。

.. code:: yaml

   brain:
       security:
           authentication:
               classname: programy.security.authenticate.clientidauth.ClientIdAuthenticationService
    

.. _security_authorisation:

承認(Authorisation)
----------------------------

| 承認は、templateの :ref:`authorise<template_authorise>` 要素において、配下にある各要素の展開を制御する為に行います。
| 承認も認証と同じく要件に応じた処理が必要であり、変更を可能にする為、コンフィグレーションで指定された承認サービスを利用する方式で行います。

``authorise`` 要素での承認処理は、以下の様に行っています。

.. code:: python

   class TemplateAuthoriseNode(TemplateNode):

       def resolve_to_string(self, client_context):

           if client_context.brain.security.authorisation is not None:
               try:
                   allowed = client_context.brain.security.authorisation.authorise(client_context.userid, self.role)
               except AuthorisationException:
                   allowed = False

承認サービスは、コンフィグレーションの定義に従って、brain.security.authorisation にロードされるクラスで、その基底クラスは次のように定義されています。

.. code:: python

   class Authoriser(object):

       def __init__(self, configuration: BrainSecurityConfiguration):
           self._configuration = configuration

       @property
       def configuration(self):
           return self._configuration

       def get_default_denied_srai(self):
           return self.configuration.denied_srai

       def authorise(self, userid, role):
           return False


以下に、authorise要素で使用している、ユーザの識別名'userid'を元に行う承認処理を説明します。

| シナリオでの承認を用いた記載は、利用制限をかけるtemplate要素を、``authorise`` タグで囲むことで指定します。
| 以下のシナリオの場合、発話文で'ALLOW ACCESS'が入力された場合、指定された'userid'が 'root' の権限(role)を持っている場合、'Access Allowed' の文字が変え入りますが、持っていない場合には空文字が返ります。

.. code:: xml

       <category>
           <pattern>ALLOW ACCESS</pattern>
           <template>
               <authorise role="root">
                   Access Allowed
               </authorise>
           </template>
       </category>

承認処理を行うクラスの実装は以下になります。

.. code:: python

   class BasicUserGroupAuthorisationService(Authoriser):

       def __init__(self, config: BrainSecurityAuthorisationConfiguration):
           Authoriser.__init__(self, config)
           self._users = {}
           self._groups = {}

       @property
       def users(self):
           return self._users

       @property
       def groups(self):
           return self._groups

       def initialise(self, client):
           self.load_users_and_groups(client)

       def load_users_and_groups(self, client):
           if client.storage_factory.entity_storage_engine_available(StorageFactory.USERGROUPS) is True:
               storage_engine = client.storage_factory.entity_storage_engine(StorageFactory.USERGROUPS)
               usergroups_store = storage_engine.usergroups_store()
               usergroups_store.load_usergroups(self)
           else:
               YLogger.warning(self, "No user groups defined, authorisation tag will not work!")

       def authorise(self, userid, role):
           if userid not in self._users:
               raise AuthorisationException("User [%s] unknown to system!" % userid)

           if userid in self._users:
               user = self._users[userid]
               return user.has_role(role)
           return False

尚、本機能を使用する為、Brainコンフィグレーションの :ref:`security<config_security>` で、以下の項目を定義する必要があります。

.. code:: yaml

       security:
           authorisation:
               classname: programy.security.authorise.usergroupsauthorisor.BasicUserGroupAuthorisationService
               denied_srai: AUTHORISATION_FAILED
               denied_text: Access Denied!

　※ ``denied_srai`` 、 ``denied_text`` は、認証失敗時の動作に関するオプションです。


.. _security_usergroups:

ユーザグループファイル
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

| 承認処理を制御するために、ユーザ（userId）と権限（role）の関係を定義するものが、Storageの :ref:`usergroupsエンティティ<storage_entity>` で指定したファイルになります。
| 定義は、yaml形式で行います。基本的な記述形式は、以下の２つの形式になります。

１つ目は、ユーザ毎に権限を記載する方式です。

.. code:: yaml

   users:
     ユーザ名:
       roles: 権限名リスト

２つ目は、グループ名を規定し、グループ毎に権限を指定し、該当するユーザ名を列記する方式です。

.. code:: yaml

   groups:
     グループ名:
       roles: 権限名リスト
       users: ユーザ名リスト


以下の例では、'administrator'の権限は'rootと'user'、'others'の権限は'user'、そして、'guest1'と'guest2'の権限は'guest'になります。

設定例

.. code:: yaml

   users:
     administrator:
       roles: root, user
     others:
       roles: user

   groups:
      general:
         users: guest1, guest2
         roles: guest

| また、groupsを子タグで指定することで関係付けができるため、以下の様な指定も可能です。
| この場合には、結果的に、'console'の権限は'user'に加えて、'root'、'admin'、’system'、'ask'が設定されます。

設定例

.. code:: yaml

   users:
     console:
       roles: user
       groups: sysadmin

   groups:
     sysadmin:
       roles: root, admin, system
       groups: user
     user:
       roles: ask

| 尚、グループを関係付けて記述した場合、最終的にユーザに対する権限が指定されないことがあります。基本的な記法をベースとしたシンプルな記述を推奨します。
| 又、記述純情に関係なく``users`` 定義の展開を先に行う為、``users`` 定義でユーザの権限を指定している場合、``groups`` 定義で同じユーザに別の権限を指定しても、``users`` での指定が優先されます。

