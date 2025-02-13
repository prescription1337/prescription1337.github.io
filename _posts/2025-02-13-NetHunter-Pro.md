---
title: "Install NetHunter Pro on a Poco F1"
date: "2025-02-13 00:00:00 +0900"
categories: [Technology, Android]
tags: [Android, NetHunter Pro, Wardriving]
---

# Poco F1にNetHunter Proを導入

背景
- Wardrivingをやってみたかった

所感
- 簡単にGrapheneOS導入できた。

結果
- 成功

課題
- 匿名で利用出来るように設定を細かくしていく

## 導入と設定

- イメージファイルをダウンロード: `https://www.kali.org/get-kali/#kali-mobile`
- 

### 1. 導入
- デバイスを購入: Poco F1 シムフリー 中古を購入
- ブートローダーをアンロック
- Mi Unlock Tool をダウンロード: [Mi Unlock Tool](https://en.miui.com/unlock/download_en.html)
- Guide通りに進めてロックを解除: [OFFICIAL GUIDE](https://new.c.mi.com/global/post/101245?utm_source=miui&utm_medium=official_web_faq&utm_campaign=official_web_miui)

- デバイス設定：
  - 開発者オプションを有効化：設定 > デバイス情報 > ビルド番号 をタップして「開発者オプション」を有効
  - OEMアンロック有効化：設定 > システム > 開発者オプション > OEMアンロックを有効
  - USBデバッグ有効化：設定 > システム > 開発者オプション > USBデバッグを有効
  - デバイスを最新のファームウェアにアップデート: 設定 > システム > システムアップデートから最新バージョンにアップデート
  - ウェブインストール方法を利用。Google Chromeを更新：[ヘルプ] > [Google Chrome について] > [再起動]
  - ブートローダーをアンロック:
  - USBとスマホを接続：デフォルトのUSB設定 > ファイル転送 （これでPCがデバイスを認識）
  - 後は公式にしたがって進めていく:
  - [Web installer](https://grapheneos.org/install/web#booting-into-the-bootloader-interface)
  - [How to install GrapheneOS on Google Pixel 7/7a (Pixel 7 Pro)](https://www.youtube.com/watch?v=ZAZlmYKrwfk)
- OSがインストールされてもWifiには繋がずに、Airplain modeをONにする

### 2. 設定

### 参照

- [Kali NetHunter Pro](https://www.kali.org/docs/nethunter-pro/)
- [Install kali linux on android natively | Kali Nethunter pro installation 2024](https://www.youtube.com/watch?v=OiU_VK8GXY4)
