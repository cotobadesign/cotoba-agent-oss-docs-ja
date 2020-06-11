Security
============================

認証と承認の2段階のセキュリティ定義があります。

認証(Authentication)
----------------------------

認証はコアコードのbrain内で実施されます。
ユーザの問い合わせ毎にask_question()が呼び出されます。
そのパラメータの1つに'clientid'があり、これは発話を行なっているユーザ毎のIDになります。
別途必要な認証手続きを実施する事を推奨します。
以下の例では、'clientid'が、コンソールクライアントの場合'console'と設定しますが、RESTでのアクセス等複数名がアクセスする場合、ユニークなIDを割り当てて設定する必要があります。



.. code:: python

       def ask_question(self, bot, clientid, sentence) -> str:

           if self.authentication is not None:
               if self.authentication.authenticate(clientid) is False:
                   logging.error("[%s] failed authentication!")
                   return self.authentication.configuration.denied_srai

認証サービスは、動的にロードされるクラスで定義され、その基本クラスは次のように定義しています。

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

この実装は'authorised'リストにあるIDを照合するシンプルなものです。
より高度な認証を行う場合、外部サービスを実装することもできますが、本プログラムをサーバで利用する場合、別途必要な認証手続きを実施する事を推奨します。

.. code:: python

   class ClientIdAuthenticationService(Authenticator):

       def __init__(self, configuration: BrainSecurityConfiguration):
           Authenticator.__init__(self, configuration)
           self.authorised = [
               "console"
           ]

       # Its at this point that we would call a user auth service, and if that passes
       # return True, appending the user to the known authorised list of user
       # This is a very naive approach, and does not cater for users that log out, invalidate
       # their credentials, or have a TTL on their credentials
       # #Exercise for the reader......
       def _auth_clientid(self, clientid):
           authorised = False # call user_auth_service()
           if authorised is True:
               self.authorised.append(clientid)
           return authorised

       def authenticate(self, clientid: str):
           try:
               if clientid in self.authorised:
                   return True
               else:
                   if self._auth_clientid(clientid) is True:
                       return True

                   return False
           except Exception as excep:
               logging.error(str(excep))
               return False

上記機能を使用するには、config.yamlのSecurityセクションで、以下の項目を有効にする必要があります。

.. code:: yaml

   brain:
       security:
           authentication:
               classname: programy.security.authenticate.clientidauth.ClientIdAuthenticationService
               denied_srai: AUTHENTICATION_FAILED

.. csv-table::
    :header: "パラメータ名","説明"
    :widths: 30,70

    "classname","基底クラス ‘Authenticator’を実装するpythonのパスを定義します。"
    "denied_srai","認証に失敗すると、インタプリタは設定で定義された文章をSRAIとして利用できます。(上記例では'AUTHENTICATION_FAILED'がSRAIに設定される) 
    AIMLファイルには、アクセスが拒否されていることを示す適切なテキストを含むカテゴリパターンとして含める必要があります。"
    

承認(Authorisation)
----------------------------

承認は、ユーザ、グループ、ロールで定義します。

.. csv-table::
    :header: "パラメータ名","説明"
    :widths: 30,70

    "User","単一ユーザの承認情報を定義します。
    ユーザを1つまたは複数のグループに含めることで、特定のロールと継承されたロールの両方を割り当てることができます。"
    "Group","一つ以上のロールが割り当てられた、ユーザのグループ単位。"
    "Role","ユーザグループに割り当てる、任意の権限文字列。"


基底承認クラスは以下のように定義されています。

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

ユーザ、グループ、ロールベースの承認を実行するこの基底クラスの実装は次のとおりです。

.. code:: python

   class BasicUserGroupAuthorisationService(Authoriser):

       def __init__(self, config: BrainSecurityConfiguration):
           Authoriser.__init__(self, config)
           self.load_users_and_groups()

       def load_users_and_groups(self):

           self._users = {}
           self._groups = {}

           if self.configuration.usergroups is not None:
               loader = UserGroupLoader()
               self._users, self._groups = loader.load_users_and_groups_from_file(self.configuration.usergroups)
           else:
               logging.warning("No user groups defined, authorisation tag will not work!")

       def authorise(self, clientid, role):
           if clientid not in self._users:
               raise AuthorisationException("User [%s] unknown to system!"%clientid)

           if clientid in self._users:
               user = self._users[clientid]
               return user.has_role(role)
           else:
               return False

上記機能を使用するには、config.yamlのSecurityセクションで、以下の項目を有効にする必要があります。

.. code:: yaml

       security:
           authorisation:
               classname: programy.security.authorise.usergroupsauthorisor.BasicUserGroupAuthorisationService
               denied_srai: AUTHORISATION_FAILED
               usergroups: ../storage/security/roles.yaml


.. csv-table::
    :header: "パラメータ名","説明"
    :widths: 30,70

    "classname","基底クラス ‘Authenticator’を実装するpythonのパスを定義します。"
    "denied_srai","認証に失敗すると、インタプリタはこの設定で定義された文章をSRAIとして利用できます。(上記例では'AUTHORISATION_FAILED'がSRAIに設定される) 
    AIMLファイルには、アクセスが拒否されていることを示す適切なテキストを含むカテゴリパターンとしてこれを含める必要があります。"
    "usergroups","ユーザ、ユーザグループ、ロール設定ファイルを指定します。"

ロール設定ファイルのフォーマットは以下の通りです。

.. code:: yaml

   users:
     console:
       roles:
         user
       groups:
         sysadmin

   groups:
     sysadmin:
       roles:
         root, admin, system
       groups:
         user

     user:
       roles:
         ask

AIMLの承認を用いた記載方法は、テンプレートを'authorise'タグで囲みます。
'ALLOW ACCESS'が入力され、ユーザが'root'権限を持っていない場合、denied_sraiで定義されたsraiタグの文字列として利用されます。

.. code:: xml

       <category>
           <pattern>ALLOW ACCESS</pattern>
           <template>
               <authorise role="root">
                   Access Allowed
               </authorise>
           </template>
       </category>
