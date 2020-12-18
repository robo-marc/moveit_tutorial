# ROS概要

<!-- TOC -->

- [ROS概要](#ros概要)
    - [ROSの歴史](#rosの歴史)
    - [ROSの情報](#rosの情報)
    - [DISTRO](#distro)
        - [現行Distro](#現行distro)
    - [ROS用語](#ros用語)
        - [グラフリソース名](#グラフリソース名)
    - [ROSビルドツール](#rosビルドツール)
        - [ROS用](#ros用)
        - [ROS2以降](#ros2以降)
    - [ROS起動ツール](#ros起動ツール)
        - [launch ファイル](#launch-ファイル)

<!-- /TOC -->

## ROSの歴史

- Willow Garage
  - 2007年設立のロボットベンチャー（米、Menlo Park）
  - 2014年事業停止
  - Scott Hassanが出資
    - googleの初期エンジンの作者の一人
  - ビジネスモデル
    - ソフト：ROS（無償）＋ハード：PR2 を販売
    - PR2を10台無償で大学などに配布
    - スピンアウト創出を狙う→多くのスピンアウトが出た
      - Suitable Technilogy (Beam)
      - Savioke
      - Unbounded Robotics
      - fetch
      - hiDOF
      - Industrial Perception, Inc.
      - Open Source Robotics Foundation → Open Robotics

## ROSの情報

ROSに関連する情報は以下の場所にまとまっています。

- ROS Wiki: [http://wiki.ros.org/ja](http://wiki.ros.org/ja)
  - 様々な情報がwiki上に集約されている
- ROS Discourse: [https://discourse.ros.org/](https://discourse.ros.org/)
  - 掲示板、メールでも読むことができる
- Qiita: [https://qiita.com/search?q=ROS](https://qiita.com/search?q=ROS)
  - 様々な人が解説、導入記録等をあげている。体系的ではないが、自分と同じトラブルの場合にはたいへん役に立つ。
- Slack: ROS Community, ROS Japan UG などがあります。

その他、検索すると様々な情報がでてきます。特に、エラーが起こった場合は、まず **エラーメッセージ** をそのまま検索窓に入れて検索してみましょう。



## DISTRO

DISTRO (=Distribution) とは配布パッケージのこと。

バージョンごとに名前がついている。(亀に関係がある)。

DISTRO nameは覚えておいた方がよい。

### 現行Distro

[http://wiki.ros.org/Distributions](http://wiki.ros.org/Distributions)
カメにまつわるコードネームがついており、頭文字はアルファベット順になっている。

- Kinetic (Kinetic-Kame)
  - Ubuntu 16.04対応版
  - 現行で一番古いが、まだユーザが多い
  - 2021年4月まで
- (Lunar Loggerhead)
  - 2019年5月サポート終了
- Melodic (Melodic Morenia (ビルマメダマガメ))
  - Ubuntu 18.04対応版
  - 次第にこちらに移りつつある
  - 2023年5月まで
- Noetic (Noetic Ninjemys (メイオラン科のカメ))
  - 推奨版
  - Ubuntu 20.04対応版
  - 2025年3月まで

## ROS用語

- ノード
  - モジュールプログラム、通常は実行可能な一つのプログラムとして、ソースおよびバイナリが提供される
- パッケージ
  - ノードや設定ファイル、コンパイル方法などをまとめたもの
- ワークスペース
  - パッケージを置く場所
  - 通常 /opt/ros/share 以下と ~/catkin_ws(名前は自由だがドキュメントなどではこの名前が多用される)  以下
- メッセージ
  - ノード間でやりとりするデータ
- トピック
  - ノード間でやりとりするデータのラベル、同じトピック名を持つ出力と入力はデータがやり取りされる
- マスター
  - ノードの参照やトピックを保持するネームサービス
  - システム全体で原則一つ→SPOF（Single Point of Failure）
  - 他のノードより先に起動しなければならない
- パラメータサーバ
  - ノードのパラメータを保持するデータベース
  - マスター内で動作、ノードからはXMLRPCでアクセス
- rosout
  - ノードに対してstdout, stderrのような役割を果たす

### グラフリソース名

- グラフリソース名
  - ノード名、トピック名、メッセージタイプ名、サービス名などに付与するユニークな名前
  - “/” から始まり、”/”で区切られて階層化される
  - /<name0>/<name1>/<name2>/…
- 相対名
  - デフォルトの名前空間が指定されている場合 <名前空間名>/<リソース名> で指定可能
  - 先頭に”/” がつかないことで区別される
  - 環境変数 ROS_NAMESPACE で指定したり、launchファイルで <node ns=“名前空間” />で指定することができる
- プライベート名
  - “~” から始まる名前、ノード内でプライベートな名前空間
- アノニマス名
  - 多数のノード名を自動でつけたい時などに使用
  - ros::init()の引数にinit_options::AnonymousName を指定することで、自動的にノードtype名＋時刻で一意な名前を自動でつけてくれる

## ROSビルドツール

ROSノードをビルドするツール。ROSは独自のビルド・パッケージツールを提供している。ROSの特徴の一つ。様々なソフトウェアを効率よく管理・ビルド・配布することができる。

### ROS用
- rosbuild
  - 廃止された最初のROS用ビルドツール
- catkin (ヤナギなどの尾状花序)
  - 現行のROS用ビルドツール

### ROS2以降
- ament (ヤナギなどの尾状花序)
  - 最初のROS2用ビルドツール
  - 現在は表向きは次のcolconが利用されているが、内部的にはamentが使われている模様
- colcon (collective construction)
  - ROS2用ビルドツール
  - メタビルドツール、内部的に用途ごとに適切なビルドツールを呼び出す
  - https://colcon.readthedocs.io/en/released/

## ROS起動ツール

ROSノード（群）を起動するツール。ROSは独自のビルド・パッケージツールを提供している。ROSの特徴の一つ。様々なソフトウェアを効率よく管理・ビルド・配布することができる。

- rosrun
  - ROSノード（1個）を起動するコマンド
- roslaunch
  - ROSノード群（複数）を起動するコマンド
  - roslaunch <package> <launch_file>

### launch ファイル

**launchファイル例**
```xml
<launch>
  <arg name=“mode” default=“true”>
  <group if=“$(arg mode)”>
    <node name=“インスタンス1” pkg=“パッケージ名” type=“ノード名” output=“出力先”/>     
  </group>
  <node name=“インスタンス名2” pkg=“パッケージ名” type=“ノード名” output=“出力先”>
      <remap from=“改名前トピック名” to=“改名後トピック名"/> 
      <param name="content" value=“hogehoge"/>
  </node>
</launch>
```

- XMLファイルとして記述
  - <launch>タグで囲む
- <node>タグ：起動するノードを指定
  - name: インスタンス名、同一ノード名でも別名をつけて複数起動させることができる
  - pkg: パッケージ名
  - type: ノード名
  - output: 出力先、screen で標準出力に出力される、複数のノードの出力を標準出力に出力可能
- <remap>タグ: トピック名を変更 from, toで改名前後のトピック名指定
- <arg>タグ：roslaunch起動時にとる引数を宣言可能
  - nameで引数名、defaultでデフォルト値を指定
- <group>タグ：グルーピング
  - If文でargの引数をとることができる
  - 上の例では、mode=falseを指定すると“インスタンス名1”のノードは起動しない
  - Ex. roslaunch <パッケージ名> <launchファイル>.launch mode:=false ← インスタンス名1を起動させない
- <param >タグ：パラメータ指定
  - コード内であらかじめ定義されているパラメータを起動時に指定する
  - Ex. contentという名前のパラメータ(string型) に “hogehoge” を代入

  







  
  

