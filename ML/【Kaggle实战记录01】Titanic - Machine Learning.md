---
title : 【Kaggle实战记录01】Titanic - Machine Learning from Disaster
date : 2023/4/7
tags : 机器学习,练习记录
author : Linno
---



# 【Kaggle实战记录01】Titanic - Machine Learning



太久没写过博客了，这是一个新系列，从小开始入门我的Kaggle。

求生欲声明：本人很菜，博客仅供参考。



传送门：https://www.kaggle.com/competitions/titanic



### 题目描述

这是一个二分类问题，训练集和测试集均会提供以下特征：

```
PassengerId : 乘客编号
Pclass ： 船舱等级
Name ： 船员姓名
Sex ： 船员性别
Age ： 船员年龄
SibSp ： 在船上的兄弟姐妹和配偶个数
Parch : 在船上的父母和孩子个数
Ticket : 船票编号
Fare : 票价
Cabin : 所在船舱编号（很多缺失）
```

训练集会提供标签值`Survived`，表示最后是否存活

**预测任务：**最后用你训练的模型预测一些乘客的存活情况，并提交预测csv文件，按准确率给分。



### 导入库和数据集

我们要用到可视化的一些库，以及常用的机器学习模式都可以尝试效果，这里用的是sklearn框架做例子。

```python
# 导入库
import pandas as pd
import numpy as np
import random as rnd

import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline

# 消除警告
import warnings
warnings.filterwarnings('ignore')

# 机器学习模型
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC,LinearSVC
from sklearn.ensemble import RandomForestClassifie
from sklearn.neighbors import KNeighborsClassifier
from sklearn.naive_bayes import GaussianNB
from sklearn.linear_model import Perceptron
from sklearn.linear_model import SGDClassifier
from sklearn.tree import DecisionTreeClassifier
```

读训练和测试集。

```python
# 导入数据
train_df = pd.read_csv('D:/Documents/titanic/train.csv')
test_df = pd.read_csv('D:/Documents/titanic/test.csv')

train_df.head() #这里用来预览字段结构和类型
# test_df.head() 同理
```

查看每个特征的非空样本量和字段类型

```python
train_df.info()
print("*"*40)
test_df.info()
```

查看数值类型特征(比如int和float)的数据分布情况

```python
train_df.describe()
```

查看非数值类(比方说object类型)特征的数据分布情况

```python
train_df.describe(include=["O"])
```

##### 上面的步骤看个大概，了解数据大概是什么情况就Ok了。



### 数据预处理

下面包括了数据清洗和数据可视化两种。



##### 床舱等级与生存量的关系

很自然地想到高级床舱的存活率也许是比较高的，作图出来关系也一目了然。

```python
# 创建船舱等级与生存量列联表
Pclass_Survived = pd.crosstab(train_df['Pclass'],train_df['Survived'])
print(Pclass_Survived)
# 绘制船舱等级与生产量条形图
Pclass_Survived.plot(kind='bar')
plt.xticks(rotation=360)
plt.show()
# 查看不同船舱等级生存率条形图
Pclass_Survived.plot(kind='line')
Pclass_Survived = train_df[["Pclass","Survived"]].groupby(["Pclass"],as_index=True).mean().sort_values(by="Pclass",ascending=False)
plt.xticks(range(1,4)[::1])
plt.show()
```

##### 性别与生存量的关系

同样地对性别进行同样处理，果然女性的存活概率是更高的。

```python
# 创建性别和生存量列联表
Sex_Survived = pd.crosstab(train_df['Sex'],train_df['Survived'])
Sex_Survived
Sex_Survived.plot(kind='bar')
plt.xticks(rotation=360)
plt.show() #横坐标0，1分别表示男性和女性

# 查看性别与生存率
train_df[["Sex","Survived"]].groupby(["Sex"],as_index=False).mean().sort_values(by="Survived",ascending=False)
```



##### 对年龄数据进行缺失值处理

缺失值使用中位数代替。

```python
# 处年龄缺失情况(用中位数代替缺失数据)
Agemedian=train_df['Age'].median() # 用年龄的中位数代替年龄缺失值
train_df.Age.fillna(Agemedian,inplace=True) # 在当前表填充缺失值
train_df.reset_index(inplace = True) #重置索引
```

年龄应该分个层先，不然太少人的年龄段统计存活率也没啥意义。

