---
title: 'Reactってなにやってるの？をRails開発者の目線から紐解く'
emoji: '✨'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: [Rails, React, Ruby]
published: false
---

# はじめに

こんにちは RUNTEQ 生の yasuda_naoto と言います。
RUNTEQ といえば、Rails という Ruby 言語がベースとなっているフレームワークを用いてアプリを開発し、自社開発企業などへの就職を目指すプログラミングスクールです。
以前のカリキュラムでは RUNTEQ 卒業後のカリキュラムで、React ではなく Vue.js を利用していたそうですが、わたしたちの代になって React を API として利用しアプリを開発するカリキュラムになりました。
私も以前から React を用いて SPA の開発に少しだけ携わったことがあるので、その経験から、Rails7 の新機能 Turbo と React を用いた SPA の違いについて試しながら見ていこうと思います。

## EC2

Elastic Computing Cloud(C が 2 つある)
仮想サーバーを構築する

### インスタンス

**インスタンス** とは仮想サーバーの単位である

### インスタンスタイプ

用途やスペックを決めるもの
t(用途)2(世代).micro(スペック) のように表す
例： t2.micro m5.large
