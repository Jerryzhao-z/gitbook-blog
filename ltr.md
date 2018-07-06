# Learning2Rank

LTR，即一类使用机器学习排序的算法。在搜索，问答，信息抽取，推荐系统中有应用。  
我们可以以搜索为框架思考一下LTR的一些算法。  
在搜索框架中，一个用户问题userQuery会首先通过分词/关键词抽取/实体抽取等一系列NLP操作，之后借助找到的相关词在索引中寻找可能相关的材料（网页/文本）保证召回率，之后再通过打分排序保证TOPx搜索结果的准确率。

传统的排序模型主要是使用网页权威度/词频等特征进行打分。主要分为两大类模型：

* RRM, Relevance Ranking Model
  * Boolean Model
  * Vector Space Model
  * LSA
  * BM25
* IRM, Importance Ranking Model
  * PageRank
  * HITS
  * HillTop
  * TrustRank

LTR就是用在打分排序这一部分的算法。由此我们可以这样表述LTR的目标。

## 目标

对于 用户给出的问题 Query 和 系统给出的待排文档表 DocList, LTR根据Query DocList中的文档打分可以满足 相关文档的得分高，不相关文档的得分低。

这样一个定性的表述，很难充分衡量LTR算法的好坏，因此，LTR算法有其一套定量的测评标准。

## 给排序结果打分

是骡子是马拉出来溜溜，对LTR算法的打分即对其排序结果的打分。

* MAP, mean average precision
* DCG/NDCG, 
* topN precision
* topN NDCG
* MRR, mean reciprocal rank
* Kendall's tau
* Spearman's Rho
* ERR, Expected reciprocal rank

MAP，topN precision，MRR用于binary定义的数据（即标签为相关/不相关）。  
而非binary的数据（即以level划分相关度）常常使用NDCG和ERR作为评测标准。

#### NDCG

#### ERR

## 数据

特征数据包含以下几种：

* query feature
* document feature
* query-doc feature
* statitics features based on previous features

## 算法

根据刘铁岩博士的分法（[http://wwwconference.org/www2009/pdf/T7A-LEARNING TO RANK TUTORIAL.pdf](http://wwwconference.org/www2009/pdf/T7A-LEARNING TO RANK TUTORIAL.pdf) ），LTR系列算法中可以分为三种类型：

* Pointwise approach
* Pairwise approach
* Listwise approach

Pointwise approach 假设每个query和doc都可以算出一个打分，而这个打分可以使用regression problem解决，即给出一个\(query, doc\)对, 预测其分数。

Pairwise approach 则把排序问题转化为分类：对于一个用户query和 doc1， doc2，正确的排序是 \(doc1, doc2\)还是\(doc2, doc1\)

Listwise approach 则试图直接优化评价测度，这里的难点是对排序结果进行评价的测度都是非连续的函数，很难直接带入损失函数进行优化。

| year | name | type |
| :--- | :--- | :--- |
| 1989 | OPRF | pointwise |
| 1992 | SLR | pointwise |
| 1999 | MART | pairwise |
| 2000 | Ranking SVM | pairwise |
| 2002 | Pranking | pointwise |
| 2003 | RankBoost | pairwise |
| 2005 | RankNet | pairwise |
| 2006 | IR-SVM | pairwise |
| 2006 | LambdaRank | pairwise/listwise |
| 2007 | AdaRank | listwise |
| 2007 | FRank | pairwise |
| 2007 | GBRank | pairwise |
| 2007 | ListNet | listwise |
| 2007 | McRank | pointwise |
| 2007 | QBRank | pairwise |
| 2007 | RankCosine | listwise |
| 2007 | RankGP | listwise |
| 2007 | RankRLS | pairwise |
| 2007 | SVM | listwise |
| 2008 | LambdaMart | pairwise/listwise |
| 2008 | ListMLE | listwise |
| 2008 | PermuRank | listwise |
| 2008 | SoftRank | listwise |
| 2008 | Ranking Refinement | pairwise |
| 2008 | SSRankBoost | pairwise |
| 2008 | SortNet | pairwise |
| 2009 | MPBoost | pariwise |
| 2009 | BoltzRank | listwise |
| 2009 | BayesRank | listwise |
| 2010 | NDCG Boost | listwise |
| 2010 | GBlend | pairwise |
| 2010 | IntervalRank | pairwise&listwise |
| 2010 | CRP | pointwise&pairwise |
| 2017 | ES-Rank | listwise |

#### 细数 pointwise算法

最初的pointwise是OPRF，使用多项式回归拟合\(query, doc\)打分。  
SLR则使用Staged logistic regression.  
Pranking对应Ordinal regression

McRank将排序问题转为多分类预测相关度level。

#### 常用的pairwise和listwise算法

比较常用的做法是pairwise或者listwise，在实验中证明效果好于pointwise算法。其中比较常用的是LambdaMart算法。这一算法源于RankNet, 之后演化为LambdaRank, 最终结合Mart提出了这一广泛应用的算法。

##### RankNet
RankNet是个存粹的Pairwise算法，RankNet这个名字，源于使用NeuralNet打分S。
RankNet 输入 一对（query, doc）,使用NN打分，之后做softmax归一化获得这一对（query, doc）某一doc排名比另一个排名靠前的概率
![](/assets/softmax.jpg)
最后结合标注结果使用CrossEntropy计算Loss
![](/assets/crossentropy.png)
由此，我们可以使用SGD对打分用的NN做优化。
##### LambdaRank
LambdaRank的发展源于对RankNet的Loss function的分析。

##### Mart


##### LambdaMart
