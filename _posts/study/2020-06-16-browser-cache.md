---
layout: post
title: "ブラウザキャッシュ"
categories: "study"
---

ブラウザキャッシュとはなにか調べたことをまとめる


## そもそもキャッシュとは

キャッシュとはパソコンやスマホに一時的にウェブページのデータを保存しておいて、  
次に同じページを開いたときに素早く表示させる仕組み。

### キャッシュが消えるタイミング

たいていブラウザごとにキャッシュの”保存容量”が決まっている。  
限界をこえると古いものが削除されていく。

## キャッシュには大きく2種類

### サーバーキャッシュ

「サーバー側」に保存されるキャッシュ

- メリット
    - サーバーの負荷を軽減できる。
    - サーバー管理者がキャッシュの動作（有効期間や、キャッシュ対象、ゴミ掃除など）を管理できる。
    - Webサイトの表示が速くなる。

- デメリット
    - 場合により、更新したコンテンツが即時反映されない。これにより、正しい表示がされない事がある。
    - AWS CloudFront等のキャッシュ専用サーバーを利用すると、費用が発生する。

### ブラウザキャッシュ

「ユーザー端末（PCやスマホ等）」に保存されるキャッシュ  
ただし設定自体は基本的に、コンテンツを配信するWebサーバー側で行う必要がある。

- メリット
    - サーバーの負荷を軽減できる。
    - パケット通信量の削減ができる。
    - サイトの表示が速くなる。
- デメリット
    - サーバー管理者がキャッシュの管理ができない。 （有効期間が長いと、ずっと古い表示のまま。）
    - キャッシュを溜めすぎると、ブラウザの動作が遅くなる事がある。

## HTTP Response Headers

キャッシュの動作（有効期間、キャッシュするか否か）は、HTTP Response Headers内に「キャッシュ動作に関するヘッダー」が含まれる  
この「キャッシュ動作に関するヘッダー」設定はWebサーバー側で行う

### Cache-Control ヘッダー

キャッシュをするか否か、する場合には何秒程度キャッシュするかと言った「振る舞い」を定義する

- no-store : レスポンス結果の一切をキャッシュしてはいけない
- no-cache : 更新が必要かを毎回Webサーバーに確認してね = キャッシュの更新が必要かを判断
- max-age : 指定した秒数以内であれば、次回はサーバーへ確認せずに（ブラウザ）キャッシュを利用し続ける

### Expires ヘッダー

有効期間（TTL）を意味するヘッダーで、 Cache-Control: max-age と同じ  
ちなみにGoogleは、こちらの使用を推奨している

### ETag ヘッダー

ファイルに更新が有るか否かを確認するためのヘッダー

### Last-Modified ヘッダー

ETag ヘッダーと同様の内容だが、こちらは値に「最後に更新した日付」が入る

## CDN（Content Delivery Network）の話

> 「コンテンツ配信ネットワーク」の意味。
>
>**インターネット上にキャッシュサーバーを分散配置し、エンドユーザーに最も近い経路にあるキャッシュサーバーから画像や動画などのWebコンテンツを配信する仕組み**
> のこと。CDNサービスは、CDNの仕組みを利用して、企業向けに画像や動画を多用するWebサイトの配信を代行するサービスを指す。
>
>CDNサービスの事業者は、キャッシュサーバーをインターネットのIX（相互接続点）に近い位置や、大手プロバイダーのネットワーク内などに配置するので、企業のWebサーバーと自動的に同期できる。
>
>
DNS設定をすれば、オリジナルのWebサーバーへのアクセスがCDN事業者のキャッシュサーバーへ振り分けられるため、キャッシュサーバーがオリジナルのWebサーバーに代わってコンテンツを配信する。また、Webサーバーへのアクセスを代理するため、アクセスが集中する場合でもWebサイトの負荷を軽減できる。

## スーパーリロード

キャッシュを読み込まずに、最新のページ情報を読み込むこと（ctrl + F5）

---

### 参考記事

[キャッシュとは？IT初心者でも分かるように解説](https://saruwakakun.com/it/web/cache)

[【Webサイト高速化】ブラウザキャッシュについてまとめてみた](https://aimstogeek.hatenablog.com/entry/2018/01/17/154537)

[キャッシュについて整理](https://qiita.com/anchoor/items/2dc6ab8347c940ea4648)