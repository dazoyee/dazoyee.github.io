---
layout: post
title: "WEBアプリラーニング"
categories: "study"
---

WEBアプリケーションについて学んだことをまとめる


## 本

2019/4/11

- リソース
    - RESTにおいて重要な概念
    - Web上に存在する、名前を持ったありとあらゆる情報のこと
    - リソースの名前とはURLのこと

---

## http,curl,framework

2019/3/26

- httpとは
    - 文字列のやり取り
        - 今はSPA（シングルページアプリケーション）が主流、単一のwebページのみで展開するwebアプリ
        - JavaScriptを用いた開発であり、これをすることが理想ではあるが、画面遷移がなく初心者にはわかりにくい
        - 上記に比べてhtmlだとデータのやり取りがhtmlの遷移の中で処理されるのでわかりやすいので、研修ではこちらの方式が採用
        - byte⇔文字列の転送（シリアライズ）のなかで中継ぎとしての役割を果たしているのがテンプレートエンジン（thymeleaf）

![図1](/img/posts/study/web-app-learning/1.png)  
↑WEBアプリケーションの仕組み①

![図2](/img/posts/study/web-app-learning/2.png)  
↑WEBアプリケーションの仕組み②

- curl
    - さまざまなプロトコルを用いてデータを転送するライブラリとコマンドラインツールを提供するプロジェクト
    - 開発者としては必須となる、コマンドラインツール
        - ツールをダウンロードして試してみた
            - curl.exeがあるフォルダでcmdを開く
            - そこでwebアプリ（google,amazon,etc...）のurlを打ち込むとhtmlの文字列が返却される
                - このようにしてデータのやり取りが実施されているということ
            - `curl localhost:8080/login`
            - これを入力すればプログラムしていたhtmlが出力される

- フレームワークと言語
    - 世の中にはたくさんのフレームワークと言語があり、会社として主流なのがSpring BootとJava
    - アプリ開発研修でやっていくのはSpring Bootの研修であるということ
    - 初心者ならPure Javaをやるよりもフレームワークを用いた言語習得のほうがよい
    - フレームワーク,言語は覚えておく必要はなく、何ができるかを知っておいて、必要となったときに調べながらやるという方法で問題ない
    - （もちろん専門的なものを覚えておく必要もあるのだが）

![図3](/img/posts/study/web-app-learning/3.png)  
いろんなフレームワークがある

---

## Spring Boot,Apache,Tomcat

2019/3/27

![図4](/img/posts/study/web-app-learning/4.png)

- Spring Bootについて知る
    - Spring徹底入門 Spring FrameworkによるJavaアプリケーション開発
        1. Spring Framework とは ← 概要説明
        2. Spring Core(DI×AOP)      ←
        3. データアクセス(Tx、JDBC)     ← JDBC、RDBの部分に触れている
        4. Spring MVC ← mvcの構成について
        5. Webアプリケーションの開発 ← バリデーション
        6. RESTful Webサービスの開発 ← REST(REpresentational State Transfer)はWebサービスの設計モデル
        7. Spring MVCの応用 ← 応用
        8. Spring Test ← テスト
        9. Spring Security ← 研修ではやらなかったが、大事なセキュリティに関すること
        10. Spring Data JPA ← JPA：Java Persistence API 、DBとのやり取りについて
        11. Spring + MyBatis ← 永続化するために必要なフレームワーク
        12. Spring + Thymeleaf ← Spring Bootにデファクトされたテンプレートエンジン
        13. Spring Boot ← 上記のフレームワーク（ライブラリ）の集合体

        - この本はSpring Bootを勉強する上で最も良い本
        - この本に沿ってSpring Bootの全体像を見ていく
            - JDBC：RDBにアクセスするために必要となるAPI
            - MVC
                - model：実際に伝達したいデータを処理するところ
                - view：データの見せ方について考えるところ
                - controller：modelとviewの処理に対して命令するところ
            - バリデーション：適切な型に適切なデータが入っていることのチェックをする
                - （金額がほしいときに文字列が入力された際の対処）
            - restについては後日詳細
            - Spring Bootのライブラリとしてセキュリティ機能がある
            - DBに対してもプロトコルのようなものがあり、それがJPA
            - 永続性：PCの電源を落としてもデータは消えることなく、HDDに保存されるし、再度電源入れればそのデータは見る事が出来る
            - jdbcやthymeleafなど一つ一つの機能はそれぞれ更新されており、いくつかの機能の互換性を保った集合体（フレームワーク）のこと

        - Spring：広義のSpring Frameworkと同義であり、フレームワークのこと。このフレームワークの中にはjdbcやthymeleaf,mvcなどの様々なフレームワーク（ライブラリ）が含まれている
        - Spring Boot：webアプリを開発する上で、使われやすいフレームワーク（ライブラリ）が集められたパッケージのことであり、これもまたフレームワークとなる
        - 背景として、もともとはjdbcやthymeleafといった機能のそれぞれが独自でバージョンアップしており、進化していた
        - その結果数多くのフレームワークが存在することとなったので、整理され機能としてよさげなフレームワークが集められ、パッケージングされたのがSpring Boot
        - 狭義のSpring Frameworkとはフレームワークの一つであり、固有名詞。Jdbcやthymeleafといったフレームワークの一つとしてSpring Frameworkがある

