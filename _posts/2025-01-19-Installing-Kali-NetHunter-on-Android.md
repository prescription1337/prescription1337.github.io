---
title: "Installing Kali NetHunter on Android"
date: "2025-01-19 00:00:00 +0900"
categories: [Technology, Hacking]
tags: [Android, Kali, Smartphone]
---

# Android端末へのKali NetHunterインストール

背景：
- 使ってないAndroidスマホ(OPPO Reno5 A)を有効活用したかった。
- https://www.devicespecifications.com/en/model/d573566f

ボトルネック:
- OPPO Reno5（CPH2199）はブートローダー解除はサポートされていない

結果：
- 失敗

所感：
- 時間を使ったが出来なかった。
- デバイスによって簡単にルート化出来るもの、不可能なものなど様々存在することが判明。
- ルート化には幾つかの道があることが分かった。
- まだやり切れてない方法があるが、気力が持たなかった。

## Kali NetHunterインストールまでの流れ

1.ブートローダーのアンロック：デバイスのモデルによって手順が異なる  
2.TWRP リカバリのインストール  
3.Magisk のフラッシュ: 成功するとルート化。  
4.Busybox のインストール：Play ストアからインストールするか、Magisk を介して Busybox ZIP ファイルをフラッシュします。  
5.Kali NetHunter のダウンロード：デバイスのアーキテクチャ (32 ビットまたは 64 ビット) に対応する ZIP ファイルをダウンロードします。  
6.NetHunter ZIP ファイルのフラッシュ：Magisk アプリのモジュールセクションから ZIP ファイルをフラッシュします。  
7.再起動  

## ルート化への道筋

- ルート化とは？
  - Androidデバイスのシステム領域へのフルアクセス権（root権限）を取得すること。
  - Androidデバイスはセキュリティ上の理由から、ユーザーがシステムファイルや設定を変更できないよう制限されている。
