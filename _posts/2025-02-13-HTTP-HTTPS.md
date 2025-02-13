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

## 


## 参照

- [TLS Handshake](https://www.youtube.com/watch?v=3ImpkOVg0YQ)
- [TLS1.2詳細](https://www.youtube.com/watch?v=fiooR6iXkCA)
- [TLS1.2補足](https://qiita.com/n-i-e/items/41673fd16d7bd1189a29)
- [TLS1.2 Wireshark](https://kuniiskywalker.github.io/2017/09/06/ssl-connect/)
- [TLS1.3詳細](https://www.youtube.com/watch?v=Ig67jwfZU4U)
- [SSL, TLS, HTTPS Explained](https://www.youtube.com/watch?v=j9QmMEWmcfo)
- [TLS Handshake Deep Dive and decryption with Wireshark](https://www.youtube.com/watch?v=25_ftpJ-2ME)
- [Wireshark](https://www.youtube.com/watch?v=aEss3CG49iI)
- [What happens when you type a URL into your browser?](https://www.youtube.com/watch?v=AlkDbnbv7dk)
- [HTTP 1 Vs HTTP 2 Vs HTTP 3!](https://www.youtube.com/watch?v=UMwQjFzTQXw)
- [TLS 暗号設定ガイドライン](https://www.ipa.go.jp/security/crypto/guideline/gmcbt80000005ufv-att/ipa-cryptrec-gl-3001-3.1.0.pdf)
