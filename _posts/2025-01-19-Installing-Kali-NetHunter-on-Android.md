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

## まとめ

- ルート化とは
  - Androidデバイスのシステム領域へのフルアクセス権（root権限）を取得すること。
  - Androidデバイスはセキュリティ上の理由から、ユーザーがシステムファイルや設定を変更できないよう制限されている。
- なぜルート化を試みたのか
  - Kali NetHunterをデバイスに導入したかったから。
  - 非ルート化でもKali NetHunter Liteを導入できるが、機能制限がかかる。
    - [Kali NetHunter \| Kali Linux Documentation](https://www.kali.org/docs/nethunter/#20-nethunter-supported-devices-and-roms)
- ルート化への道
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



- 試したこと（ログ）：
  1. Fastbootを使用する方法：Fastbootモードに入れず失敗
    - 「開発者向けオプション」→「OEMロック解除」を有効化し、USBでPCと接続。
    - デバイスの認識を確認：`adb devices`
    - Fastbootモードへ移行：`adb reboot bootloader`
      -  `已进入Fastboot 长按电源6~10秒或重装电池退出`という表示が一瞬出て再起動してしまう。つまり一瞬でこのモードから出てしまう。
      - この現象はOPPOの他の機種でも確認されていた。
        - [Recovery/fastboot/download modes howto \| XDA Forums](https://xdaforums.com/t/recovery-fastboot-download-modes-howto.3994957/)
      - 解決策を探したが見つからなかった。
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
    

知見：
- Qualcommデバイス用のツールを使用してファームウェアを書き換えることは出来るが、古い公式ファームウェアにBLU出来るものが無い。
  - https://support.halabtech.com/index.php?a=downloads&b=folder&id=128600\
  - https://support.halabtech.com/index.php?a=downloads&b=folder&id=128600
- ADBとFastbootを使用して解除：Fastbootモードに入れないので不可。
- DeepTesting(DT)アプリでfastbootモードが使えるようになる可能性：非対応機種はDT入れないので不可
  - https://itest.5ch.net/egg/test/read.cgi/android/1517398294/l-
- 手順：
  - 公式ファームウェアのダウンロード:
    - https://azrom.net/rom-reno-5a-cph2199-a103op-a101op-all-file-fix-official-firmware/
    - https://support.halabtech.com/index.php?a=downloads&b=folder&id=128600
  - https://note.com/reindex/n/nbcaf08c67359
- ROOT化にbootloaderのアンロックは必ずしも必要ではない？：https://xdaforums.com/t/how-to-root-android-device-with-factory-disabled-fastboot.4341307/

## fastboot出来ないデバイスをルート化する手順まとめ

- 1. boot.imgを取得
  - EDLモードを使ってファームウェアのバックアップを取る
    - USBデバッグ経由で「adb reboot edl」を実行したが、edlモードに入れない。
    - 上手く入れると：USBのPIDが`0x9008`なので「Qualcomm 9008 Mode」となる。
  - ファームウェアのダウンロード（なるべく古いバージョンを試す）
    - Now: C.42
    - Old：CPH2199export_11_A.01_2021041419395298.zip
  - ファームウェアの解凍・boot.imgの抽出（OFP ExtractorやMTK Extractorを使用）
  
- ソースによると、デバイスがハードブリック状態、つまりFastbootモードにもアクセスできない場合に、ハードウェアキーの組み合わせを使ってEDLモードに起動する必要があるとされています。EDLモードに入るためのハードウェアキーの組み合わせは、OEMによって異なります。
**OPPOデバイスのEDLモードに入るためのキーの組み合わせは、デバイスのモデルによって異なる可能性があります。**OPPOの公式サポートサイトやフォーラムで、お使いのデバイスのモデルに合った情報を探してみてください。
デバイスが「EDL（9008）」モード

- OEMロック解除が設定にあったので、USBに指してadbコマンドでFastbootモードに入る
- `adb devices`：デバイスがリストに表示された。
- `adb reboot bootloader`: BLU起動
  - `已进入Fastboot 长按电源6~10秒或重装电池退出`という表示が一瞬出て再起動してしまう。


## CPH2199の準備

- 1.ブートローダーのアンロック：デバイスのモデルによって手順が異なる
  - CPH2199無理かも
  - https://xdaforums.com/t/cph2239-how-to-unlock-bootloader-and-root-the-oppo-a54.4396001/
  - MediaTekプロセッサではないため、SP Flash ToolやMTK Clientは直接利用できません。
  - Qualcommデバイス用のツールを使用してファームウェアを書き換えることで、ブートローダーのアンロックや制限解除を試みる方法がある。
  - QFILはQualcommチップを搭載したデバイス向けのファームウェア書き込みツールです。このツールを使用して、ファームウェアやブートイメージをデバイスにフラッシュすることができる
  - QFILを使用してブートローダーをアンロックするためには、公式ファームウェアを改変するか、既存の制限を解除したファームウェアをフラッシュすることが考えられる。
  - 公式のファームウェア: https://azrom.net/rom-reno-5a-cph2199-a103op-a101op-all-file-fix-official-firmware/
  - https://support.halabtech.com/index.php?a=downloads&b=folder&id=128600
- 2.TWRP リカバリのインストール: 
- 3.Magisk のフラッシュ
- 4.Busybox のインストール：Play ストアからインストールするか、Magisk を介して Busybox ZIP ファイルをフラッシュします。
- 5.Kali NetHunter のダウンロード：デバイスのアーキテクチャ (32 ビットまたは 64 ビット) に対応する ZIP ファイルをダウンロードします。
- 6.NetHunter ZIP ファイルのフラッシュ：Magisk アプリのモジュールセクションから ZIP ファイルをフラッシュします。
- 7.再起動

- 参考：
  - [Kali NetHunter \| Kali Linux Documentation](https://www.kali.org/docs/nethunter/#20-nethunter-supported-devices-and-roms)
  - [\[GUIDE\] How to install Kali NetHunter on any android Rooted device? \| XDA Forums](https://xdaforums.com/t/guide-how-to-install-kali-nethunter-on-any-android-rooted-device.4624197/)
  - EDLの公式ツールのleak：[General - EDL Flash Tool Leak \| XDA Forums](https://xdaforums.com/t/edl-flash-tool-leak.4494211/)
  - 様々なEDLモードの入り方：[How to Boot any Android Device to EDL Mode (Bricked/Unbricked)](https://droidwin.com/boot-android-edl-mode/#Method_1_Boot_Android_Device_to_EDL_Mode_via_ADB_Command)
  - windows版のadb driver: [Oppo Reno 5A USB Driver, ADB Driver and Fastboot Driver \[DOWNLOAD\] - Android ADB Driver](https://androidadbdriver.com/oppo-reno-5a-usb-drivers/)
  - [QualcommのEDLモードの話 - ラック・セキュリティごった煮ブログ](https://devblog.lac.co.jp/entry/20221206)
  - [EDL - Get Physical dump from EDL](https://forensic.manuals.mobiledit.com/MM/physical-extraction-edl-method)
  - [realme GT Neo5 SEのブートローダーアンロック、Root化、Magisk導入方法](https://mitanyan98.hatenablog.com/entry/2023/05/30/151602#%E4%B8%AD%E5%9B%BD%E7%89%88%E3%81%AEOppo%E7%B3%BBOS%E3%81%AF%E7%99%96%E3%81%A4%E3%82%88)
  - [Deep TEST fot OPPO RENO ACE(unlock bootloader) \| XDA Forums](https://xdaforums.com/t/deep-test-fot-oppo-reno-ace-unlock-bootloader.4093589/)
