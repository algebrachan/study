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
  
  归一化、标准化：量纲化
  
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
    
    def variance_demo():
    	data = pd.read_csv("xx.csv")
        data = data.iloc[:,1:-2]
    	transfer = VarianceThreshold()
        data_new = transfer.fit_transform(data)
        print("data_new:\n",data_new,data_new.shape)
        
        
    ```
    
    