```python
# 对年龄进行分组，绘制年龄与幸存数量条形图
bins = [0,9,18,27,36,45,54,63,72,81,90]
train_df['GroupAge'] = pd.cut(train_df.Age,bins)

GroupAge_Survived = pd.crosstab(train_df['GroupAge'],train_df['Survived'])
GroupAge_Survived.plot(kind='bar',figsize=(10,6))

plt.xticks(rotation=360)
plt.title('Survived status by GroupAge')

# 绘制不同年龄对应生存率折线图

# 不同年龄段幸存数
GroupAge_Survived_1 = GroupAge_Survived[1]
# 不同年龄段幸存率
GroupAge_all = GroupAge_Survived.sum(axis=1)
GroupAge_Survived_rate = round(GroupAge_Survived_1/GroupAge_all,2)
GroupAge_Survived_rate.plot(figsize=(10,6))
plt.show()
```

可以看出老人和小孩是存活率更高的（有一段太少人拉低了）



##### 重复处理其他数值类型特征

代码是如出一辙的就不用过多讲解了，贴在下面。

```python
# 创建兄弟姐妹及配偶数量与生存量列联表
SibSp_Survived = pd.crosstab(train_df['SibSp'],train_df['Survived'])
SibSp_Survived
```

```python
SibSp_Survived.plot(kind='bar')
plt.xticks(rotation=360)
plt.show()
```

```python
# 查看兄弟姐妹配偶数量和生存率的关系
SibSp_Survived = train_df[["SibSp","Survived"]].groupby(["SibSp"],as_index=True).mean().sort_values(by="SibSp")
SibSp_Survived.plot(kind='line')
plt.show()
```

```python
# 创建父母与孩子数量与生存量列联表
Parch_Survived = pd.crosstab(train_df['Parch'],train_df['Survived'])
Parch_Survived
```

```python
Parch_Survived.plot(kind='bar')
plt.xticks(rotation=360)
plt.show()
```

```python
# 查看父母与孩子数与生存率的关系
Parch_Survived = train_df[["Parch","Survived"]].groupby(["Parch"],as_index=True).mean().sort_values(by="Parch")
Parch_Survived.plot(kind='line')
plt.show()
```

```python
# 查看不同票价对应与幸存情况的关系
# 划分船票价格，创建不同的船票对应生存量列联表
bins = [0,60,120,180,240,300,360,420,480,540,600]
train_df['GroupFare']=pd.cut(train_df.Fare,bins,right=False)
GroupFare_Survived = pd.crosstab(train_df['GroupFare'],train_df['Survived'])
GroupFare_Survived
```

```python
# 绘制不同船票价格对应幸存量簇状柱形图
GroupFare_Survived.plot(kind='bar',figsize=(10,6)) #绘制簇状柱形图
plt.xticks(rotation=360) #调整刻度
plt.title('Survived status by GroupFare')

GroupFare_Survived.iloc[2:].plot(kind='bar',figsize=(10,6))
plt.xticks(rotation=360)
plt.title('Survived status by GroupFare(Fare>=120)')
```

```python
# 绘制不同票价对应生存率折线图

GroupFare_Survived_1 = GroupFare_Survived[1] #不同票价对应存活数
GroupFare_all = GroupFare_Survived.sum(axis=1)
GroupFare_Survived_rate = round(GroupFare_Survived_1/GroupFare_all,2)
GroupFare_Survived_rate.plot()
plt.show()
```

分析完了之后可以把创建的年龄分组和票价分组连同编号给去了。

```python
train_df=train_df.drop(["index","GroupAge","GroupFare"],axis=1)
train_df.head()
```



##### 其他无关字段

因为Ticket表示船票的名字，与乘客的生存率无关联。 删除Cabin是因为通过HasCabin来代替Cabin字段。

```python
# 讲乘客是否有船舱分为两类
train_df['HasCabin'] = train_df["Cabin"].apply(lambda x: 0 if type(x)==float else 1)
test_df['HasCabin'] = test_df["Cabin"].apply(lambda x: 0 if type(x)==float else 1)
train_df.head()
```

```python
train_df = train_df.drop(["Ticket","Cabin"],axis=1)
test_df = test_df.drop(["Ticket","Cabin"],axis=1)
combine = [train_df,test_df]
print(train_df.shape,test_df.shape,combine[0].shape,combine[1].shape)
```



##### 姓名也有一部分关联！！！

注意到了吗？姓名里的Mr,Mrs这些显然也是一部分相关因素。使用名字字段作为特征之一是因为姓名带有该乘客的头衔，姓名越长对应头衔可能越长，相对社会地位可能较高。

