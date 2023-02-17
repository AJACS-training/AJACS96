## Track Hubを使ってオリジナルデータと公共データを比較した図を作成する

**UCSC Track Hub**には、さまざまな**公共データ**が登録されており、ゲノム情報を必要に合わせてカスタマイズすることができます。また、**Custom track**でUCSC genome browser上にファイルをアップロード、もしくはURLを指定することで、ゲノム地図上にオリジナルデータなどを載せ、目的とする遺伝子座などの地図を作成して論文のfigureなどを作成することができます。

UCSC GB上で可視化し、図を作成する際は、主にエピゲノムデータなどを表示してデータ間のオーバーラップを確認できる図を作成することが多いと思います。今回は先ほど紹介した**ATF3遺伝子**について、その上流シス領域に注目して、ATF3のChIP-seqデータをブラウザに加えてATF3が自身の転写制御（autoregulation)に関係している可能性を示唆するデータを作成してみたいと思います。



## Track Hubからテストデータをつくる

---

オリジナルデータの例として、既にUCSC GBに存在するATF3 ChIP-seqデータのダウンロードを行いたいと思います。UCSC GBでは、主に**bigWig**や、**bigBed**とよばれるバイナリーファイルが読み込まれています。オリジナルのデータから、この２つのファイルに変換するには、UCSC GBに存在するUtility スクリプトを用いることで変換することができます。

