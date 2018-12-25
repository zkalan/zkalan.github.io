[TOC]

# 统计分析
## 自然语言处理中的歧义
CFG赋予语言一种层次化的结构，但是这种层次化结构往往受到模糊性的挑战。根据一个CFG来构建语法分析树，常常会得到不止一个结果。
## 基于概率的上下文无关文法
PCFG是一个五元组，定义为$(S,\sigma,R,N,P)$。这与CFG类似，知识多出一个$P$，表示语料中规则出现的概率，使用$P$定义一棵语法树出现的概率为树中所有出现的规则概率之积。当一个句子在可选范围内能生成多个语法树时，可以选择概率较大的语法树。
### PCFG的训练
对于PCFG中的CFG部分，一般由语言学领域的专家给出。而PCFG中的$P$是从语料库中统计而来，运用最大似然估计，如下所示：
$P(X\rightarrow Y)=\frac{count(X\rightarrow Y)}{count(X)}$
### 使用PCFG进行语法树解析
在使用CFG产生语法树时，通常采用左递归的方法，从S开始，每次使用一条规则的右部替换左部中第一个非终结符，直到左部中全为待解析句子的但词序列为止。这种方法的解析时间按照句子长度指数级增长，而使用CKY算法，可以降低为多项式级。

CKY是一种DP算法，其要求PCFG中的R必须满足Chromsky Normal Form，即
- $X\rightarrow Y_1Y_2$，其中$Y_1$和$Y_2$为非终结符
- $X\rightarrow Y$，其中$Y$为终结符

满足了这种形式后，可以定义CKY的最优子结构为$\pi (i,j,X)$，其表示以$X$为根节点的非终结符，涵盖句子中第i个到第j个单词的语法树的概率。在这样的顶一下，目标是找到一棵语法树t，使得$\pi (i,n,S)$最大。DP算法的特点就是问题的解释贪婪的，较大问题的解依赖于较小问题的解。按照自底向上的方式，从解决较小问题开始，逐渐构建对于目标问题的解，CKY定义的递归如下：
- Basic case: $\pi (i,i,X)=P(X\rightarrow w_i)$
- Recursive case: $\pi (i,j,X)=max(P(X\rightarrow YZ)*\pi (i,s,Y)*\pi (s+1,j,Z))$
  
#### Probabilistic CKY
- $
\left\{\begin{matrix}
P(X\rightarrow x_i) &if X\rightarrow x_i\in R\\ 
0 &otherwise
\end{matrix}\right.
$
- $\pi (i,i,X)=\sum_{X\rightarrow XY\in R,\\s\in \{i...(j-1)\}}(P(x\rightarrow YZ)\times \pi (i,s,Y)\times \pi (s+1,j,Z))$
PCKY和CKY的区别在于把递归公式中的max改为sum
[TOC]
