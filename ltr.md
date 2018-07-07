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

LambdaRank的发展源于对RankNet的Loss function的分析。设$$S_{ij}=[0,1,-1]$$表示doc\(i\)和doc\(j\)相关度相同，i大于j，j大于i三种情况。这三种情况带入Loss function可以将简化为  
![](/assets/simplifiedCrossEntropy.png)  
分析这个函数，可以画出一下曲线，横轴是$$s_i - s_j$$, 竖轴是Cost。可见，当i文档度高于j文档时，$$s_i - s_j$$越大，也就是与标注一致，Cost越接近0，越小，Cost越接近线性  
![](/assets/lambda.png)
由此，我们在做梯度下降时，可以展开为  
![](/assets/SGD.png)  
其中  
![](/assets/costDerivative.png)
全面展开后格式为  
![](/assets/costExtends.png)

这样的一个定义，我们假设参数$$\sigma=1$$, 
$$S_{ij}=-1$$时，$${\lambda}_{ij} = 1- 1/(1+exp(s_i-s_j))$$, 
$$S_{ij}=1$$时，$${\lambda}_{ij} = -1/(1+exp(s_i-s_j))$$,
![](/assets/lambdaij.png)
横轴是$$s_i-s_j$$

对于一系列这样的(Doc(i), Doc(j))对给定query的相关度对比数据,我们可以整合$$\lambda_{ij}$$为$$\lambda_i=\sum_{j:(i,j)}\lambda_{ij}+\sum_{j:(j,i)}\lambda_{ji}=\sum_{j:(i,j)}\lambda_{ij}-\sum_{j:(j,i)}\lambda_{ij}$$

这里 $$(i,j)$$表示标注数据为i比j文档更相关。

![](/assets/costGradient.png)

$$\lambda_i$$ 如我们所见是一个负数，决定了doc(i)在迭代中的移动方向和幅度。如果完美排序后，排在doc(i)之前的doc越少，之后的越多，那么文档doc(i)向前移动的幅度越大，$$|\lambda|$$越大

LambdaRank的创新点在把评价测度引入Loss计算，变成了一个Listwise算法。
我们考虑标注数据只考虑类似(i,j)的情况（(j,i)标注为-1时交换一下可以转换过来）），可定义$$\lambda_{ij}$$

$$\lambda_{ij}\mathrel{\stackrel{\mbox{def}}{=}} - \frac{\sigma}{1 + \exp\bigl(\sigma\cdot(s_i - s_j)\bigr)}\cdot\lvert\Delta Z_{ij}\rvert.$$

其中$$\Delta Z_{ij}$$表示交换doc(i)和doc(j)获得的评价测度收益。

由此，对模型更新不再是优化cost function，而是通过迭代$$\lambda_i$$和NN,最终使用某一评测标准做早停。

##### Mart

Mart，Multiple Additive Regression Tree又称为GBRT, Gradient Boost Decision Tree. Boost是一种典型的ensembling方法，ensembling即通过学习多个learner，共同构造一个强learner，Boost是一种串行学习的一种算法，每次个learner的学习目标都是改建之前leaner的结果。

_使用决策树来预测结果；
用到的决策树有很多个；
每个树都比之前的树改进一点点，逐渐回归、拟合到真实结果。_ _liam0205.me_

原始的Boost是用来分类的，通过放大被错误分类的sample的权重，组合多个模型，通过加权或投票获得结果。 GBDT的树是回归树，不是分类树，Learner拟合残差，最终输出结果是各个树估算结果的累加。

![](/assets/gradientBoost.png)
```
L: loss
Y, X: output, input
F: 预测function，即累加得到的预测函数
H(x,a): 以a为parameter的函数，每次迭代学到的弱learner

初始化：选取满足argmin条件的constant 𝜌 初始化函数F
For M轮迭代：
	对loss函数做偏导，对于每个数据，带入x计算相应的偏移y’ pseudo-residuals
	学习一棵拟合y’的函数，即一个regression function. 
	line search获得步长/学习率 
	更新模型，旧F与相应增量相加获得新的F
```

以上是Gradient Boost算法，当与Regression Tree结合时，就是GBRT了。
GBRT的H(x,a)就是一个decision tree. 一颗有$$J_m$$个叶节点的Decision Tree可以根据特征把空间分成$$J_m$$个相互之间不相交的子空间$$R_{1,m}, R_{2,m}... R_{m,m}$$


##### Mart for two class classification

便于回顾Mart，我们跟随 _From RankNet to LambdaRank to LambdaMART: An Overview_ 一起针对二分类问题使用Mart。

定义$$P_{+}=P(y=1|x), P_{-}=P(y=-1|x)$$模型给出的条件概率，$$I_{+}(x_i)=1 如果y_i=1，否则为0, I_{-}(x_i)=1表示y_i=-1,否则为0$$，由此写出cross entropy loss

![](/assets/crossentropyMart.png)


##### LambdaMart

结合Mart和LambdaRank发现，LamdbaRank可以为Mart提供梯度，构建出如下算法

![](/assets/LambdaMART.png)