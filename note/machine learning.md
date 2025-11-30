AI 基础 · 回归 & 决策树  
Learning Notes (中英双语 | 逐点拆解 | 全知识点覆盖 | 带例子)

================================================
0. 课程定位
------------------------------------------------
四份课件的逻辑顺序  
L37 → L38 → L39 → L40  
Intro to ML → Regression I → Regression II → Regression III  
（决策树在 L37 出现，随后回归连续讲三讲）

================================================
PART-A  L37 机器学习概论 (Introduction to Machine Learning)
================================================
1. 为什么需要学习？Why Learning?
------------------------------------------------
- 设计者无法全知 (lack of omniscience)  
- 环境未知或动态变化 → 靠数据修正模型  
- 例：垃圾邮件过滤规则不可能人工写全，让程序自己从用户“点 spam/非 spam”中更新。

2. 学习的形式化定义 Formal Definition
------------------------------------------------
Mitchell 1998：  
A computer program is said to learn from experience E w.r.t. task T and performance measure P, if its performance on T, as measured by P, improves with experience E.  
例：  
T=分类邮件为 spam；E=用户手动标记；P=被正确分类的比例。

3. 学习范式 Taxonomy（What / From / For / How / Output）
------------------------------------------------
What：parameters, structure, hidden concepts  
From：supervised, unsupervised, reinforcement  
For：prediction, diagnostics, summarisation  
How：passive (旁观数据), active (主动提问), online, offline  
Output：classification, regression, clustering, …  
⇒ 用表格记忆，横轴是“数据有没有标签”，纵轴是“想得到什么输出”。

4. 监督 vs. 非监督 Supervised vs Unsupervised
------------------------------------------------
Supervised：每条样本 (x⁽ⁱ⁾, y⁽ⁱ⁾) 有“标准答案”。  
- 回归 y∈ℝ：房价预测  
- 分类 y∈{0,1,…}：肿瘤良性/恶性  

Unsupervised：只有 x⁽ⁱ⁾，没有 y。  
- 聚类：把新闻自动聚成同一事件  
- 降维：可视化高维基因数据  

小练习：  
“给定已标记地震/爆炸数据，训练一个区分器”→ Supervised  
“给定网页文章，自动发现话题”→ Unsupervised

5. 过拟合 Overfitting
------------------------------------------------
定义：模型太贴合训练集，导致测试集误差高。  
表现：训练误差 ↓，测试误差 ↑。  
原因：假设空间过大 & 数据噪声/不足。  
例子：用 12 次多项式拟合 10 个点，穿过所有点但震荡剧烈。  
预防：  
- 交叉验证 Cross-validation（train/validation/test 划分）  
- 奥卡姆剃刀：同样精度选简单模型  
- 剪枝、正则化、早停、数据增强

6. 决策树 Decision Trees
------------------------------------------------
6.1 表示能力 Representational Power  
- 可表达任意布尔函数；根→叶的一条路径 = 一个合取式；整棵树 = 析取式。  
- 若属性连续，可通过“阈值拆分”转为布尔测试。

6.2 何时好用？  
- 实例用“属性-值”描述，属性取值少  
- 目标函数离散  
- 需要可解释的“if-else”规则  
- 训练集可含缺失值 & 噪声

6.3 学习算法（贪心递归）  
伪代码核心：  
function Decision-Tree-Learning(examples, attributes, parent_examples)  
1) 若 examples 空 → 返回 parent_examples 的多数类  
2) 若全同一类 → 返回该类  
3) 若属性用完 → 返回当前多数类  
4) 选信息增益最大属性 A ← argmax Gain(a)  
5) 对 A 的每个取值 vₖ：  
   exs ← {e∈examples | e.A = vₖ}  
   subtree ← 递归调用(exs, attributes–A, examples)  
   把分支 (A=vₖ, subtree) 挂到树

