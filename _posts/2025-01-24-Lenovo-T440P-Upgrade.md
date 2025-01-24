---
title: "Upgrade Lenovo t440p"
date: "2025-01-24 00:00:00 +0900"
categories: [Technology, Laptop]
tags: [Hak5, Lenovo, Lenovo T440P, Wi-Fi Whitelist]
---

# Lenovo t440pの改造

背景：
- linux用のlaptopを探していた。
- Laptopの分解や自作に興味があった。
- Lenovo T440PはUpgradeが容易であることを知る。
- せっかくの機会なので、全パーツのUpgradeをする。

ボトルネック:
- ハードウェアの分解に対して経験が無く少し抵抗感があった。
- WiFiのWhitelist化が少し難易度が高いが、ソースの動画の通りに実行すれば上手くいく。
- ディスプレイに関してガイドの型番を見つけるのが困難なため代替品を利用。

結果：
- 成功
- Kali linuxをTranscendのSSDに入れて利用中
- Moneroのフルノードなどは1TBSSDへ

所感：
- T440Pの構成上容易にアップグレードが出来た。
- ハードウェアの分解に対しての抵抗感が薄れた。
- 記憶媒体はメジャーメーカーを選んだのでコストがかかった。

Source
- [Thinkpad T440p Ultimate Buyer's Guide](https://web.archive.org/web/20230103232559/https://octoperf.com/blog/2018/11/07/thinkpad-t440p-buyers-guide/)
- [Lenovo Thinkpad T440p Upgrade Guide](https://seiba.gitlab.io/thinkpad-t440p-upgrade-guide/)
- [Building the ultimate ThinkPad! - YouTube](https://www.youtube.com/watch?v=vtkQiLkNgOI)
- [Removing Wi-Fi Whitelist on Haswell Thinkpads (T440p, T540, W540) & Unlocking Advanced BIOS Settings - YouTube](https://www.youtube.com/watch?v=ce7kqUEccUM)
- [Keyboard - Reddit](https://www.reddit.com/r/thinkpad/comments/vtcxl2/t450s_liteon_keyboard_models/)


## 購入パーツまとめ

| パーツ            | 型番                                | 値段  | 追記                             |
|------------------|-----------------------------------|------|---------------------------------|
| Laptop           | ThinkPad T440P                    | 8600 | i5-4300M / 8GB / 500GB          |
| CPU              | Core i7-4712MQ                     | 3980 |                                 |
| Storage          | Crucial MX500 1TB                  | 5985 | 使用時間 900h                   |
|                  | Crucial MX500 1TB                  | 6980 | 使用時間 110h                   |
|                  | Transcend TS512GMTS430S 512GB       | 9554 | 新品                             |
| RAM              | Crucial DDR3L-1600 SODIMM 8GB (x2)  | 3400 | 8GB x 2枚                        |
| Display          |                                   | 6462 | 新品                             |
| TouchPad         | P/N: B149220I1 RS1                 | 2206 | 新品                             |
| Keyboard         | FRU: 01AX310                       | 3975 | 新品                             |
| Battery          | KingSener 8960mAh                  | 6207 | 新品                             |
| WiFi Adapter     | Intel AX210                        | 2243 | 新品                             |
| Programmer       | CH341A set                         | 1043 | 新品                             |
| Ultrabay Adapter | SATA 22pin aHDD HD 9.5mm           | 882  | 新品                             |
| 冷却用部品セット | 銅板、グリス、シート、工具類           | 3057 | 新品                             |
| **合計**         |                                   | **64574** |                                 |

- 値段は送料や税込々。
- DisplayはFHM-N41を購入したが、FHM-N31が届き互換性が無かったので返品。
- 他の互換性のあるDisplayを新たに購入。型番に注意すること。