```python
# 创建训练集和测试集姓名长度字段
train_df['NameLength'] = train_df['Name'].apply(len)
test_df['NameLength'] = test_df['Name'].apply(len)

```

根据姓名创建称号特征，会包含性别和阶层信息

dataset.Name.str.extract('([A-Za-z]+)\.') 把空格开头、结尾的字符串提取出来和性别匹配，看各类称号分别属于男or女，方便后续归类。

```python
for dataset in combine:
    dataset['Title'] = dataset.Name.str.extract('([A-Za-z]+)\.',expand=False)
pd.crosstab(train_df['Title'],train_df['Sex']).sort_values(by=["male","female"],ascending=False)
```

把称号归类为Mr,Miss,Mrs,Master,Rare_Male,Rare_Female(按男性和女性区分了Rare)

```python
for dataset in combine:
    dataset["Title"] = dataset["Title"].replace(['Lady','Countess','Dona'],"Rare_Female")
    dataset["Title"] = dataset["Title"].replace(['Capt','Col','Don','Dr','Major','Rev','Sir','Jonkheer',],'Rare_Male')
    dataset["Title"] = dataset["Title"].replace('Mlle','Miss')
    dataset["Title"] = dataset["Title"].replace('Ms','Miss')
    dataset["Title"] = dataset["Title"].replace('Mme',"Miss")
```

绘制不同称号对应的存活率

```python
# 按Title汇总计算Survived均值，查看相关性
T_S = train_df[["Title","Survived"]].groupby(["Title"],as_index=False).mean().sort_values(by='Survived',ascending=True)
plt.figure(figsize=(10,6))
plt.bar(T_S['Title'],T_S['Survived'])
```

PS：称为Miss、Mrs、Rare_Female的乘客存活率居高，看来在逃生时，大家遵循女士优先的原则。

```python
# 将Title 特征映射成数值
title_mapping = {"Mr":1,"Miss":2,"Mrs":3,"Master":4,"Rare_Female":5,"Rare_Male":6}
for dataset in combine :
    dataset["Title"] = dataset["Title"].map(title_mapping)
    dataset["Title"] = dataset["Title"].fillna(0)
    # 为了避免有空数据的常规操作·
train_df.head()
```

完成了信息的挖掘，删除名字字段

```python
train_df = train_df.drop(["Name","PassengerId"],axis=1)
test_df = test_df.drop(['Name'],axis=1)
train_df.head()

# 每次删除特征时都要重新combine
combine = [train_df,test_df]
combine[0].shape,combine[1].shape
```

将性别字段转化为数值，女性为0，男性为1

```python
for dataset in combine :    dataset["Sex"]=dataset["Sex"].map({"female":1,"male":0}).astype(int) #加astype(int)避免类型错误
train_df.head()
```

```python
guess_ages = np.zeros((6,3)) #对年龄字段进行空值处理
guess_ages 

# 对age年龄字段的空值
# 使用相同Pclass和Title的Age中位数来代替(对于中位数为空的组合，使用Title整体的中位数来代替)
for dataset in combine:
    #取六种组合的中位数
    for i in range(0,6):
        for j in range(0,3):
            guess_title_df = dataset[dataset["Title"]==i+1]["Age"].dropna()
            guess_df = dataset[(dataset["Title"]==i+1)&(dataset["Pclass"]==j+1)]['Age'].dropna()
            # age_mean = guess_df.mean()
            # age_std = guess_df.mean()
            # age_guess = rnd.uniform(age_mean - age_std,age_mean + age_std)
            age_guess = guess_df.median() if ~np.isnan(guess_df.median()) else guess_title_df.median()
            #print(i,j,guess_df.median(),guess_title_df.median(),age_guess)
            guess_ages[i,j]=int(age_guess/0.5+0.5)*0.5
    # 给满足6种情况的Age字段赋值
    for i in range(0,6):
        for j in range(0,3):
            dataset.loc[(dataset.Age.isnull()) & (dataset.Title == i+1) & (dataset.Pclass == j+1),'Age'] = guess_ages[i,j]
    dataset['Age'] = dataset['Age'].astype(int)
    
    
train_df.head()
```

##### 划分年龄层次

创建年龄区间特征。

pd.cut是按值的大小均匀切分，每组值区间大小相同，但样本数可能不一致；pd.qcut是按照样本在值上的分布频率切分，每组样本数相同

```python
train_df['AgeBand'] = pd.qcut(train_df['Age'],8)
print(df.head()) 
train_df[['AgeBand','Survived']].groupby(['AgeBand'],as_index=False).mean().sort_values(by='AgeBand',ascending=True)
```

