---
layout: post
title: "dockerのトラブルシューティング"
categories: "cheatsheet"
---

dockerを使ったときのトラブル対処の覚え書き


### 前提

下記サイトに沿ってセットアップする  
<https://ops.jig-saw.com/tech-cate/docker-for-windows-install>


Failed to create or configure Hyper-V VM: シーケンスに要素が含まれていません.
---

これがでたとき。。

> An error occurred  
> Failed to create or configure Hyper-V VM: シーケンスに要素が含まれていません.
>
> One of the most common reasons is virtualization features not working properly,
> see <https://docs.docker.com/docker-for-windows/troubleshoot/#virtualization>.

![An error occurred](https://user-images.githubusercontent.com/20193699/94943881-645f9500-04a6-11eb-8927-4ff091923f57.png)

以下の設定にて確認するとできた

![](https://docs.docker.com/docker-for-windows/images/wsl2-enabled.png)
![](https://docs.docker.com/docker-for-windows/images/hyperv-enabled.png)

“0x80070569 ログオン失敗:要求された種類のログオンは、このコンピューターではユーザーに許可されていません。”
---

これがでたとき。。

![An unexpected error occurred](https://user-images.githubusercontent.com/40160582/147994418-e0e6abb9-f336-47a5-a46e-d7becd5c0780.PNG)

powershellで`gpupdate /force`をたたく

[Hyper-V 仮想マシンの移行を開始またはライブで実行すると、エラーが発生して失敗0x80070569](https://docs.microsoft.com/ja-jp/troubleshoot/windows-server/virtualization/starting-or-live-migrating-hyper-v-vms-fails)

（windowsアップデートにより変わってしまうっぽい）
