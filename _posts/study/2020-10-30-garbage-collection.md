---
layout: post
title: "ガベージコレクション"
categories: "study"
---

ガベージコレクションについて


## 最初に読んだ参考記事

スライド

https://www.slideshare.net/YujiKubota/garbage-first-garbage-collection

[5分で分かるガベージコレクションの仕組み](https://geechs-job.com/tips/details/35)

---

## GCログ

### 取得方法

- GCログを取得するときのオプション<br/>
  [JavaのGCに関するオプションについてまとめてみた。](https://qiita.com/ynkgw/items/5e5c57d52ac1379daf6d)

GCログを取得するにはこのようなコマンドになりそう。

```
java -Xlog:gc*=info:file=./gc_%t.log:time,uptime,level,tags:filecount=10,filesize=10M -jar natal.
jar
```

いろいろためしてみると、`-jar`は最後に書かないとできなかった。。

下記はjava8までのようで実施できない
>
>```
>-Xloggc:.¥gc.log
>-XX:+PrintGCDetails
>-XX:+PrintGCDateStamps
>```
>> -Xloggc:.¥gc.log
>
>出力したGCログを指定したファイルへ出力する。
>
>> -XX:+PrintGCDetails
>
>GCログの詳細表示。これがすべて
>
>> -XX:+PrintGCDateStamps
>
>DateStampの追加。日付と時間が追加される。

### ログの見方

- GCViewer
    - ダウンロード
        - https://github.com/chewiebug/GCViewer/releases/tag/1.36
        - jarをダウンロードしてきて、`java -jar gcviewer-1.36.jar`をする
    - view

| 名称                     | 内容                                    | 単位  |
|------------------------|---------------------------------------|-----|
| Full GC Lines          | Full GCを表すライン                         | 秒   |
| Inc GC Lines           | Incremental GCを表すライン？                 | 秒   |
| GC Times Line          | GCにかかった時間を表すライン                       | 秒   |
| GC Times Rectangles    | GCにかかった時間などを矩形で表現しているっぽい              | 秒   |
| Total Heap             | ヒープの合計サイズを表示 `-Xms`と`-Xmx`の設定が反映されている | サイズ |
| Tenured Generation     | OLD領域サイズ                              | サイズ |
| Young Generation       | NEW領域サイズ                              | サイズ |
| Used Heap              | ヒープの利用サイズを表す                          | サイズ |
| Used Tenured Heap      | OLD領域の利用サイズを表す（めちゃめちゃ見づらい）            | サイズ |
| Used young Heap        | NEW領域の利用サイズを表す（めちゃめちゃ見づらい）            | サイズ |
| Initial mark level     |                                       |     |
| Concurrent collections |                                       |     |
|                        |                                       |

- 確認観点は３つ
    - minor/full GCの処理時間が極小であること
    - minor GCの処理回数が1秒に複数回実施されていないこと（4～5分に1回程度であればよい）
    - Heap使用量がGC直後に落ちていること

- Summary

|                          |                                             |
|--------------------------|---------------------------------------------|
| Total heap               | ヒープ使用量                                      |
| Max heap after full GC   | full GC後のヒープ解放量                             |
| Freed Memory             | 解放済みメモリー？                                   |
| Freed Mem/min            |                                             |
| Total Time               | GCログ取得開始からの時間                               |
| Accumulated pauses       | GC発生時に停止した時間の合計                             |
| Throughput               | Accumulated pauses/Total Time （100%でないとまずい） |
| Number of full gc pauses | full GC発生回数                                 |
| Full GC Performance      | 1秒あたりのfull GCによるヒープ解放量                      |
| Number of gc pauses      | GC発生回数                                      |
| GC Performance           | 1秒あたりのGCによるヒープ解放量                           |

- Pause

|                                |                                    |
|--------------------------------|------------------------------------|
| Accumulated pauses             | [minor/full] GC発生時に停止した時間の合計       |
| Number of gc pauses            | [minor/full] GC発生回数                |
| Avg Pause                      | [minor/full] GC発生時の平均停止時間          |
| Min/Max Pause                  | [minor/full] GC発生時の最大/最小停止時間       |
| Min/max pause interval         | [minor/full] GC発生の最大/最小間隔          |
| Accumulated full GC            | [full] full GCで停止した時間              |
| Number of full gc pauses       | [full] full GC発生回数                 |
| Avg full GC                    | [full] full GC発生時の平均停止時間           |
| Min/Max full gc pause          | [full] full GC発生時の最大/最小停止時間        |
| Min/max full gc pause interval | [full] full GC発生の最大/最小間隔           |
| Accumulated GC                 | [minor/full] minor GC発生時に停止した時間の合計 |
| Number of gc pauses            | [minor/full] minor GC発生回数          |
| Avg GC                         | [minor/full] minor GC発生時の平均停止時間    |
| Min/Max gc Pause               | [minor/full] minor GC発生時の最大/最小停止時間 |

## GC講座

- GCには NEW領域とOLD領域が存在する
- さらにNEW領域にはEdenとSurvivor0とSurvivor1が存在する

1. new Instanceで作られたものはEdenに溜まる
2. Edenが溜まったらminor GC発生し、まだ使われるならSurvivor0、もう使われないなら削除
3. Survivor0が溜まったらminor GC発生し、まだ使われるならSurvivor1、もう使われないなら削除
4. Survivor1が溜まったらminor GC発生し、まだ使われるならSurvivor0、もう使われないなら削除
5. minor GC発生とともにSurvivor0とSurvivor1を行き来する
6. 一定回数をSurvivor0とSurvivor1を行き来したら、OLD領域にいく（OLD領域のものはずっと使われる想定となる）
7. OLD領域にいったらminor GCで削除されることはなく、溜まり続けることになる
8. OLD領域に空きがなくなったときにfull GC発生して、大幅なヒープ解放が見込まれる

- アプリ起動直後にはいろんなヒープを使用するので、full GCが起こりやすいかも
- アプリ起動直後のfull GCはスワップアウトされていないので、時間かかりにくいっぽい
- スワップアウト：メモリの中身を一時的にハードディスクに移すこと
- ある程度起動した中でのfull GCはスワップアウトされている可能性があり、時間かかることが見込まれる

---

## ヒープダンプまで

https://qiita.com/i_matsui/items/4997ebedbdd7a6495509

- ヒープダンプのコマンドとか<br/>
  [OutOfMemoryError の調べ方](https://qiita.com/opengl-8080/items/64152ee9965441f7667b)

```
jcmd -l
jcmd 15248 GC.heap_dump -all heapdump-all.hprof
```

> jcmd -l

現在実行されている Java プロセスを表示する

> jcmd 15248 GC.heap_dump -all heapdump-all.hprof

`-all`とすることでfull GCなしにすることができる

プロセスIDを指定して（15248）コマンド実施（保管場所は./app/target）

---

## Eclipse

### Memory Analyzer

- Eclipseのプラグインに「Memory Analyzer」を加える
