## BLAT、In-Sillico PCRを使って配列を検索する/Gene Sorterを使って発現データを解析する

### BLATとは

BLATは、NCBI-BLASTを高速化したツールとして、UCSC GBに実装されています。localで利用することも容易でバイナリーファイルや整形済みBLAT用のDBもダウンロードすることができます。このような配列検索は、NCBI-BLASTをはじめとしてGGGenomeなどでも可能ですが、**配列検索後の結果と他のゲノムワイドな情報を並べて比較する**場合は、UCSC GBが便利だと思います。 

#### WebのBLASTよりも有利な点など (UCSC GBサイトの記述より)

---

- スピード（キューなし、数秒での応答）、その代償として相同性深さは劣る。
- 同時に多数のクエリを書いた長いFastaフォーマットで送信できる。
- 5種類の出力ソートオプション
- UCSC ブラウザへの直接リンク
- アライメントブロックの詳細を自然なゲノム順序で表示
- **カスタムトラックの一部**としてアライメントを後で起動するオプション

#### BLATの動作

---

DNA探索の際のBLAT は、ゲノム全体のインデックスをメモリ上に保持することで動作します。このインデックスは、繰り返しに大きく関与するものを除き、重なり合う全ての11-merを5段に分割して構成され、約2GbytesのRAMを消費します。**ゲノム自体はメモリに保持されないため、local mathineでも快適に動作**します。タンパク質BLATは、11-merではなく4-merであることを除けば、同様の方法で動作します。**タンパク質のインデックスは2Gbytesを少し超える容量が必要**でです。

BLATにおけるDNA searchは、長さ**25塩基以上の類似度95%以上**の配列を高速に検索するように設計されています。BLATは、20塩基の完全な配列一致を見つけることができますが、**より分岐の多い配列や短い配列のアラインメント**は見逃すことがあります。タンパク質のBLATは、20アミノ酸以上の長さで、80%以上の類似性のある配列を見つけることができます。今回の**例では34塩基**の塩基配列を検索用の文字列として与えています。

---

UCSCの上面タブから**Tools -> Blat**を選択してください。

