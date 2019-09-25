
[TOC]

# 文本分类
## 文本表征学习
- 热编码
- 词袋模型
  - TF-IDF Weighting：$tf_{ij}$表示在文档j中的对象i的频数除以在该文档上频数最高对象的频数（正则化）；$idf_i=log_2(N/df_i)$，其中N表示文档总数，$df_i$表示包含对象i的文档频数
- word2vec、doc2vec

## 朴素贝叶斯模型
$arg max_{c_k}P(c_k|D)=arg max\frac{P(D|C_k)P(c_k)}{P(D)}=arg max_{c_k}P(D|C_k)P(c_k)$
- 如何表示文本$D$
- 如何获得$P(D|C_k)$和$P(C_k)$

### 伯努利文本模型
文档由二进制特征向量表示，其元素表示文档中没有或存在对应的词。
- $D_i$表示第i个文档的特征向量
- $D_{it}$表示$D_i$中，单词$w_t$是否出现，取值0或1
- $P(w_t|c_k)$是单词$w_t$出现在$c_k$类文档中的概率，同样的有$1-P(w_t|c_k)$

$P(D_{it}|c_k)=D_{it}P(w_i|c_k)+(1-D_{it})(1-P(w_i|c_k))$
$P(D_{i}|c_k)=\prod_{t=1}^{|V|}P(D_{it}|c_k)=\prod_{t=1}^{|V|}(D_{it}P(w_t|c_k)+(1-D_{it})(1-P(w_t|c_k)))$
- 记$n_k(w_t)$为单词$w_t$在类别为$c_k$的文档中被观察到的文档的频数
- 记$N_k$为类别$c_k$的文档总数
单词的似然概率为$\hat{P}(w_t|c_k)=\frac{n_k(w_t)}{N_k}$，类别的先验概率$\hat{P}(c_k)=\frac{N_k}{N}$
所以我们有如下公式：
$arg max_{c_k}P(c_k|D)=arg max\frac{P(D|C_k)P(c_k)}{P(D)}=arg max_{c_k}P(c_k)\prod_{t=1}^{|V|}(D_{it}P(w_t|c_k)+(1-D_{it})(1-P(w_t|c_k)))$
### 多项式文档模型：
文档由整数特征向量表示，其元素指示文档中对应词的频率。特征向量包含词语的频率信息。
- $D_i$表示第i个文档的特征向量
- $D_{it}$表示$D_i$中，单词$w_t$出现的次数
- $n_i=\sum_{t}D_{it}$表示文档$D_i$中的单词总数
- $P(w_t|c_k)$表示单词$w_t$出现在$c_k$类文档中的概率
$P(D_i|c_k)=\frac{n_{i}!}{\prod_{t=1}^{|V|}D_{it}!}\prod_{t=1}^{|V|}P(w_t|c_k)^{D_{ij}}$，(注意！一个文档向量能够表示一个文档集合，所以等式右边第一个分式表示的是，根据文档向量能够获得的所有全排列数量，紧接着乘以单词在选定分类下的后验概率，指数表示该单词出现多次。乘在一起就表示所有集合中文档出现的后验概率之和)
- 记$z_{ik}$等于1表示第i个文档为类别$c_k$，否则记为0
- $N_k$表示类别$c_k$下的文档总数
- $N$表示文档总数
单词的似然后验概率为$\hat{P}(w_t|c_k)=\frac{\sum_{i=1}^{N}D_{it}z_{ik}}{\sum_{s=1}^{|V|}\sum_{i=1}^{N}D_{is}z_{ik}}$
类别的先验概率为$\hat{P}(c_k)=\frac{N_k}{N}$
将相关内容代回朴素贝叶斯公式得到如下
$arg max_{c_k}P(c_k|D_j)\\
=arg maxP(D_j|C_k)P(c_k)\\
=arg max_{c_k}P(c_k)\frac{n_{i}!}{\prod_{t=1}^{|V|}D_{it}!}\prod_{t=1}^{|V|}P(w_t|c_k)^{D_{ij}}\\
=arg max_{c_k}P(c_k)\prod_{t=1}^{|V|}P(w_t|c_k)^{D_{ij}}\\
=arg max_{c_k}P(c_k)\prod_{h=1}^{len(D_i)}P(u_h|c_k)$
($u_k$表示$D_i$中的单词)
## 特征选择
有关特征选择需要进一步收集材料，常用的有频数、停止词、互信息、卡方检测。
[TOC]