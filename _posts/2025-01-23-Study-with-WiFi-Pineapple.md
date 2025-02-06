---
title: "Study with WiFi Pineapple"
date: "2025-01-23 00:00:00 +0900"
categories: [Technology, Hacking]
tags: [Hak5, WiFi-Pineapple, Study With, DNS Spoofing]
---

# Make Most of WiFi Pineapple

- ツールを活用しながら学んでいく。

## DNS Spoofing

概要：
- **DNS Spoofing（DNSスプーフィング）**とは、DNS（Domain Name System）を悪用して、ユーザーを偽のウェブサイトやネットワークリソースに誘導する攻撃手法のこと。
- この攻撃により、ユーザーは正規のドメインにアクセスしていると思い込んだまま、悪意のあるサイトへ誘導され、個人情報の盗難やマルウェア感染のリスクに晒される。
  
DNS Spoofingの主な手法:
- キャッシュポイズニング（Cache Poisoning）
  - DNSサーバーのキャッシュに偽のDNSレコードを注入し、ユーザーが正規のサイトへアクセスしようとすると、攻撃者が用意したIPアドレスへ誘導される。
  - 例: example.com の正規IP 192.168.1.1 を 203.0.113.5（攻撃者のIP）に偽装。
- 中間者攻撃（Man-in-the-Middle Attack）
  - ネットワーク上でDNSクエリを傍受し、偽のDNSレスポンスを返して正規のDNSサーバーを偽装する。
  - Wi-Fiの公共ネットワークなどが攻撃対象になりやすい。
- ローカルホストファイルの改ざん
  - PCやスマートフォンの hosts ファイルを編集し、特定のドメインを攻撃者のIPに向ける。
  - 例: C:\Windows\System32\drivers\etc\hosts や /etc/hosts を改ざん。
- ルータのDNS設定の改ざん
  - ルーターの管理画面に侵入し、DNS設定を攻撃者のDNSサーバーに書き換え、すべての接続機器に影響を与える。
  - 初期パスワードのまま放置されているルーターが狙われやすい。
  
DNS Spoofingの影響
- フィッシング詐欺（偽のウェブサイトに誘導され、パスワードなどの情報を盗まれる）
- マルウェア感染（偽サイト経由でウイルスを仕込まれる）
- セッションハイジャック（オンラインバンキングやSNSのセッションが乗っ取られる）
  
