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

## DNS Spoofing with WiFi Pineapple


