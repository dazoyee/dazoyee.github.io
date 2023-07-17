---
layout: post
title: "HikariCP"
categories: "study"
---

HikariCPのパラメータについて


### 必須設定

| プロパティ               | 概要                                                | デフォルト |
|---------------------|---------------------------------------------------|-------|
| dataSourceClassName | JDBCドライバが提供するDataSourceクラスの名前                     | なし    |
| jdbcUrl             | DriverManagerベースの設定を使用するようHikariCPに指示するプロパティ      | なし    |
| username            | 基礎となるドライバからConnectionsを取得する際に使用されるデフォルトの認証ユーザ名    | なし    |
| password            | 基礎となるドライバから Connections を取得する際に使用されるデフォルトの認証パスワード | なし    |

### よく使われる設定

| プロパティ               | 概要                                                                         | デフォルト              |
|---------------------|----------------------------------------------------------------------------|--------------------|
| autoCommit          | プールから返される接続のデフォルトの自動コミット動作                                                 | true               |
| connectionTimeout   | クライアントがプールからの接続を待機する最大ミリ秒数                                                 | 30000 (30秒)        |
| idleTimeout         | 接続がプール内でアイドル状態であることを許可される最大時間                                              | 600000 (10分)       |
| keepaliveTime       | HikariCPがデータベースやネットワーク・インフラストラクチャによってタイムアウトすることを防ぐために、接続を維持しようとする頻度        | 0 (無効)             |
| maxLifetime         | プール内の接続の最大有効期間                                                             | 1800000 (30分)      |
| connectionTestQuery | データベースへの接続がまだ生きていることを検証するために、プールから接続が渡される直前に実行されるクエリ                       | なし                 |
| minimumIdle         | HikariCPがプール内で維持しようとするアイドル接続の最小数                                           | maximumPoolSizeに同じ |
| maximumPoolSize     | アイドル接続と使用中接続の両方を含む、プールが到達することを許可される最大サイズ                                   | 10                 |
| metricRegistry      | 様々なメトリクスを記録するためにプールで使用されるCodahale/Dropwizard MetricRegistryのインスタンスを指定      | なし                 |
| healthCheckRegistry | 現在のヘルス情報を報告するためにプールが使用する、Codahale/Dropwizard HealthCheckRegistryのインスタンスを指定 | なし                 |
| poolName            | 接続プールのユーザー定義の名前                                                            | 自動生成               |

### 使用頻度の低い設定

| プロパティ                     | 概要                                                                            | デフォルト          |
|---------------------------|-------------------------------------------------------------------------------|----------------|
| initializationFailTimeout | プールに初期接続を正常にシードできない場合に、プールが "高速フェイル" するか                                      | 1              |
| isolateInternalQueries    | HikariCPがコネクション・アライブ・テストのようなプール内部のクエリーを独自のトランザクションで分離するか                      | false          |
| allowPoolSuspension       | JMXを介してプールを中断および再開できるか                                                        | false          |
| readOnly                  | プールから取得したConnectionsがデフォルトで読み取り専用モードであるか                                      | false          |
| registerMbeans            | JMX Management Beans ("MBeans") が登録されるか                                       | false          |
| catalog                   | カタログの概念をサポートするデータベースのデフォルトカタログを設定                                             | driver default |
| connectionInitSql         | 新しい接続が作成されるたびに、その接続をプールに追加する前に実行される SQL 文を設定                                  | なし             |
| driverClassName           |                                                                               | なし             |
| transactionIsolation      | プールから返される接続のデフォルトのトランザクション分離レベルを制御                                            | driver default |
| validationTimeout         | 接続が有効かどうかテストされる時間の最大値                                                         | 5000           |
| leakDetectionThreshold    | 接続漏れの可能性を示すメッセージが記録される前に、接続がプールから外れることができる時間の長さ                               | 0              |
| dataSource                | プールによってラップされるDataSourceのインスタンス                                                | なし             |
| schema                    | スキーマの概念をサポートするデータベースのデフォルト・スキーマ                                               | driver default |
| threadFactory             | プールで使用されるすべてのスレッドを作成するために使用される java.util.concurrent.ThreadFactory のインスタンス     | なし             |
| scheduledExecutor         | 様々な内部スケジュールタスクに利用される java.util.concurrent.ScheduledExecutorService のインスタンスを設定 | なし             |
