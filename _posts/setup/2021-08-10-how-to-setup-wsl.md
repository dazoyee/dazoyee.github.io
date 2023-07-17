---
layout: post
title: "WSLのセットアップ"
categories: "setup"
---

WSLを使い始め方


## セットアップ

1. 次のコマンドを実行する
    ```
    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
    ```
2. WSL へ CentOSインストール

   次の個所から CentOS.zip をダウンロード  
   <https://github.com/wsldl-pg/CentWSL/releases>  
   CentOS7のほうがいいかも

   任意のディレクトリに移動する  
   管理者権限でCentOS.exe を実行する

   次の表示が出たら WSL で CentOS のインストール完了
   ```
   Installing...
   Installation Complete!
   Press any key to continue...
   ```

   再度 CentOS.exe を実行すると、ターミナル起動する

## Windows Terminal

1. 設定->プロファイル->新しいプロファイルを追加します
2. コマンドライン

- C:\Windows\System32\wsl.exe
