ログ設定
=====================

.. _config_logging:


Pythonのロギング機能を用いロギングを行います。 
ログ設定オプションを指定する方法の詳細については、 `Pythonロギングドキュメント <https://docs.python.jp/3/library/logging.config.html#user-defined-objects>`__ を参照してください。

/tmp/y-bot.logに出力するログ設定例です。

.. code:: yaml

       version: 1
       disable_existing_loggers: False

       formatters:
         simple:
           format: '%(asctime)s  %(name)-10s %(levelname)-7s %(message)s'

       handlers:
         file:
           class: logging.handlers.RotatingFileHandler
           formatter: simple
           filename: /tmp/y-bot.log

       root:
         level: DEBUG
         handlers:
             - file
