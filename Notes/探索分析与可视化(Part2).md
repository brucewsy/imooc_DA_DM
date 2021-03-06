# 2. 探索分析与可视化（Part 2）

探索性数据分析（复合分析）可视化

## 2.5 复合分析
- 交叉分析
- 分组分析
- 相关分析
- 因子分析
- 聚类分析（暂略）
- 回归分析（暂略）

### 2.5.1 交叉分析
有一张数据表，最简单的方法，我们可以通过一行或者一列的角度分析。但有时并不能得到最真是最客观的结论，我们忽略了数据间、属性间的关联性。

我们可以任意取两列，根据假设检验来判断两者间是否有联系；或者，以一个或几个属性为行，令一个或几个属性为列，做成一张交叉表（也叫透视表）。

Note:
The relative Jyputer Notebooks in Code folder are:
- 2.5.1 Cross Analysis.ipynb

### 2.5.2 分组分析
分组分析有两种含义：
- 将数据分组后，再进行分析比较；
- 根据数据的特征，将数据进行切分。分成不同的组，是的组内成员尽可能的靠拢，组间的成员尽可能远离。

根据第二个含义，如果指定每条数据的分组，来计算未知数据的分组，这个过程为分类。而如果不知道分组，只是想把数据物以类聚，这个过程为聚类。这部分以后讨论。

这里讨论第一个含义。一般需要结合其他分析方法一起使用，所以分组分析更像是一种辅助的手段，而不是目的性分析。例如在对比分析与交叉分析中，都是讲到了数据分组。常用的手段是：钻取
- 钻取是改变维度的层次，变化分析粒度的过程。

根据方向不同可分：向上钻取与向下钻取。
- 向下钻取：展开数据，查看数据细节的过程；
- 向上钻取：汇总分组数据的过程。

离散数据的分组相对容易，而连续属性的分组，在分组前需要进行离散化。在连续属性的离散化前，可以看下数据分布，看下是否有明显的区分标志。例如，将数据从小到大排列后，有没有明显的分隔或是明显的拐点。
- 连续分组
    - 分隔（一阶差分）：相邻两个数据的差
    - 拐点（二阶差分）
    - 聚类：连续属性的分组要具有相同的分组比较聚拢，不同的分组比较分离的特点。
    - 不纯度（Gini）：考虑标注的话，可以考虑不纯度的指标

- 不纯度（Gini系数）

    $$ Gini(D) = 1 - \sum{(\frac{C_k}{D})^2}$$

    其中，$D$为目标的标注，$C$是相对于关注的属性来说要比较要对比的属性。例如下表中，$X$相对于$Y$是否具有很好的区分度，

    <table>
        <tr>
            <td>A</td>
            <td>B</td>
        </tr>
        <tr>
            <td>X1</td>
            <td>Y1</td>
        </tr>
        <tr>
            <td>X1</td>
            <td>Y1</td>
        </tr>
        <tr>
            <td>X2</td>
            <td>Y1</td>
        </tr>
        <tr>
            <td>X1</td>
            <td>Y2</td>
        </tr>
        <tr>
            <td>X1</td>
            <td>Y2</td>
        </tr>
    </table>

    所以，$Gini$系数为：

    $$
    Gini(D) = 1 - ((\frac{Count_{X1|Y1}}{Count_{Y1}})^2 + (\frac{Count_{X2|Y1}}{Count_{Y1}})^2 + (\frac{Count_{X1|Y2}}{Count_{Y2}})^2 + (\frac{Count_{X2|Y2}}{Count_{Y2}})^2)
    $$

    连续值的$Gini$系数的确定：需要先将表按连续值的大小进行排序，相邻两两间划定界限。分别确定分组值，然后分别计算$Gini$系数，取$Gini$系数最小的为分界。

    例如：

    <table>
        <tr>
            <td>A</td>
            <td>B</td>
        </tr>
        <tr>
            <td>0.1</td>
            <td>Y1</td>
        </tr>
        <tr>
            <td>0.2</td>
            <td>Y1</td>
        </tr>
        <tr>
            <td>0.7</td>
            <td>Y1</td>
        </tr>
        <tr>
            <td>0.8</td>
            <td>Y2</td>
        </tr>
        <tr>
            <td>0.9</td>
            <td>Y2</td>
        </tr>
    </table>

    这里，当切分为$X1 = [0.1, 0.2, 0.7]$，$X2 = [0.8, 0.9]$时，$Gini$系数等于0是最小。所以可以以$0.75$进行切分，将连续数据进行分组。