- RDB：関係データベースと訳され、データを複数の表として管理し、表と表の関係を定義することで、複雑なデータの関連性を扱えるようにしたデータベース管理方式
    - この中にはOracle,MySQL,Aurora,MariaDBなど数多くある
    - h2もPostgreSQLも含まれており、アプリ開発研修時は永続化されないh2のほうがデータがその都度消えるのでよかった
    - プロファイル：設定ファイル
    - プロファイルをやりたいことに合わせて変え、STGのためのDB、商用のためのDBにあわせて設定する
    - OSS：オープンソースソフトウェアとは、利用者の目的を問わずソースコードを使用、調査、再利用、修正、拡張、再配布が可能なソフトウェアの総称

![図5](/img/posts/study/web-app-learning/5.png)  
↑RDBの種類について

- ApacheとTomcat
    - Apache：Webサーバソフトウェアのこと。ホームページに使われていたりしている。Htmlが静的で、アクセスされたあとに情報が動的に表示されていない状態の画面。ブログとか
    - これをダウンロードしてcmdでhttpd:exeをする
    - Apacheが起動したので、ブラウザを立ち上げ「http://localhost/」へアクセスするとhtmlが表示される

    - Tomcat：Webアプリを動かすときのソフトウェア。いまはだいたいがトムネコを使っている。Twitterのように常に情報が更新されるような動的な表示を可能とする
    - Nginx（エンジンエックス）：これもWebサーバの一つで力をつけてきているソフトウェアだ

    - Tomcat：サーブレットコンテナ ← サーブレット（＝web上で動くjavaプログラム）のためのソフト
    - Apache：Webサーバ

---

## rest

2019/3/28

- REST
    - REST(REpresentational State Transfer)はWebサービスの設計モデル
        - GET（取得）、POST（登録）、PUT（更新）、DELETE（削除）
        - これをインターフェースとして統一したのがRESTモデルとなる
        - RESTfulなサービスを作ることで、異なるサービスでもAPIドキュメントさえ読めば同じように使うことができる
        - SPA方式ではよくつかわれるが、アプリ開発研修では使わないかな

        - httpでデータを送るときにはurlが必須であり、そのurlのエンコードにリソースを載せて渡す
            - `/getUsers`
            - ↑のときはusersをgetするということを表しており、省略するので`/users`となる

![図6](/img/posts/study/web-app-learning/6.png)

![図7](/img/posts/study/web-app-learning/7.png)  
↑RESTの種類

- 環境変数の登録
    - ダウンロードしたらそのダウンロードしたものを利用するという環境変数の登録をしておくとよい
        - `curl https://www.google.com`
        - `mvn install`
        - このcurlやmvnは環境変数の登録をしたのでこのように利用できる

---

## Spring Boot機能

2019/4/1

