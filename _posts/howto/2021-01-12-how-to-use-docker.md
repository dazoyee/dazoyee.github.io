---
layout: post
title: "docker 取扱説明書"
categories: "howto"
---

dockerを用いたセットアップをまとめておく


コマンド
---

- docker-compose.yml の起動

```
docker-compose up -d
docker-compose restart
docker-compose stop
docker-compose down
docker-compose logs
```

- spring-boot アプリを docker で起動
    - 対象アプリディレクトリで

  ```
  mvn spring-boot:build-image
  docker run -it --rm -d --name [name] --network [name] --expose [port] [image name]
  ```

    - また、こんなのも

  ```
  docker run --rm `
      --privileged `
      -d `
      --name wallet-meisai-reference `
      --network docker_default `
      -v "$(Get-Location)/target/wallet-meisai-reference-linux.tar.gz:/tmp/app.tar.gz" `
      -v "$(Get-Location)/dev/script/setup.sh:/tmp/setup.sh" `
      -v "$(Get-Location)/dev/script/start-wallet-meisai-reference-test.sh:/tmp/start-wallet-meisai-reference-test.sh" `
      -p "17091:17091" `
      -p "18082:18082" `
      -p "18092:18092" `
      -e "APP_NES_CLIENT_BASE_URL=http://arc-tester:8099/nes" `
      centos:7 /sbin/init
  ```

    - そのあとに下記で linux 環境を操作できる

  ```
  docker exec -it wallet-meisai-reference /bin/bash
  ```

- 資材のコピー
    - ホスト -> コンテナ
      ```
      docker cp <資材元> centos7-local:/tmp
      ```
    - コンテナ -> ホスト
      ```
      docker cp centos7-local:<資材元> C:\Users\user\Desktop
      ```

docker 知識
---

### docker network

コンテナ間通信をする際のネットワークを指定できる。  
なにも指定しないと`docker_default`となるらしい

```
docker network create [name]
docker network ls
```

これでネットワーク一覧がわかる

### CREATED で止まってしまったとき

```
docker ps -a
docker start { コンテナID }

or

-v オプションをつけることでvolumeも削除できる
docker-compose down -v

or

volumeを直接削除する
```

CentOS7
---

```
docker pull centos:centos7
docker images
docker run -it -d --name centos7-local --restart=always centos:centos7
docker exec -it centos7-local /bin/bash
```

[【初心者向け】Docker で CentOS7 の環境を構築しよう！](https://www.sejuku.net/blog/86137)

- java 構築

    1. ダウンロード

    - <https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz>

    2. SCP
       ```
       docker cp C:\Users\user\Downloads\openjdk-11.0.2_linux-x64_bin.tar.gz centos7-local:/tmp
       ```
    3. 解凍
    ```
    tar zxf openjdk-11.0.2_linux-x64_bin.tar.gz -C /apps
    ```

  or

    1. `yum search java-11`
    2. `yum install java-11-openjdk` or `yum install java-1.8.0-openjdk`
    3. `java -version`
    4. `update-alternatives --config java`

RabbitMQ
---

```
docker run --name rabbitmq-local --restart=always -p 15672:15672 -p 5672:5672 rabbitmq:3-management
```

<https://github.com/Pivotal-Japan/spring-cloud-stream-tutorial>

zipkin
---

```
docker run --name zipkin-local --restart=always -d -p 9411:9411 openzipkin/zipkin
```

Nexus
---

```
docker run --name nexus-local --restart=always -d -p 8081:8081 sonatype/nexus3
```

Prometheus
---

- docker-compose.yml

  ```
  version: '3'
  services:
  prometheus:
      image: prom/prometheus
      container_name: prometheus_local
      volumes:
      # ローカルの./prometheusフォルダをdockerのLinux上/etc/prometheusに渡す
      - ./etc/prometheus:/etc/prometheus
      command: "--config.file=/etc/prometheus/prometheus.yml"
      ports:
      - 9090:9090
      restart: always

  ```

- prometheus.yml

  ```
  scrape_configs:
    - job_name: 'spring'
      # prometheusの依存を入れることでアクチュエータに/prometheusが加わる
      metrics_path: '/actuator/prometheus'

      static_configs:
      # 実行ホスト名を指定しないと動かないヨ
      # ここがたかれる先のポート（ローカルにつなぐ場合はルーターのIPとなるipconfig）
      - targets: ['172.18.31.225:18092']
  ```

Grafana
---

- docker-compose.yml

  ```
  version: '3'
  services:
  grafana:
      image: grafana/grafana
      container_name: grafana_local
      ports:
      - 3000:3000
      restart: always
  ```

- 初期設定

    1. Configuration > Data Sources > Add Data Source
    2. URL: <http://host.docker.internal:9090>
    3. Save
    4. Dashboards > Manage > import
    5. <https://grafana.com/grafana/dashboards/4701>
    6. <https://grafana.com/grafana/dashboards/11378>

    - このサイトでいろんなダッシュボードが用意されており、好みをインポートできる。
    - Copy ID to Clipboard

    7. Load

    - シェルをたたくときは以下
      ```
      docker-compose exec prometheus /bin/sh
      ```

gitbucket
---

```
docker run --name gitbucket-local -d -p 8080:8080 gitbucket/gitbucket
```

<http://localhost:8080>
user:root
password:root

wiremock
---

```
docker pull rodolpheche/wiremock
docker run -it --rm --name wiremock-local -p 9928:8080 --restart=always rodolpheche/wiremock
```
