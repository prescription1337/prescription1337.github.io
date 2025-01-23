---
title: "Clone WiFi Pineapple"
date: "2025-01-23 00:00:00 +0900"
categories: [Technology, hacking]
tags: [Hak5, WiFi Pineapple]
---

# WifiルーターをHak5のWiFi Pineappleへ

背景：
- WiFi Pineappleを試したかった。
- DIY決定。

ボトルネック:
- 今回使用する`MANGO GR-MT300N-V2`は日本でもグローカルネット社より出ているが、そちらでは要件を満たしていない可能性があったためGL.iNet社のデバイスを輸入。
- wifiアダプター`RT5370`では2.4Gしか対応できないため、`RT5572`を購入。（githubで互換性は確認済み）

結果：
- 成功

所感：
- かなり簡単にクローンの作成が出来た。
- 18000円 → 5300円 なのでコスパが良い。

課題：
- WiFi Pineappleで出来ることを実践の中で把握していく。

Source
- [Build a $23 Wi-Fi Pineapple in 6 Minutes — EASIEST Method! - YouTube](https://www.youtube.com/watch?v=udnxagkSzoA)
- [github wifi-pineapple-cloner](https://github.com/xchwarze/wifi-pineapple-cloner)

## 作成方法
- ステップ
  1. 使うデバイスの用意
  2. ファームウェアのダウンロード
      - `GL-MT300N v2 19.07.7`を選択してダウンロード
      - [wifi-pineapple-cloner-builds](https://gitlab.com/xchwarze/wifi-pineapple-cloner-builds)
  
  3. OpenWrtファームウェアのダウンロード
      - `GL-MT300N v2 19.07.7`を選択してダウンロード
        - [OpenWrt](https://firmware-selector.openwrt.org/)
          - ![alt text](../assets/images/Screenshot_2025-01-23_145122.png)
  
  4. ファームウェアをルータにフラッシュ
      - Recovery Modeにする
        - LANケーブルでPCとルータを接続→Resetボタンを長押し→その状態でUSBケーブルでPCとルータを接続。
        - ルータにある3つのランプで左と真ん中が点灯状態になるまで待つ。
      - OpenWrtをフラッシュ
        - Windows 11（日本語環境）で「ネットワーク接続（Network Connections）」を確認
          - [コントロールパネル] → [ネットワークとインターネット] → [ネットワークと共有センター] → 左側のメニューから [アダプターの設定の変更] をクリック
        - Wifiの接続を切る
        - デバイスと接続しているイーサネットを選択 → プロパティ → IPV4を選択 → `192.168.1.2`と入力 → サブネットマスクは自動で入力される
        - `192.168.1.1`に接続 → ダウンロードしたOpenWrtファームウェアをアップロード → 3分ほど待つ → ルータの真ん中以外が点灯の状態になれば完了
          - ![alt text](../assets/images/Screenshot_2025-01-23_151933.png)
          - ![alt text](../assets/images/Screenshot_2025-01-23_152025.png)
      - Pineapple イメージをフラッシュ
        - もう一度`http://192.168.1.1/`にアクセス → `http://192.168.1.1/cgi-bin/`に進むのでパスワードを設定
          - ![alt text](../assets/images/Screenshot_2025-01-23_153042.png)
        - System → Backup / Flase Firmware → 最下層まで行ってcloneのファームウェアをアップロード → keysettingのクリックを外してもう一度アップロードし3分待つ → ルータのランプ3つが点灯状態になれば完了（一番右は点滅状態で大丈夫）
          - ![alt text](../assets/images/Screenshot_2025-01-23_153530.png)
  
  5. WiFi Pineappleを設定
      - 4.で設定したIPV4の設定を戻す
        - IP アドレスを自動的に取得する と DNSサーバーのアドレスを自動的に取得する にチェックを入れる
      - `172.16.42.1:1471`に接続するとWiFi Pineappleのインターフェースに入れる
          - ![alt text](../assets/images/Screenshot_2025-01-23_154800.png)
      - `Get Started`をクリック。さらに進むとWifiの設定に行くので、デバイスにWifiアダプター`RT5370`を付けて、Resetボタンを一回押す。成功するとPopupが消えて設定画面へ。
      - Root Passwordなどを動画のように設定しCompleteをクリック → ルートでログイン
          - ![alt text](../assets/images/Screenshot_2025-01-23_155512.png)
      - Reconなどで試してみる。
          - ![alt text](../assets/images/Screenshot_2025-01-23_155618.png)