<br><br>
- なぜルート化を試みたのか？
  - Kali NetHunterをデバイスに導入したかったから。
  - 非ルート化でもKali NetHunter Liteを導入できるが、機能制限がかかる。
    - [Kali NetHunter \| Kali Linux Documentation](https://www.kali.org/docs/nethunter/#20-nethunter-supported-devices-and-roms)
<br><br>
- ルート化へ方法
  1. Fastbootを使用する方法（最も一般的で安全）
    - 「開発者向けオプション」→「OEMロック解除」を有効化し、USBでPCと接続。
    - ADB経由でFastbootモードへ移行：`adb reboot bootloader`
    - ブートローダーのアンロック（OEMアンロック）：`fastboot oem unlock`
    - Boot.img抽出 → Magiskでパッチ適用 → Fastbootで書き込み: `fastboot flash boot magisk_patched.img`
    - 再起動してルート化完了: `fastboot reboot`
  2. EDL（Emergency Download）モードを使用する方法
  - デバイスをEDLモードにする
    - ハードウェアキー（電源＋音量上下）: デバイスによって異なる
    - ADBコマンド（`adb reboot edl`）
    - Test Point方式（マザーボードの接点短絡）
  - Qualcomm EDLツール（bkerler/edlなど）を使用し、boot.img を取得：`python edl.py r boot_a boot.img`
  - Magiskでパッチ適用後、再度EDL経由でフラッシュ: `python edl.py w boot_a magisk_patched.img`
  3. 出回っているツールを利用：
  - [UNLOCKTOOL](https://unlocktool.net/models/?q=Oppo+reno+5): Reset FRP, Factory Resetのみ可能

## 1.Android端末のfastbootモード起動方法

- Fastbootモード: 
  - 通常のAndroidのリカバリモードよりも、さらに低レベルで強力な操作を行うことができる
  - Fastbootは、PCとの接続を前提としたモードであり、通常のリカバリモードよりも細かい操作が可能
<br><br>

- AndroidでFastbootモードに入る方法：
  1. ADBコマンド：完全に正常に動作しているデバイスにのみ利用可能
    - デバイスでUSBデバッグを有効にてPCに接続
    - デバイスの認識を確認：`adb devices`
    - EDLモードに移行：`adb reboot bootloader`
  2. Fastbootコマンド
    - デバイスをFastbootモードで起動: デバイス任意のハードウェアキーがあれば。
    - USBケーブルでPCに接続する
    - fastbootモードに移行
  3. Magiskアプリ: デバイスがルート化されており、ブリックされていない状態でなら可能。
  4. ハードウェアキー操作：デバイス任意のハードウェアキーがあれば。

## 2.Android端末のEDLモード起動方法

- EDL (Emergency DownLoad) : 
  - Qualcommチップセットを搭載したAndroidデバイスに存在する特別なモードで、デバイスにストックファームウェアを強制的に書き込めるようにする。
  - デバイスがハードブリックされた場合、つまり、Fastboot、Bootloader、Recovery Modeなどのどのパーティションにもアクセスできず、ファイルを書き込むことができない場合に役立つ
<br><br>

- AndroidでEDLモードに入る方法：
  1. ADBコマンド：完全に正常に動作しているデバイスにのみ利用可能
    - デバイスでUSBデバッグを有効にてPCに接続
    - デバイスの認識を確認：`adb devices`
    - EDLモードに移行：`adb reboot edl`
  2. Fastbootコマンド
    - デバイスをFastbootモードで起動: デバイス任意のハードウェアキーがあれば。
    - USBケーブルでPCに接続する
    - fastbootモードに移行：`fastboot oem edl` or `fastboot reboot-edl` または `fastboot reboot edl`
  3. Magiskアプリ: デバイスがルート化されており、ブリックされていない状態でなら可能。
  4. ハードウェアキー操作：デバイス任意のハードウェアキーがあれば。


## 実際に試したこと
1. Fastbootを使用する方法：Fastbootモードに入れず失敗
  - 「開発者向けオプション」→「OEMロック解除」を有効化し、USBでPCと接続。
  - ハードウェアキー：そもそもこのデバイスにハードウェアキーでfastboot入れない
  - デバイスの認識を確認：`adb devices`
  - adbコマンドでFastbootモードへ移行：`adb reboot bootloader`
    -  `已进入Fastboot 长按电源6~10秒或重装电池退出`という表示が一瞬出て再起動してしまう。つまり一瞬でこのモードから出てしまう。
    - この現象はOPPOの他の機種でも確認されていた。
      - [Recovery/fastboot/download modes howto \| XDA Forums](https://xdaforums.com/t/recovery-fastboot-download-modes-howto.3994957/)
    - 解決策を探したが見つからなかった。
<br><br>
2. EDL（Emergency Download）モードを使用する方法：EDLモードに入れず失敗
- デバイスをEDLモードにする
  - ハードウェアキー（電源＋音量上）: 失敗
    - リカバリーモードには入れた：（電源＋音量下）
    - [How to Boot Oppo Reno 5A Recovery Mode and Fastboot Mode - Droid Recovery](https://droidrecovery.com/oppo-reno-5a-recovery-mode/)
  - ADBコマンド（`adb reboot edl`）: 失敗
    - 再起動されただけ。
    - 成功するとUSBのPIDが`0x9008`なので「Qualcomm 9008 Mode」となるはず。
    - bkerler/edlを試したがデバイスを認識せず。
      - [GitHub - bkerler/edl: Inofficial Qualcomm Firehose / Sahara / Streaming / Diag Tools :)](https://github.com/bkerler/edl)
  - Test Point方式（マザーボードの接点短絡）
    - ハードウェアをいじる気概がないので断念。

## 他の方法
- EDLケーブルの利用: [QualcommのEDLモードの話 - ラック・セキュリティごった煮ブログ](https://devblog.lac.co.jp/entry/20221206)
- Fastbootモードを開放するAPKの導入：[realme GT Neo5 SEのブートローダーアンロック、Root化、Magisk導入方法](https://mitanyan98.hatenablog.com/entry/2023/05/30/151602#%E4%B8%AD%E5%9B%BD%E7%89%88%E3%81%AEOppo%E7%B3%BBOS%E3%81%AF%E7%99%96%E3%81%A4%E3%82%88)
- [Deep TEST fot OPPO RENO ACE(unlock bootloader) \| XDA Forums](https://xdaforums.com/t/deep-test-fot-oppo-reno-ace-unlock-bootloader.4093589/)

## 参考
- [Kali NetHunter \| Kali Linux Documentation](https://www.kali.org/docs/nethunter/#20-nethunter-supported-devices-and-roms)
- [\[GUIDE\] How to install Kali NetHunter on any android Rooted device? \| XDA Forums](https://xdaforums.com/t/guide-how-to-install-kali-nethunter-on-any-android-rooted-device.4624197/)
- EDLの公式ツールのleak：[General - EDL Flash Tool Leak \| XDA Forums](https://xdaforums.com/t/edl-flash-tool-leak.4494211/)
- 様々なEDLモードの入り方：[How to Boot any Android Device to EDL Mode (Bricked/Unbricked)](https://droidwin.com/boot-android-edl-mode/#Method_1_Boot_Android_Device_to_EDL_Mode_via_ADB_Command)
- windows版のadb driver: [Oppo Reno 5A USB Driver, ADB Driver and Fastboot Driver \[DOWNLOAD\] - Android ADB Driver](https://androidadbdriver.com/oppo-reno-5a-usb-drivers/)
- [EDL - Get Physical dump from EDL](https://forensic.manuals.mobiledit.com/MM/physical-extraction-edl-method)
- [How to root android device with factory disabled fastboot \| XDA Forums](https://xdaforums.com/t/how-to-root-android-device-with-factory-disabled-fastboot.4341307/)
