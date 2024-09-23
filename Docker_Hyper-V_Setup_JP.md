# Hyper-V仮想マシンでDockerを動かす方法

Created on September 23, 2024  
Copyright (c) 2024 led-mirage  

## はじめに

Hyper-V仮想マシンでDockerを動かすには、単に仮想マシン側にDockerをインストールするだけでなく、ホスト側でも設定を行う必要があります。この資料ではその方法について記載します。なお、Hyper-Vの仮想マシンの作成方法についてはこの資料では言及しません。

## 検証環境

- ホスト側：Windows 11 Pro 23H2
    - Hyper-V：有効
    - Windowsハイパーバイザープラットフォーム：無効
    - 仮想マシンプラットフォーム：無効
- ゲスト側：Windows 11 Pro 23H2
    - Hyper-V：無効
    - Windowsハイパーバイザープラットフォーム：無効
    - 仮想マシンプラットフォーム：有効
    - Docker Desktop 4.33.1 (WSL2上で実行)

## ホスト側の設定

仮想マシン上でDockerを動かすためには、ホスト側でNested Virtualization（入れ子の仮想化機能）を有効にしておく必要があります。有効化するには、ホスト側でPowerShellを管理者モードで起動し、以下のコマンドを実行します（仮想マシンの名前は、Hyper-Vマネージャーに表示されている仮想マシン名に置き換えてください。Windowsのコンピューター名ではないことに注意してください）。

```powershell
Set-VMProcessor -VMName "仮想マシンの名前" -ExposeVirtualizationExtensions $true
```

設定を確認するには以下のコマンドを実行します。`True`と表示されればNested Virtualizationが有効になっています。

```powershell
(Get-VMProcessor -VMName "仮想マシンの名前").ExposeVirtualizationExtensions
```

ホスト側の設定は以上です。

## Docker Desktopのインストール

DockerのWebサイト（ https://www.docker.com/ja-jp/ ）にアクセスし、Docker Desktopをダウンロードします。

インストールは特に難しいところはありません。表示される案内に従ってインストールすればOKです。

`Use WSL2 instead of Hyper-V(recommended)`の項目はデフォルトでONになっていますので、そのままインストールしましょう。DockerはWSL2上で動くモードと、Hyper-V上で動くモードの2種類の動作モードがありますが、WSL2の方が軽量で高速に動作します。このオプションをONにしておくと、WSL2が未インストールの場合はDockerが自動的にインストールしてくれます。

インストール中にDockerのアカウントの作成を促されますが、基本的なことをするだけなら作成しなくてもOKです。Dockerの商用利用を考えている人や、Docker Hubに自分のイメージをアップする場合などはアカウントが必要になるので、アカウントを作成してください。

## Dockerの動作確認

### バージョン番号の確認

PowerShellやコマンドプロンプトで、以下のコマンドを実行します。

```powershell
docker --version
```

```
Docker version 27.1.1, build 6312585
```
などと表示されればOKです。ここではDocker Engineのバージョン番号が表示されます。

### Hello Worldコンテナの実行

PowerShellやコマンドプロンプトで、以下のコマンドを実行します。

```powershell
docker run hello-world
```

画面に以下のように表示されればOKです。このコマンドを実行すると、Docker Hubからhello-worldイメージを取得し、そのイメージからコンテナを作成し実行します。

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

（後略...）
```

## このあと

ここまでくればDockerは正常に動作していますので、あとは好きなイメージをプルして使うだけです。Dockerの使い方に関しては、ネット上にたくさん情報があると思いますので、それらを参照してください。

