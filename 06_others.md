OpenStack のサポートサービス
===========================

Ceilometer を使った Metering の実装について
---------------------

### Ceilometer を使った Openstack metering の使用例



### Ceilometer が供給する様々なデータ

以下のようなメータリング情報を収集可能。

* storage.objects

    オブジェクト数

* storage.objects.size

    総容量

* storage.objects.containers

    コンテナ数

* storage.objects.incoming.bytes

    アップロードバイト数

* storage.objects.outgoing.bytes

    ダウンロードバイト数

* storage.api.request

    API リクエスト数

### ユーザー作成を含むデータ収集のワークフロー

Horizon が提供するダッシュボード
--------------

### Horizon が提供するダッシュボードサービス

### Horizon が依存する各種サービス

Horizon は Apache などの Web サーバーのは以下で動作するので、単独のデーモンプロセスは存在しない。Horizon が停止するとダッシュボード機能が利用できなくなるが、各今ポーネネントの API を直接呼び出すコマンドラインツールによる操作には影響しない。

Heat を使ったオーケストレーション
------------

### Heat が提供するオーケストレーション機能

### Heat のテンプレートの役割

### Heat スタックで実施可能な操作

