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



