---
title: "Run Bitcoin Full Node with Bitcoin Knots"
date: "2025-01-25 00:00:00 +0900"
categories: [Technology, Crypto]
tags: [Monero, Crypto, Bitcoin, BTC, Bitcoin Knots]
---

# LinuxにBitcoin Full NodeをBitcoin Knotsで構築
背景
- BTCは匿名性が低いことはなんとなく知っていた。
- 匿名性かつセキュアな環境でBTCを管理したかった。

## 導入と設定
- 公式github通りにインストールしていく
- [Github bitcoin knots build-unix](https://github.com/bitcoinknots/bitcoin/blob/27.x-knots/doc/build-unix.md)

1. ダウンロード  
   必要な依存関係のインストール: 
   `sudo apt-get update`
   `sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3`

2. ライブラリのインストール:
   イベントライブラリとブーストライブラリ: `sudo apt-get install -y libevent-dev libboost-dev`
   ZMQライブラリ: `sudo apt-get install libzmq3-dev`
   USDTライブラリ: `sudo apt install systemtap-sdt-dev`
   GUIサポート（bitcoin-qtをビルドする場合）: `sudo apt-get install -y qtbase5-dev qttools5-dev qttools5-dev-tools qtwayland5`
   QRコード生成ライブラリ（オプション）: `sudo apt-get install -y libqrencode-dev`

3. GitHubからBitcoin Knotsのソースコードをクローン
   `git clone https://github.com/bitcoinknots/bitcoin.git`
   `cd bitcoin`

4. ビルドの構成(configure)と実行
   ビルドシステムの初期化: `./autogen.sh`
   ウォレットを無効化するし、Makefileが生成（フルノードのみ実行する場合）: `./configure --disable-wallet`
   ⇢ sparrow walletを利用するためbuilt-inのwalletはいらないため。
   ビルド実行: `make`
   ⇢ `make: *** No targets specified and no makefile found.  Stop.`のようなエラーがでたらログの内容をChatGPTに投げて解決。
   ⇢ `rsvg-convert`がシステムにインストールされていなかったのでインストール: `sudo apt-get install librsvg2-bin`
   ビルド再実行: `make`
   `Nothing to be done for 'all'`と`Nothing to be done for 'all-am'`と出れば完了。

5. ブロックチェーンの同期を開始:フルノードだと現在720GB前後
   Bitcoinのフルノードがブロックチェーンデータを保存: `./src/bitcoind -datadir=/media/bentham/SSD2/BTC/full_node_data`
   完了まで数日かかる。

参考：
- [Github bitcoin knots build-unix](https://github.com/bitcoinknots/bitcoin/blob/27.x-knots/doc/build-unix.md)

