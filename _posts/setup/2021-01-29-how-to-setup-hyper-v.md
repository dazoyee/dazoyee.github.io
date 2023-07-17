---
layout: post
title: "Hyper-vのセットアップ"
categories: "setup"
---

Hyper-vを使い始め方


インストール
---


※再起動します

1. 管理者として PowerShell コンソールを開く
2. 次のコマンドを実行する
    ```
    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
    ```
3. [設定] で Hyper-V ロールを有効にする

- コントロール パネル\プログラム\プログラムと機能

1. Windows ボタンを右クリックし、[アプリと機能] を選択する
2. 右側の関連する設定にある [プログラムと機能] を選択する
3. [Windows の機能の有効化または無効化] を選択する
4. [Hyper-V] を選択して、 [OK] をクリックする

CentOS
---

[Hyper-VにCentOS7をインストール](https://qiita.com/gate9/items/6a179f4ef5c2fcee2ae5)

Support
---

[Hyper-V 仮想マシンの移行の開始またはライブ マイグレーションが失敗し、エラー 0x80070569 が表示される場合があります。](https://docs.microsoft.com/ja-jp/troubleshoot/windows-server/virtualization/starting-or-live-migrating-hyper-v-vms-fails)

このコマンドを打つことで、アカウントポリシーが更新される  
（もしかしたらHyper-v上でこのコマンドを打つ必要がある？）

```
gpupdate /force
```

CentOSまわりのネットワーク設定も記載の通りに実施
> IPアドレス、ネットマスク、ゲートウェイ、DNSサーバー

-> 192.168.100.169とかにする
