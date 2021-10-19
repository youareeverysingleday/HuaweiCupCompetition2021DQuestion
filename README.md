# HuaweiCup Competition 2021 D Question

- 相比于原始代码。对没必要的代码进行了删除。同时为了方便上传，赛后重新完整运行了一次。
- 编译环境如下：

|编号|开发环境名称|版本|
|---|---|---|
|1|windows|1803|
|2|python|3.7.9|
|3|pandas|1.3.0|
|4|numpy|1.19.5|
|5|scikit-learn|0.24.2|
|6|tensorflow|2.5.0|
|7|scikit-opt|0.6.5|
|8|matplotlib|3.4.2|
|9|seaborn|0.11.2|
|10|vscode|1.16.1|
|11|jupyter|1.0.0|
|12|markdown|3.3.4|
|13|xgboost|1.4.2|
|14|math|defualt|
|15|keras-nightly|2.5.0|

## 1. 问题1

- 目标：对分子描述符进行特征排序。共用三步，步骤如下：
- 步骤：
  1. 第一步，对数据进行样本分析。对其化学含义、统计特性和特征具体性质进行掌握。
  2. 第二步，对数据进行预处理。考虑到无法穷尽分子组合和未来分子化合物的发展的不确定性，对于数据处理考虑的重点在于：1. 保留特征的存在性；2. 降低大量缺失值特征对最终结果的影响性。因此采用同类特征均值去整填充。
  3. 第三步，对特征进行排序。采用利用极限梯度提升算法以化合物对ERα的生物活性值为标签，对分子描述符特征进行重要性打分，依据特征权重的大小，对特征进行排序，进而确定前20个对生物活性产生显著影响的分子描述符变量。

## 2. 问题2

- 针对问题二：目标：构建有效的对ERα的生物活性连续值进行预测的模型。
- 共用二步，步骤如下：
  1. 第一步，去除强相关性特征。通过第一问中方法筛选出权重值由高向低排列的前40个主要特征进行相关性分析，以方便去除特征之后递补。将强相关性变量按照保留高权重，去掉低权重的方法保留20个主要特征。
  2. 第二步，构建模型。针对目标值为连续值的特点，建立多种模型对ERα的生物活性值进行预测，并对多种模型采用多个评价指标进行评估，选择出其中评价情况最优的模型。第三步对测试数据进行预测。

## 3. 问题3

- 针对问题三：目标：构建有效的对Caco-2、CYP3A4、hERG、HOB、MN五个离散值进行预测的二分类模型。
- 共用二步，步骤如下：
  1. 第一步，选择模型。在第一问和第二问的基础上对五个目标选择XGboost、LR逻辑回归、SVM支持向量机、随机森林算法分别构建化合物的五个ADMET性质的分类预测模型。采用ROC、TP等方法对模型进行评估，选择评价结果最优模型。
  2. 第二步：采用模型对测试数据进行预测。

## 4. 问题4

- 针对问题四：目标：在两个约束条件下，对特征数据进行优化，最终得出最优特征取值和活性值。考虑将两个约束条件进行分解然后分步完成。
- 共用三步，步骤如下：
  1. 第一步：按ADMET性质对特征进行筛选。在考虑hERG和MN是毒性表示为1的情况下，按照Caco-2、CYP3A4、hERG、HOB、MN标识情况对特征进行条件过滤，选择至少三个性质较好的特征。由此产生的数据不均衡问题，通过去采样来保持正负样本的均衡性。
  2. 第二步，重新训练模型。在第二、三问的基础上以plc50为目标值，以第一步中筛选出来的特征为训练和验证集对模型进行重新训练模型。
  3. 第三步；对数据进行优化并求解。将化学问题转化为数学问题进行求解。将第二个约束条件plc50值最大为智能优化算法的适应度函数，使用遗传算法、粒子群算法等相关智能优化算法对第二个约束进行求解，并最终得出结果。
