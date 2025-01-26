---
title: "Run Bitcoin Full Node with Bitcoin Knots"
date: "2025-01-25 00:00:00 +0900"
categories: [Technology, Crypto]
tags: [Crypto, Bitcoin, BTC, Bitcoin Knots]
---

# LinuxにBitcoin Full NodeをBitcoin Knotsで構築
背景
- BTCは匿名性が低いことはなんとなく知っていた。
- 匿名性かつセキュアな環境でBTCを管理したかった。

所感: 
- Bridge導入でめちゃくちゃハマった。
- これでBTCもかなり匿名性の高い方法で管理と取引ができるようになった。

課題: 
- Sparrow walletを導入
- Bisqの設定を見直し匿名性を上げる


## 導入
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

5. ブロックチェーンの同期を開始:フルノードだと現在700GB前後
   Bitcoinのフルノードがブロックチェーンデータを保存: `./src/bitcoind -datadir=/media/bentham/SSD2/BTC/full_node_data`
   進捗ログの確認：`tail -f /media/bentham/SSD2/BTC/full_node_data/debug.log`
   12時間前後で完了
   起動の確認：`bitcoin-qt -datadir=/media/bentham/SSD2/BTC/full_node_data`
   ![alt text](../assets/images/2025-01-26_09-30.png)
   ![alt text](../assets/images/2025-01-26_09-31.png)

6. 起動方法の変更
   bitcoin.conf にデフォルトのデータディレクトリを記載:`nano ~/.bitcoin/bitcoin.conf`
   追加:`datadir=/media/bentham/SSD2/BTC/full_node_data`
   起動の確認完了: `bitcoin-qt `と`bitcoind`

## セキュアな設定
- フルノードの利点を活かす

1. RPCのセキュリティ強化
   RPCは外部からノードにアクセスするためのインターフェースであるので適切に設定する。
   RPCユーザーとパスワードの設定: 
   `nano ~/.bitcoin/bitcoin.conf`
   ```bash
   rpcuser=yourusername
   rpcpassword=yourstrongpassword
   ```
   RPCポートの制限：ローカルネットワークだけにアクセスを許可
   `nano ~/.bitcoin/bitcoin.conf`
   `rpcbind=127.0.0.1`を追加

2. ノードのセキュリティ強化
   Bitcoinノードが接続するピア（他のノード）の最大数を制限する設定: デフォルトは125
   この設定は、**P2P接続（Bitcoinネットワークの他のノードとの接続）**に関するもの
   接続数が 少なすぎることで特定されるリスクを避けるためには、最小限の接続数で安全に運用することが重要
   `nano ~/.bitcoin/bitcoin.conf`
   `maxconnections=XX`  最大接続数をXXに設定(20-60)くらいで設定するのが良い。

3. 匿名性の強化
   Torを使用した通信の匿名化
      ```bash
      proxy=127.0.0.1:9050
      bind=127.0.0.1
      listen=1
      discover=1
      dnsseed=0
      onlynet=onion
      ```
      tor起動:`tor`
      bitcoin knot起動:`bitcoin-qt `
      bitcoin-qtがTorネットワーク上で動作していることを確認:
      ![alt text](../assets/images/2025-01-26_12-21.png)
      paringタブでもOnionアドレスを確認
   Bridgeの設定
      1. Bridgeを取得: 
         Torブラウザで接続しbridgeを取得:`https://bridges.torproject.org/`
            get bridges ⇢ obsf4: ２つのアドレスが出るのでコピー
         取得したアドレスをチェック: `https://metrics.torproject.org/rs.html`
            速度やadditional flagでセキュリティ上問題ないかチェック
      2. Torの設定にBridgeを追加
         torrcファイルを開く: `sudo nano /etc/tor/torrc`
         ファイル末尾に追加してBridgeを設定:
         ![alt text](../assets/images/2025-01-26_20-15.png)
         obfs4プラグインのインストール:`sudo apt install obfs4proxy`
   Bridgeを通したTor接続の確認: めちゃくちゃハマった
      通常の起動では使えるブリッジを設定しても接続できなかった: `tor`も`sudo systemctl start tor`不可。
      tor-browserでbuilt-in bridgeもカスタムブリッジも接続不可。
      windowsのPCで試すと、tor-browserでのブリッジ成功した。← 謎
      `sudo systemctl status tor@default.service`: この起動方法で成功した
         ![alt text](../assets/images/2025-01-26_20-25.png)

4. bitcoin knot起動
   必ず最初にtorを起動:``sudo systemctl status tor@default.service``
   その後に起動:`bitcoin-qt`
         

## 理解促進

- RPC (Remote Procedure Call) 
   - リモートでコンピュータに対して命令を実行させる仕組み
   - bitcoind の場合、RPCを使用することで、Bitcoinノードに対してブロックやトランザクション情報を取得したり、送金を行ったりすることができる。
   - 例えば、ラップトップ上にセットアップしたBitcoinノード（bitcoind）に対して、外部から以下のような操作をすることが可能：
      - 現在の残高を取得する
      - 送金をリクエストする
      - トランザクション履歴を取得する
   - この操作は、ローカルのbitcoind ノードをコマンドラインやスクリプトから呼び出すことで行うが、RPCを使うことで、外部のコンピュータやサーバーからも同じように操作することができるようになる。


参考：
- [Github bitcoin knots build-unix](https://github.com/bitcoinknots/bitcoin/blob/27.x-knots/doc/build-unix.md)