- Spring Bootの主要機能
    - DIコンテナ ← シングルトン
    - プロファイル
    - Auto Configuration

![図8](/img/posts/study/web-app-learning/8.png)

- シングルトン
    - 一種のデザインパターン
    - 生成するインスタンスの数を1つに制限するデザインパターン
    - 複数のインスタンスが作られると困るときに用いる

    - とあるライブラリを利用するとき：javaClass → ライブラリ
    - javaClassがライブラリを使うのであって、ライブラリ自体はどこで使われているかは把握していない
    - （javaClass ← ライブラリ ではない！）
    - 同様にJavaで書くとき、 class A extends B となる（A→Bの関係）
        - AクラスはBクラスのインスタンスを使うということになる
        - extendsは継承のこと、既存のクラスをもとに新しいクラスを作ること

    - このようにAがBを使うための方法がある
        - 継承 extends
        - 実現 implements
            - implementsはinterfaceが必要となる
            - implementsでinterfaceに定義されている全ての抽象メソッドをオーバーライドして実装できる
                - オーバーライド：スーパークラスで定義しているメソッドをサブクラスで再定義すること
                - ライドは上書き
        - DI（Dependency Injection: 依存性注入）
            - 2つのクラスがまたがるとき、一方を変更したとしても、もう一方を変更することなく実装できる
            - テストのときに役立つ

- DIコンテナ
    - DIとDIコンテナは違う
    - DIでは足りないところを補っているのがDIコンテナ
    - Spring Bootで重要な機能となる

- プロファイル
    - 定義すること
    - application.ymlで定義している
    - Main→Apple→Bingoの順で、他クラスを利用するとき、Bingo（JdbcTemplate)ではymlでいくつか定義している
    - ここで定義しておくことで楽になる

- Auto Configuration
    - Spring Bootを利用すると、Bean定義しなくてもSpringアプリケーションが作成できる
    - これは定義していないわけではなく、自動で定義してくれている
    - それぞれのクラスにインスタンスを用意しておき、ユーザとしては定義することなく、利用可能となる
    - STS起動時にここらへんが行われている

---

## thymeleaf

2019/4/4

- thymeleaf
    - テンプレートエンジン
        - テンプレートと呼ばれる雛形と、あるデータモデルで表現される入力データを合成し、成果ドキュメントを出力するソフトウェアまたはソフトウェアコンポーネント

      ![図9](/img/posts/study/web-app-learning/9.png)
      ![図10](/img/posts/study/web-app-learning/10.png)

    - ↑"aaaaaaaa"
    - デザイン側としてはこのように書いているが、thymeleafが用いることで”entries”を表示している
    - これができるのがthymeleafのおかげ
    - modelで処理したものを文字列で返却する
    - 返却された文字列に対し、thymeleafが変換をしてentriesという情報を持たせる
    - 変換された情報をもってデザインする
    - ⇒ SSR（サーバーサイドレンダリング）
        - ユーザ（view）側ではなく、サーバ（model）側でHTML生成を行う
        - ユーザ側での生成がないので、早くwebページが表示される

    - SPAの方式だとviewに文字列を渡してから、view側で変換して情報を持たせるという方式となる

---

## jdbc

2019/4/5

![図11](/img/posts/study/web-app-learning/11.png)

- JDBC
    - Java Database Connectivityの略で、Javaアプリケーションからデータベースを操作するAPIのこと

- JdbcTemplate
    - Spring Bootでjdbcの機能を使うためにうまくパッケージングされたフレームワーク
    - これを使うことでSpring Bootにおけるデータベースへのアクセスができる
    - jdbcを使うためにはいくつか指定しておくものがある
        ```
        .url("jdbc:h2:mem:jdbc-template-sample")
        .driverClassName("org.h2.Driver")
        .username("oiso")
        .password("password")
        ```
    - h2のドライバにアクセスするときに求められる
    - そのあとh2にアクセスして、実際にデータベースになんらかの処理を実行する
    - `jdbc.queryforobject` とか

    - あと、アクセスするためにデータベースもまた指定しておく必要がある（addScripts）
    - データベースへのアクセスはSpring Bootがデフォルトで指定してくれているsql名なら指定しなくてもできる
    - `schema.sql`はCREATEとか、`data.sql`はINSERTとかを書く

    - 上記urlとかはJava内で書かなくても、別ファイルで定義してあげることで自動的に読み込んでくれる
    - それがオートコンフィグレーション

