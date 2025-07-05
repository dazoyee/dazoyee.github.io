---
layout: post
title: "Package Manager"
categories: "cheatsheet"
---

Scoop と Windows 標準パッケージマネージャー winget を併記し、用途に応じて使い分けるチートシート

## URL

- Scoop: <https://scoop.sh/>  
- winget: <https://learn.microsoft.com/windows/package-manager/winget/>

## Set Up

### Scoop

```powershell
# 権限変更
Set-ExecutionPolicy RemoteSigned -scope CurrentUser

# インストール
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
````

### winget

```powershell
# Windows 10 1809 以降なら App Installer 更新だけで有効化
# 必要なら GitHub から MSIXBundle をダウンロードして手動インストール
Invoke-WebRequest `
  -Uri "https://github.com/microsoft/winget-cli/releases/latest/download/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle" `
  -OutFile "$env:TEMP\AppInstaller.msixbundle"
Add-AppxPackage -Path "$env:TEMP\AppInstaller.msixbundle"
```

---

## Install

### Scoop （CLI／軽量ユーティリティ／自作バケット向け）

```powershell
scoop install curl
scoop install git
# scoop install apache
scoop install maven
# scoop install mariadb
# scoop hold mariadb
scoop install nodejs
# scoop install nssm
# scoop install php
# scoop hold php
scoop install sudo
scoop install busybox
# scoop install docker
# scoop install concourse-fly
scoop install concourse-fly@6.7.1
# scoop install azure-cli
# scoop install jenkins
# scoop install prometheus
# scoop install nginx
# scoop install winsw
scoop install telnet
# scoop install scala

scoop bucket add java
scoop install openjdk8-redhat
scoop install openjdk11
scoop install openjdk17
scoop install openjdk21
scoop install visualvm

scoop bucket add extras
# scoop install tomcat
scoop install jetbrains-toolbox
# scoop install sourcetree
# scoop install teraterm
# scoop install winmerge
# scoop install winscp
# scoop install windows-terminal
# scoop install slack
# scoop install zoom
scoop install jmeter
# scoop install firefox
# scoop install springboot
# scoop install cloudfoundry-cli
scoop install cloudfoundry-cli@6.53.0
# scoop install vscode
# scoop install elasticsearch
# scoop install notion
# scoop install nexus-repository-oss
# scoop install grafana
scoop install jenv

scoop bucket add nerd-fonts
sudo scoop install monoid-NF

scoop bucket add iyokan-jp https://github.com/tetradice/scoop-iyokan-jp
scoop install sakura-editor

# scoop bucket add scoop-bucket-ioridazo https://github.com/ioridazo/scoop-bucket-ioridazo
# scoop install scoop-bucket-ioridazo/lhaplus
# scoop install scoop-bucket-ioridazo/adminer
# scoop install scoop-bucket-ioridazo/VC_redist.x64
# scoop install scoop-bucket-ioridazo/zipkin
# scoop install scoop-bucket-ioridazo/kibana
# scoop install scoop-bucket-ioridazo/elastic-agent
# scoop install scoop-bucket-ioridazo/logstash

scoop bucket add shibadog https://github.com/shibadog/my-bucket2
scoop install gcviewer
scoop install samurai
# scoop install ytt
```

### winget （公式GUI／システム統合／Microsoft系向け）

```powershell
winget install --id Google.GoogleDrive # Google Drive
winget install --id Google.ChromeRemoteDesktopHost # Chrome Remote Desktop Host

winget install --id Microsoft.VisualStudioCode # VS Code
winget install --id WinMerge.WinMerge # WinMerge
winget install --id Winscp.WinSCP # WinSCP

winget install --id SlackTechnologies.Slack # Slack
winget install --id Zoom.Zoom # Zoom

winget install --id Docker.DockerDesktop # Docker Desktop

winget install --id 9NSBB9XTJW86 # A5:SQL Mk-2 (x64)

```

---

## Update / Upgrade

### Scoop

```powershell
scoop update # scoop
scoop update * # application
```

### winget

```powershell
winget upgrade --id Microsoft.Winget.Source # winget 本体ソース更新
winget upgrade --all # 全アップグレード
```

---

## Reset / Uninstall

### Scoop

```powershell
scoop reset concourse-fly
scoop reset concourse-fly@6.7.1 # バージョンの切り替え
scoop uninstall mariadb
```

### winget

```powershell
winget uninstall --id MariaDB.Server
winget uninstall --id Concourse.Fly
winget install --id Concourse.Fly --version 6.7.1
```

---

## Hold / Pin

* **Scoop**: `scoop hold <package>` で固定可能
* **winget**: 現状内蔵機能なし。明示的バージョン指定運用を推奨

---

## Cache / Cleanup

### Scoop

```powershell
# インストールするための元ファイルを検索、削除
scoop cache show
scoop cache rm *

# 使用していないバージョンのインストール済みScoopアプリを削除
scoop cleanup *
```

### winget

* キャッシュは自動管理
* 不要ソース削除:

  ```powershell
  winget source remove -n extras
  ```

---

## Troubleshooting

### Scoop

```
$ scoop update
Updating Scoop...
Updating 'extras' bucket...
error: Your local changes to the following files would be overwritten by merge:
        scripts/everything/install-context.reg
        scripts/everything/uninstall-context.reg
```

> everythingパッケージなどに関連するレジストリファイルが、そのファイルの扱いでしくじったせいで scoop update などに支障が出るようになった。

```
scoop bucket rm extra
scoop bucket add extra
```

[Scoop、レジストリファイルの扱いのせいで地獄を見る](https://zenn.dev/akaregi/articles/bc7017b2c871a7)
