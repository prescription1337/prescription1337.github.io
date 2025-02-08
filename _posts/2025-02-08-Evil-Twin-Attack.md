---
title: "Evil Twin Attack with WiFi Pineapple and Kali Linux"
date: "2025-02-08 00:00:00 +0900"
categories: [Hacking, WiFi]
tags: [Hak5, WiFi-Pineapple, WPA, WPA2, WiFi Hacking]
---

# Evil Twin Attack

所感: 
- WiFi Pineappleの仕様でアダプターが認識されなかったり、Access Point(AP)が表示されなかったりで少し時間がかかった。
- 公共のフリーWiFiに模したAPを作成して、認証情報を奪取するのは容易。

結果: 
- **成功**

課題: 
- ダウンロードしたカスタムポータルは英語なので、日本で利用するには自分でログインページを作成する必要がある。

## Evil Twin Attackとは

- **概要:** 正規のWiFiと同じSSIDの偽WiFiを作成し、ターゲットを接続させる。
- **目的・用途:** 認証情報の窃取、MITM攻撃。
- **使用モジュール / ツール:** Evil Portal, Occupineapple。

## 実行環境

- **ターゲット:** PoketWiFi (SSID: 1337_WiFi)
- **ターゲットに接続しているデバイス:** Android スマホ
- **Laptop (Windows):** WiFi Pineappleとの接続とWebUI操作
- **Laptop (Kali):** PW Crack用
- **WiFi Pineapple**

## ステップ
1. ターゲットWiFiの情報収集 → **Recon モジュール**
2. 偽のWiFi（Evil Twin）を作成 → **Networking モジュール**
3. ターゲットを Deauth 攻撃で切断 (公共WiFi利用の場合はスキップ) → **Deauth モジュール**
4. 偽のログインページを表示 & クライアントが入力した認証情報を取得 → **Evil Portal モジュール**

## 実践_01: 公共のオープンなFree WiFiを想定

### 1. ターゲットWiFiの情報収集

- WiFi Pineapple の Web インターフェース にアクセス（172.16.42.1:1471）。
- **Recon:** スキャンを実行 し、ターゲットの WiFi 情報を取得。
   - **SSID（ネットワーク名）:** 1337_WiFi
   - **BSSID（MACアドレス）:** C0:5B:12:1E:88:DF
   - **チャンネル:** 6
      ![alt text](../assets/images/Screenshot_2025-02-08_085010.png)  

### 2. 偽のWiFi（Evil Twin）を作成

- **現在のネットワーク設定:**
   - **wlan1:** Mode: Managed
   - **wlan0-1:** Mode: Master
   - **wlan2:** Mode Managed (ESSID: "接続先のWiFi SSID") → これは UI Networking → WiFi Client Mode で接続済。
- **SSID をターゲットと同じにする**（例：Free WiFi → Free WiFi）。
- **チャンネルを合わせる**（ターゲットAPと同じチャンネルに設定）。
   - **ターゲットがPW付きのWiFiなら** Management SSID を変更: Disable Open SSID チェック
   - **ターゲットがPW無しの公共WiFiなら** Open SSID を変更: Disable Management AP をチェック。
      ![alt text](../assets/images/Screenshot_2025-02-08_101658.png)  

### 3. ターゲットを Deauth 攻撃で切断: スキップ

- **Deauth モジュール を開く。**
- ターゲットWiFiを選択（BSSIDを指定）。
- ターゲットのクライアントを選択（MACアドレスを指定）。
- Deauth 攻撃を開始。

### 4. 偽のログインページを表示

- **Evil Portal モジュール を開く。**
- **Basic を試す**
   - **Work Bench → Basic → Create New Portal**
   - **Controls → Captive Portal → Start**
   - **Activate**
   - スマホで設定した偽のWiFiをタップ。
   - **「WiFiネットワークにログインしてください」の通知が出る → Basicのデフォルトログイン画面が表示される。**
   - **Authorize を押すとWiFiに接続。**
   - **Modules → ConnectedClients で確認。**
      ![alt text](../assets/images/Screenshot_2025-02-08_114414.png)
      ![alt text](../assets/images/Screenshot_2025-02-08_121223.png)


- **カスタムログインページ を試す**
   - ダウンロード: [kleo/evilportals - Github](https://github.com/kleo/evilportals)
   - Googleログインページをテストするので、フォルダを移動: `scp -r C:/projects/wifi-pineapple/evilportals-master/evilportals-master/portals/google-login root@172.16.42.1:/sd/portals/google-login/`
   - うまくいかない場合は [WinSCP](https://winscp.net/eng/download.php) を使用。
   - 成功:
      ![alt text](<../assets/images/Screenshot_2025-02-08 _143913.png>)
      ![alt text](../assets/images/Screenshot_2025-02-08_143940.png)
   - Modules >> Evil Portal >> Google Login Activated >> Start。
   - スマホとPCがWiFi接続時に認証情報を取得することに成功。
      ![alt text](../assets/images/Screenshot_2025-02-08_152643.png)
      ![alt text](../assets/images/Screenshot_2025-02-08_153029.png)

### **トラブルシューティング**

- **スマホやPCでポータルが出ずにログインされる場合:**
  - WiFi PineappleのDHCP Leasesを削除: `rm /tmp/dhcp.leases`
  - スマホ: 「設定」→「システム」→「リセット」→「Wi-Fi、モバイル、Bluetoothをリセット」
  - それでも上手く行かないときは、WiFi Pineapple 再起動。

参照: 
- [Evil Portal Module - Wi-Fi Pineapple Mark VII](https://www.youtube.com/watch?v=EEJtdj8i4Jg)
- [kleo/evilportals - Github](https://github.com/kleo/evilportals)

