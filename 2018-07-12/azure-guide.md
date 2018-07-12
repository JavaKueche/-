# やること
Azure実践ガイド


# 第2章のサンプルアプリの構築

##  基本構成
ロードバランサー 1 
     /    \
    VM     VM
     \     /
    SQL Server

## ResourceGroupの作成
ResourceGroupというのはAzureの各リソースを論理的な単位でグルーピングするためのもの。
ResourceGroupを作ってその中にサービスを構築することであとで参照したり、すべてのサービスを一斉に削除したりしやすくなっている。

## Virtual Networkの作成
そのあとVirtual Network(仮想ネットワーク、VNet)の構築を行った。
仮想ネットワークはAzure内に作る専用のプライベートネットワークで、他のユーザーとは論理的に分離されたネットワーク環境が構築できる。
VNetにはアドレス空間(例: 10.0.0.0/16)、サブネット名(例: web)とサブネットのアドレス範囲(例: 10.0.0.0/24)を設定して構築できる。
VNetのアドレス空間の範囲であれば、サブネットを複数作ることもできる。

## Network Security Groupの作成
そのあとネットワークセキュリティグループ(NSG)の構築を行った。
NSGはVnetに接続されたリソースへのネットワークトラフィックを許可または拒否するセキュリティ規則で、サブネットと仮想マシンのネットワークインターファイス(NIC)に設定できます。
ひとつのNSGを別々のサブネットやNICに接続することも可能です。

## 可用性セットの作成
可用性セットの構築をおこなった。
可用性セットとは、Azureで仮想マシンの冗長構成を取るための論理的なグループです。
可用性セットを使って複数のVMをグルーピングすると、その複数のVMは電源やネットワーク機器などを共有する範囲(障害ドメイン)を分散して配置されるようになります。
またホストのパッチ適用などのメンテナンスが可用性セット内のマシンで同時に実行されないようになります(更新ドメイン)

## Azure Load Balancerの作成
Azure Load Balancerの作成を行った
Azureにはロードバランサーのサービスが複数あります。
- Azure Load Balancer(L4までの負荷分散)
- Application Gateway(L7層での負荷分散、WAFの機能やパスによるルーティング機能もあり)
- Traffic Manager(DNSレベルでの広域負荷分散)

今回はAzure Load Balancerを利用した。

LoadBalancerの設定には以下の項目が必要
- バックエンドプールの作成
- 正常性プルーフの作成
- 負荷分散規則の作成

### バックエンドプールの作成
バックエンドプールとは、ロードバランサーの配下に設定するリソースのこと。
設定は以下の単位で設定できる。
- 単一のVM単位、
- 可用性セットの単位
- 仮想マシンのスケールセットの単位
今回は可用性セットの単位で設定した。

### 正常性プルーフの作成
正常性プルーフとは、パックエンドプールが正常に動いているか検証するための仕組みのことです。
TCPでの接続での検証、または HTTPでリクエストのパスを指定してそれが200を返すかで検証することができます。
正常性プルーフによって正常でないと判断されるとそのバックエンドプールにトラフィックが流れないようになります。

### 負荷分散規則の作成
負荷分散規則とは、VMに対するトラフィックの分散方法を定義するための仕組みです。
着信トラフィック用のフロントエンドIPとトラフィックを受信するためのバックエンドIPプールを必要な発信元ポートと宛先ポートともに定義します。
ここで先程作成したバックエンドプールの設定(どこにトラフィックを流すか)や正常性プルーフも設定(正常性を判断するルールはなにか)します。

### SQL Serverの作成
SQL Serverは普通にサービスつくっただけです。

# 仮想マシンについて
Azureの仮想マシンはHyper-Vを採用している。
物理マシンをホスト、その上で動く仮想マシンのことをホストと呼ぶ。

仮想マシンには様々なOSとその種類(シリーズ)がある。

## インスタンスシリーズ
Linuxの仮想マシンのシリーズには以下のような特徴がある。

|インスタンスタイプ|説明|用途|
|---|---|---|
|Aシリーズ|最も入門レベル|開発環境や小規模Webサーバー| 
|Dシリーズ|AシリーズよりCPUが60%程度高速、一時ディスクがSSD|汎用WebサーバーorDB|
|Dv2シリーズ|DシリーズよりCPUが35%程度高速|汎用Webサーバー or DB|
|Gシリーズ|大規模メモリを搭載|大規模キャッシュ、RDB，インメモリDBなど|
|Fシリーズ|CPUはDv2シリーズと同等で対メモリのCPU比が高い| Webサーバ、ネットワークアプライアンス|

## VMのディスク選択
2種類ある。
- Standard
- Premium

StandardディスクはHDD。
PremiumディスクはSSD。

ディスクはVMに追加することも可能。(VMのサイズやシリーズよって追加できるディスクの数が違う)

## 管理ディスク(Managed Disk)
Azureによって管理、抽象化されたディスク。
いままではディスクを使うためにBLOBストレージを操作するためのストレージアカウントを作って紐づけたり、性能、容量の設計をする必要があったがManaged Diskにするとその手間がなくなる。
特別な理由がない限りは基本Managed Diskにするべき。

Managed Diskはローカル冗長ストレージ(同日のデータセンター内で3つのコピーが作られる。)がサポートされている。
MDを使用している複数のVMを可用性セットに含めることで障害ドメイン・更新ドメインが分散されて可用性が向上する。