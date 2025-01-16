---
title: "AAXC File Conversion"
date: "2025-01-16 00:00:00 +0900"
categories: [Technology, Audio]
tags: [amazon audible, AAXC file, m4b file, AAXtoMP3, Style-Bert-VITS2, Pay-to-play]
---


# Amazon Audibleから音声ファイルを取得して任意のファイル形式に変換する

## 概要

背景：
- Style-Bert-VITS2を利用した音声合成の際に、音声学習に使う質の良い音声データを探していた。
- 無料で使える高品質の音声データを試した後に、実際の人物の声を学習させたくなった。
- Amaozn Audibleは声優や芸能人がナレーターとして朗読していることを知った。

ボトルネック:
- Amazon Audibleはライブラリーの音声をPC上にダウンロード不可。
- Amazon Audibleはaaxcファイル形式を採用している。
- デジタル著作権管理（DRM）で保護されており、ファイルを再生するにはAudibleアプリや認証されたデバイスが必要。

結果：
- PC上でAAXC → m4b → wav の順番でローカルに音声ファイルを保存成功。
- Style-Bert-VITS2での音声合成：セリフによってはほとんど違和感の無い自然な音声発話に成功。(省略)

所感：
- AAXtoMP3を利用をした際にcover.jpgを認識出来ないバグがあり、ハマった。
- 最終的にスクリプト内部を修正して解消。
- wav形式は重い。

## 目次

1. 必要なツールやパッケージをインスト―ル
2. Activation Bytesを取得
3. AAXCファイルとvoucherファイルを取得
4. AAXtoMP3のリポジトリをダウンロードして実行
5. Asinを取得してチャプターとカバー情報を取得
6. AAXCファイルを解読
7. m4b→wavに変換

## 詳細

- URL