参照：
- [DNSについて - YouTube](https://www.youtube.com/watch?v=M10upNHjLh8)
- [UnBoxing the WiFi Pineapple Mark VII (Setup + First Payload) - YouTube](https://www.youtube.com/watch?v=6O7cFZ1zqAI)
- [WiFi Pineapple - Crack WPA2 WiFi Passwords - YouTube](https://www.youtube.com/watch?v=lcaATUox4Gw)

## DNS Spoofing with WiFi Pineapple
- 

## WiFi Pineapple モジュール一覧（系統別）

### **① 管理・制御系モジュール**  

| モジュール名       | バージョン | 説明 | 作者 | サイズ |  
|-----------------|------------|--------------------------------|-------------|------------|  
| Cabinet        | 1.1        | Web インターフェース用のファイルマネージャー | newbi3      | 4.01K      |  
| Commander      | 2.1        | IRC 経由で Pineapple を制御                 | foxtrot     | 3.91K      |  
| Key Manager    | 1.1        | SSH キーマネージャー                       | whistlemaster | 8.74K  |  
| Log Manager    | 1.4        | 各モジュールのログ管理                      | whistlemaster | 2.79K  |  
| Modem Manager  | 1.1        | 3G/4G モデムの管理                         | foxtrot     | 5.39K      |  
| Module Maker   | 1.0        | モジュールの作成支援                       | foxtrot     | 4.17K      |  
| Status        | 1.5        | デバイスのステータス情報を表示             | whistlemaster | 43.79K  |  
| Terminal       | 1.5        | ttyd ターミナルを統合                     | DSR!        | 269.03K    |  
| Themes         | 1.3        | カスタムテーマの作成・共有                 | trashbo4t   | 56.09K     |  

---

### **② ネットワーク攻撃系モジュール**  

| モジュール名       | バージョン | 説明 | 作者 | サイズ |  
|-----------------|------------|----------------------------------|-------------|------------|  
| ConnectedClients | 1.4        | 接続クライアントの表示、DHCP管理、ブラックリスト化 | r3dfish     | 2.14K      |  
| Deauth          | 1.7        | 近隣のAPに接続中の全デバイスへのDeauth攻撃 | whistlemaster | 6.90K  |  
| Evil Portal    | 3.2        | キャプティブポータルを悪用する   | newbi3      | 23.33K     |  
| Occupineapple  | 1.7        | 偽のSSIDをブロードキャスト     | whistlemaster | 11.04K  |  
| PMKIDAttack    | 3.2        | PMKID 攻撃の自動化              | DSR!        | 8.24K      |  
| Responder      | 1.2        | LLMNR, NBT-NS, MDNS を悪用して認証情報を盗む | whistlemaster | 139.08K |  
| SSLsplit       | 1.5        | MITM 攻撃で SSL 通信を傍受      | whistlemaster | 6.67K   |  
| Site Survey    | 1.6        | WiFiサイト調査                 | whistlemaster | 10.01K  |  
| WPS           | 1.9        | WPS 攻撃（Reaver, Bully, Pixiewps対応） | Whistle Master | 12.17K  |  

---

### **③ パケット解析・スニッフィング系モジュール**  

| モジュール名       | バージョン | 説明 | 作者 | サイズ |  
|-----------------|------------|----------------------------------|-------------|------------|  
| DNSMasq Spoof  | 1.2        | DNSMasq を使って偽のDNS応答を生成 | whistlemaster | 4.51K   |  
| DNSspoof       | 1.7        | DNSspoof を使って偽のDNS応答を生成 | whistlemaster | 6.19K   |  
| DWall         | 1.6        | HTTPのURL, Cookie, POSTデータを取得 | Sebkinne & DSR! | 6.82K |  
| HTTP Proxy    | 1.0        | HTTP プロキシ                   | Malduhaymi  | 34.84K     |  
| MAC Info      | 1.3        | MACアドレス情報の取得            | DJEngineer  | 1.71K      |  
| Meterpreter   | 1.1        | Meterpreter 設定ユーティリティ   | audibleblink | 2.04K   |  
| ngrep         | 1.7        | ネットワークパケットのデータ部分をフィルタリング | whistlemaster | 7.09K   |  
| nmap          | 1.9        | セキュリティスキャナー nmap の GUI | whistlemaster | 6.53K   |  
| p0f           | 1.3        | 受動的なトラフィックフィンガープリント | whistlemaster | 5.68K   |  
| tcpdump       | 1.8        | ネットワークトラフィックのダンプ | whistlemaster | 6.21K   |  
| tor           | 1.0        | Tor ネットワーク接続、Hidden Services管理 | catatonicprime | 8.34K  |  
| urlsnarf      | 1.9        | HTTPトラフィックからリクエストURLを取得 | whistlemaster | 5.67K   |  

---

### **④ その他のツールモジュール**  

| モジュール名       | バージョン | 説明 | 作者 | サイズ |  
|-----------------|------------|--------------------------------|-------------|------------|  
| HackRF         | 1.4        | HackRF を WiFi Pineapple で使用 | foxtrot     | 4.66K      |  
| Internet Speed Test | 1.0   | Pineapple の速度テスト        | trashbo4t   | 3.30K      |  
| Locate        | 1.0        | IPアドレスの位置情報を取得     | trashbo4t   | 2.74K      |  
| Papers        | 2.0        | TLS/SSL & SSH 証明書の生成・管理 | sud0nick    | 26.52K     |  
| Portal Auth   | 2.0        | キャプティブポータルのクローン作成 | sud0nick    | 939.20K    |  
| RandomRoll    | 1.2        | Pineapple に接続したクライアントをトロール | foxtrot     | 20403.76K  |  
| SignalStrength | 1.0       | WiFi電波の強度を測定し、APを物理的に特定 | r3dfish     | 16.42K     |  
| ZeroTier      | 1.1        | ZeroTierネットワークに接続   | m5kro       | 3.76K      |  
| autossh       | 1.2        | 持続的なSSH接続             | audibleblink | 4.11K   |  
| base64encdec  | 1.0        | Base64エンコード・デコード   | dustbyter   | 2.12K      |  
| dump1090      | 1.2        | ADS-B信号の受信（航空機トラッキング） | whistlemaster | 6.19K  |  
| get           | 1.2        | ブラウザのプラグイン情報を取得 | dustbyter   | 9.61K      |  

---

### **まとめ**
WiFi Pineapple のモジュールは、次の4カテゴリに分けられる：  

✅ **管理・制御系**（Cabinet, Terminal, Status など）  
✅ **ネットワーク攻撃系**（Deauth, Evil Portal, PMKIDAttack など）  
✅ **パケット解析・スニッフィング系**（DNSspoof, DWall, ngrep, tcpdump など）  
✅ **その他ツール**（HackRF, ZeroTier, Internet Speed Test など）  