- Auto Configuration
    - Spring Bootを利用すると、Bean定義しなくてもSpringアプリケーションが作成できる
    - application.yml で定義することで、JdbcTemplateを自動認識してアクセスできるようにする
        - アプリの設定をしてくれる
        - さらに言うと、オートコンフィグレージョンの機能で定義しなくても（ymlに書かなくても）アクセスできる

---

## RestTemplate

2019/4/8

![図12](/img/posts/study/web-app-learning/12.png)

- RestTemplate
    - RestTemplateの何がすごいかというと、Rest APIを呼び出すのに1行だけで済んでしまうという点
        - RESTful API(REST API)とは
            - Webシステムを外部から利用するためのプログラムの呼び出し規約(API)の種類の一つで、RESTと呼ばれる設計原則に従って策定されたもの

    - `restTemplate.getForObject("http://localhost:8090/", String.class);`
    - これで、`localhost:8090/`にGETし、レスポンスされた文字列を表示等することができる

    - rest-template-sample → thymeleaf-sample で指定したurlにリクエストして、Stringで書かれたクラスをもらう
    - thymeleaf-sample → rest-template-sample でreturn "index.html"をレスポンスとして受け取っている

- HTTPステータスコード
    - 2xx Success 成功
    - 4xx Client Error クライアントエラー
    - 5xx Server Error サーバエラー

    - 大きくこの3つに分かれる

- 命名規則
    - camelCase
    - PascalCase
    - snake_case
    - kebab-case

- IDE
    - Ctrl+Space をうまく使う

---

## git

2019/4/9

- git：ソースコードのバージョンを管理するツール
- GitHub：Gitを利用した、開発者を支援するWebサービス
    - リポジトリ：ファイルやディレクトリの状態を記録する場所

    - gitの流れ
        - ![図13](/img/posts/study/web-app-learning/13.png)

        1. 作業ディレクトリのファイルを修正します。
        2. 修正されたファイルのスナップショットをステージング・エリアに追加して、ファイルをステージします。
        3. コミットします。（訳者注：Gitでは）これは、ステージング・エリアにあるファイルを取得し、永久不変に保持するスナップショットとしてGitディレクトリに格納することです。

    |    種類     | バイナリコードの提供 | ソースコードの提供 | 使用料 |
    |:---------:|:----------:|:---------:|:---:|
    |    OSS    |    Yes     |    Yes    | 無償  |
    | Freeware  |    Yes     |    No     | 無償  |
    | Shareware |    Yes     |    No     | 有償  |
    | 商用ソフトウェア  |    Yes     |    No     | 有償  |

    - コンピューターに処理を依頼する命令を、CPUが理解できるように2進数で表したコードのこと。
    - 人間にとって理解しやすいプログラミング言語で記述されているソースコードをコンパイルすることでバイナリーコード（プログラム）に変換される

- cmd
  ```
  dir　ディレクトリ
  mkdir　make directory
  git init　git作成
  git add oiso.txt
  git status
  git commit -m " "
  git log
  git reset --hard xxxxxx
  ```

2019/4/10

![図14](/img/posts/study/web-app-learning/14.png)

- commitについて
    - 機能ごとに細かくcommitすること

    - ローカルでcommitしたあと、githubにpushすることでweb上で書いたコードに対し、コミュニケーションをとることができる

- 命名について
    - repoの名前等命名については目的（OperationManagement）、機能（TicketViewer）、固有名詞（PoPcorn）といった命名に沿ってできそう
    - 固有名詞だと一意、覚えてもらえる、機能が変わっても問題ないのでよいかも

      ```
      git init
      git add xxx.html
      git commit -m "first commit"
      git remote add origin http///
      git push origin master
      ```

