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
- 特征工程
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
  # feature_name:特征名
  # target_name:标签名
  ```
  
  
  
  