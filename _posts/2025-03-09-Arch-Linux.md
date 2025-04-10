---
title: "Install Arch Linux on m.2 ssd"
date: "2025-03-07 00:00:00 +0900"
categories: [Hardware, Laptop]
tags: [Thinkpad X1 carbon, ssd]
---

# Thinkpad X1 Carbon 6gen 

## 概要

背景：
- メインのPCのm.2ssdを交換した際に出た500GBのssdを有効活用したかった
- Arch Linux を試したかった。

結果：
- 成功

所感：
- 動画を参考に簡単にできた

## 1. 導入先のssdのクリーニング

- windowsを利用
  - `diskpart`
  - `list disk`
  - `select disk 1`
  - 個別にパーティションを消す場合: `list partition`
    - `select partition 1`
    - `delete partition override`
  - disk全体を消す場合：`clean`

## 2. USBにArch Linuxのisoを焼く

- Rufusを利用
- 設定してスタート
  - ![alt text](../assets/images/Screenshot_2025-03-09_175814.png)

## 3 設定とインストール

- mainのBIOSに入ってbootの順番を変える。(USB)
- wifiに接続
  - `iwctl`
  - `device list`
  - `station wlan9 scan`
  - `station wlan0 get-networks`
  - `station wlan0 connect "SSID"`
  - PWを入れる
  - `exit`
  - 接続確認：`ping google.com`
- インストール
  - `archinstall`
  - 以下を参考に設定する
    - [How to Install Arch Linux: Step-by-Step Guide (archinstall)](https://www.youtube.com/watch?v=LiG2wMkcrFE)
  - `Installation completed without any errors.`が出れば完了
  - `sudo shutdown now`
  - USBを取り外す
  - 起動して確認

## 4. 設定

- リポリスト更新とアップグレード
  - `sudo pacman -Sy`
  - `sudo pacman -Syu`
- システム情報の確認
  - `neofetch`
- 日本語入力: 成功
  - [Arch Linuxの初期設定](https://qiita.com/poyotanp/items/e59336dd6b42283fda2e#fn-1)
- エディタ
  - `sudo pacman -S code`
- スクリーンショット
  - `sudo pacman -S flameshot`：バグが起きる
  - バグ：[Weird behavior on Wayland with KDE](https://github.com/flameshot-org/flameshot/issues/3204)
  - flameshot直し方： [Flameshot doesn't capture full desktop w/ multi-monitor](https://github.com/flameshot-org/flameshot/issues/3073#issuecomment-1740187784)
    - `ArjixWasTaken`が言っているように`flatpak`から`flameshot`をインストールして、`Flatseal`で`Environment`を変更することで起動できるようになった
  - 手順：
    - 1.`flatseal`をインストール
      - `sudo pacman -S git base-devel`
      - `git clone https://aur.archlinux.org/yay.git`
      - `cd yay`
      - `makepkg -si`
      - `yay -S flatseal`
    - 2. flatpak経由でflatsealを実行
      - `flatpak install flathub com.github.tchx84.Flatseal`
      - `flatpak run com.github.tchx84.Flatseal`
    - 3. flatsealでFlameshotの設定
      - `Flameshot`の`Environment`に新しい環境変数を追加
        - `QT_QPA_PLATFORM: xcb`
    - 4. flameshotをインストールし起動
      - `flatpak install flathub org.flameshot.Flameshot`
      - `flatpak run org.flameshot.Flameshot gui `
    - 5. 起動方法を変更
      - `echo 'alias flameshot="flatpak run org.flameshot.Flameshot"' | tee -a ~/.bashrc`
      - `source ~/.bashrc`
      - これでコマンド`flameshot gui`で起動可能
    - 6. Ctrl+S を Flameshot の保存専用にする
      - Ctrl + S が XON/XOFF フロー制御（ソフトウェアフロー制御）の「出力停止（Suspend Output）」として機能してしまっているので、変更
      - `echo "stty -ixon" | tee -a ~/.bashrc`
      - `source ~/.bashrc`
- 画像ビューア
  - `sudo pacman -S sxiv`
- Google Chrome
  - `yay -S google-chrome`
- Obsidian
  - `yay -S obsidian`
- Wi-fiのGUI設定
  - `sudo systemctl status NetworkManager`
  - `sudo systemctl start NetworkManager`
  - `sudo systemctl enable NetworkManager`