![スクリーンショット 2023-01-13 17 53 06](https://user-images.githubusercontent.com/296176/217183739-62ec7272-5272-49f1-8b96-db48eb4bec2c.png)

Blatに検索する文字列を入力する画面になります。今回はこのページで紹介されている**SOD1**の塩基配列を例に検索してみようと思います。この際に、fasta形式で入力するとゲノムブラウザで検索配列の名前が表示されます。その後、submitもしくはIm feeling luckyを押します。Im feeling luckyは、googleの検索と同様に、検索結果のtopヒットのリンク先をダイレクトに表示してくれます。今回の例の配列はSOD1にtopヒットすることが自明なので、まず**Im feeling lucky**を押してみましょう。



![スクリーンショット 2023-01-13 17 58 56](https://user-images.githubusercontent.com/296176/217183743-28d9a2e8-7a81-4bb7-a27e-a65d9d189c79.png)

結果のtopヒットのリンク先であるゲノム地図がダイレクトに表示されました。検索文字列は黒いバーで表示されています。この地図の窓枠全体が検索文字列（34塩基）になっています。また、その次の行のGENCODE V41のアノテーションが濃い青で表示されていますが、ゲノムをズームアップしすぎて遺伝子構造まで見えないので、ズームアウトしましょう。zoom outを利用してx30倍くらいまでズームアウトしてください。

![スクリーンショット 2023-01-13 18 57 22](https://user-images.githubusercontent.com/296176/217183747-7e0f5bc0-d94e-403b-a601-92fcca8dbb6d.png)

x30倍ズームアウトすると、今回検索した文字列がSOD1遺伝子の５’UTR付近にアライメントしていることがわかります。また、シス領域を示すヒストン修飾とのオーバーラップ、配列の種間保存性などの情報とのオーバーラップを眺めることができました。地図の画面上で、興味のあるトラックで右クリックを押すとその周囲の情報の表示変更やトラックの設定を変更することができます。







## BLATの結果の解釈

---

少し戻り、BLATのtopヒット以外の結果を眺めます。

![スクリーンショット 2023-01-14 1 27 18](https://user-images.githubusercontent.com/296176/217183752-1c1890f6-0276-4558-bc09-4a93ca71810a.png)

今回の結果では、結果が一つのため、ゲノムにユニークにアライメントされたことがわかります。BLASTでは標準でside by side アライメントなどの結果が表示されるのに対して、1行でシンプルな結果表示がBLATの特徴の一つです。**結果をそのままカスタムトラックとして作成**するためのタブ**Buld a custom track with these results**も表示されています。ゲノムの大域での探索の目的だけでなく、このBLAT-カスタムトラック連携機能を使って、BLATを間接的にゲノム上の作図ツールとしても利用できるわけです。

一方で、BLASTの結果に表示されているような**side by sideのアライメント**も見たい、もしくは、ヒットした前後の配列も取得したいという要求もあると思います。この場合は、**detail**をクリックしてください。

![スクリーンショット 2023-01-14 1 32 51](https://user-images.githubusercontent.com/296176/217183754-3783bc04-a115-4ac5-8acb-2c107714500f.png)

与えた配列の情報と、そのゲノム周囲の配列情報（200塩基+与えた配列の長さ）、そしてアライメントの結果が表示されます。









## In-Sillico PCR

----

### BLATの条件を振り返って... qPCR primerのような短い配列の場合はどうするのか？

qPCRプライマーのようなエキソンジャンクションにかかるような**非常に短い配列**は、BLATではゲノムに存在しないため見つけることができません。このような場合は、In-Silico PCRを使い、ターゲットとなる遺伝子セットを選択してみてください。**In-Silico PCRの方が感度が高い**ので、プライマーのペアにはこちらを利用する方がいいと思います。

また、現在の実験科学の環境下では、プライマー設計は受託会社の設計プログラムを用いて行うことが一般的だと思います。融解温度やPCR条件について詳細な情報が得られ、PCRの設計に時間をかけないで済むため、UCSCGBのようなフリーサイトで作成することがほとんどありません。しかしながら、受託会社のプログラムに慣れた人でも時折おとずれるPCRがかからない現象や、条件がマッチせず**手作業で作らなくてはいけない状況**、**作成したPCR primerをゲノム上に図示した図を作りたい**状況は未だに遭遇します。そういったときに、このツールが使えるようになっておくと便利と思います。

**Tools -> In-Silico PCR**を選択してください。

![スクリーンショット 2023-01-16 13 30 45](https://user-images.githubusercontent.com/296176/217183758-42cd06cb-aec4-4586-b18b-de60ecf58fdc.png)

In-Silico PCRは、PCRの経験者なら特に迷うことはないシンプルな入力内容になっていると思います。目的とする生物種を選択し、両端のプライマー配列を入力して、実行するだけです。一方で、先ほどもお伝えした通り、Targetには一考の余地があります。スプライシングアイソフォームを考慮したPCR設計ではエキソンのジャンクションをまたぐ設計が多くなされます。PCRプライマーの合成サービスが連携するようなプライマー作成ツールがそのような設計を採用している場合、ゲノムに探索すると上手くいかないので、**ターゲットをUCSC GenesもしくはENCODE Genes**にすることで、転写産物を対象とした探索に切り替える方が上手くいきます。

![スクリーンショット 2023-01-16 13 35 44](https://user-images.githubusercontent.com/296176/217183762-cf8e7517-3e11-44ba-bb4c-c66e08cbef6d.png)

### 実践的にPCRプライマーを設計する

---

では、解析する候補遺伝子名だけ明らかになったような**まっさらな状態**からスタートして、UCSC GBを使って**Primerを設計するにはどのような順序が良いか**考えてみました。

1. ターゲットの遺伝子をUCSC GBで確認する
2. ターゲットの遺伝子のmRNA配列情報を取得する
   1. UCSC GBのツールに慣れるために、**NCBIを介した方法、UCSC GB Table browserの２つの方法**を紹介します。
3. mRNAのエキソンジャンクションを調べるためのBLATの実行
   1. qPCRなどの場合に備え、特異性をもたせる目的でエキソンジャンクションを挟んだ設計にするためにBLATを実行して、配列をハイライトします。
4. In-Silico PCRを実行し、PCR primerを設計します。



#### 1. ターゲット遺伝子をUCSC GBで確認する。

----

まずATF3をUCSC genome browserで眺めます。GenomesからHuman GRCh38.p13を選択してください。次に、search boxにATF3と入力します。すると、suggestionが現れますので、クリックしてください。

![スクリーンショット 2023-01-16 14 01 02](https://user-images.githubusercontent.com/296176/217183764-2ededb61-0ea8-4abb-8cec-c793502cfb4f.png)

ATF3遺伝子座が現れたら、ATF3遺伝子のsplicing variantも表示したいと思います。GENCODEとNCBI Refseq genesのトラック上で右クリックをして、fullを選択してください。

![スクリーンショット 2023-01-16 14 03 37](https://user-images.githubusercontent.com/296176/217183768-7bd02484-e62a-4b6a-82f1-e7178d6b8a50.png)

すると遺伝子座にある転写産物が全て表示されます。今回は中でも**最も長いATF3の転写産物**をターゲットにしてみたいと思います。この地図からも、ターゲットは5'に特徴があるので、第1,2 exonをまたぐ配列をPrimerにすることを考えてみたいと思います。

![スクリーンショット 2023-01-16 14 05 16](https://user-images.githubusercontent.com/296176/217183771-e12eb2a5-9b97-4844-9f51-138a4aabc927.png)





#### 2. mRNAの配列情報を取得する

----

NCBI Refseq Genes**トラックの一番上の転写産物の配列(NM_001030287.4)**を取得したいと思います。カーソールを合わせて右クリックをしてください。

![スクリーンショット 2023-01-16 14 09 32](https://user-images.githubusercontent.com/296176/217183775-0d09b3f3-790c-45d6-952c-abdcdd950f27.png)

ここで注意点があります。図に示したようにGet DNA for NM...という欄がありますがこれをクリックしてもmRNAの配列を取得ではなく、転写産物の座標領域全体の塩基配列を取得してしまいます。この状況からmRNAの配列を取得するには**私が知る限りは2つの方法**があります。

- 1. **Show details for NM_00~**から、NCBIのリンクをたどり、FASTAを出力させる。GENCODEの場合は、Show detailsからmRNAのリンクをクリックする。
- 2. Table browserに移動してアクセッションナンバーを入力して配列を取得する。

個人的にはmRNAを１種類だけ調べるときは1のやり方の方がおすすめで、2のやり方はtable browserで行うにはやや面倒なやり方のように思います。ここでは２つとも紹介します。

1. NCBI Refseqのリンクに飛び取得する

   **Show details for NM_00~**をクリックしてください。

![スクリーンショット 2023-01-16 14 43 50](https://user-images.githubusercontent.com/296176/217183781-6ccb250e-80d8-4373-8f0e-b8d261e8696c.png)

NM_00~をクリックしてください。NCBI Genbankに移動できるはずです。

![スクリーンショット 2023-01-16 14 45 10](https://user-images.githubusercontent.com/296176/217183782-778e715c-2e1c-4e5d-ad0a-dd172323200f.png)

矢印のFASTAをクリックしてください。すると、mRNAの配列がfasta形式で表示されますので、コピーしておいてください。ファイルに保存したい場合は、**send to**をクリックするとプルダウンメニューが表示され、Fileを選択すると操作できます。

![スクリーンショット 2023-01-16 14 49 42](https://user-images.githubusercontent.com/296176/217183787-8090e0bc-160a-4426-86a0-d11f432ae43c.png)

   **GENCODEの場合**
   先ほどと同様に以下のShow detailsの中にあるmRNA情報をクリックしてください。こちらの方が簡単ですね。
 ![スクリーンショット 2023-02-07 17 34 23](https://user-images.githubusercontent.com/296176/217198822-64131aa3-2ffe-473e-9e35-0b91eb2c57bf.png)

2. UCSC GBのTable browserを利用する

   1.の時と同様にShow details for NM_00~をクリックしてください。

![スクリーンショット 2023-01-16 14 52 25](https://user-images.githubusercontent.com/296176/217183791-ec9caca6-8c75-4488-a0cf-3f5ec9abe915.png)

その後、**NM_001030287.4**の文字列をコピーしてください。次に、上段のメニューにあるToolsから**Table browser**をクリックしてください。



![スクリーンショット 2023-01-16 14 53 05](https://user-images.githubusercontent.com/296176/217183794-f3376050-698a-4189-b5ed-3cd291b7fcd0.png)

すると次のような画面が現れます。Table Browserは、UCSC GBの中でも中核となっているツールで、UCSCが保持しているデータからさまざまなサブセットデータを抽出できる便利なツールです。

![スクリーンショット 2023-01-16 14 55 47](https://user-images.githubusercontent.com/296176/217183795-af63f953-3371-4c10-a49d-cb509bc5e603.png)

しかしその反面、多くのデータ抽出の用途に使えるが故に、設定を１つ間違えると目的に沿わないサブセットが得られてしまうので注意が必要です。今回の目的には以下の図のSelect datasetのセッティングを行なってください。

- clade: Mammal
- genome: Human
- Group: Genes and Predictions
- Assembly: GRCh38
- track: NCBI RefSeq
- table: RefSeq All

![スクリーンショット 2023-01-16 14 58 21](https://user-images.githubusercontent.com/296176/217183797-28b2e85f-d25b-41b5-9919-bd41515fb7a7.png)

その後、Define region of interestを利用してアクセッションナンバーを入力するために、**paste list**をクリックします。

![スクリーンショット 2023-01-16 14 59 53](https://user-images.githubusercontent.com/296176/217183801-9c875703-0a73-4126-bc90-15131fee8e4f.png)

すると、IDリストを入力するフォームがありますので、先ほどコピーしておいた**NM ナンバー(NM_001030287.4)**を入力し、submitしてください。

![スクリーンショット 2023-01-16 15 05 40](https://user-images.githubusercontent.com/296176/217183803-65a2d396-3a57-438c-8e1b-bf78d9ca76f9.png)

すると、Table Browserの画面に自然に戻ります。最後に、Retrive and display dataに出力形式やファイル名を入力して、get outputします。

![スクリーンショット 2023-01-16 15 07 50](https://user-images.githubusercontent.com/296176/217183805-cfb5dd2b-40c9-43a7-902f-227db3d00ffd.png)

genomicという選択のみですが、submitしてください。

最後に、どの領域を取得するのか選択する必要があります。今回はmRNA配列部分を取得するので、intronsのcheckを外して**get sequence**してください。

![スクリーンショット 2023-01-16 15 09 52](https://user-images.githubusercontent.com/296176/217183807-63741cdf-88c5-4ea2-b217-999e6b384acf.png)

するとファイルが保存されますので、そのファイルの中身をテキストエディタで開いてください。下の図はIntronsを除いた場合と除いていない場合を並べてご紹介しています。

![スクリーンショット 2023-01-16 15 14 35](https://user-images.githubusercontent.com/296176/217183810-89df86e2-5486-41ef-b101-e2cc5105dcf4.png)

左はcheckを外す前、右は、Intronを除いたもので、**第二エキソンの位置をハイライト**してみました。確かにmRNA配列を取得できています！

#### 3. mRNAのexon-exon junctionを調べるためのBLATの実行

配列の準備が終わりましたので、今度はこの配列をBLATしたいと思います。fastaファイルをコピーしておき（ファイルに保存しても構いません）、**Tools-> BLAT**へ移動してください。移動後、配列をペーストもしくはsubmitしてください。ファイルを保存した場合はファイルを選択、submit fileしてください。

![スクリーンショット 2023-01-16 15 24 19](https://user-images.githubusercontent.com/296176/217183814-e78f734f-2d44-4059-bc7a-c7fde1c8438d.png)

BLATの結果が表示されます。top hitの結果のdetailを表示してください。

![スクリーンショット 2023-01-16 15 28 06](https://user-images.githubusercontent.com/296176/217183815-758d810b-d648-40f0-910e-b24d8163bc7a.png)



BLATしてexon-exon junctionをハイライトすることができ、かつイントロンとの配列の状況も綺麗に可視化できました。では、SYBER GreenでのqPCR primerを設計することを想定して、FRのPrimer位置を決定しましょう。その際にsplice位置をうまく跨ぎたいので、色付けしたわけです。



### 4. Primerを設計する

PrimerはPracticalには以下のように決定すると思います。

1. 増幅サイズは100-200bp

2. Tm 55-60度（反応効率上、高すぎないように、できればFRを揃える）

3. 3'末端にGまたはCが3個以上連続する配列は避ける（が意識しすぎない）。一方で、3'末端がTになる配列はミスマッチでアニールしやすいので避ける。

4. ダイマーがふえすぎないように、3'末などと一致する配列をさける（が意識しすぎない）

   

### １回目のトライ

- F: 5- CCTCTATATAGGATGCTCTG-3

- R: complementary処理する前の配列->　5-GATTTTGCTAACCTGACGC-3

- **Forward:** 49.2 C cctctatataggatgctctg
- **Reverse:** 56.0 C gattttgctaacctgacgc

#### 2回目のトライ

- F: 5-AGGATGCTCTGCTGTTTCC-3

- R: complementary処理する前の配列->　5-GATTTTGCTAACCTGACGC-3

- **Forward:** 58.0 C aggatgctctgctgtttcc
- **Reverse:** 56.0 C gattttgctaacctgacgc

![スクリーンショット 2023-01-16 16 06 12](https://user-images.githubusercontent.com/296176/217183818-8a1a12ca-9509-4a7e-801d-78b07a42e0eb.png)

結果的に２つの転写産物のprimerになっていますが、どうやらNCBIのサイトで調べたところ、この２つの違いはより3'側にあるようです。

![スクリーンショット 2023-01-16 17 08 13](https://user-images.githubusercontent.com/296176/217183819-f9d44dbb-9bb4-4f68-950d-ceaca9565812.png)



![スクリーンショット 2023-01-18 18 12 46](https://user-images.githubusercontent.com/296176/217183822-7726efe8-b78a-4a0c-939b-90f930692266.png)

結果のリンクをクリックすると、Track情報にIn Sillico PCRで設計したプライマーの構造的な関係が描かれていると思います。このようにPCR設計の位置をゲノム地図上にダイレクトに図示することができます。





## GeneSorterを使った発現解析

---

こちらは時間が余った時のおまけに近いのですが、Gene Sorterを使った発現解析の流れを説明します。

- 1. Gene SorterでHBB遺伝子を検索
     1. 表示対象のセッティング
     2. データの抽出
- 2. GenomeGraphで、あるLD領域（SNPsマーカーで囲まれている）の遺伝子を抽出し、発現データを得る。
     1. SNPマーカーを入力する。
     2. Gene SorterでLD領域の遺伝子抽出。

### 1. Gene SorterでHBB遺伝子を検索

----

![スクリーンショット 2023-01-19 16 51 11](https://user-images.githubusercontent.com/296176/217183836-d26eda8a-d1c1-4727-8147-4772031a9946.png)

上段のメニューから、**Tools -> Gene Sorter**を選択してください。



![スクリーンショット 2023-01-19 16 58 53](https://user-images.githubusercontent.com/296176/217183841-4e0e7b86-a296-4019-ac5c-d63dd69372fb.png)

Gene Sorterでは、検索した遺伝子と発現プロファイルの似ている遺伝子を各種データベースから取得することができます。今回は**HBB**遺伝子を検索してみたいと思います。下の図のようにHBBを検索してみてください。その時に、データは**GTEx**を利用してみましょう。

![スクリーンショット 2023-01-19 17 04 02](https://user-images.githubusercontent.com/296176/217183844-747116f1-376e-46b7-8347-527951609ea3.png)

検索結果として、さまざまな遺伝子がヒットしますが、HBB遺伝子を選択してください。

![スクリーンショット 2023-01-19 17 07 22](https://user-images.githubusercontent.com/296176/217183847-5e6a6cda-fed7-44f1-a2e9-a1022db837d2.png)

HBB遺伝子とGTExデータの中で発現プロファイルが似ている遺伝子が上から表示されます。GTExは、約53の健常組織のデータから発現データを取得したものです。しかし、GTExのデータは、13列ほどしか表示されていないので、**Configure**で設定を変えてみましょう。

![スクリーンショット 2023-01-19 17 11 27](https://user-images.githubusercontent.com/296176/217183849-35458edd-c847-4449-9529-1fc37590477a.png)

赤矢印の箇所を編集してください。GTExのすべての閲覧をできるようにして、GTEx Delta（Deltaは差分の意味のdeltaだと思います）を使って発現量としてどの程度開いているのか（0だと同じ発現量）を確認できるようにします。最後に**"submit"**を押してください。間違ってcustom columnsを押さないように気をつけて。

![スクリーンショット 2023-01-19 17 15 50](https://user-images.githubusercontent.com/296176/217183851-f3fcdd4d-b937-4a3a-842e-86c9882c8995.png)

するとその他のGTEx profileも表示されました！あとは好みで**"filter"**発現量でフィルタリングなどもすることができます。最後に他の解析に利用するために、outputをsequenceや、発現量のtext tableにすることもできます。

![スクリーンショット 2023-01-19 17 18 48](https://user-images.githubusercontent.com/296176/217183852-509bcf55-c225-4f71-95f5-eb0bfd884df0.png)



### GenomeGraphで、（SNPsマーカーで囲まれている）あるLD領域の遺伝子を抽出する。

---

これは特殊な事例なので、使う人はそうそういないと思いますが、ゲノムの座標から遺伝子抽出することができます。用途を考えてみましたが、連鎖不平衡（LD）の領域がわかった場合、その中の遺伝子が気になることがあるかもしれません。LD領域の遺伝子をごっそり取得して、そしてGTExの発現テーブルを作成してみます。

今回用意した連鎖不平衡の領域は、染色体一番の約65kbの領域（chr1: 2556224-2622185）になります。次のマーカーで囲まれた位置になります。このLSに乗っているSNPsは、GWASの結果、Eosinophil percentage of white cells, Chronic inflammatory diseases, Chronic inflammatory diseases などの炎症に関連しそうな形質に関わっている可能性が示唆されています。どんな遺伝子が乗っているでしょうか？駆け足で説明します。

![スクリーンショット 2023-01-19 16 51 11](https://user-images.githubusercontent.com/296176/217183836-d26eda8a-d1c1-4727-8147-4772031a9946.png)

Gene Sorterを開いてください。

![スクリーンショット 2023-01-19 16 47 57](https://user-images.githubusercontent.com/296176/217183835-e86111c8-8cdd-4a9b-866c-b6642617c7ff.png)

```
rs2227312	1
rs6671426	1
```

上記のSNPs（LDブロックに使われていた終端マーカーs）を適当なスコアをつけてタブ区切りでsubmitしてみます。名前や詳細については適当に入力しました。みやすいようにと、スコアを-2から2にしてみました。submit!

![スクリーンショット 2023-01-19 16 47 43](https://user-images.githubusercontent.com/296176/217183833-acfa5cef-2888-49a0-a4bb-b7bafc603b5f.png)

OK

![スクリーンショット 2023-01-19 16 47 35](https://user-images.githubusercontent.com/296176/217183828-3bba932d-77a8-4c2c-913d-992a1d15dd5f.png)

何も写ってないじゃないか？と思うかもしれませんが、領域が60kbほどの領域は目に見えないものですよ。実は、矢印のところになります。ゲノム全長からすると小さな領域に見えますね。さて、**sort genes!**

![スクリーンショット 2023-01-19 16 47 20](https://user-images.githubusercontent.com/296176/217183827-4619114b-83c2-448e-9a5a-bf6db3619b5d.png)

go to gene sorter!

![スクリーンショット 2023-01-19 16 47 08](https://user-images.githubusercontent.com/296176/217183826-6b845bbc-ba27-43af-8830-2d22fc6e9680.png)

というわけで、LD領域の遺伝子たちが抽出できました。先ほどと同じ要領でデータを増やしたりしてみてください。ただし、GTExは健常組織なので炎症性の遺伝子が発現しているようすは厳しいかも。そして、ごらんいただくとTNFのファミリーに属する遺伝子がいたことがわかります。LDの形質情報と遺伝子がつながりました！
