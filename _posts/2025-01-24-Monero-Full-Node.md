---
title: "Run Monero Full Node"
date: "2025-01-24 00:00:00 +0900"
categories: [Technology, Crypto]
tags: [Monero, Crypto, Monero, XMR]
---

# LinuxにMonero Full Nodeを構築
背景
- Windowsメインホストの容量の枯渇。
- LinuxでCrypto系はすべて管理したかった。

## 導入と設定

1. ダウンロード  
   [Monero GUI Wallet](https://www.getmonero.org/downloads/#gui)

2. 解凍してフォルダに移動してインストール: `.\monerod`  
   Blockchainデータ(60GB~)の保存場所はデフォルトでルートディレクトリに保存される(`~/.bitmonero/lmdb/`)  
   24hほどかかった。  
   ![alt text](../assets/images/2025-01-24_12-58.png)  

3. Blockchainデータを変更  
   任意のSSDにデータを移動: `mv ~/.bitmonero /media/...`

4. Monero GUI起動してWalletを作成  
   シードの情報などを紙に書いて保存  
   Blockchain location(ブロックチェーンの保存場所)の設定画面で3で移動した場所を選択: `/media/.../.bitmonero`

5. 不要になったデータの削除: `rm -rf ~/.bitmonero`


参考：
- [Linux Monero GUI Wallet With Full Node Tutorial](https://www.youtube.com/watch?v=8hrWaDVfqOU)
- [How Monero Users Get Traced (RUN YOUR OWN NODE!)](https://www.youtube.com/watch?v=WkphgF6Hn4w)

