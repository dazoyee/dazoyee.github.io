---
layout: post
title: "adminerのセットアップ"
categories: "setup"
---

mysqlを使うときのコンソール画面としてadminerを利用するための初期設定の手順を書いておく


## 手順

### 1. ダウンロード

- adminer  
  <https://www.adminer.org/>

- php  
  <https://windows.php.net/download/>  
  Apache利用も想定してThread Safe版にしておく

### 2．phpのiniファイルの書き換え

このままの利用では  
「拡張機能がありません。」  
と言われてしまうので、iniファイルから拡張機能を追加してあげなければならない。

1. まずphpのPath登録をしておく。
2. php.ini-developmentをコピーしてphp.iniファイルに変更する。
3. php.iniファイルを変更する。
    ```
    ;extension=mysqli
    ↓
    extension=php_mysqli.dll
    ---
    ;extension_dir = "./"
    ↓
    extension_dir = "./ext"
    ```
4. 念のため、extフォルダに変更した.dllが存在していることを確認しておく。
5. 以下が発生した場合<br/>
   `'vcruntime140.dll' 14.0 is not compatible with this php build linked with 14.16 in unknown on line 0`
   > - vcruntime140.dllって何？
       >
       >   PHPを動かすためにはVisualCのランタイムが必要でvcruntime140.dllというのがランタイムにあたるようだが、今入ってるPHPのバージョンと互換性がないよ！と言われているっぽい。
   >
   >- じゃあどうすれば良いの？
      >
      >   以下ページから--VisualStudio2019のMicrosoft Visual C++ 再頒布可能パッケージ--をインストールして再度Apacheを起動したら起動出来た！
      >
      >   <https://visualstudio.microsoft.com/ja/downloads/>

### 3. adminerを起動

`php -S 0.0.0.0:8080 adminer-4.7.7.php`

<http://localhost:8080>  
アクセスできることを確認する。

user,passwordだけ入力すればデータベースを選択できるようになる。

### 4. サービス登録

<https://engineer-ninaritai.com/php-warning-vcruntime140/#i>  
これを事前に実施すること！

---

## 参考記事

[Windows環境でApache+PHP+MySQL環境を整える](http://kurowasi2525.hatenablog.com/entry/2017/02/16/203233)

[Adminerを設置する](https://qiita.com/nissuk/items/2b1aee7f81f351c7ab05)

[XAMPPでApache起動出来ずにハマった話](https://qiita.com/ricchan_k/items/5b05851a351d3d590f34)
