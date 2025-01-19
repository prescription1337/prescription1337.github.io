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
- ほぼ不可能。

所感：
- 色々情報を漁ったがそもそもCPH2199が日本向けの為、情報が少なかった。

知見：
- 幾つかのツールでCPH2199対応したものが有ったが、Reset FRP, Factory Resetのみ
  - https://unlocktool.net/models/?q=Oppo+reno+5
- Qualcommデバイス用のツールを使用してファームウェアを書き換えることは出来るが、古い公式ファームウェアにBLU出来るものが無い。
  - https://support.halabtech.com/index.php?a=downloads&b=folder&id=128600\
  - https://support.halabtech.com/index.php?a=downloads&b=folder&id=128600
- ADBとFastbootを使用して解除：Fastbootモードに入れないので不可。
- DeepTesting(DT)アプリでfastbootモードが使えるようになる可能性：非対応機種はDT入れないので不可
  - https://itest.5ch.net/egg/test/read.cgi/android/1517398294/l-
- カスタムROM（カスタムリカバリ）をインストールして解除: TWRP（Team Win Recovery Project）**のようなカスタムリカバリをインストールし、その後にカスタムROM(Magisk)をフラッシュする方法
  - Fastbootモードにしないといけないので不可。
- EDLモード（Emergency Download Mode）を使用して解除: これもしかしたらできるかも。
  - boot.imgを取得しMagisk Managerで公式ファームウェアから抽出したboot.imgに自動的にパッチを適用しroot権限を提供。修正済みboot.imgをPCに転送し、EDLを使用してフラッシュ。
  - https://github.com/topjohnwu/Magisk
- 手順：
  - 公式ファームウェアのダウンロード:
    - https://azrom.net/rom-reno-5a-cph2199-a103op-a101op-all-file-fix-official-firmware/
    - https://support.halabtech.com/index.php?a=downloads&b=folder&id=128600
  - boot.imgの抽出: ダウンロードしたファームウェア内のboot.imgを取り出す。
  - Magisk Managerアプリをインストール（Google Playにはないため、公式GitHubページからAPKをダウンロード）。
    - https://github.com/topjohnwu/Magisk
  - Magiskが自動的にパッチ適用を行い、magisk_patched.imgというファイルを生成
  - 修正済みboot.imgをデバイスにフラッシュ: 一般的にQualcomm専用ツール（QFILやbkerler/edlツール）を使用してパーティションをフラッシュ
    - デバイスをEDLモードに入れる必要があります。これには、QPSTやQFIL、bkerler/edlのツールを使用してデバイスを認識させます。
    - 通常、EDLモードに入れる方法には、特定のボタン操作（ボタンを押しながらデバイスを接続）や、特定のツール（例：EDLケーブル）を使用する方法があります。
    - QFILやedlツールは通常、full firmwareのフラッシュがメインですが、手動でboot.imgのみにフラッシュを絞ることも可能です。
- 注意：
  - boot.img以外のパーティションを誤って書き換えると、デバイスが起動しなくなる
  - 通常、EDLモードではパーティション単位でのフラッシュが行われますが、これにはツールやドライバーが必要
- 使えそうなツール
  - https://note.com/reindex/n/nbcaf08c67359
- ROOT化にbootloaderのアンロックは必ずしも必要ではない？：https://xdaforums.com/t/how-to-root-android-device-with-factory-disabled-fastboot.4341307/


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