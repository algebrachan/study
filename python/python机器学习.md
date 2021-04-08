## 黑马程序员入门python机器学习

### 1.概述

人工智能 >> 机器学习 >> 深度学习

- 用途
  - 自然语言处理
  - 图像识别
  - 传统预测

- 机器学习是人工智能的实现途径

  - 数据
  - 模型
  - 预测

- 深度学习是机器学习的一个方法发展而来

- 机器学习算法分类

  - 监督学习

    目标值：类别 - 分类问题

    目标值：连续型的数据 - 回归问题

  - 无监督学习
  
- 开发流程
  
  - 获取数据
  
  - 数据处理
  
  - 特征工程（处理数据为算法可使用数据）
  
  - 机器学习算法进行训练 - 模型
  
  - 模型评估
  
    

### 2.特征工程

- 可用数据集

  [sklearn](https://scikit-learn.org/stable/)

  [kaggle](https://www.kaggle.com/datasets)

  [uci](http://archive.ics.uci.edu/ml/index.php)

- sklearn：可使用的功能

  - 分类、聚类、回归

  - 模型选择、调优

  ```python
  pip3 install Scikit-learn
  import sklearn
  skleran.datasets
  load_*
  fetch_*
  sklearn.datasets.load_iris()# 获取小数据集
  sklearn.datasets.fetch_xx(data_home=None,subset='all')
  
  # 数据集返回值
  datasets.base.Bunch()#继承字典类型
  # data:特征数据数组 二维数组 [n_samples*n_features]
  # target:标签数组 n_samples的一维 numpy.ndarray数组
  # DESCR:数据描述
  # feature_names:特征名
  # target_names:标签名
  from sklearn.datasets import load_iris
  from sklearn.model_selection import train_test_split

  def datasets_demo():
    """
      sklearn 学习使用
      """
      iris = load_iris()
      # print("数据集2:\n", iris)
      # 数据集划分
      x_train, x_test, y_train, y_test = train_test_split(
          iris.data, iris.target, test_size=0.2, random_state=22)
      print("训练集的特征值x：\n", x_train, x_train.shape)
      print("测试集的特征值x：\n", x_test, x_test.shape)
      print("训练集的特征值y：\n", y_train, y_train.shape)
      print("测试集的特征值y：\n", y_test, y_test.shape)
      return None

  if __name__ == "__main__":
      datasets_demo()
  ```
  
  - 数据集的划分
  
    训练数据：用于训练，构建模型
  
    测试数据：在模型检验时使用，用于评估模型是否有效，测试集 20%~30%

- 特征抽取/特征提取

  - 机器学习算法 - 统计方法 - 数学公式

    - 文本类型 -> 数值
    - 类型 -> 数值

  - 字典特征提取 - 类别 - > one-hot编码

    ```python
    from sklearn.feature_extraction import DictVectorizer
    sklearn.feature_extraction.DictVectorizer(sparse=True)
    DictVectorizer.fit_transform(X) #传入字典 返回sparse矩阵 稀疏矩阵：节省存储
    DictVectorizer.inverse_transform(X)# 转换之前数据格式
    DictVectorizer.get_feature_names()# 返回类别名称
    ```
    
  - 文本特征提取
    
      ```python
      sklearn.feature_extraction.text.CountVectorizer(stop_words=[])# stop_words是不需要的文本
      CountVectorizer.fit_transform(X) # 文本或包含文本字符串的可迭代对象
      CountVectorizer.inverse_transform(X) # 返回转换之前的数据格式
      CountVectorizer.get_feature_names() # 返回单词列表
      
      # 将文本分词
      import jieba
      def cut_word(text):
          text=" ".join(list(jieba.cut(text)))
          return text
      # Tf-idf文本特征提取
      # 总文件数目除以包含该词语之文件的数目，再将得到的商取以10为底的对数得到
      sklearn.feature_extraction.text.TfidfVectorizer(stop_words=[])
      ```
  
- 数据预处理
  
  归一化、标准化：无量纲化
  
    ```python
  from sklearn.preprocessing import MinMaxScaler,StandardScaler
  import pandas as pd
    # 归一化
  def minmax_demo():
  	data = d.read_csv("datine.txt")
  	data = data.iloc[:,:3]
      transfer = MinMaxScaler(feature_range=[2,3])
      data_new = transfer.fit_transform(data)
    # 标准化
  def stand_demo()
    # 重复上述代码
  	transfer = StandardScaler.fit_transform(data)
    ```
  
- 降维
  
    过滤式Filter：方差选择法、相关系数
    
    ```python
    from sklearn.feature_selection import VarianceThreshold
    from scipy.stats import pearsonr
    
    def variance_demo():
    	data = pd.read_csv("xx.csv")
        data = data.iloc[:,1:-2]
    	transfer = VarianceThreshold(threshold=10)
        data_new = transfer.fit_transform(data)
        print("data_new:\n",data_new,data_new.shape)
        # 计算相关系数
        r = pearsonr(data["pe_ratio"],data["pb_ratio"])  
    ```
    

### 3.分类算法

#### 3.1 转换器和估计器

- 转换器是特征工程的父类：实例化transformer类；调用fit_transfrom()

- 估计器estimator是算法的父类：实例化estimator；调用fit(x_train,y_train)特征值

- 模型评估：

```python
# 直接比对真实值和预测值
y_predict = estimator.predict(x_test)
y_test == y_predict
# 计算准确率
accuracy = estimator.score(x_test,y_test)
```

#### 3.2 KNN算法 (k-近邻算法)

- 定义

  如果一个样本在特征空间中的k个最相似(即特征空间中最邻近)的样本中的大多数属于某一个类别,则该样本也属于这个类别。

  需要用到距离公式

- 距离公式

  欧式距离，曼哈顿距离

- k值取得过小，容易收到异常点的影响；k值取得过大，样本不均衡的影响

- 总结：

  - 优点：简单易于理解，易于实现，无需训练
  - 缺点：懒惰算法，内存开销大；必须确定k值
  - 使用场景：小数据场景，几千~几万样本

#### 3.3 模型选择与调优

交叉验证（cross validation）

超参数搜索-网格搜索（Grid Search）

```python
def knn_iris_gscv():
    """
    用knn算法对鸢尾花进行分类,添加网格搜索和交叉验证
    """
    # 1.获取数据
    iris = load_iris()

    # 2.划分数据集
    x_train, x_test, y_train, y_test = train_test_split(iris.data,iris.target,random_state=6)
    # 3.特征工程：标准化
    transfer = StandardScaler()
    x_train = transfer.fit_transform(x_train)
    x_test = transfer.transform(x_test)

    # 4.KNN算法预估器
    estimator = KNeighborsClassifier()
    # 加入网格搜索与交叉验证
    param_dict = {"n_neighbors": [1,3,5,7,9,11]}
    estimator = GridSearchCV(estimator,param_grid=param_dict,cv=10)

    estimator.fit(x_train,y_train)

    # 5.模型评估
    # 比对真实值和预测值
    y_predict = estimator.predict(x_test)
    print("y_predict:\n",y_predict)
    print("直接比对真实值和预测值：\n",y_test == y_predict)
    # 计算准确率
    score = estimator.score(x_test,y_test)
    print("准确率为：\n",score)

    print("最佳参数：\n",estimator.best_params_)
    print("最佳结果: \n",estimator.best_score_)
    print("最佳估计器: \n",estimator.best_estimator_)
    print("交叉验证结果: \n",estimator.cv_results_)
    return None
```



#### 3.4 朴素贝叶斯算法

朴素：假定特征与特征之间是相互独立的

贝叶斯公式

- 优点：

  - 来源于古典数学理论，有稳定的分类效率
  - 对缺失数据不太敏感、算法比较简单、常用于文本分类
  - 分类准确度高，速度快

- 缺点：

  - 由于使用样本属性独立性的假设，所以如果特征属性有关联时其效果不好

  ```python
  def nb_news():
      """
      用朴素贝叶斯算法对新闻进行分类
      """
      # 1.获取数据
      news = fetch_20newsgroups(subset="all")
  
      # 2.划分数据集
      x_train, x_test, y_train, y_test = train_test_split(news.data,news.target)
  
      # 3.特征工程：文本特征抽取
      transfer = TfidfVectorizer()
      x_train = transfer.fit_transform(x_train)
      x_test = transfer.transform(x_test)
  
      # 4.朴素贝叶斯算法预估
      estimator = MultinomialNB()
      estimator.fit(x_train,y_train)
  
      # 5.模型评估
      y_predict = estimator.predict(x_test)
      print("y_predict:\n",y_predict)
      print("直接比对真实值和预测值：\n",y_test == y_predict)
  
      score = estimator.score(x_test,y_test)
      print("准确率为：\n",score)
      return None
  ```

#### 3.5 决策树

- 高效进行决策：特征的先后顺序
- 信息论基础

  - 信息熵

  - 信息增益：
- 优点：可解释能力强
- 缺点：不能推广到过于复杂的数，容易产生过拟合
- 改进：减枝cart算法，随机森林

#### 3.6 随机森林

森林：包含多个决策树的分类器

训练集：特征值、目标值

​	训练集随机 bootstrap 随机有放回抽样

​	特征随机

```python
param ={"n_estimators":[120,200,300,500,800,1200],"max_depth":[5,8,15,25,30]}
# 超参数调优
gc = GridSearchCV(rf,param_grid=param,cv=2)
gc.fit(x_train,y_train)

```

- 总结
  - 在当前所有算法中，具有极好的准确率
  - 能够有效的运行在大数据集上，处理具有高维特征的输入样本，而且不需要降维
  - 能够评估各个特征在分类问题上的重要性

### 4 回归与聚类算法

#### 4.1 线性回归

- 定义：利用回归方程(函数)对一个或多个自变量(特征值)和因变量(目标值)之间关系进行建模的一种分析方式
- 广义线性模型： y=kx+b 线性关系、非线性关系
- 线性回归的损失和优化原理
  - 目标：求模型参数，(系数矩阵)
  - 真实关系: ...
  - 假定关系: Y=AX+B
  - 损失函数/cost/成本函数/目标函数：最小二乘法
- 优化方法-正规方程：不需要学习率，一次运算得出，需要计算方程（转置求解）
- 优化方法-梯度下降：需要选择学习率，需要迭代求解，特征数量较大可以使用

#### 4.2 过拟合和欠拟合

- 过拟合：原始特征过多，存在一些嘈杂特征，模型过于复杂
  - 解决：正则化
    - L1 损失函数+λ惩罚项 LASSO回归
    - L2 损失函数+λ惩罚项 Ridge回归
- 欠拟合：学习到数据的特征过少
  - 解决：增加数据的特征数量

#### 4.3 岭回归

- 带有L2正则化的线性回归-岭回归
  - alpha：正则化力度=惩罚项系数，λ
    - 取值：0~1,1~10
  - solver：会根据数据自动选择优化方法
    - sag：如果数据集、特征都比较大，选择随机梯度下降优化
- 正则化力度越大，权重系数越小；正则化力度越小，权重系数越大

#### 4.4 逻辑回归与二分类

- 线性回归的输出 就是 逻辑回归的输入

  ```python
  sklearn.linear_model.LogisticRegression(solver='liblinear',penalty='l2',C=1.0)
  # solver:优化求解方式
  # penalty:正则化种类
  # C:正则化力度
  ```

- sigmoid函数

- 混淆矩阵：

- 精确率与召回率

  ```python
  sklearn.metrics.classification_report(y_true,y_pred,lables=[],target_names=None)
  # y_true:真实值
  # y_pred:估计器预测目标值
  # labels:指定类别对应的数字
  # target_names:目标类别名称
  # return:每个类别精确率与召回率
  ```

- ROC曲线和AUC指标

  ```python
  sklearn.metrics.roc_auc_score(y_true,y_score)
  # y_true:每个样本的真实类别 0,1
  # y_score:预测得分，
  ```

#### 4.5 模型保存与加载

- ```python
  import joblib
  # 保存
  joblib.dump(rf,'test.pkl')
  # 加载
  estimator = joblib.load('test.pkl')
  ```

#### 4.6 无监督学习 K-means算法

- 步骤：

  - 随机设置k个特征空间内的点作为初始的聚类中心
  - 对于其他每个点计算到k个中心的距离，未知的点选择最近的一个聚类中心点作为标记
  - 接着对着标记的聚类中心之后，重新计算出每个聚类的新中心点（平均值）
  - 如果计算得出的新中心点与原中心点一样，那么结束，否则重新进行第二步

- ```python
  sklearn.cluster.KMeans(n_clusters=8,init='k-means++')
  # n_clusters:开始的聚类中心数量
  # init:初始化方法
  # labels_:默认标记的类型，可以和真实值比较
  ```

- 无监督学习包含算法

  - 聚类
  - k-means
  - 降维
  - PCA

- 流程分析

  - 降维
  - 预估器流程
  - 看结果
  - 模型评估

