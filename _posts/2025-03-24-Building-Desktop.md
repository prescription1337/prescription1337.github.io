---
title: "Building Desktop PC"
date: "2025-03-24 00:00:00 +0900"
categories: [Hardware, Desktop]
tags: [Desktop]
---

# Building Desktop PC

## 概要

背景：
- 軽量Linuxサーバー, NAS, マイニング等様々な用途に対応できる環境を作りたかった
- 出来るだけ低コストで試験的に実装してみたかった。
- マザーボードやGPUなどの理解も同時に深めたかった

結果：
- 

所感：
- 

## 1. 構成

- ケース、ストレージ(6TB HDD, 500GB HDD)、WiFiカードは転用で余っていたパーツを利用 
- 実質：49,763円 


| パーツ                           | 型番                                                        | 値段   |
|----------------------------------|-----------------------------------------------------------|--------|
| **タワーケース**                 | IW-PE663                                                   | 0      |
| **マザーボード + CPU + メモリセット** | QIYIDA X99 Motherboard + Xeon E5 2680 V4 + 32GB DDR4 REG ECC | 11,068 |
| **電源ユニット（PSU）**           | Thermalright TR-TGFX850 750W                               | 11,071 |
| **CPU冷却**                       | Lightless-3fan (LGA2011-X79-X99-E5対応)                   | 1,292  |
| **GPU**                           | SOYO AMD Radeon RX5700XT 8GB GDDR6 Memory                  | 20,147 |
| **6TB HDD**                       | ST6000DM003                                               | 14,800 |
| **500GB HDD**                     | ST500LM021                                                | 1,000  |
| **無線LANカード**                 | Intel 8265NGW                                            | 500    |
| **HUADISK NVMe M.2 SSD 1TB**     | HUADISK NVMe M.2 SSD 1TB (read:2100MB/s)                   | 5,185  |
| **合計**                          |                                                           | 66,063 |



## マザーボード

- マザボの選び方：
- 1. 使いたいCPUを決める
- 2. CPUに対応したチップセットを決める
- 3. マザーボードを決める（ソケットタイプに注意）

- CPU Socket Type：
  - 型番：`LGA2011-v3 (Socket R3)`
  - CPUとマザーボードを接続するための物理的な規格で、マザーボードには、対応するCPUソケットがあり、対応するCPUしか取り付けられない。CPUクーラーを選ぶ際も注意する。
  - [LGA2011詳細](https://ja.wikipedia.org/wiki/LGA2011)

- Chipset：
  - 型番：`Intel X99 Express`
  - 役割：CPUやメモリ、PCIe、USB、SATAなどの接続を制御する役割
  - 対応するCPUやメモリの種類があり、事前に調べる必要がある
    - [Intel X99 Express 対応CPU一覧](https://www.intel.com/content/www/us/en/products/sku/81761/intel-x99-chipset/compatible.html) + Intel Xeon E5 v3 / v4 シリーズ（Haswell-EP / Broadwell-EP）: Xeon E5-1600 v3/v4 + Xeon E5-2600 v3/v4

- Size：
  - `QIYIDA X99 Motherboard`は`Micro-ATX`
  - サイズ：`ATX` / `Micro-ATX` / `Mini-ITX`
    - ケース選びにも関わってくる部分
    - `ATX`: 標準的なサイズで、拡張性に優れており、組みやすい。キャプチャーボードを入れたいならこれ。
    - `Micro-ATX`: 少し小さめで、手ごろなモデルが多い。
    - `Mini-ITX`: 小さい。拡張性は無く、値段は高め。

- RAM(Random Access Memory)：
  - 購入メモリ：`DDR4-3200 REG ECCメモリ（サーバーやワークステーション向け）16GB×2`
　- 役割：CPUが計算や処理を行うために必要なデータを一時的に保存

- VRMフェーズ
  - 8つ確認
  - 役割：CPUに供給される電圧を調整するパーツで数が多いほど電圧を一定に保てる。

- メモ
  - `Chipset`が様々なCPUに対応していても`Socket Type`によって取り付けられるCPUが決まる
  - `Chipset`が様々なMemory(DDR4, DDR5など)に対応していても、マザーボードによってどちらかしか対応してない場合がある。

## 3. M.2 SSDにUbuntu Server 22.04 LTSをインストール

- M.2 SSDの準備
  - Windowsキー + X を押して「ディスクの管理」を開く
  - GPTを選ぶと、初期化されて未割当の状態になる。
    - ![alt text](../assets/images/Screenshot_2025-03-28_180802.png)
- USBの準備
  - OSのisoをダウンロード：[Ubuntu Server 22.04 LTS](https://ubuntu.com/download/server)
  - RufusでisoをUSBに焼く
    - ![alt text](../assets/images/Screenshot_2025-03-28_182302.png)
- 設定とインストール
  - サブ機のBIOSに入ってbootの順番を変える。(USB)
  - 再起動して、USBからブートする
  - Try or Install Ubuntu Serverを選ぶ
  - 動画を参照しながらインスト―ルを行う

## 4. 構築

- system information確認
  - `landscape-sysinfo`



## 参考

- 理解：
- [マザーボードの選び方＆基礎知識をわかりやすく解説](https://www.youtube.com/watch?v=2Q6KD1FEJDg)
- [How do computers work?](https://www.youtube.com/watch?v=4knBXkN1GEU)
- [Install Ubuntu Server 22.04 LTS](https://www.youtube.com/watch?v=zs2zdVPwZ7E)




