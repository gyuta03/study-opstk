OpenStack と ストレージ
=======================

イメージ及び Glance の使い方
----------------------------

### イメージサービス Glance の操作と機能

* glance-api

    ユーザからのリクエストを受けつ受ける REST API のサーバー機能に加えて、実際にテンプレーtのイメージやスナップショットの保存、取り出し処理までを行う。

* glance-registry
    
    格納されたテンプレートイメージやスナップショットの情報をデータベースに記録する。

### イメージ作成プロセス

イメージを作成する際には以下の項目を指定する。

* image_name

    追加する新規イメージの名前を指定する。

* disk_format

    以下のディスク形式のいずれかを指定する。

    - raw

        構造化されていないディスク・イメージ形式。

    - qcow2

        QEMU エミュレータによってサポートされる、動的に拡張できて copy-on-write をサポートするディスク形式。

    - vmdk

        多くの一般的な仮想マシン・モニターによってサポートされる、VMware ハイパーバイザの内もう1つのディスク形式。

* container-format

    イメージのコンテナ形式を指定する。いかの形式が許可されている。

    - aki

        Amazon カーネルイメージ

    - ami

        Amazon RAMディスクイメージ

    - ari 

        Amazon マシンイメージ

    - bare 

        コンテナやメタデータを持たないイメージ

    - ovf

        OVF コンテナイメージ

* --is-public

    他のユーザーがイメージにアクセスできるかどうかを指定する。

* image_path

    追加するイメージの絶対パス。

### Glance に適用できるセキュリティ
### コンテナフォーマットの概念

* コンテナフォーマット

    VM イメージの保存用に Glance が使用する「封筒」。マシン状態, OS, ディスクサイズ等のメタデータに紐付けられる。

### Clance での Cinder や Swift などのバックエンドの使い方

ボリューム及び Cinder の使い方について
-------------------------------------

### Cinder の主な機能

仮想マシンに接続するブロックボリュームを提供することが Cinder の役割。プロセスの構造が Nova に似ている。
Cinder は以下のプロセスを持つ。

* cinder-api

   Cinder に対する各種リクエストを受け付ける API を提供する。

* cinder-scheduler

    新規のブロックボリュームを選択する際に、Cinder が管理するストレージ装置の中から、どれを使用するかを決定するプロセス。Nova のスケジューラと同様に「フィルター」と「重み」で使用するストレージを決定する。

* cinder-volume

    管理対象のストレージ装置に対する実際の制御を行うプロセス。cinder-volume が停止した場合は新規ボリューの作成ができなくなる。

* cinder-backup

    Cinder のバックアップ機能を提供するプロセス。Grizzly 以降では Cinder が作成したブロックボリュームを Swift にバックアップすることも可能。

### ブロックストレージの利点

インスタンスが終了したあとも永続的にデータを保管するために必要。

### Cinder アーキテクチャと拡張性
### ボリュームタイプと追加仕様の使い方
### スナップショットとバックアップの操作と使い方

オブジェクトストレージ及び Swift の使い方
------------

### オブジェクトストレージの使用事例
### Swift の主要コンポーネント

* サーバー

    - プロキシ・サーバー

        ユーザーからのアクセス窓口。コンテナ作成, ファイルアップロード, メタデータ変更等のリクエストを受け取ると、リング内でのアカウント, コンテナ, またはオブジェクトの位置を判別して、該当するサーバーに転送します。

    - オブジェクト・サーバー

        オブジェクトの変更、取得を実行するサーバでオブジェクトの実体が保存される。オブジェクトには、オブジェクト名のハッシュとタイムスタンプに基づいたパスがしてされる。
    
    - コンテナ・サーバー

        オブジェクトのディレクトリで特定のコンテナへの異オブジェクト割当処理を行い、リクエストに応じてコンテナのリストをて提供する。

    - アカウント・サーバー

        オブジェクトストレージサービスを使用してアカウントを管理する。動作はリストを提供するという点でコンテナサーバーと同様。

* プロセス

    - レプリケーションサービス

* リング

    物理的な位置のマッピング情報

### アカウント, コンテナ及びオブジェクトの配布メカニズム

ユーザーはプロキシサーバーを経由して、コンテナ情報やコンテナ内のオブジェクトの実体にアクセスする。プロキシサーバーにアクセスする際には kyestone によるユーザー認証が行われる。アカウント, コンテナ, オブジェクトは分散配置される。実際には3種類のファイルはディレクトリを分けるように同一サーバー内で構成する。

### Swift で使用できる主な管理ツール
### Swift でのリージョンとゾーンの使い方

オブジェクトのレプリカは必ず異なるリージョン, ゾーンを満たす場所に格納される。つまり、ゾーンやリージョンを増やすまたは減らしてサーバの数を変更するとリングの内容も変更される。

* ゾーン

    各デバイスが特定のゾーンに配置される。

* リージョン

   ゾーンの上の概念で複数のゾーンをまとめる。各ゾーンはリージョンごとに定義される。
