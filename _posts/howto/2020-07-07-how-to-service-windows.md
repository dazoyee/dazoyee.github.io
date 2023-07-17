---
layout: post
title: "Windowsのサービス化"
categories: "howto"
---

Windowsにおいてサービス登録するためにすること


Windowsのサービスはjavaのサポートがないため、自らjarをwrapしなければならない

## 手順（NSSM編）

### 1. NSSMのダウンロード

NSSM(the Non-Sucking Service Manager)

ここからダウンロードする

<http://nssm.cc/download/?page=download>

インストールしたらPathを登録してもいいかもしれない  
ex) `C:\nssm-2.24\win64`

### 2. ディレクトリ作成

任意のディレクトリを作成する。階層に決まりはないが、今回は以下のとおりにする

- C/app
    - ├ bin/
    - └ (java/jdk)

javaは必要に応じて

### 3. batファイル作成

- start.bat

```text
@ECHO OFF
REM start.bat  code
java -jar  app(対象のjarファイル).jar
```

以下ファイルは必要に応じて。

- install.bat

```text
nssm install app(サービス化するアプリケーションの名前)
```

- uninstall.bat

```text
nssm remove app(サービス化するアプリケーションの名前)
```

ディレクトリは以下のようになる

- C/app
    - ├ bin/
        - ├ install.bat
        - ├ start.bat
        - ├ uninstall.bat
        - ├ nssm.exe(Path指定しない場合はここに.exeをもってくる)
        - └ app.jar
    - └ (java/)

### 4. install.batを実行

すると、  
![nssm-1.png](/img/posts/howto/nssm-1.png)

- GUIが立ち上がるので、上述の`start.bat`をファイル選択ダイアログで指定
- [Details]タブにDisplay nameと説明を適当に記述
- [Install service]ボタンを押すとサービスとして登録

サービスをアンインストールするときは、`uninstall.bat`

### 参考記事

[Windowsプログラムのサービス化](https://www.torutk.com/projects/swe/wiki/Windows%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E5%8C%96)

---

## 手順（java service wrapper編）

### 1．java service wrapperのダウンロード

<https://wrapper.tanukisoftware.com/doc/japanese/download.jsp>

ここからダウンロードする

### 2. ディレクトリ作成

任意のディレクトリを作成する。階層は以下のとおり

- C/
    - ├ bin/
    - ├ conf/
    - ├ lib/
    - └ log/

### 3．wrapperの必要なファイルのコピー

1.でダウンロードしたzipを展開し、必要なファイルを以下の通りコピーする。

- `bin/wrapper.exe` → `C/app/bin`
- `lib/wrapper.jar` → `C/app/lib`
- `lib/wrapper.dll` → `C/app/lib`
- `src/conf/wrapper.conf.in` → `C/app/conf/wrapper.conf`
- `src/conf/wrapper-license-time.conf` → `C/app/conf/wrapper-license.conf`
- `src/bin/App.bat.in` → `C/app/bin/App.bat`
- `src/bin/InstallApp-NT.bat.in` → `C/app/bin/InstallApp-NT.bat`
- `src/bin/UninstallApp-NT.bat.in` → `C/app/bin/UninstallApp-NT.bat`

ディレクトリは以下のようになる

- C/app
    - ├ bin/
        - ├ App.bat
        - ├ InstallApp-NT.bat
        - ├ UninstallApp-NT.bat
        - ├ wrapper.exe
        - └ app.jar
    - ├ conf/
        - ├ wrapper.conf
        - └ wrapper-license.conf
    - ├ lib/
        - ├ wrapper.dll
        - └ wrapper.jar
    - └ log/

### 4. wrapper.confの修正

#### javaの設定

自らのjavaバージョンや設定によってどれにするか選ぶ必要がある。

- `42 wrapper.java.command=java`
- `45 wrapper.java.command=%JAVA_HOME%/bin/java`
- (追加) `wrapper.java.command=C:\app\java\jdk-11.0.3\bin\java.exe`  
  作成したディレクトリに使用するjavaを用意する場合は上記のように追加する。

（いずれかひとつ）

#### wrapperメインクラスの変更

- `56 wrapper.java.mainclass=org.tanukisoftware.wrapper.WrapperJarApp`

#### クラスパスの追加

- `61 wrapper.java.classpath.2=app.jar`

#### エントリクラスの設定

- `79 wrapper.app.parameter.1=C:\app\bin\app.jar`

#### サービス名の設定

- `125 wrapper.console.title=任意の名前`
- `193 wrapper.name=任意の名前`
- `196 wrapper.displayname=任意の名前`
- `199 wrapper.description=任意の名前`

### 5. 動作確認

bin/App.batを実行  
出力はlogs/wrapper.logに吐かれる

### 6. サービス化と解除

bin/InstallApp-NT.batを実行  
サービス化の解除はbin/UninstallApp-NT.batを実行すればOK
