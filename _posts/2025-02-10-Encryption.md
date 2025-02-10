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

## 参照

- [共通鍵暗号方式と公開鍵暗号方式](https://www.youtube.com/watch?v=BbuGVf_oNSc)