根据不纯度进行分组，使用最多的是分类模型中的决策树算法中的CART算法，这里以后会解释。

Note:
The relative Jyputer Notebooks in Code folder are:
- 2.5.2 Group Analysis.ipynb

### 2.5.3 相关分析

相关分析是衡量两组数据或两组样本分布趋势或变化趋势大小的分析方法。最常使用的就是相关系数：

- Perason相关系数

    $$r(X,Y) = \frac{Cov(X,Y)}{\sigma_x \sigma_y} = \frac{E[(X-\mu_x)(Y-\mu_y)]}{\sigma_x\sigma_y}$$

- Spearman相关系数

    $$\rho_s = 1 - \frac{6\sum{d_i^2}}{n(n^2-1)}$$

这里的相关分析是用相关系数直接衡量连续值的相关性，而离散属性的相关性如何分析？

先考虑一个特例：二类离散属性的相关性。二类属性是可以直接用Pearson相关系数来计算，可能会有失真。也可以用过不纯度计算。
多类离散属性的数据，如果是定序数据的话例如low、media、high，可以编码为0、1、2进行Pearson相关系数的计算，但这样会有失真。

更为一般的，可以使用熵。熵是用来衡量一个不确定性的值。如果值接近于0，不确定值越小。如果$\log$以2为底，那么熵的单位是bit。
- 熵

    $$H(X)=-\sum{p_i \log(p_i)}$$
- 条件熵

    $$H(Y|X)=\sum{p(x_i)H(Y|X=x_i)}$$
- 互信息（熵增益）

    $$I(X,Y)=H(Y)-H(Y|X)=H(X)-H(X|Y)$$

    缺点，对于分类数目多的特征有不正确的偏向，即不具有归一化的特点。它的不确定性是上不封顶，对于界定相关性不方便。
- 熵增益率

    $$GainRatio(X \to Y) = \frac{I(X, Y)}{H(Y)}$$

    值是介于$(0,1)$之间，但是它是不对称性，即$GainRatio(X \to Y) \neq GainRatio(Y \to X)$。
- 相关性

    $$Corr(X,Y)=\frac{I(X,Y)}{\sqrt{H(X)H(Y)}}$$

    值是介于$(0,1)$之间，同时满足对称性。

Note:
The relative Jyputer Notebooks in Code folder are:
- 2.5.3 Correlation Analysis.ipynb

### 2.5.4 因子分析

从多个属性变量中分析共性、相关因子的方法。
因子分析可以分为：
- 探索性因子分析

    通过协方差矩阵、相关性矩阵等指标，分析多元属性变量的本质结构，并可以进行转化、降维等操作。得到数据空间中，或者一小目标属性的最主要因子。例如主成分分析法。
- 验证性因子分析
    
    验证一个因子与我们关注的属性之间是否有关联、有什么样的关联，是不是符合我们的预期等等。例如假设检验、相关分析、回归分析等等。

例如：
- 用户对某产品时都满意？（满意度）
    信息可以包括 编号、年龄、性别、学历。。。生活习惯、行为习惯。。。

    关注满意情况，可以用主成分分析，把除满意度情况外的全部因子进行主成分降维，然后直接分子降维后的因子与满意情况进行对比分析。或者也可以直接看相关矩阵，看哪个其他属性与关注属性有比较强的关系。

    或者想了解某个属性与满意度的关系，我们就会去验证这个属性与满意度是否有关系，得到像相关性、一致性的指标，或者可以直观上猜测某几个属性与满意度有关系，也可以用回归方法去拟合这几个属性与满意度的关联，根据误差大小来应验假设是都准确。

Note:
The relative Jyputer Notebooks in Code folder are:
- 2.5.4 Factor Analysis.ipynb

## 2.6 小结

<table align='center'>
    <tr>
        <td align='center'>数据类型</td>
        <td align='center'>可用方法</td>
    </tr>
    <tr>
        <td>连续 --- 连续</td>
        <td>相关系数、假设检验</td>
    </tr>
    <tr>
        <td>连续 --- 离散（二值）</td>
        <td>相关系数，连续二值化（最小Gini切分，最大熵增益切分）</td>
    </tr>
    <tr>
        <td>连续 --- 离散（非二值）</td>
        <td>相关系数（定序）</td>
    </tr>
    <tr>
        <td>离散（二值） --- 离散（二值）</td>
        <td>相关系数，熵相关，F分值</td>
    </tr>
    <tr>
        <td>离散--- 离散（非二值）</td>
        <td>熵相关，Gini，相关系数（定序）</td>
    </tr>
</table>