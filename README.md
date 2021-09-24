# CoV2network

SARS-CoV-2ゲノムに対するハプロタイプネットワーク作成例（MAFFT，DNAsp, PopART利用版）

### 入力データ

DDBJで公開された静岡株47配列を使用する．
getentryでゲノムFASTA配列をダウンロードする．
http://getentry.ddbj.nig.ac.jp/getentry/na/BS001145-BS001191/?format=fasta&filetype=text&trace=true&show_suppressed=false&limit=100
```
47配列のPANGO lineage内訳
B.1.1.214: 8
  BS001152 hCoV-19/Japan/SZ-NIG-Y21030/2021
  BS001154 hCoV-19/Japan/SZ-NIG-Y21032/2021
  BS001155 hCoV-19/Japan/SZ-NIG-Y21033/2021
  BS001157 hCoV-19/Japan/SZ-NIG-Y21035/2021
  BS001158 hCoV-19/Japan/SZ-NIG-Y21036/2021
  BS001159 hCoV-19/Japan/SZ-NIG-Y21037/2021
  BS001160 hCoV-19/Japan/SZ-NIG-Y21038/2021
  BS001179 hCoV-19/Japan/SZ-NIG-Y21077/2021

B.1.1.7: 39
```
### 0. 配列IDの修正
 コメント行に含まれるID名として，hCoV-19/Japan/SZ-NIG-Y21018/2021のような株名のみにしたほうが良い．
 
 サンプルファイル：SH47_210927.fas
 
 
### 1. リファレンスゲノムとのマルチプルアラインメントを行う．
 [MAFFT](https://mafft.cbrc.jp/alignment/software/)で作成する．
 
 コマンド例：mafft --6merpair --thread -1 --keeplength --addfragments SH47_210927.fas MN908947_3.fasta > output1.fas

 MN908947_3.fasta：リファレンスゲノム（Wuhan-Hu-1）【各自，ダウンロードしてご用意ください．】
 http://getentry.ddbj.nig.ac.jp/getentry/na/MN908947.3/?format=fasta&filetype=html&trace=true&show_suppressed=false&limit=10

### 2. DNAspでハプロタイプデータを作成する
  [DNAsp](http://www.ub.edu/dnasp/)で作成する．

2-1. output1.fas を DNAspで読み込む．（図１の１）

*注意：ATGC以外が含まれると，エラーとなる．*

2-2. 読み込み完了後，【Generate】-【Haplotype Data File..】を選択．（図１の２）

2-3. その際，出力ファイル形式を選択できるので，NEXUS形式を選択し，ファイルを保存する．（図１の３）

ファイル名：SH47_210927.nex

![Fig1](https://user-images.githubusercontent.com/89957075/134689148-6e42e2a4-7ebd-4976-8432-cdae9668b70d.PNG)

図１．DNAspでの実行画面例

### 3. ネットワーク作成
   [PopART](http://popart.otago.ac.nz/index.shtml)で作成する．
   
   出力されたNEXUS形式ファイル（SH47_210927.nex)を入力ファイルとして，PopARTで読み込む．
    【注意】読み込んだとき，CHARLABELSについて，エラーが出るが，無視して良い（図２の１）．
 
 【Network】-【Median Joining Network】を選択し，実行．(解析結果例：図２の２)
  
  Median Joining Networkは，サンプルサイズが大きく，遺伝的距離が小さい種内の系統関係を見るのに適したネットワーク作成法．
  
  Epsilonは，重み付けされた遺伝距離の尺度である．基本は，「0」で良い．
  
![Fig2](https://user-images.githubusercontent.com/89957075/134666234-d1ed8f60-1b90-4740-85d6-57a1ab267804.PNG)

図２ PopARTの実行例

### 3-1. 【参考】NEXUS形式ファイルに，TRAITS情報を追加する．
 
  NEXUSファイルの[Hap#  Freq. Sequences]から，ハプロタイプに含まれる株の件数や採取地，採取月などをTRAITS情報として付与すると，
  ノードの大きさで件数，サンプルの採取地，採取月などを色別に表示することができる（図３）．（入力ファイル名：SH47_210927_add_trait.nex）
  
  複数のTRAITSを設定し，色分けすることもできる．

![Fig3](https://user-images.githubusercontent.com/89957075/134686954-44680b80-b1b6-462e-97fc-f488c953ce10.png)

図３．TRAITS情報を加えたネットワーク結果
 
  NEXUS形式ファイル（ファイル名：SH47_210927.nex）の最後に，以下を追加
 
 
 ```
  ここで，項目を追加する場合，TraitLabelsに，スペース区切りで，項目名を追加する．
  NTRAITSを設定した項目数にする．
  各Hapに，対応する項目をカンマ区切りで加える．（例えば，２項目の場合，【Hap_1 1,0】となる．）
 
 BEGIN TRAITS;
  Dimensions NTRAITS=2;
  Format labels=yes missing=? separator=Comma;
  TraitLabels Ref Shizuoka;
  Matrix
Hap_1 1,0
Hap_2 0,4
Hap_3 0,1
Hap_4 0,3
Hap_5 0,1
Hap_6 0,5
Hap_7 0,1
Hap_8 0,1
Hap_9 0,4
Hap_10 0,1
Hap_11 0,2
Hap_12 0,1
Hap_13 0,1
Hap_14 0,1
Hap_15 0,1
Hap_16 0,1
Hap_17 0,3
Hap_18 0,1
Hap_19 0,2
Hap_20 0,1
Hap_21 0,1
Hap_22 0,2
Hap_23 0,1
Hap_24 0,1
Hap_25 0,3
Hap_26 0,1
Hap_27 0,1
Hap_28 0,1
Hap_29 0,1
;
END;
 ```