##### 将年龄区间转化为数值

```python
for dataset in combine:
    dataset.loc[dataset['Age'] <= 17,'Age'] = 0
    dataset.loc[(dataset['Age'] > 17) & (dataset['Age'] <= 21),'Age'] = 1
    dataset.loc[(dataset['Age'] > 21) & (dataset['Age'] <= 25),'Age'] = 2
    dataset.loc[(dataset['Age'] > 25) & (dataset['Age'] <= 26),'Age'] = 3
    dataset.loc[(dataset['Age'] > 26) & (dataset['Age'] <= 31),'Age'] = 4
    dataset.loc[(dataset['Age'] > 31) & (dataset['Age'] <= 36.5),'Age'] = 5
    dataset.loc[(dataset['Age'] > 36.5) & (dataset['Age'] <= 45),'Age'] = 6
    dataset.loc[(dataset['Age'] > 45),'Age'] = 7
train_df.head()
```

##### 移除AgeBand特征

```python
train_df = train_df.drop('AgeBand',axis=1)
combine = [train_df,test_df]
train_df.head()
```

##### 创建家庭规模FamilySize组合特征。

```python
for dataset in combine:
    dataset["FamilySize"] = dataset["Parch"]+dataset["SibSp"] + 1
FamilySize_Survived = train_df[["FamilySize","Survived"]].groupby(["FamilySize"],as_index = True).mean().sort_values(by="FamilySize")
FamilySize_Survived.plot(kind="line")
plt.xticks(range(12)[::1])
plt.show()
```

##### 创建是否独自一人IsAlone特征

```python
for dataset in combine : 
    dataset["IsAlone"] = 0
    dataset.loc[dataset["FamilySize"]==1,"IsAlone"]=1
IsAlone_Survived = train_df[["IsAlone","Survived"]].groupby(["IsAlone"],as_index=True).mean().sort_values(by="IsAlone")
IsAlone_Survived.plot(kind="bar")
plt.xticks(rotation=360)
plt.show()
```

##### 移除Parch,Sibsp 字段

```python
train_df = train_df.drop(["Parch","SibSp"],axis=1)
test_df = test_df.drop(["Parch","SibSp"],axis=1)
combine = [train_df,test_df]
train_df.head()
```

##### 港口因素

```python
# 给Embarked补充空值/获取上船最多的港口
freq_port = train_df["Embarked"].dropna().mode()[0]
freq_port
```

##### 处理缺失值(利用众数填充缺失值)!!!

```python
for dataset in combine :
    dataset["Embarked"] = dataset["Embarked"].fillna(freq_port)

# 创建列联表
Embarked_Survived = pd.crosstab(train_df['Embarked'],train_df['Survived'])
Embarked_Survived
```

绘制不同港口对应的幸存量条形图

```python
Embarked_Survived.plot(kind='bar')
plt.xticks(rotation=360)
plt.show()
```

不同港口与幸存量的关系

```python
Embarked_Survived = train_df[["Embarked","Survived"]].groupby(["Embarked"],as_index=True).mean().sort_values(by="Embarked",ascending=True)
Embarked_Survived.plot(kind="bar")
plt.xticks(rotation=360)
plt.show()
```

把Embarked数字化

```python
for dataset in combine:
    dataset["Embarked"] = dataset["Embarked"].map({"S":0,"C":1,"Q":2}).astype(int)
train_df.head()
```

给测试集中的Fare(票价)填充空值，使用中位数

```python
test_df["Fare"].fillna(test_df["Fare"].dropna().median(),inplace=True)
test_df.info()
```

创建FareBand区间特征

```python
train_df["FareBand"] = pd.qcut(train_df["Fare"],4)
train_df[["FareBand","Survived"]].groupby(["FareBand"],as_index=False).mean().sort_values(by="FareBand",ascending=True)
```

将不同票价所在区间数字化

```python
# 根据FareBand将Fare特征转化为序数值
for dataset in combine:
    dataset.loc[dataset['Fare'] <=7.91,'Fare'] = 0
    dataset.loc[(dataset['Fare'] > 7.91) & (dataset['Fare'] <= 14.454),'Fare'] = 1
    dataset.loc[(dataset['Fare'] >14.454) & (dataset['Fare'] <=31),'Fare'] = 2
    dataset.loc[dataset['Fare']>31,'Fare'] = 3
    dataset['Fare'] = dataset['Fare'].astype(int)

# 移除FareBand
train_df = train_df.drop(['FareBand'],axis=1)
combine = [train_df,test_df]

train_df.head(10)
```

