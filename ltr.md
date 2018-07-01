# Learning2Rank

LTR，即机器学习排序算法。在搜索，问答，信息抽取，推荐系统中有应用。
我们可以以搜索为框架思考一下LTR的一些算法。
在搜索框架中，一个用户问题userQuery会首先通过分词/关键词抽取/实体抽取等一系列NLP操作，之后借助找到的相关词在索引中寻找可能相关的材料（网页/文本）保证召回率，之后再通过打分排序保证TOPx搜索结果的准确率。
LTR就是用在打分排序这一部分的算法。由此我们可以这样表述LTR的目标。

## 目标
对于 用户给出的问题 Query 和 系统给出的待排文档表 DocList, LTR根据Query DocList中的文档打分可以满足 相关文档的得分高，不相关文档的得分低。

这样一个定性的表述，很难充分衡量LTR算法的好坏，因此，LTR算法有其一套定量的测评标准。

##给排序结果打分 
是骡子是马拉出来溜溜，对LTR算法的打分即对其排序结果的打分。
-    MAP, mean average precision
-    DCG/NDCG, 
-    topN precision
-    topN NDCG
-    mean reciprocal rank
-    Kendall's tau
-    Spearman's Rho

## 