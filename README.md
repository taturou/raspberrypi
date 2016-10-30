# Rasbian on Raspberry Pi 3

## git

デフォルトで入っているものを使用。

## 日本語IME & 日本語フォント

    $ sudo ap-get install -y ibus-mozc fonts-takao

## Raspberry Pi の設定

メニューから。

### システム

デフォルトのまま。

### インターフェイス

SSHとVNCを有効に。

### パフォーマンス

デフォルトのまま。

### ローカライゼーション

ロケール:
* 言語: ja (Japanese)
* 国: JP (Japan)
* 文字セット: UTF-8

タイムゾーン:
* 地域: Asia
* 位置: Tokyo

キーボード:
* Country: 日本
* Variant: 日本語 (OADG 109A)

無線LANの国:
* 国: JP Japan

## Node.js

    $ sudo apt-get install -y nodejs npm
    $ node -v
    v0.10.29
    $ npm -v
    1.4.21
    $ which node
    /usr/bin/node
    $ which npm
    /usr/bin/npm
    $ sudo npm cache clean
    $ sudo npm install n -g
    /usr/local/bin/n -> /usr/local/lib/node_modules/n/bin/n
    n@2.1.4 /usr/local/lib/node_modules/n
    $ sudo n stable
    ...
    $ sudo rm /usr/bin/node /usr/bin/npm
    $ node -v
    v7.0.0
    $ npm -v
    3.10.8
    $ which node
    /usr/local/bin/node
    $ which npm
    /usr/local/bin/npm
    $ sudo ln -s /usr/local/bin/node /usr/bin/node
    $ sudo ln -s /usr/local/bin/npm /usr/bin/npm

## azure-iot-sdks

    $ git clone https://github.com/Azure/azure-iot-sdks

## Azure IoT Explorer

### インストール

    $ sudo npm install -g iothub-explorer@latest

### デバイス登録

"HostName" から始まる <connection-string> は以下から取得する。
rpi-hub (ここは作成した IoT Hub 名) -> 共有アクセスポリシー -> iothubowner -> 接続文字列—プライマリキー

    $ iothub-explorer "HostName={IoT Hub名}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={プライマリキー}" create {デバイス名} --connection-string

    Created device {デバイス名}

    -
      deviceId:                   {デバイス名}
      generationId:               636134277097566209
      etag:                       MA==
      connectionState:            Disconnected
      status:                     enabled
      statusReason:               null
      connectionStateUpdatedTime: 0001-01-01T00:00:00
      statusUpdatedTime:          0001-01-01T00:00:00
      lastActivityTime:           0001-01-01T00:00:00
      cloudToDeviceMessageCount:  0
      authentication:
        SymmetricKey:
          primaryKey:   {プライマリキー}
          secondaryKey: {セカンダリキー }
        x509Thumbprint:
          primaryThumbprint:   null
          secondaryThumbprint: null
    -
      connectionString: HostName={IoT Hub名}.azure-devices.net;DeviceId={デバイス名};SharedAccessKey={プライマリキー}

以下が成功すれば、デバイス登録できてる。

    $ iothub-explorer "HostName={IoT Hub名}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={プライマリキー}" monitor-events {デバイス名}

### データ送信サンプル

デバイス接続名は、上の iothub-explorer で作ったやつ。

    $ cd (azure-iot-sdks)/node/device/samples
    $ vi simple_sample_device.js
    var connectionString = '[IoT device connection string]';
    ↓
    var connectionString = 'HostName={IoT Hub名}.azure-devices.net;DeviceId={デバイス名};SharedAccessKey={プライマリキー}'
    $ sudo npm install
    $ node simple_sample_device.js

## Azure for Node-RED

Node-RED を自動起動させる。

    $ sudo systemctl enable nodered.service

Node-RED に Azure のコマンドを追加する。

    $ cd (azure-iot-sdks)/node/device/node-red
    $ sudo npm install -g
    $ sudo ln -s /usr/local/lib/node_module/node-red-contrib-azureiothubnode /usr/lib/node_module/node-red-contrib-azureiothubnode

スタートメニューから Node-RED を起動。
Webブラウザから http://localhost:1880 にアクセス。


## TI SensorTag for Node-RED

これは失敗。
ビルドできないっぽい。

    $ sudo npm install -g node-red-node-sensortag

## コマンドラインから SensorTag のデータを取得する

### SensorTagのアドレスを取得

    $ sudo hcitool lescan
    LE Scan ...
    {SensorTag address} (unknown)
    {SensorTag address} SensorTag

### bluepyに付属のサンプルでデータ取得

理由はわからないけど、失敗する。

    $ sudo apt-get install -y libglib2.0-dev
    $ git clone https://github.com/IanHarvey/bluepy.git
    $ cd bluepy
    $ python setup.py build
    $ sudo python setup.py install
    $ sudo python sensortag.py --all {SensorTag address}
    Connecting to 34:B1:F7:D5:5B:8C
    Traceback (most recent call last):
      File "sensortag.py", line 470, in <module>
        main()
      File "sensortag.py", line 432, in main
        tag.gyroscope.enable()
      File "sensortag.py", line 29, in enable
        self.ctrl = self.service.getCharacteristics(self.ctrlUUID) [0]
      File "/home/pi/iot_hackathon/bluepy/bluepy/btle.py", line 106, in getCharacteristics
        self.chars = self.peripheral.getCharacteristics(self.hndStart, self.hndEnd)
      File "/home/pi/iot_hackathon/bluepy/bluepy/btle.py", line 420, in getCharacteristics
        rsp = self._getResp('find')
      File "/home/pi/iot_hackathon/bluepy/bluepy/btle.py", line 334, in _getResp
        resp = self._waitResp(wantType + ['ntfy', 'ind'], timeout)
      File "/home/pi/iot_hackathon/bluepy/bluepy/btle.py", line 290, in _waitResp
        raise BTLEException(BTLEException.DISCONNECTED, "Device disconnected")
    btle.BTLEException: Device disconnected


