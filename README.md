# CoV2network

SARS-CoV-2ゲノムに対するハプロタイプネットワーク作成

### 入力データ

DDBJで公開された静岡株47配列を使用する．
getentryでゲノムFASTA配列をダウンロードする．
http://getentry.ddbj.nig.ac.jp/getentry/na/BS001145-BS001191/?format=fasta&filetype=text&trace=true&show_suppressed=false&limit=100
```
47配列のPANGO lineage内訳
B.1.1.214: 8 
B.1.1.7: 39
```
### 0. 配列IDの修正
 コメント行に含まれるID名として，hCoV-19/Japan/SZ-NIG-Y21018/2021のような株名のみにすると良い．
 
 ファイル名：SH47_210927.fas
 
### 1. MAFFTにて，多重整列を行う．
 
 コマンド例：mafft --6merpair --thread -1 --keeplength --addfragments SH47_210927.fas MN908947_3.fasta > output1.fas

 MN908947_3.fasta：リファレンスゲノム（Wuhan-Hu-1）【各自，ご用意ください．】

### 2. DNAspでハプロタイプデータを作成する
2-1. output1.fas を DNAspで読み込む．（図１の１）

  [DNAsp](http://www.ub.edu/dnasp/)をダウンロードし，セットアップする．

2-2. 読み込み完了後，【Generate】-【Haplotype Data File..】を選択．（図１の２）

2-3. その際，出力ファイル形式を選択できるので，NEXUS形式を選択し，ファイルを保存する．（図１の３）

![Fig1](https://user-images.githubusercontent.com/89957075/134666227-8f696327-e73e-4a63-b6ee-fbbce29ad360.PNG)

図１．DNAspでの実行画面例

### 3. 出力されたNEXUS形式ファイル（拡張子.nex)を入力ファイルとして，PopARTで読み込む．
  【注意】読み込んだとき，CHARLABELSについて，エラーが出るが，無視して良い（図２の１）．
 
 【Network】-【Median Joining Network】を選択し，実行．(解析結果例：図２の２)
  
  Median Joining Networkは，サンプルサイズが大きく，遺伝的距離が小さい種内の系統関係を見るのに適したネットワーク作成法．
  
  Epsilonは，重み付けされた遺伝距離の尺度である．基本は，「0」が良い．
  
![Fig2](https://user-images.githubusercontent.com/89957075/134666234-d1ed8f60-1b90-4740-85d6-57a1ab267804.PNG)

図２ PopARTの実行例

### 3-1. 【参考】NEXUS形式ファイルに，TRAITS情報を追加する．
 
  NEXUSファイルの[Hap#  Freq. Sequences]から，ハプロタイプに含まれる件数をTRAITS情報として付与すると，
  サンプル情報をノードに加えることができる（図３）．
  
  複数のTRAITSを設定し，色分けすることもできる．

![Fig3](https://user-images.githubusercontent.com/89957075/134666239-b01d2477-c2c4-4b5b-aebd-01f5fd9b3cad.PNG)

図３．TRAITS情報を加えたネットワーク結果
 
  NEXUS形式ファイルの最後に，以下を追加
 
 
 ```
  ここで，項目を追加する場合，TraitLabelsに，スペース区切りで，項目名を追加する．
  NTRAITSを設定した項目数にする．
  各Hapに，対応する項目をカンマ区切りで加える．（例えば，２項目の場合，【Hap_1 1,0】となる．）
 
 BEGIN TRAITS;
 Dimensions NTRAITS=1;
 Format labels=yes missing=? separator=Comma;
 TraitLabels num;
 Matrix
 Hap_1 1
 Hap_2 4
 Hap_3 1
 Hap_4 3
 Hap_5 1
 Hap_6 5
 Hap_7 1
 Hap_8 1
 Hap_9 4
 Hap_10 1
 Hap_11 2
 Hap_12 1
 Hap_13 1
 Hap_14 1
 Hap_15 1
 Hap_16 1
 Hap_17 3
 Hap_18 1
 Hap_19 2
 Hap_20 1
 Hap_21 1
 Hap_22 2
 Hap_23 1
 Hap_24 1
 Hap_25 3
 Hap_26 1
 Hap_27 1
 Hap_28 1
 Hap_29 1
 ;
 END;
 ```
