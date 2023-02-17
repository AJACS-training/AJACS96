## BLAT、In-Sillico PCRを使って配列を検索する/Gene Sorterを使って発現データを解析する

###  1. UCSC Genome Browserの準備

- Genomeのバージョンを確認する
- Sessionsを使って設定を保存、共有する

####  Genomeのバージョンを確認する

- UCSC genome browserにアクセスする
  - https://genome-asia.ucsc.edu/
- ”Genomes”のタブ（もしくはそのタブの中にある**Others**）をクリックする

![スクリーンショット 2023-01-10 17 04 59](https://user-images.githubusercontent.com/296176/217182529-20435d40-110d-4543-b8db-42e4693a5642.png)

- **ゲートウェイページ**が表示されます。**"Assembly"**を変更することで、UCSC Genome Browser（以後、UCSC GB）が保持しているさまざまなバージョンのゲノムの情報についての**細かい情報**を確認することができます。たとえば、その**ゲノムバージョンでの大きな変更点をハイライトとして記載**してあり、ゲノムバージョン間の違いを理解するのに役立ちます。UCSC GBは、各種ゲノムプロジェクトが行なったGenome Assemblyのデータを取得して、ブラウザ上で可視化しているので、その元となっている情報が存在することにご留意いただきたいと思います。

![スクリーンショット 2023-01-10 17 15 20](https://user-images.githubusercontent.com/296176/217183476-47098849-f209-4e10-a63f-149c22006a26.png)

一方で最新ゲノムの一つであるhs1を眺めてみるとまだ情報が整備されていない様子も分かります。

![スクリーンショット 2023-01-10 17 21 16](https://user-images.githubusercontent.com/296176/217183499-0c69ea70-ca65-42a9-83d3-9257bf8ed899.png)

次に、**Genome**タブの中にある**Genome Archive GenArk**をクリックすると、UCSC GBが持つ千を超えるゲノムアッセンブリーが種別にリストアップされます。hub gatewayの生物カテゴリーをクリックして各種のゲノムを選ぶとゲノム地図を閲覧することができます。登録されているゲノムをリストで眺めたい時にはこの手段がよいと思います。

![スクリーンショット 2023-01-10 17 31 27](https://user-images.githubusercontent.com/296176/217183523-f7bab076-c3ea-4568-9b21-633096594a67.png)

#### Sessionsを使って設定を保存、共有する

UCSC genome browserにはユーザー別にセッションを残す機能が存在します。**My DataからMy Sessions**をクリックしてください。

![スクリーンショット 2023-01-10 17 42 24](https://user-images.githubusercontent.com/296176/217183527-f42df96a-0e6d-455a-bd22-783dec413e78.png)

My Sessionsでは、アカウントを作成することで、ゲノム地図上で調べたトラックの情報やセッティングを保存し、さらには他のユーザーと共有、非共有することができます。今回私が別のアカウントを作り試したところ、アカウントの登録は自動メッセージですぐに到着しましたので、ぜひこの機会にアカウントを作ってみましょう！

![スクリーンショット 2023-01-10 17 47 03](https://user-images.githubusercontent.com/296176/217183530-99d4bfdb-2a06-4990-af1e-ccf841a090b7.png)

各種情報を入力してsign upするとメールが送られてきますので、メールに記載されたURLでアカウント登録を完了してください。

**さ す れ ば そ な たは UCSC GB に 歓 迎 さ れ る で あ ろ う 。**

![スクリーンショット 2023-01-10 18 14 46](https://user-images.githubusercontent.com/296176/217183532-d6cdf864-887c-43a5-86fc-724d85ab4ad2.png)

さて、MySessionsでは、ユーザーがすでに開いた**トラックなどの情報の保存**や、**復元**ができる設定画面が用意されています。必要に応じて履歴のセッションに名前をつけてsubmitして保存しておくと、後でもう一度同じ地図を見たい時に便利です。折角なのでこの機能を使ってみたいと思います。今回は例としてブラウザで閲覧した情報を保存、そして読み込んでみたいと思います。赤矢印の**Public Session**をクリックしてください。

![スクリーンショット 2023-01-10 18 20 23](https://user-images.githubusercontent.com/296176/217183536-da7d9fa3-ea26-4be9-98ec-f4d1b6574642.png)

Public Sessionは、他のユーザーが登録した共有セッションを閲覧することができます。好みの順にソートして、ぜひ眺めてみてください。セッティングの参考になるものがあるかもしれません。

この中で今回私が生物学的で面白いと思ったデータを読み込みたいと思います。Searchのboxに**hemoglobin**を入力してください。

![スクリーンショット 2023-01-13 16 39 44](https://user-images.githubusercontent.com/296176/217183538-2e0d9427-3338-4db9-882a-580c7f11a6af.png)

するとyrajat7900さんの**ヘモグロビン遺伝子の遺伝子座 (hg19)**のセッションが現れます（なぜかデモでは現れなかった。同じようなセッションが複数ありますので、好きなものを選んでください）。このセッションを例に編集、保存してみます。**赤い矢印の場所のサムネイル**をクリックしてください。ゲノム地図が表示されます。

![スクリーンショット 2023-01-13 16 48 35](https://user-images.githubusercontent.com/296176/217183541-d31f46be-f793-41c7-a202-aaf0c370a57c.png)

表示されたゲノム地図は、HBB遺伝子を中心としたヒトのゲノムh19を表示しています。これは、yarajat7900さんが保存して共有した画面のそのままの状況を閲覧しています。血液とその他の組織のトランスクリプトーム解析を実施したことがある人にとっては非常に馴染みのある知見ですが、HBB (hemoglobin beta)とHBD (hemoglobin delta)は、発現する組織に異なる傾向があります。HBBは発現がほぼユビキタスに発現している様子がGTExのデータからよくわかると思います。一方でHBDは他の臓器より血液で多く発現しています。全ヘモグロビンの97%がHbA (alpha x2とbeta x2の4量体)からなり、HbA-2 (alpa x2とdelta x2の4量体)は、HbFとともに残りの3%を占めています。

HBDを見たいので地図のWindowをドラックしたまま左へずらします。HBB遺伝子の全体も地図上に表示されるように調節してください。

![スクリーンショット 2023-01-13 17 08 40](https://user-images.githubusercontent.com/296176/217183544-69ade001-667f-427f-858e-63521363766d.png)

上図のようにHBD, HBB両方の遺伝子が１つの画面に入れば両遺伝子の関係がわかりやすいですね。ではこの画面を今後呼び出せるように、保存してみたいと思います。先ほどユーザー登録をした時と同様に**MyData -> My Sessions**を選択して次の画面を表示してください。

![スクリーンショット 2023-01-13 17 13 19](https://user-images.githubusercontent.com/296176/217183545-a8a73d54-47f8-493e-a292-f48d6cac30ee.png)

Save Settingsの中にある**current settings**の欄に名前をつけて、submitをすれば先ほどの図（Session）が保存されます。また、ローカル環境に保存したい場合は、**Save current settings to local file**で、ファイル名を指定し、ダウンロードすることをお勧めします。保存したファイルをアップロードすれば復元できます。

![スクリーンショット 2023-01-13 17 17 40](https://user-images.githubusercontent.com/296176/217183548-43318088-62ff-47cc-8724-974f516794d4.png)

Save current settingsを行なった場合はこのようにMySessionsに保存されます（保存期限はおそらくないと思いますが、気になるようでしたら先ほど紹介したようにローカル環境にダウンロードしておいてください）。**view/edit**をクリックして編集すれば、メモ書きも残せます。読み込む場合は、**対象のsession nameのリンク**をクリックしてください。

![スクリーンショット 2023-01-13 17 23 19](https://user-images.githubusercontent.com/296176/217183550-7a685526-b7df-421b-a943-11f185d6bda6.png)

先ほどの地図が復元されています。HBD遺伝子全体も画面にしっかり入っている（**保存直前の地図である**）ことがお分かりいただると思います。localに保存した場合のテキストファイルの中身もご覧いただきましょう。

```
_ 1673597223254
altSeqLiftOverPsl hide
avada pack
c chr11
cartVersion 3
clade mammal
cons100way hide
cytoBandIdeo hide
db hg19
dbSnp153Composite hide
dbSnp155Composite hide
dinkL 2.0
dinkR 2.0
expOrder_cons100way phyloP100wayAll 
expOrder_wgEncodeReg wgEncodeRegMarkH3k27ac wgEncodeRegTxn 
fixSeqLiftOverPsl hide
goButton go
gtexGeneV8 full
hgFind.matches 
hgPS_DataTableState 
---以下略---
```

利用していないトラックの情報まで非常に細かく情報が記載されています。このファイルを**MySessions**の中にあるRestore SettingsのUse settings from a local fileに読み込ませると同じものが復元できます。これで、ゲノムブラウザのセッションの保存と復元ができるようになりました。また、この機能を利用することで、他の人のUCSCGBでのセッティングを真似ねて学習することができます。

