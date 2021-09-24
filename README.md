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
 コメント行に含まれるID名は，hCoV-19/Japan/SZ-NIG-Y21018/2021のような株名のみにしたほうが良い．
 
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

NEXUSファイルの記載内容について
```
#各HapIDに含まれる株情報【Hap# 件数　株名（スペース区切り）】
[Hap#  Freq. Sequences]
[Hap_1:  1    MN908947.3]
[Hap_2:  4    hCoV-19/Japan/SZ-NIG-Y21018/2021 hCoV-19/Japan/SZ-NIG-Y21027/2021 hCoV-19/Japan/SZ-NIG-Y21043/2021 hCoV-19/Japan/SZ-NIG-Y21073/2021]
[Hap_3:  1    hCoV-19/Japan/SZ-NIG-Y21022/2021]
[Hap_4:  3    hCoV-19/Japan/SZ-NIG-Y21023/2021 hCoV-19/Japan/SZ-NIG-Y21047/2021 hCoV-19/Japan/SZ-NIG-21465/2021]
[Hap_5:  1    hCoV-19/Japan/SZ-NIG-Y21024/2021]
[Hap_6:  5    hCoV-19/Japan/SZ-NIG-Y21028/2021 hCoV-19/Japan/SZ-NIG-Y21029/2021 hCoV-19/Japan/SZ-NIG-Y21059/2021 hCoV-19/Japan/SZ-NIG-Y21099/2021 hCoV-19/Japan/SZ-NIG-Y21104/2021]
[Hap_7:  1    hCoV-19/Japan/SZ-NIG-Y21030/2021]
[Hap_8:  1    hCoV-19/Japan/SZ-NIG-Y21031/2021]
[Hap_9:  4    hCoV-19/Japan/SZ-NIG-Y21032/2021 hCoV-19/Japan/SZ-NIG-Y21033/2021 hCoV-19/Japan/SZ-NIG-Y21035/2021 hCoV-19/Japan/SZ-NIG-Y21036/2021]
[Hap_10:  1    hCoV-19/Japan/SZ-NIG-Y21034/2021]
[Hap_11:  2    hCoV-19/Japan/SZ-NIG-Y21037/2021 hCoV-19/Japan/SZ-NIG-Y21038/2021]
[Hap_12:  1    hCoV-19/Japan/SZ-NIG-21313/2021]
[Hap_13:  1    hCoV-19/Japan/SZ-NIG-Y21039/2021]
[Hap_14:  1    hCoV-19/Japan/SZ-NIG-Y21041/2021]
[Hap_15:  1    hCoV-19/Japan/SZ-NIG-Y21044/2021]
[Hap_16:  1    hCoV-19/Japan/SZ-NIG-Y21045/2021]
[Hap_17:  3    hCoV-19/Japan/SZ-NIG-Y21052/2021 hCoV-19/Japan/SZ-NIG-21361/2021 hCoV-19/Japan/SZ-NIG-Y21100/2021]
[Hap_18:  1    hCoV-19/Japan/SZ-NIG-Y21053/2021]
[Hap_19:  2    hCoV-19/Japan/SZ-NIG-Y21054/2021 hCoV-19/Japan/SZ-NIG-Y21061/2021]
[Hap_20:  1    hCoV-19/Japan/SZ-NIG-Y21056/2021]
[Hap_21:  1    hCoV-19/Japan/SZ-NIG-Y21072/2021]
[Hap_22:  2    hCoV-19/Japan/SZ-NIG-Y21074/2021 hCoV-19/Japan/SZ-NIG-Y21075/2021]
[Hap_23:  1    hCoV-19/Japan/SZ-NIG-Y21076/2021]
[Hap_24:  1    hCoV-19/Japan/SZ-NIG-Y21077/2021]
[Hap_25:  3    hCoV-19/Japan/SZ-NIG-Y21078/2021 hCoV-19/Japan/SZ-NIG-21357/2021 hCoV-19/Japan/SZ-NIG-21358/2021]
[Hap_26:  1    hCoV-19/Japan/SZ-NIG-21420/2021]
[Hap_27:  1    hCoV-19/Japan/SZ-NIG-21466/2021]
[Hap_28:  1    hCoV-19/Japan/SZ-NIG-Y21096/2021]
[Hap_29:  1    hCoV-19/Japan/SZ-NIG-21568/2021]


BEGIN CHARACTERS;
DIMENSIONS NCHAR=104;
FORMAT DATATYPE=DNA  MISSING=? GAP=- MATCHCHAR=.;
CHARLABELS #SNVが検出された塩基位置情報．サイト別に記載
	[1]	nt_240	[2]	nt_241	[3]	nt_275	[4]	nt_313	[5]	nt_894
	[6]	nt_913	[7]	nt_1048	[8]	nt_1292	[9]	nt_2048	[10]	nt_2146
	[11]	nt_2445	[12]	nt_2509	[13]	nt_2842	[14]	nt_3037	[15]	nt_3267
	[16]	nt_3695	[17]	nt_4784	[18]	nt_4794	[19]	nt_4904	[20]	nt_5388
	[21]	nt_5724	[22]	nt_5986	[23]	nt_6380	[24]	nt_6954	[25]	nt_8917
	[26]	nt_9724	[27]	nt_10036	[28]	nt_10996	[29]	nt_11173	[30]	nt_11195
	[31]	nt_11620	[32]	nt_11750	[33]	nt_11884	[34]	nt_12049	[35]	nt_12741
	[36]	nt_12747	[37]	nt_13308	[38]	nt_14408	[39]	nt_14676	[40]	nt_15096
	[41]	nt_15279	[42]	nt_15783	[43]	nt_15819	[44]	nt_16176	[45]	nt_17019
	[46]	nt_17247	[47]	nt_17427	[48]	nt_17502	[49]	nt_18049	[50]	nt_18167
	[51]	nt_18486	[52]	nt_19164	[53]	nt_20251	[54]	nt_20429	[55]	nt_20436
	[56]	nt_20844	[57]	nt_21077	[58]	nt_21518	[59]	nt_21600	[60]	nt_21660
	[61]	nt_22482	[62]	nt_22487	[63]	nt_22624	[64]	nt_22661	[65]	nt_23063
	[66]	nt_23271	[67]	nt_23403	[68]	nt_23426	[69]	nt_23557	[70]	nt_23587
	[71]	nt_23604	[72]	nt_23659	[73]	nt_23664	[74]	nt_23709	[75]	nt_24210
	[76]	nt_24506	[77]	nt_24914	[78]	nt_25096	[79]	nt_25549	[80]	nt_26456
	[81]	nt_26464	[82]	nt_26763	[83]	nt_26831	[84]	nt_26834	[85]	nt_27972
	[86]	nt_28048	[87]	nt_28095	[88]	nt_28109	[89]	nt_28111	[90]	nt_28280
	[91]	nt_28281	[92]	nt_28282	[93]	nt_28698	[94]	nt_28881	[95]	nt_28882
	[96]	nt_28883	[97]	nt_28975	[98]	nt_28977	[99]	nt_29250	[100]	nt_29353
	[101]	nt_29468	[102]	nt_29679	[103]	nt_29688	[104]	nt_29697
;
```

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