##### 特征相关性的可视化

```python
# 用seaborn的heatmap对特征之间的相关性进行可视化
colormap = plt.cm.RdBu
plt.figure(figsize=(14,12))
plt.title('Pearson Correlation of Features',y=1.05,size=15)
sns.heatmap(train_df.astype(float).corr(),linewidths=0.1,vmax=1.0,
            square=True,cmap=colormap,linecolor='white',annot=True)
plt.show()
```



### 模型训练和比较

开始准备训练数据

```python
X_train = train_df.drop(['Survived'],axis=1)
Y_train = train_df["Survived"]
X_test = test_df.drop("PassengerId",axis=1).copy()
X_train

# X_train.shape,Y_train.shape,X_test.shape
```

Logistic Regression 逻辑回归模型

```python
logreg = LogisticRegression()
logreg.fit(X_train,Y_train)
Y_pred_logreg = logreg.predict(X_test)
acc_log = round(logreg.score(X_train,Y_train)*100,2)

# Y_pred_logreg # 预测结果
acc_log # 准确率
```

计算相关性

```python
coeff_df = pd.DataFrame(train_df.columns.delete(0))
coeff_df.columns = ['Feature']
coeff_df["Correlation"] = pd.Series(logreg.coef_[0])

coeff_df.sort_values(by='Correlation',ascending=False)
```

Support Vector Machines 支持向量机模型

```python
svc = SVC()
svc.fit(X_train,Y_train)
Y_pred_svc = svc.predict(X_test)
acc_svc = round(svc.score(X_train,Y_train)*100,2)
# Y_pred_svc
acc_svc
```

KNN k近邻分类模型

```python
knn = KNeighborsClassifier(n_neighbors = 3)
knn.fit(X_train,Y_train)
Y_pred_knn = knn.predict(X_test)
acc_knn = round(knn.score(X_train,Y_train)*100,2)
# Y_pred_knn
acc_knn
```

Gaussian Naive Bayes 贝叶斯分类算法

```python
gaussian = GaussianNB()
gaussian.fit(X_train,Y_train)
Y_pred_gaussian = gaussian.predict(X_test)
acc_gaussian = round(gaussian.score(X_train,Y_train)*100,2)
print(Y_pred_gaussian)
acc_gaussian
```

Perceptron 模型

```python
perceptron = Perceptron()
perceptron.fit(X_train,Y_train)
Y_pred_perceptron = perceptron.predict(X_test)
acc_perceptron = round(perceptron.score(X_train,Y_train)*100,2)
acc_perceptron
```

Linear SVC

```python
linear_svc = LinearSVC()
linear_svc.fit(X_train,Y_train)
Y_pred_linear_svc = linear_svc.predict(X_test)
acc_linear_svc = round(linear_svc.score(X_train,Y_train)*100,2)
print(Y_pred_linear_svc)
acc_linear_svc
```

Stochasticc Gradient Descent 随机梯度下降

```python
sgd = SGDClassifier()
sgd.fit(X_train,Y_train)
Y_pred_sgd = sgd.predict(X_test)
acc_sgd = round(sgd.score(X_train,Y_train)*100,2)
print(Y_pred_sgd)
acc_sgd
```

Decision Tree 决策树

```python
decision_tree = DecisionTreeClassifier()
decision_tree.fit(X_train,Y_train)
Y_pred_decision_tree = decision_tree.predict(X_test)
acc_decision_tree = round(decision_tree.score(X_train,Y_train)*100,2)
print(Y_pred_decision_tree)
acc_decision_tree
```

随机森林算法

```python
from sklearn.model_selection import train_test_split
X_all = train_df.drop(['Survived'],axis=1)
y_all = train_df['Survived']

num_test = 0.20
X_train,X_test,y_train,y_test = train_test_split(X_all,y_all,test_size=num_test,random_state=23)

# Random Forest
from sklearn.metrics import make_scorer,accuracy_score
from sklearn.model_selection import GridSearchCV
random_forest = RandomForestClassifier()

parameters = {'n_estimators':[4,6,9],
              'max_features':['log2','sqrt','auto'],
              'criterion':['entropy','gini'],
              'max_depth':[2,3,5,10],
              'min_samples_split':[2,3,5],
              'min_samples_leaf':[1,5,8]
             }
acc_scorer = make_scorer(accuracy_score)
grid_obj = GridSearchCV(random_forest,parameters,scoring=acc_scorer)
grid_obj = grid_obj.fit(X_train,y_train)
clf = grid_obj.best_estimator_
clf.fit(X_train,y_train)
pred = clf.predict(X_test)
acc_random_forest_split = accuracy_score(y_test,pred)
print(pred)
acc_random_forest_split
```

