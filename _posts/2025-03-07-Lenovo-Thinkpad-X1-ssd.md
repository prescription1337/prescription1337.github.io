---
title: "Thinkpad X1 Carbon 6gen Full Maintenance"
date: "2025-03-07 00:00:00 +0900"
categories: [Hardware, Laptop]
tags: [Thinkpad X1 carbon, ssd]
---

# Thinkpad X1 Carbon 6gen 

## 概要

背景：
- Touchpad, Keyboardの故障によりパーツ交換の必要性が出た
- この際、m.2ssdも500GB → 2TBにして、wifiもアップグレードしたかった。

交換パーツ：  


| パーツ                          | 自作 | 価格 (円) |
|--------------------------------|------|----------|
| Palmrest + Keyboard 02HL884   |      | 21652    |
| Touchpad                      |      | 3374     |
| 内蔵無線LANカード (Intel AX210) |      | 2149     |
| m.2 SSD (2TB)                 |      | 15000    |
| NVMe M.2 SSD用USBアダプター    |      | 1738     |
| **合計**                      |      | **43913** |


結果：

所感：

## 1. 新しいssdにデータをクローン

ツール：[Macrium Reflect Free Edition]
- New SSD
- 初期化
- フォーマット
- クローン

1. 初期化
- 「ディスクの管理」で新しいSSDが 「ディスク〇（不明・未初期化）」 となっているのでまず初期化
- 「ディスクの管理」を開く（Win + X →「ディスクの管理」）
- 未割り当てのディスク（例：「ディスク1」）を右クリック
- 「ディスクの初期化」を選択
- 「GPT（GUIDパーティションテーブル）」を選択（2TB以上ならGPT推奨）
- ![alt text](../assets/images/Screenshot_2025-03-07_153251.png)
2. フォーマット
- 「未割り当て」の場合はフォーマットが必要
- SSDの未割り当て領域を右クリック
- 「新しいシンプルボリューム」を選択
- **NTFS（またはexFAT）**でフォーマット
- ドライブレター（例：D:）を指定
- ![alt text](../assets/images/Screenshot_2025-03-07_153637.png)
- 完了後、エクスプローラーに表示されるか確認
- ![alt text](../assets/images/Screenshot_2025-03-07_153725.png)
3. クローン: Macrium Reflect
- [Macrium Reflect 使い方](https://4ddig.tenorshare.com/jp/partition-manager/macrium-reflect.html)
- ![alt text](../assets/images/Screenshot_2025-03-07_160524.png)

## 2. Thinkpad X1 Carbon 6gen を分解

- バッテリー取り外し
- SSD・WLAN・WWANの無線モジュールを外す
- ファンの取り外し+クリーニング