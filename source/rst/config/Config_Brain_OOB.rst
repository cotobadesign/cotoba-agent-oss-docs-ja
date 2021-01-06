OOBの設定
=====================================

OOB(Out of Band)処理には、使用するOOBの機能毎にPythonクラスが必要です。
このクラスはAIMLのoob要素で引き渡された文字列を引数として処理し、クライアント側で該当するシステムコマンドを実行します。

以下は、対話エンジンが保持する実装クラスの例です。実行関数内は引数チェックを行なっているだけで具体的な動作は実施していません。
実際に利用する場合は、個々のシステムに応じた実装が必要です。

.. code:: yaml

    oob:
      default:
        classname: programy.oob.defaults.default.DefaultOutOfBandProcessor
      alarm:
        classname: programy.oob.defaults.alarm.AlarmOutOfBandProcessor
      camera:
        classname: programy.oob.defaults.camera.CameraOutOfBandProcessor
      clear:
        classname: programy.oob.defaults.clear.ClearOutOfBandProcessor
      dial:
        classname: programy.oob.defaults.dial.DialOutOfBandProcessor
      dialog:
        classname: programy.oob.defaults.dialog.DialogOutOfBandProcessor
      email:
        classname: programy.oob.defaults.email.EmailOutOfBandProcessor
      geomap:
        classname: programy.oob.defaults.map.MapOutOfBandProcessor
      schedule:
        classname: programy.oob.defaults.schedule.ScheduleOutOfBandProcessor
      search:
        classname: programy.oob.defaults.search.SearchOutOfBandProcessor
      sms:
        classname: programy.oob.defaults.sms.SMSOutOfBandProcessor
      url:
        classname: programy.oob.defaults.url.URLOutOfBandProcessor
      wifi:
        classname: programy.oob.defaults.wifi.WifiOutOfBandProcessor


.. csv-table::
    :header: "パラメータ","説明","例","デフォルト"
    :widths: 10,20,60,10

    "classname","OOB実装のフルPythonクラスパス","programy.oob.defaults.alarm.AlarmOutOfBandProcessor","None"

OOBの処理方法は、:doc:`OOB<../OOB>` を参照してください。