**補足**: オリジナルのBAMファイルからBigWigファイルを作成するには、[こちらのパイプライン](https://github.com/Danko-Lab/utils/blob/master/atacseq/BamToBigWig/README.md)を利用すると可能です。このパイプラインは、**samtools, bedtools**そしてUtilityのスクリプトであるbedGraphToBigWigをインストールすると利用することができます。

変換したファイルをUCSC GB上に表示する方法は２つの方法があります。

1. UCSC GBとは異なる外部サーバーにファイルをアップロードして、その情報(URL)をUCSC GBに入力する。

   自前の公開サーバーなどを用意してファイルをアップロードする必要がある。Track Hubの中にあるUCSC GB以外の機関が登録した情報は、この方法でトラック情報を公開している。

2. UCSC GBに直接ファイルをアップロードする。

   簡単ですが、ファイルのサイズに制限がある。

今回は、2の方法について解説します。

まず、Track Hubへ移動します。

![スクリーンショット 2023-01-17 14 46 28](https://user-images.githubusercontent.com/296176/217183987-35e9a910-9d3e-4d6b-9c6c-543496131123.png)

Track Hubでは、UCSC GBに登録されている接続可能なデータを**横断的に検索**することができます。**Search terms**に**ATF3**,**assembly**に**hg38**を入力して**submit**してください (結果が表示されるまでに少し時間がかかります)。

![スクリーンショット 2023-01-17 14 52 56](https://user-images.githubusercontent.com/296176/217183989-031abc55-ce32-4ea9-b100-29fd8dc4c2a6.png)

![スクリーンショット 2023-01-17 14 54 22](https://user-images.githubusercontent.com/296176/217183991-64fba1c5-0e1b-4639-8fde-141f13852a80.png)

ATF3に関連するデータセットが接続されているHubとして、5つの候補が存在するようです。この中からChIP-seqのデータを得るためにENCODE DNA Track hubの**Search details**をクリックして階層を辿っていきます。注意点として、右にある**Assemblies**に表示されているゲノムバージョンのみ、そのトラック情報を閲覧することができます。

![スクリーンショット 2023-01-17 14 55 17](https://user-images.githubusercontent.com/296176/217183993-a445df35-cb7c-4214-b14a-4a5314c35a8b.png)

**ATF3のChIP-seqの複数のデータ**に辿り着くことができました。このHubに接続したいと思いますのでブランチの一番上にある**Connect**ボタンを押します。成功すれば **~ Successful **という表示が一瞬だけ現れてgatewayのページに飛びます。gatewayから、**genomes -> hg38**をクリックしてください。

![スクリーンショット 2023-01-17 15 08 56](https://user-images.githubusercontent.com/296176/217183998-49c68cb1-33fd-4e47-b480-e8daf49b8240.png)

ゲノムブラウザは、 ATF3遺伝子座が表示された以前の状態のままですが、その下段に**ENCODE DNA Trackhub**が上段にきて接続されている様子がわかります。この中から、**TF ChIP-seq by target**の文字をクリックしてください。

![スクリーンショット 2023-01-17 15 07 40](https://user-images.githubusercontent.com/296176/217183995-d8d01c07-e84f-4e21-80de-e79d1057ee58.png)

ChIP-seqのデータ一覧が表示されました。ATF3を上記のように**dense**モードで**submit**して表示させましょう。

![スクリーンショット 2023-01-17 15 13 28](https://user-images.githubusercontent.com/296176/217184001-987cfb5e-e251-47f8-9cb4-058e55feb36e.png)

最下段にATF3のChIP-seqのpeak summitが現れました。この遺伝子座の面白いのは、ATF3のプロモーターのシス領域にATF3自身が結合しているところです。さらに、最長ATF3の上流にあるENCODEでは遺伝子アノテーションとして登録されている**exon1つの領域**がReMap, ATF3の結合サイト、GenHancerと密にオーバーラップしています（これらのトラックをONにしていない場合表示されていないかもしれません、ここでは、テストデータ作成の解説のためにONの操作を省いています）。

さあ、ATF3の自己応答の目処がつきました。トラックにすでにシス情報を載せて確認している時点でチートで恐縮ですが、このChIP-seqデータをオリジナルデータの代わりにするためにダウンロードします。ATF3/liver signalのtrackの上で右クリックして、**Configure ATF3/liver signal (by target) ATF3 track set...**を選択してください。

![スクリーンショット 2023-01-17 15 19 09](https://user-images.githubusercontent.com/296176/217184004-1cd09265-fda8-4395-9634-174970a92a54.png)

![スクリーンショット 2023-01-17 15 23 14](https://user-images.githubusercontent.com/296176/217184007-4f177918-56f6-47f2-a045-ac4babb23ed2.png)

するとtrackの設定を変更するページが出てきます。ATF3/liver peaksの右側にある**Schema**をクリックしてください。

![スクリーンショット 2023-01-17 15 26 47](https://user-images.githubusercontent.com/296176/217184011-bd5139e6-6da1-4c1c-95c2-f22cb0536ca1.png)

すると、データの詳細な情報（実験の情報、論文の情報など）を閲覧することができます。実は、このページからこのデータが**インターネット上のどこにあるか**が記載されています。上段の**Binary file of type bigWig stored at~**に続くURLになります。つまり、track hubは、ENCODEのサーバー上のデータを読み込んでブラウザに表示しています。今回このデータをダウンロードしてテストデータとして利用します。データはご覧の通り**bigWigファイル**ですね。ダウンロードには20分ほどかかりますので、レクチャー内では行わないでください。その後のファイル整形は例として記載します。ではまず、**?proxy=true**を除いてwgetでダウンロードします。

```
wget https://www.encodeproject.org/files/ENCFF608OBE/@@download/ENCFF608OBE.bigWig
```

ダウンロードしたbigWigファイルを編集し、UCSC GBで読み込みやすいように**染色体1番だけのbedGraphファイル**を作成します。そのために**bedGraphへのファイル変換操作**を行います。

```
# 必要なツールとファイルをダウンロード
wget http://hgdownload.soe.ucsc.edu/admin/exe/macOSX.x86_64/bigWigToBedGraph
wget https://genome.ucsc.edu/goldenPath/help/hg38.chrom.sizes

chmod 755 bigWigToBedGraph
chmod 755 bedGraphToBigWig
# 染色体単位で変換を実行
# ./bigWigToBedGraph -chrom=chr1 ENCFF608OBE.bigWig ENCFF608OBE.chr1.bedGraph
# 座標を指定してATF3遺伝子座がある限定的な領域で実行
./bigWigToBedGraph -chrom=chr1 -start=212509895 -end=212676211 ENCFF608OBE.bigWig ENCFF608OBE.chr.1212509895.212505D9895.bedGraph

```

bedGraphは、テキストデータなので、テキストエディタで編集することができます。テキストエディタで開き、グラフの設定用のheaderを追加します。ENCFF608OBE.chr.1212509895.212505D9895.bedGraphファイルを開き、先頭に以下の文字を入力し、保存します。

```
browser position chr1:212509895-212676211
browser hide all
track type=bedGraph name=ATF3_ChIP description="ATF3 ChIP-seq" visibility=full color=200,128,0 altcolor=0,100,200
#chrom chromStart chromEnd score
```

この設定の詳細は、[こちらに](https://genome.ucsc.edu/goldenPath/help/customTrack.html#lines)記載されています。編集後に保存したbedGraphを**Custom Tracks**から読み込みます。

![スクリーンショット 2023-01-17 19 36 44](https://user-images.githubusercontent.com/296176/217184020-1499240c-d225-4351-80af-59bcb497db84.png)

![スクリーンショット 2023-01-17 19 38 16](https://user-images.githubusercontent.com/296176/217184021-6236055a-0c88-4cdd-b7e9-5b1f1f935dab.png)

assemblyがhg38になっていることを確認後、ファイルを選択し、**submit**してください。

![スクリーンショット 2023-01-17 19 39 29](https://user-images.githubusercontent.com/296176/217200534-005ed5a9-261c-4791-a7fd-7560c1564fd5.png)
無事読み込まれたら、**Manage Custom Tracks**にデータが表示されているはずです。**return to current position**を押して、genome browserに移動します。

![スクリーンショット 2023-01-17 19 35 39](https://user-images.githubusercontent.com/296176/217184017-d4b4f531-084a-4215-ba52-6a5b077165ec.png)

ATF3のChIP-seqデータが表示されました。上の図のようにcustom trackでデータをロードすると今まで表示されていた**他のtrackの情報は失われてしまう点にご注意ください。必要に応じて、再度情報を足していきます。



### 公共データの情報をゲノム地図に追加する

---

**地図の下にあるデータのコレクション**からGENCODE V41,、**DNase-seq**、**CpGの情報**、そしてエンハンサー候補を眺めるために**ReMap, GeneHancer**をONにしていきます。初めは画面を見やすくするようにdenseやpackモードで表示するといいかもしれません。**GeneHancerは相互作用の推定**が見えるようにfullで設定します。



![スクリーンショット 2023-01-17 14 20 18](https://user-images.githubusercontent.com/296176/217183974-4e38f320-f4cb-4a97-a9cb-f76b7acc7510.png)

![スクリーンショット 2023-01-17 14 20 58](https://user-images.githubusercontent.com/296176/217183983-09cb7755-885b-4fa2-b637-76836d0e5064.png)

![スクリーンショット 2023-01-17 14 22 06](https://user-images.githubusercontent.com/296176/217183986-43345772-0044-4958-b222-d1db43b7b5e8.png)

最後に一番底にある**refreshボタン**を押すと選んだデータが表示されます。

![スクリーンショット 2023-01-17 19 47 06](https://user-images.githubusercontent.com/296176/217184024-980151ba-78be-4f5b-969c-dfa9cee7a7e0.png)

次に、表示したデータの順番を変更します。**トラックの左端**を**ドラックアンドドロップ**すると上下を入れ替えることができます。入れ替えた後は次のような図です。どう見せるかは好みなので、見えやすい順序を模索してください。

![スクリーンショット 2023-01-17 19 49 45](https://user-images.githubusercontent.com/296176/217184028-8db65a9c-b511-41e5-8fb5-e52392a2c8e0.png)

一番上の段にGENCODEアノテーションにしました。ATF3の結合がプロモーター付近や、gene body最長のATF3より**上流の位置**にシグナルがみられます。そして、**GeneHancer**によって推定されたエンハンサー領域の相互作用として、この２つが繋がっています。上流シスのChIP-seqのpeakは、indirectなATF3結合ではないかと想像させる一枚です。

ここから少しだけ見栄えを調節します。まず、ChIP-seqのデータのトラックの高さを調節してみます（そのままでも良いかもしれませんが、練習のため）。ATF3-ChIPのトラック上で右クリックをしてください。

![スクリーンショット 2023-01-17 19 59 01](https://user-images.githubusercontent.com/296176/217184030-b909955c-c041-48fb-a458-cc4ff5aeb551.png)

**Track heightを60**、**smoothing windowを5**ぐらいにセットしてみたいと思います。

![スクリーンショット 2023-01-17 20 02 16](https://user-images.githubusercontent.com/296176/217184031-d15fbd81-40db-458f-aa07-67cf55f8ac47.png)

### 論文用の図のために、魅せたいところをハイライトする

---

ATF3のChIP-seq peakとATF3プロモーター、エンハンサー相互作用の推定箇所のオーバーラップをハイライトする図を作りたいと思います。

まず、強調したい座標を**Shift key**を押しながら左クリックで囲います。

![スクリーンショット 2023-01-17 20 07 05](https://user-images.githubusercontent.com/296176/217184035-7e1fc1c8-77de-4274-b2a9-22a84b1b9ab0.png)

すると選択領域が薄青い枠でかこまれた後、**Drag-and-select**のwindowがあらわれますので、**zoom in**してください。

![スクリーンショット 2023-01-17 20 09 53](https://user-images.githubusercontent.com/296176/217201264-d862f7cb-2e42-4bfa-9f81-4549be83a4b1.png)

ChIP-seqのpeak領域を拡大することができました。この領域を綺麗に囲ってハイライト（もしくはGenHancerのエリアを囲ってもいいかもしれません）しやすいように拡大した訳です。先ほどと同様に**Shift**を押しながら左クリックでPeakを裾野から綺麗に包含するように囲ってください。

![スクリーンショット 2023-01-17 20 12 30](https://user-images.githubusercontent.com/296176/217184038-4ff89156-cf56-4e1d-a6b6-166658448c32.png)

色はデフォルトの水色にして、**Add Highlight**してください（Single Highlightを選択した場合、１つのエリアをハイライトしたら他のハイライトしていたエリアが消えてしまいます）。同様の操作を下流のプロモータ領域でも行いましょう。



![スクリーンショット 2023-01-17 20 15 31](https://user-images.githubusercontent.com/296176/217184042-fb549ffe-a1f9-4abf-900c-eabf34dfef06.png)

![スクリーンショット 2023-01-17 20 17 49](https://user-images.githubusercontent.com/296176/217184044-073d2c38-6c8c-4119-adf7-619763d42598.png)

これで図の完成です。



## 完成した図をPDF or EPSで保管する

---

作成した図をillustratorなどで編集可能な**PDF, EPS**で保管する方法になります。ViewからPDF/PSを選択してください。

![スクリーンショット 2023-01-17 20 24 25](https://user-images.githubusercontent.com/296176/217184045-bd59a100-7451-4f5c-8bef-29b478297a8e.png)

![スクリーンショット 2023-01-17 20 26 46](https://user-images.githubusercontent.com/296176/217184047-eea055d8-5c7b-4492-82fe-cace9ca6edcd.png)

**PDFやEPS**としてダウンロードすることができます。フォントや線の太さ、いらない空白などの削除など細かい設定は描画ソフトで調整する方がよいでしょう。また、保存したファイルはトラックが縦に長くてもしっかり保存されますので、必要な情報を全て載せておいて保存することもできます。















## おまけ: Track Collection Builderはトラックを統合する

---

UCSC genome browserで**複数のオリジナルファイルを一つのトラックで表示**したり、**ファイルを結合**したい場合に**Track Collection Builder**を利用します。MyDataからTrack Collection Builderを選択してください。

![スクリーンショット 2023-01-17 20 54 55](https://user-images.githubusercontent.com/296176/219510966-9fb7c5f1-2efd-4701-a9ae-7c90544fb3c3.png)

まず、利用するデータのコレクション（フォルダのようなもの）を右の**Add Collection**から作成します。この時の名前がトラックに表示されるので、コレクションを代表する名前にしたほうが良いと思います。

<img src="https://user-images.githubusercontent.com/296176/219511041-17929194-5e30-44a5-8567-3b982cce6232.png" alt="スクリーンショット 2023-01-17 20 35 24" style="zoom:50%;" />

saveを押した後、そのフォルダにファイルを加えていきます。

![スクリーンショット 2023-01-17 20.36.10](/Users/suimye/research/ss/スクリーンショット 2023-01-17 20.36.10.png)

右のファイルリストから加えたいファイルの**plusボタン**を押すと、右のコレクションに追加されます。その後、右上にある**GO!**ボタンを押しましょう。

![スクリーンショット 2023-01-17 20.39.01](/Users/suimye/research/ss/スクリーンショット 2023-01-17 20.39.01.png)

先ほどの図に6つのATF3 ChIP-seqのトラックが追加されました。このように**グループにまとめて１つのトラック**にすると、トラックを並べる順番を調節しやすく、編集にも便利だと思います。今回追加したトラックの配色が薄いですが、これは私の色の選択ミスです（後で修正しています）。つぎに、このコレクション内の６つのファイルのシグナルを結合し、１つのトラック上で表示する操作を実施してみたいと思います。**トラック上で右クリックを押して、configure**を選択します。

<img src="/Users/suimye/research/ss/スクリーンショット 2023-01-17 20.44.37.png" alt="スクリーンショット 2023-01-17 20.44.37" style="zoom:67%;" />

このsetting画面は今までの設定画面とは異なり、**Merge method**が加わっています。この項目では、コレクションの中にあるトラックのシグナル値をマージする**add**や、**transpalent, solid, stacked, subtract**があります。

まず試しにaddを行い、somoothing windowを5で可視化してみたものを表示します。

![スクリーンショット 2023-01-17 21 12 40](https://user-images.githubusercontent.com/296176/219511261-f2cf17d9-aedc-485a-b6df-5a28c5ebd8c6.png)

少し見辛いですが、データが足されたものが表示されています。次に**transpalent**を試してみましょう。

![スクリーンショット 2023-01-17 21 15 33](https://user-images.githubusercontent.com/296176/219511498-4dbdc7ae-f6a2-4df6-9f58-1f72f8f5dff6.png)

Peakの変化を透過色で眺めることができました。例えば**時系列データ**などでこの描画を行うと美しい変化を描画できるかもしれません。このコレクションの情報は**MySessions**で保存しなければ残りませんので、必要な場合はかならず保存するようにしてください。また、コレクションの名前や色はTrack Collection Builderの画面に戻って、コレクションをダブルクリックすれば後からでも変更することができます。