2019/4/11

![図15](/img/posts/study/web-app-learning/15.png)

- branch
    - `git branch` : gitにあるbranchをみる
    - `git branch develop/a` : develop/aというbranchを作った
    - `git checkout develop/a` : develop/aにbranchをスイッチした

    - 今まではmasterでcommitしていたところ、新たにbranchを作り、そこでcommitする

    - `git add` と `git commit` する

    - ここでさらに別のbranch（develop/b）を作り、そこでcommitする
    - すると枝が2つに分かれる
    - この２つはmasterから派生してできたbranchとなる
    - 2つのbranchをmasterに取り込みたいとき、mergeする

    - 今はdevelop/bのbranchにいるので、checkoutしてmasterにスイッチしてから、
    - `git merge develop/b`
    - masterにdevelop/bをmergeする

    - さらにdevelop/aをmergeしようとしても、体裁等違うためconflictとなってしまう
    - 手動で良しなに直してcommitする
    - すると画像の通りとなる

    - このあと、oisoとhaineというbranchを作ったとして、oisoにcommitしたあと
    - `git push origin oiso`

    - GitHub上で、oiso→masterにmergeしたいときoisoからmergeはできないのでGitHubから pull request をすることでmergeできるようになる
    - merge完了
    -
  haineをmergeしたいときも同様だが、conflictとなるので、GitHub上で差分を修正することもできるし、一度ローカルでhaineを取り込んでconflictとならない状態にしてからcommitしてpushするという方法もある

    - GitHub上で完了したら、ローカルに同期してあげる
    - （masterを中心とするとき）branchをmasterにしてから、`git pull origin master` githubの情報をローカルに引っ張る

    - これで同期完了となった

---

## jenkins

2019/4/15

- jenkins
    - javaで書かれた一webアプリ
    - CI（継続的インテグレーション）：結合する場所として用意した共有リポジトリに、開発者たちがソースコードをコミットすると、マージするためにソースコードのビルドとテストが自動に実行される
    - ソフトウェアのビルド、検証、サーバへのインストール等の一連作業を自動化できる
    - 利用するときにはローカル環境にサーバを立ててから始める
    - 初期値はポート番号が8080となっているが、そもそも8080は使わないことが理想的なので、`aplication.yml`でポート番号を変えておく
        - ポート番号は自由に変えられるが、一部既に使っていることもある
        - 4桁が主流なところがあるが、5桁の人もいる
        - 1つのアプリだけであれば、ぞろ目8888とかが多い
        - 複数のサーバをまたがるアプリのときは良しなにつける
        - ポート番号は部屋番号、IPアドレスはマンション

    - WindowsサービスというところでPCに入っている各アプリの状態を確認できる
    - アプリの起動、停止も可能

  ![図16](/img/posts/study/web-app-learning/16.png)

2019/4/16

- ソフトウェアのテストを継続的に実施するということ
    - コーディング初期段階からコミットごとにビルドし、ビルドに失敗するとメールで通知され、即座にビルド失敗の原因を取り除きながら開発できる
    - つまり、全て終わってからテストをするのではなく、こまめにテストしたほうがよく、それを可能にさせるのがJenkinsということ

    - ジョブ：一度に実行する処理の集まりのこと

    - ビルド・トリガ

    | 分 |  時   | 日 | 月 | 曜日 |      説明      |
    |:-:|:----:|:-:|:-:|:--:|:------------:|
    | 0 |  0   | 1 | 1 | 0  | 1/1 SUN 0:00 |
    | 0 | */12 | * | * | *  |    12時間ごと    |

    |    |               |
    |:--:|:-------------:|
    | 分  |     0~59      |
    | 時  |     0~23      |
    | 日  |     1~31      |
    | 月  |     1~12      |
    | 曜日 | 0~6（0を日曜日とする） |

    - カバレッジ：実装したプログラムをテストする際に、テスト対象のプログラムがどの程度テストされたかを示す
    - インスペクション：コーディングし終わったプログラムが規約どおりに書かれているか確認する作業
