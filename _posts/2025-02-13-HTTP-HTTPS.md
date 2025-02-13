---
title: "HTTP/HTTPS"
date: "2025-02-13 00:00:00 +0900"
categories: [Knowledge, Protocol]
tags: [TLS Handshake, Network, HTTPS, HTTP, SSL/TLS, TCP]
---

# HTTP/HTTPS 通信

## HTTP/HTTPS 通信の流れ

### **1️. TCP 3-way ハンドシェイク**（通信の確立）  
1. クライアント → サーバー: `SYN` (接続要求)  
2. サーバー → クライアント: `SYN-ACK` (応答)  
3. クライアント → サーバー: `ACK` (確認)  

- 詳細: [TCP 3-Way Handshake](https://prescription1337.github.io/posts/TCP-3-Way-Handshake/)

---

### **2️. TLS ハンドシェイク（HTTPS のみ）**（暗号化通信の準備）  
1. **ClientHello**（クライアントの暗号方式・ランダム値を送る）  
2. **ServerHello**（サーバーの暗号方式・ランダム値・証明書を送る）  
3. **鍵交換**（RSA/ECDHE で Pre-Master Secret を共有）  
4. **セッションキー作成**（Client & Server Random + Pre-Master Secret）  
5. **暗号化通信開始**（`Finished` メッセージ）  

- 詳細: 
- [Message Authentication Code (MAC)](https://prescription1337.github.io/posts/Message-Authentication-Code-MAC/)
- [Digital Signiture](https://prescription1337.github.io/posts/Digital-Signature/)
- [SSL/TLS Handshake](https://prescription1337.github.io/posts/SSL-TLS-Handshake/)

---

### **3️. HTTP / HTTPS データ送受信**（実際の通信）  

1. **クライアント → サーバー**: `GET /index.html HTTP/1.1`  
   - HTTP: **そのまま送信**  
   - HTTPS: **暗号化して送信**  

2. **サーバー → クライアント**: `HTTP/1.1 200 OK <HTMLデータ>`  
   - HTTP: **そのまま送信**  
   - HTTPS: **暗号化して送信**  

---

### **4️. 通信終了（TCPコネクションの終了）**  
1. クライアント → サーバー: `FIN`（終了要求）  
2. サーバー → クライアント: `FIN-ACK`（応答）  
3. クライアント → サーバー: `ACK`（確認）  

## **HTTPS データ送受信**（実際の通信）  

## **TLS ハンドシェイク後のセッションキーの活用（HTTPS 通信）**  

- TLS ハンドシェイクが完了すると、クライアントとサーバーは**共通のセッションキー**を持つ。  
- このセッションキーが、**HTTPS 通信の暗号化・認証・整合性チェック**に使われる。

---

### **1️. HTTPS のデータ送受信に使われるセッションキー**

- TLS ハンドシェイク後、以下のセッションキーが確立され、HTTPS 通信で活用される。  

| **キーの種類** | **用途** | **タイミング** |
|--------------|---------|-------------|
| **Client Encryption Key** | クライアント → サーバーのデータ暗号化 | クライアントがリクエストを送るとき |
| **Server Encryption Key** | サーバー → クライアントのデータ暗号化 | サーバーがレスポンスを送るとき |
| **Client MAC Key** | クライアント → サーバーのデータ整合性チェック（HMAC） | クライアントがリクエストを送るとき |
| **Server MAC Key** | サーバー → クライアントのデータ整合性チェック（HMAC） | サーバーがレスポンスを送るとき |
| **Client IV / Server IV** | 暗号化の初期化ベクトル（IV） | AES-GCM などのブロック暗号で使用 |

---

### **2️. 具体的な HTTPS 通信の流れ**

#### **クライアント → サーバー (HTTPS リクエスト)**

1. **リクエストの整合性チェック用（HMAC）**
   - クライアントは、暗号化前の `HTTP リクエストデータ` を `Client MAC Key` を使って `HMAC` を計算
   - `MAC = HMAC-SHA256(Client MAC Key, HTTPリクエストデータ)`
   - `Client MAC Key` は`TLS handshake`の `6. Key Generation`で得た鍵
   
2. **リクエストの暗号化**
   - `HTTPリクエストデータ` + `HMAC` を `Client Encryption Key` で暗号化する。
   - `Ciphertext = AES-256-GCM-Encrypt(Client Encryption Key, HTTPリクエスト + MAC, Client IV)`
     - AES-GCM などの AEAD（Authenticated Encryption with Associated Data）を使う場合、MAC を追加しなくても、暗号化時に GCM タグが生成されるため、HMAC は不要になることもある。
     - しかし、AES-CBC のような AEAD でない暗号方式の場合は、HMAC が必要 となる。

3. **サーバーが復号 & 整合性チェック**
   - サーバーがリクエストを受信後
     - 復号: `Plaintext = AES-256-GCM-Decrypt(Client Encryption Key, Ciphertext, Client IV)`
       - ここで取得できるのは「HTTPリクエストデータ + MAC」
     - HMAC を再計算し、改ざんチェック: `Verify MAC = HMAC-SHA256(Client MAC Key, HTTPリクエストデータ)`
       - 受信した MAC 値とサーバー側で計算した MAC 値が一致すれば改ざんなし！
       - 一致しなければ、改ざんされた可能性があるためリクエストを拒否する。

---

#### **サーバー → クライアント (HTTPS レスポンス)**

1. **レスポンスの整合性チェック用（HMAC）**  
   - サーバーは、暗号化前の `HTTP レスポンスデータ` を `Server MAC Key` を使って `HMAC` を計算  
   - `MAC = HMAC-SHA256(Server MAC Key, HTTPレスポンスデータ)`  
   - `Server MAC Key` は `TLS handshake` の `6. Key Generation` で得た鍵  

2. **レスポンスの暗号化**  
   - `HTTPレスポンスデータ` + `HMAC` を `Server Encryption Key` で暗号化する。  
   - `Ciphertext = AES-256-GCM-Encrypt(Server Encryption Key, HTTPレスポンス + MAC, Server IV)`  
     - **AES-GCM などの AEAD を使う場合** → MAC は不要（暗号化時に GCM タグが生成されるため）  
     - **AES-CBC のような AEAD でない暗号方式の場合** → HMAC が必要  

3. **クライアントが復号 & 整合性チェック**  
   - クライアントがレスポンスを受信後  
     - 復号: `Plaintext = AES-256-GCM-Decrypt(Server Encryption Key, Ciphertext, Server IV)`  
       - ここで取得できるのは「HTTPレスポンスデータ + MAC」  
     - HMAC を再計算し、改ざんチェック: `Verify MAC = HMAC-SHA256(Server MAC Key, HTTPレスポンスデータ)`  
       - 受信した MAC 値とクライアント側で計算した MAC 値が一致すれば改ざんなし！  
       - 一致しなければ、改ざんされた可能性があるためレスポンスを拒否する。  

---

#### **まとめ**

✔ **サーバー → クライアントも、クライアント → サーバーとほぼ同じ流れ**  
✔ **AES-GCM の場合は HMAC 不要、AES-CBC の場合は HMAC 必要**  
✔ **TLS 1.3 では AEAD しか使えないので、HMAC を使うことはない**  

---

### **3️. 例: HTTPS で `https://bank.com` にアクセス**

1. **クライアント（ブラウザ）が「GET /account」リクエストを送信**
   - 暗号化前のリクエスト:  
     ```
     GET /account HTTP/1.1
     Host: bank.com
     Cookie: sessionid=abc123
     ```
   - 🔒 **HMAC 計算（整合性チェック用）**
     - `MAC = HMAC-SHA256(Client MAC Key, HTTPリクエストデータ)`
   - 🔒 **リクエストの暗号化**
     - `Ciphertext = AES-256-GCM-Encrypt(Client Encryption Key, HTTPリクエスト + MAC, Client IV)`
   - → サーバーに送信

2. **サーバーがリクエストを復号 & 整合性チェック**
   - 🔓 **リクエストの復号**
     - `Plaintext = AES-256-GCM-Decrypt(Client Encryption Key, Ciphertext, Client IV)`
     - 復号後のデータ: `GET /account HTTP/1.1` + `MAC`
   - 🔓 **HMAC 検証**
     - `Verify MAC = HMAC-SHA256(Client MAC Key, HTTPリクエストデータ)`
     - → HMAC 一致: 改ざんなし！処理続行  
     - → HMAC 不一致: リクエスト拒否

3. **サーバーがレスポンスを生成 & 暗号化**
   - サーバーが `GET /account` を処理し、レスポンスを作成:
     ```
     HTTP/1.1 200 OK
     Content-Type: text/html
     
     <html>Account details...</html>
     ```
   - 🔒 **HMAC 計算**
     - `MAC = HMAC-SHA256(Server MAC Key, HTTPレスポンスデータ)`
   - 🔒 **レスポンスの暗号化**
     - `Ciphertext = AES-256-GCM-Encrypt(Server Encryption Key, HTTPレスポンス + MAC, Server IV)`
   - → クライアントに送信

4. **クライアント（ブラウザ）がレスポンスを復号 & 整合性チェック**
   - 🔓 **レスポンスの復号**
     - `Plaintext = AES-256-GCM-Decrypt(Server Encryption Key, Ciphertext, Server IV)`
     - 復号後のデータ: `<html>Account details...</html>` + `MAC`
   - 🔓 **HMAC 検証**
     - `Verify MAC = HMAC-SHA256(Server MAC Key, HTTPレスポンスデータ)`
     - → HMAC 一致: 改ざんなし！レスポンスを表示  
     - → HMAC 不一致: データ破棄

---

### **4️. まとめ**
- **Client Encryption Key / Server Encryption Key**  
  → **データを暗号化して送受信**
- **Client MAC Key / Server MAC Key**  
  → **データの整合性（改ざん検知）**
- **Client IV / Server IV**  
  → **ブロック暗号の初期化ベクトル**

- HTTPS 通信のたびに、クライアントとサーバーは **このセッションキーを使って安全にデータをやりとりする！**


## 参照

- [SSL, TLS, HTTPS Explained](https://www.youtube.com/watch?v=j9QmMEWmcfo)
- [TLS Handshake Deep Dive and decryption with Wireshark](https://www.youtube.com/watch?v=25_ftpJ-2ME)
- [Wireshark](https://www.youtube.com/watch?v=aEss3CG49iI)
- [What happens when you type a URL into your browser?](https://www.youtube.com/watch?v=AlkDbnbv7dk)
- [HTTP 1 Vs HTTP 2 Vs HTTP 3!](https://www.youtube.com/watch?v=UMwQjFzTQXw)
