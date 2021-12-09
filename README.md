# docker base ELK stack

dockerベースで動作するELK Stackのサンプル

- E(Elasticsearch): 検索・分析エンジン
- L(Logstash): サーバサイドのデータ処理パイプライン
- K(Kibana): Elasticsearchのデータをチャートやグラフで可視化

以上で完成されるログ収集&見える化システムのこと

下のコマンドで5601 portでkibanaが立ち上がる

```
$ docker-compose up
```

## Elasticsearch

Elasticsearchはオープンソースの全文検索エンジン

大量のドキュメントから目的の単語を含むドキュメントを高速に抽出することができる

Elasticsearchを勉強すればKibanaでやれることが増える

## Logstash

Logstashはオープンソースのデータ処理パイプライン

データソースをスタッシュを取り込み、INPUTS(データ入力)、FILTERS(変換)、OUTPUTS(データ出力)のパイプラインで処理する。

input, filter, outputについてconfファイルに設定

### Fluendとの比較

FluendもLogstashと同じくLog Collectorで、各サーバにインストールしてログを収集、さらに各サーバで収集されたログを集約し、ストレージやデータベースに挿入するところまで行う

入力はinputとして分離され、outputもプラグインという形で分離することで、さまざまなアプリケーションのログを入力として受け取り、色々な出力先を選択できるようになっている

Logstashとの違いについては、結局このスライドがよくまとまっていて神
https://www.slideshare.net/td-nttcom/fluentd-vs-logstash-for-openstack-log-management

色々Fluendがイケてる部分があったりする

また、イベントルーティングの方法が違う

- Fluendは「タグ」を使用してイベントをルーティングする
- Logstashは「if-thenステートメント」の記述を通してイベントをルーティングする

グローバルでは、Logstashの方がやや一般的らしい

## kibana

Elasticsearchデータを可視化したり、Elastick Stackを制御するオープンソースのユーザインターフェース


kibanaは一般的に以下の順序でデータをビジュアライズする

- データの準備（DevTools/Management）
- 検索式を作る（Discover）
- 表示方法を決める（Visualize）
- 素早くみられるようにする（Dashboard）

### Discover

レコード１件づつの詳細をみることができるモード

実際にレコードの中身を参照する
1. index patternを作成・選択する
2. 時間の範囲を指定する
3. 検索窓で絞り込み条件を記入する（ElasticsearchのQuery String記述方式）
4. Saveで保存、Shareで再現できる共有Linkを作成

### Visualize

検索結果をいろいろなグラフで表示するモード

1. 作成するグラフの種類を選択
2. グラフ化するインデックスを選択する
3. X-Axis（Data Histogram）やY-Axis（Percentilesなど）の設定を行う
4. Saveでグラフを名前をつけて保存する

### Dashboard

Discover, Visualizeで作成したパーツを並べて表示するダッシュボード

1. Addを押して追加したいVisualizeかDiscoverを選択
2. 表示されたパーツの配置を変える
3. 検索窓、Filterで絞り込み条件を指定（Visualizeされたものからさらに絞り込むことができる）
4. SaveでDashboardを名前をつけて保存する


