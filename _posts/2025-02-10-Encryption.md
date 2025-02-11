---
title: "Encryption"
date: "2025-02-10 00:00:00 +0900"
categories: [Knowledge, Encryption]
tags: [Digital Signaturte, CA]
---

# Encryption / 暗号化

- 暗号化: 情報を送るときに、盗聴されても解読できない形式に変換すること
- 2つの暗号化
  - 共通鍵暗号方式: 送受信双方は同じ秘密鍵を使用して、暗号&復号する
    - ![alt text](../assets/images/Screenshot_2025-02-10_204249.png)
  - 公開鍵暗号方式: 送信側は公開鍵で暗号化、受信側は秘密鍵で復号化
    - RSA暗号の図
    - ![alt text](../assets/images/Screenshot_2025-02-10_204709.png)
- セッション鍵方式：
  - 上記の2つの方式を合わせたもの。
  - 平文は共通鍵方式、共通鍵は公開鍵暗号方式で送る方式。

## 公開鍵暗号方式 -  Asymmetric Encryption

- 公開鍵と秘密鍵の2つの鍵ペアを使用して情報を暗号化・復号化する方式

### RSA

- 桁数の大きな整数を素因数分解するのが困難である特性を利用
- ![alt text](../assets/images/Screenshot_2025-02-11_133632.png)
- linux: `openssle genrsa`

## 参照

- [共通鍵暗号方式と公開鍵暗号方式](https://www.youtube.com/watch?v=BbuGVf_oNSc)
- [RSA](https://www.youtube.com/watch?v=FmoOn5HFNx4)
- [電子政府における調達のために参照すべき暗号のリスト（CRYPTREC暗号リスト）](https://www.cryptrec.go.jp/list/cryptrec-ls-0001-2022r1.pdf)
- [RSA暗号による公開鍵暗号方式を5分で絶対に理解する](https://www.youtube.com/watch?v=CUhDu5praMQ)