6.4 信息增益 Information Gain  
熵 Entropy：B(q) = –q log₂q – (1–q)log₂(1–q)  
反映“还需多少位才能确定类别”。  
Gain(A) = B(p/(p+n)) – Remainder(A)  
其中 Remainder 是按 A 分割后的加权平均熵。  
例子：餐厅 12 样本，Patrons 增益 0.541 bits，Type 增益 0 bits → 优先选 Patrons。

6.5 避免过拟合的策略  
- 预剪枝：提前停止（难定阈值）  
- 后剪枝：先过拟合，再用验证集剪回去（常用）  
  Reduced-Error Pruning：逐节点试剪，若验证精度不下降则真剪。  
- 处理连续值：对每个连续属性排序，取使信息增益最大的切点。  
- 处理缺失：①最常见值填充；②概率填充；③分裂时把缺失样本按比例分给各分支。

================================================
PART-B  L38 回归 I (Linear Regression I)
================================================
1. 任务定义 Task Definition
------------------------------------------------
监督学习 → 输出 y∈ℝ → 回归问题  
例：Portland 房价，输入 x=面积(ft²)，输出 y=价格(\$1000s)

2. 符号 Notation
------------------------------------------------
m：训练样本数  
(x⁽ⁱ⁾, y⁽ⁱ⁾)：第 i 个样本  
x：输入特征（可多维，本节先讲单变量）  
y：目标变量

3. 假设表示 Hypothesis Representation
------------------------------------------------
单变量线性回归 h_w(x) = w₀ + w₁x  
w₀：截距 intercept  
w₁：斜率 slope  
图像：一条直线；目标：找最佳 (w₀,w₁) 使预测尽量靠近真值。

4. 如何选择参数？→ 进入 L39
------------------------------------------------
先直观：画多条直线，肉眼挑最好的一条 → 需量化“好坏”→ 引入代价函数。

================================================
PART-C  L39 回归 II (Cost Function & Error Surface)
================================================
1. 代价函数 Cost Function (均方误差 MSE)
------------------------------------------------
定义：  
Loss(w₀,w₁) = 1/(2m) Σᵢⁿ (h_w(x⁽ⁱ⁾)–y⁽ⁱ⁾)²  
加 ½ 仅为求导后消 2，方便手写。  
目标：min_{w₀,w₁} Loss(w₀,w₁)

2. 一参数简化版直觉 Simplified Intuition
------------------------------------------------
令 w₀=0，则 h_w(x)=w₁x  
Loss(w₁) = 1/(2m) Σ (w₁x⁽ⁱ⁾–y⁽ⁱ⁾)²  
画曲线：开口向上的抛物线，唯一最小值。  
例子：3 个点 (1,1), (2,2), (3,3) → w₁=1 时 Loss=0；w₁=0.5 时 Loss≈0.58。

3. 两参数情形 Error Surface
------------------------------------------------
Loss(w₀,w₁) 是二元二次函数 → 3D 图呈“碗形”(convex bowl)。  
等高线：同心椭圆；中心即全局最优。  
凸性保证：无局部极小，梯度下降必达全局最优点。

4. 误差函数 vs. 代价函数
------------------------------------------------
两词常混用，本课把“Error”用于单个样本的差值，“Cost”用于整体平均损失。

================================================
PART-D  L40 回归 III (Gradient Descent & Regression for Classification)
================================================
1. 梯度下降算法 Gradient Descent
------------------------------------------------
目的：最小化任意可微函数；这里用来找最小 Loss。  
伪代码（同步更新）：  
repeat until convergence {  
  temp0 ← w₀ – α·(1/m)Σ(h⁽ⁱ⁾–y⁽ⁱ⁾)  
  temp1 ← w₁ – α·(1/m)Σ(h⁽ⁱ⁾–y⁽ⁱ⁾)x⁽ⁱ⁾  
  w₀ := temp0; w₁ := temp1  
}  
α：学习率，若太小收敛慢，太大可能发散。  
技巧：  
- 特征缩放 (feature scaling) 可让等高线更圆，加快下降。  
- 固定 α 即可收敛，因为靠近极小值时梯度自动变小。

