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

所感
- Bridge導入でめちゃくちゃハマった。
- これでBTCもかなり匿名性の高い方法で管理と取引ができるようになった。

課題
- Sparrow Walletを導入
- Bisqの設定を見直し匿名性を上げる

---

## 導入手順
### 1. ダウンロード  
必要な依存関係のインストール:
```bash
sudo apt-get update
sudo apt-get install build-essential libtool autotools-dev automake pkg-config bsdmainutils python3
```

### 2. ライブラリのインストール
```bash
sudo apt-get install -y libevent-dev libboost-dev  # イベントライブラリ & ブーストライブラリ
sudo apt-get install libzmq3-dev  # ZMQライブラリ
sudo apt install systemtap-sdt-dev  # USDTライブラリ
sudo apt-get install -y qtbase5-dev qttools5-dev qttools5-dev-tools qtwayland5  # GUIサポート
sudo apt-get install -y libqrencode-dev  # QRコード生成ライブラリ（オプション）
```

### 3. GitHubからBitcoin Knotsのソースコードをクローン
```bash
git clone https://github.com/bitcoinknots/bitcoin.git
cd bitcoin
```

### 4. ビルドの構成と実行
```bash
./autogen.sh #ビルドシステムの初期化
./configure --disable-wallet  # Sparrow Walletを使用するためビルトインウォレット無効化
make
```

#### エラー対処
`make: *** No targets specified and no makefile found. Stop.` の場合:
```bash
sudo apt-get install librsvg2-bin  # rsvg-convertのインストール
make  # 再実行
```

ビルド成功の確認:
```
Nothing to be done for 'all'
Nothing to be done for 'all-am'
```

### 5. ブロックチェーンの同期開始
```bash
./src/bitcoind -datadir=/media/bentham/SSD2/BTC/full_node_data
tail -f /media/bentham/SSD2/BTC/full_node_data/debug.log  # 進捗ログ確認
```
約12時間前後で完了。

### 6. 起動方法の変更
`nano ~/.bitcoin/bitcoin.conf` に以下を追加:
```
datadir=/media/bentham/SSD2/BTC/full_node_data
```
起動コマンド:
```bash
bitcoin-qt
bitcoind
```

- ![alt text](../assets/images/2025-01-26_09-30.png)
- ![alt text](../assets/images/2025-01-26_09-31.png)

---

## セキュアな設定

### 1. RPCのセキュリティ強化
設定ファイルを編集:
```bash
nano ~/.bitcoin/bitcoin.conf
```
以下を追加:
```bash
rpcuser=yourusername
rpcpassword=yourstrongpassword
rpcbind=127.0.0.1 #ローカルネットワークだけにアクセスを許可
```

### 2. ノードのセキュリティ強化
最大接続数の制限:
```bash
nano ~/.bitcoin/bitcoin.conf
maxconnections=50
```

### 3. 匿名性の強化
Torを使用した通信の匿名化:
```bash
proxy=127.0.0.1:9050
bind=127.0.0.1
listen=1
discover=1
dnsseed=0
onlynet=onion
```
Torの起動:
```bash
tor
bitcoin-qt
```

- ![alt text](../assets/images/2025-01-26_12-21.png)

#### Bridgeの設定 (ハマった)
1. [Tor公式サイト](https://bridges.torproject.org/) でobfs4ブリッジを取得。
2. [ブリッジのチェック](https://metrics.torproject.org/rs.html)
3. `nano /etc/tor/torrc` を編集し、取得したブリッジを追加。
- ![alt text](../assets/images/2025-01-26_20-15.png)
4. obfs4proxyをインストール:
```bash
sudo apt install obfs4proxy
```

Torの起動方法:
```bash
sudo systemctl start tor@default.service
```
- ![alt text](../assets/images/2025-01-26_20-25.png)


注意
- 通常の起動では使えるブリッジを設定しても接続できなかった: `tor`も`sudo systemctl start tor`不可。
- tor-browserでbuilt-in bridgeもカスタムブリッジも接続不可。
- windowsのPCで試すと、tor-browserでのブリッジ成功した。← 謎
`sudo systemctl status tor@default.service`: この起動方法でのみ成功した。

### 4. Bitcoin Knots起動
```bash
sudo systemctl status tor@default.service  # Tor起動確認
bitcoin-qt
```

---

## 理解促進

### RPC (Remote Procedure Call) とは？
- ノードに対してブロックやトランザクション情報を取得したり、送金を行ったりする仕組み。
- 外部のコンピュータからも操作可能。
- 例:
  ```bash
  bitcoin-cli -rpcuser=yourusername -rpcpassword=yourstrongpassword getblockcount
  ```

---

## 参考
- [Bitcoin Knots GitHub Build Guide](https://github.com/bitcoinknots/bitcoin/blob/27.x-knots/doc/build-unix.md)