k-fold 交叉验证模型

```python
from sklearn.model_selection import KFold 

def run_kfold(clf):
    kf = KFold(n_splits=10, random_state=233, shuffle=True)
    outcomes = []
    fold = 0
    for train_index,test_index in kf.split(train_df):
        fold += 1
        X_train,X_test = X_all.values[train_index],X_all.values[test_index]
        y_train,y_test = y_all.values[train_index],y_all.values[test_index]
        clf.fit(X_train,y_train)
        predictions = clf.predict(X_test)
        accuracy = accuracy_score(y_test,predictions)
        outcomes.append(accuracy)
    mean_outcome = np.mean(outcomes)
    print("Mean Accuracy:{0}".format(mean_outcome))

run_kfold(clf)
```



### 最终结果

##### 模型比较

```python
Y_pred_random_forest_split = clf.predict(test_df.drop("PassengerId",axis=1))

models = pd.DataFrame({
    'Model':['SVM','KNN','Logistic Regression','Random Forest',
             'Naive Bayes','Perceptron','SGD','Linear SVC','Decision Tree'],
    'Score':[acc_svc,acc_knn,acc_log,acc_random_forest_split,
             acc_gaussian,acc_perceptron    ,acc_sgd,acc_linear_svc,acc_decision_tree]
})
M_s = models.sort_values(by='Score',ascending=False)
M_s
```

做个柱状图

```python
plt.figure(figsize=(20,8),dpi=80)
plt.bar(M_s['Model'],M_s['Score'])
plt.show()
```

##### 保存结果

```python
# 导入时间模块，利用时间戳作为文件名
import time

# 最后取随机森林模型的预测结果进行提交
tim = time.strftime('%Y%m%d%H%M',time.localtime(time.time()))
submission = pd.DataFrame({
    "PassengerId":test_df['PassengerId'],
    "Survived":Y_pred_random_forest_split
})
submission.to_csv('C:/Users/12645/Kaggle/submission_random_forest_' + tim + '.csv',index=False)

#保存决策树模型预测结果
tim = time.strftime('%Y%m%d%H%M',time.localtime(time.time()))
submission = pd.DataFrame({
    "PassengerId":test_df['PassengerId'],
    "Survived":Y_pred_decision_tree
})
submission.to_csv('C:/Users/12645/Kaggle/submission_decision_tree_' + tim + '.csv',index=False)

#保存KNN模型预测结果
tim = time.strftime('%Y%m%d%H%M',time.localtime(time.time()))
submission = pd.DataFrame({
    "PassengerId":test_df['PassengerId'],
    "Survived":Y_pred_knn
})
submission.to_csv('C:/Users/12645/Kaggle/submission_knn_' + tim + '.csv',index=False)

#保存SVC模型预测结果
tim = time.strftime('%Y%m%d%H%M',time.localtime(time.time()))
submission = pd.DataFrame({
    "PassengerId":test_df['PassengerId'],
    "Survived":Y_pred_svc
})
submission.to_csv('C:/Users/12645/Kaggle/submission_svc_' + tim + '.csv',index=False)

#保存SGD模型预测结果
tim = time.strftime('%Y%m%d%H%M',time.localtime(time.time()))
submission = pd.DataFrame({
    "PassengerId":test_df['PassengerId'],
    "Survived":Y_pred_sgd
})
submission.to_csv('C:/Users/12645/Kaggle/submission_sgd_' + tim + '.csv',index=False)

#保存Linear SVC模型预测结果
tim = time.strftime('%Y%m%d%H%M',time.localtime(time.time()))
submission = pd.DataFrame({
    "PassengerId":test_df['PassengerId'],
    "Survived":Y_pred_linear_svc
})
submission.to_csv('C:/Users/12645/Kaggle/submission_linear_svc_' + tim + '.csv',index=False)

#保存逻辑回归模型预测结果
tim = time.strftime('%Y%m%d%H%M',time.localtime(time.time()))
submission = pd.DataFrame({
    "PassengerId":test_df['PassengerId'],
    "Survived":Y_pred_logreg
})
submission.to_csv('C:/Users/12645/Kaggle/submission_logreg_' + tim + '.csv',index=False)
```

##### 提交结果

0.74401