2. 凸性 Convexity
------------------------------------------------
线性回归的 MSE 是凸函数 ⇒ 梯度下降保证找到全局最优。  
对比：神经网络代价函数非凸，可能卡在局部极小。

3. 回归用于分类 Regression for Classification
------------------------------------------------
3.1 硬阈值线性分类器 Linear Classifier with Hard Threshold  
hw(x)=Threshold(w·x), Threshold(z)=1 if z≥0 else 0  
决策边界：超平面 w·x=0  
更新规则（感知机 Perceptron）：  
w_i ← w_i + α(y–h_w(x))·x_i  
解释：  
- 预测正确 → 不更新  
- y=1 但预测 0 → 加大 w 与 x 同向分量  
- y=0 但预测 1 → 减小 w 与 x 同向分量  
缺点：仅对线性可分数据收敛；对噪声/重叠数据震荡。

3.2 软阈值 —— 逻辑回归 Logistic Regression  
用可微的 Sigmoid 代替阶跃：  
σ(z)=1/(1+e^(–z))  
假设：h_w(x)=σ(w·x)∈(0,1)  
解释成分：P(y=1|x;w)  
损失：交叉熵 Cross-entropy  
优化：同样用梯度下降，导数形式与线性回归类似但多乘 σ'(z)。  
优点：  
- 可输出概率  
- 可处理非线性可分 & 带噪声数据  
- 凸损失，保证收敛  
实验对比：地震/爆炸数据，硬阈值需 700 轮且精度 90%，逻辑回归 5000 轮后 >99%。

================================================
PART-E  一站式速查表 Cheat-Sheet
================================================
| 场景 | 算法 | 假设/模型 | 损失 | 优化 | 注意 |
| 回归 | 线性回归 | h=w₀+w₁x | MSE | 梯度下降 | 特征缩放 |
| 二分类 | 感知机 | threshold(w·x) | 0-1 误分 | 感知机更新 | 仅线性可分 |
| 二分类 | 逻辑回归 | σ(w·x) | 交叉熵 | 梯度下降 | 输出概率、凸 |
| 非线性 | ？ | ？ | ？ | ？ | 下节课神经网络 |

================================================
PART-F  最小可运行 Python 片段（供实验）
================================================
```python
import numpy as np
import matplotlib.pyplot as plt
plt.style.use('seaborn')

# 1. 造数据
X = np.array([1, 2, 3, 4])
y = np.array([1, 2.2, 2.9, 4.1])
m = len(y)

# 2. 梯度下降
alpha = 0.01
w0, w1 = 0, 0
for epoch in range(10000):
    h = w0 + w1*X
    temp0 = w0 - alpha * (1/m) * np.sum(h - y)
    temp1 = w1 - alpha * (1/m) * np.sum((h - y)*X)
    w0, w1 = temp0, temp1
print(f"拟合直线: h(x) = {w0:.3f} + {w1:.3f}x")

# 3. 画图
plt.scatter(X, y, label='Data')
plt.plot(X, w0 + w1*X, c='r', label='Linear fit')
plt.legend(); plt.show()
```
跑通后：  
- 改 α 看发散/变慢  
- 换 MSE 为交叉熵 + sigmoid 即变逻辑回归

================================================
PART-G  延伸阅读 & 关键词
================================================
关键词：MSE, Gradient Descent, Learning Rate, Convexity, Sigmoid, Cross-Entropy, Perceptron, Overfitting, Information Gain, Reduced-Error Pruning  
书：Russell & Norvig Ch.19；Andrew Ng CS229 讲义  
工具：scikit-learn LinearRegression / LogisticRegression，graphviz 画决策树

================================================