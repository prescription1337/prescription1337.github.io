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
- すこし時間がかかったが問題なく導入できた

結果
- 成功

課題
- 

## 導入と設定

### 1. ブートローダーをアンロック
- デバイスを購入: Poco F1 シムフリー 中古を購入
- ブートローダーをアンロック
  - Mi Unlock Tool をダウンロード: [Mi Unlock Tool](https://en.miui.com/unlock/download_en.html)
  - Guide通りに進めてロックを解除: [OFFICIAL GUIDE](https://new.c.mi.com/global/post/101245?utm_source=miui&utm_medium=official_web_faq&utm_campaign=official_web_miui)

### 2. TWRPでデータを削除

- [TWRP Img ダウンロード](https://dl.twrp.me/beryllium/)
- [SDK Platform-Tools ダウンロード](https://developer.android.com/tools/releases/platform-tools?hl=ja)
- 解凍してディレクトリを移動: `cd C:\projects\android\platform-tools-latest-windows\platform-tools`
- デバイスをFASTBOOTモードにしてPCと接続
- デバイスの認識を確認: `.\fastboot devices`
- `twrp.img`を同じフォルダに移動
- TWRPをフラッシュ: `.\fastboot flash recovery twrp.img`
- TWRPを直接起動`.\fastboot boot twrp.img`
- スマホの画面が変わりTWRPに入り、中のデータを消去：`「Wipe」→「Format Data」を実行`
- kaliの`img`を同じフォルダに移動
- TWRP から Fastboot モードに戻る: `Reboot` → `Bootloader` を選択
- kaliのbootイメージをフラッシュ: `.\fastboot flash boot nethunterpro-20241215-sdm845-phosh.boot-beryllium-ebbg.img`
- リブート: `.\fastboot reboot`

### 3. kaliでの設定

- PW: `1234`でログイン
- Terminalで`sudo apt update`
 - 401 Errorが出た場合: `sudo vi /etc/apt/sources.list`で下のように書き換える
```
deb http://http.kali.org/kali kali-rolling main non-free contrib
deb-src http://http.kali.org/kali kali-rolling main non-free contrib
```



### 参照

- [How To UNLOCK Bootloader Of Poco F1](https://www.youtube.com/watch?v=v5ytDelaa4w)
- [How to unlock bootloader and install twrp in poco f1](https://www.youtube.com/watch?v=jK-6AovD3w0&)
- [Kali NetHunter Pro](https://www.kali.org/docs/nethunter-pro/)
- [Install kali linux on android natively | Kali Nethunter pro installation 2024](https://www.youtube.com/watch?v=OiU_VK8GXY4)
