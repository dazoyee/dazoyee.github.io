---
layout: post
title: "スレッドダンプ"
categories: "study"
---

スレッドダンプの取り方


1. JavaのプロセスID取得
    ```
    $JAVA_HOME/bin/jps -v
    ```
2. スレッドダンプ取得
    - 実行中のスレッドの状態を取得する（スタックトレース）
        ```
        // linux
        $JAVA_HOME/bin/jstack -F 67890 > threaddump.txt
        $JAVA_HOME/bin/jstack -F 67890 > threaddump.$date +"%Y%m%d%I%M%S"
        
        // ps
        jcmd 152264 Thread.print > threaddump.$(Get-Date -Format "yyyyMMddHHmmss")
        
        // cmd
        jcmd 152264 Thread.print > threaddump.%date:/=%%time:~0,2%%time:~3,2%%time:~6,2%
        ```

    - 1秒毎にスレッドの状態を取得する
        ```
        count=1;while true;do date;sleep 1;$JAVA_HOME/bin/jstack 42252 > threaddump.$count;let ++count; done
        ```
      数十秒間取得できればよい

3. 解析ツール
    - https://samuraism.jp/samurai/ja/index.html
    - 見方については随時更新する
