ログ設定
=====================

.. _config_logging:

対話エンジンのログ出力は、Pythonのロギング機能を用いて行います。 
ログ設定オプションを指定する方法の詳細については、 `Pythonロギングドキュメント <https://docs.python.jp/3/library/logging.config.html#user-defined-objects>`__ を参照してください。

以下は、ログコンフィグの指定として、/tmp/y-bot.logにログ出力する設定例です。

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
