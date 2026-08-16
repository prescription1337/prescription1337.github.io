---
title: "arch linux customize"
date: "2026-08-08 00:00:00 +0900"
categories: [Hardware, Laptop]
tags: [Thinkpad X1 carbon, ssd]
---

# arch linux customize

## 概要

背景：
- 特定の目的のラップトップとして徐々にアップデートしていきたい。
- 自分のラップトップの構成などをいちから把握したい。


## クラック用にサーバーのGPUをArchから利用できる環境設定

### Ubuntu

#### クラック用のユーザーをubuntuに作成

- 1. ユーザーの作成
    - sudo useradd -m -s /bin/bash crack-lab
- 2. パスワードの設定
    - sudo passwd crack-lab
- 3. crack-lab に GPU アクセス権限を付与
    - sudo usermod -aG render,video crack-lab
- 4. メインユーザーにも念のため権限を付与
    - sudo usermod -aG render,video $USER
- 5. 再起動
    - sudo reboot

### Arch

#### SSHキーの設定：PGU利用時にPW入力の手間を省く

- 1. 手元（arch）の SSH 鍵を crack-lab に転送・登録
    - ssh-copy-id crack-lab@$ubuntu_ip
- 2. 手元（arch）の SSH 設定ファイル（~/.ssh/config）を編集
    - sudo nano ~/.ssh/config
- 3. サーバー再起動
    - sudo reboot
- 4. Archからのログイン確認
    - ssh crack-lab

##### GPUを利用したクラック

- 1. Arch 側からハッシュファイルを転送
    - scp~/wifit3/captures/XXXXX.hc20000 crack-lab:~/
- 2. クラック
    - hashcat -m 22000 -a 0 -w 3 ~/XXXXX.hc20000 ~/wordlists/WPA-Dictionary/*.txt

## Wifite3
- 1. インストール
    - mkdir -p ~/wifit3 && cd ~/wifit3
    - curl -LO https://github.com/derv82/wifit3/releases/latest/download/wifit3-linux-x64
    - chmod +x wifit3-linux-x64
- 2. wifiアダプターの設定
    - デバイスの表示名把握：ip link
    - モードの確認：sudo iw dev wlanXX info
    - モニターモードに変更：
        - sudo ip link set wlanXX down
        - sudo iw dev wlanXX set type monitor
    - モードの確認：sudo iw dev wlanXX info
    - 起動：sudo ip link set wlanXX up
- 3. 実行
    - sudo ./wifit3-linux-x64