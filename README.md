# Python数据分析过程（基础版）
此篇文章介绍了如何用Python进行数据分析，主要目的是理清思路、梳理逻辑，涉及的也是一些常用的命令代码。
## 1. 读取 CSV 文件
先尝试读取一份银行贷款数据集（ csv 文件，编的实验数据不代表真实性）。使用的是 `read_csv()` 方法，用于将数据从 csv 文件加载到 Pandas 数据框中。只需要指定数据的文件路径。我已经将 `bankloan.csv` 存储在与 Jupyter notebook 相同的目录下，所以只需要提供文件名，否则需要添加 `绝对路径` 。
```python
import pandas as pd
df = pd.read_csv('bankloan.csv', encoding='gbk') #当出现中文乱码时，尝试使用不用编码方式解码
```
`head()` 是一个有用的功能，可以在数据框上调用，用于显示前几行。
```python
df.head() # 默认是显示前5行，也可以显示指定的行数，如df.head(2)只显示前2行
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/1.png" width="50%" height="50%" /><br>
请记住，CSV 代表逗号分隔值，但这些值实际可用不同的字符、制表符、空格等分隔。例如，如果文件用逗号分隔，仍然可以将 `read_csv()` 与 `sep` 参数一起使用。
```python
df = pd.read_csv('bankloan.csv', encoding='gbk', sep=':')
df.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/2.png" width="50%" height="50%" /><br>
明显没有成功，因为 CSV 文件是用逗号分隔的。由于没有冒号，没有被分隔的值，所有值都被读取到一个列！
### 1.1 标题
`read_csv` 的另一个功能是指定文件的哪一行作为标题，而标题指定了列标签。通常第一行是标题，但有时如果文件顶部有额外的元信息，我们希望指定另一行作为标题。可以这样操作。
```python
df = pd.read_csv('bankloan.csv', encoding='gbk', header=2)
df.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/3.png" width="50%" height="50%" /><br>
```python
df = pd.read_csv('bankloan.csv', encoding='gbk', header=None)
df.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/4.png" width="50%" height="50%" /><br>
还可以用以下方法自己指定列标签。
```python
labels = ['id', 'age', 'education', 'w_age', 'income', 'debt_ratio', 'cardloan', 'o_loan', 'default']
df = pd.read_csv('bankloan.csv', encoding='gbk', names=labels)
df.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/5.png" width="50%" height="50%" /><br>
如果想告诉 pandas，正在替换的数据包含标题行，可以用以下方法指定这一行。
```python
labels = ['id', 'age', 'education', 'w_age', 'income', 'debt_ratio', 'cardloan', 'o_loan', 'default']
df = pd.read_csv('bankloan.csv', encoding='gbk', header=0, names=labels)
df.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/6.png" width="50%" height="50%" /><br>
### 1.2 索引
除使用默认索引（从 0 递增 1 的整数）之外，还可以将一个或多个列指定为数据框的索引。
```python
df = pd.read_csv('bankloan.csv', encoding='gbk', index_col='id')
df.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/7.png" width="50%" height="50%" /><br>
```python
df = pd.read_csv('bankloan.csv', encoding='gbk', index_col=['id', '年龄'])
df.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/8.png" width="50%" height="50%" /><br>
这个功能可单独用于进行多种操作，例如解析日期、填充空值、跳行等。可以在  `read_csv()` 后面进行不同步骤，实现这些操作。可以在 [这里：pandas 读取csv文档](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) 查看如何用这个功能进行操作。
# 2. 评估和理解数据（建立直觉）
一旦将数据加载到数据框中，Pandas 会非常简单、快速地对数据进行调查。
```python
import pandas as pd
df = pd.read_csv('bankloan.csv', encoding='gbk')
df.head() # 返回数据框中的前几行，默认返回前五行，也可以显示指定的行数，如df.head(2)只显示前两行
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/9.png" width="50%" height="50%" /><br>
```python
#返回数据框维度的元组
df.shape
```
output：(735, 9)
```python
#返回列的数据类型
df.dtypes
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/10.png" width="20%" height="20%" /><br>
```python
# 显示数据框的简明摘要，包括每列非空值的数量
df.info()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/11.png" width="40%" height="40%" /><br>
```python
# 返回每列数据的有效描述性统计
df.describe()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/12.png" width="70%" height="70%" /><br>
```python
# `.tail()` 返回最后几行，但是也可以指定你希望返回的行数
df.tail(2)
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/13.png" width="50%" height="50%" /><br>
## 2.1 在 Pandas 中进行数据索引和选择
有时我们只需要分析其中的部分列，因此需要对数据进行列的选择，主要有以下几种办法。假设我们需要筛选出从 `id` 到`收入` 列的数据。
```python
# 查看每列的索引号和标签
for i, v in enumerate(df.columns):
    print(i, v)
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/14.png" width="15%" height="15%" /><br>
可以使用 `loc` 和 `iloc` 选择数据。可以点击  [这里](https://pandas.pydata.org/pandas-docs/stable/indexing.html)，了解  `loc` 和 `iloc` 的更多信息。`loc` 使用行标签或列标签选择数据，而 `iloc` 使用索引号。
```python
# 使用loc选择从 'id' 到 '收入' 列
df1 = df.loc[:,'id':'收入']
df1.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/15.png" width="20%" height="20%" /><br>
```python
# 使用iloc重复以上步骤
df1 = df.iloc[:,:5]
df1.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/16.png" width="20%" height="20%" /><br>
## 2.2 在 Pandas 中选择多个范围
选择上述数据框的列非常简单，因为需要选择的列都在一起。但如果所需列是分开的，无法在一个范围内指定全部，就需要用其他方法。[点击这里：stackoverflow 链接](https://stackoverflow.com/questions/41256648/select-multiple-ranges-of-columns-in-pandas-dataframe) 学习如何在 Pandas 中选择多个范围。<br>
假设我们需要筛选出 `id` 、`年龄`、 `收入`、 `其他负债` 列的数据。
```python
import numpy as np

df2 = df.iloc[:, np.r_[0:2, 4, 7]]
df2.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/17.png" width="20%" height="20%" /><br>
```python
# 另一种方法
df2 = df[['id', '年龄', '收入', '其他负债']]
df2.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/18.png" width="20%" height="20%" /><br>
## 3. 清理数据
练习缺失值和重复值的处理。
```python
# 读入 `bankloan.csv`
import pandas as pd
df = pd.read_csv('bankloan.csv', encoding='gbk')
```
## 3.1 缺失值
```python
# 用 info() 检查哪些列有缺失值
df.info()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/19.png" width="20%" height="20%" /><br>
```python
# 用pandas的fillna函数将平均值填充到各空值处
df.fillna(df.mean(), inplace=True) #inplace参数为True时表示在源表上进行更改，False时不会改变源表
# 用 info() 确认修改
df.info()
```
## 3.2 重复值
```python
# 检查数据中的重复
sum(df.duplicated())
```
output：<br>41
```python
# 丢弃重复
df.drop_duplicates(inplace=True) #inplace参数为True时表示在源表上进行更改，False时不会改变源表
# 再次检查数据中的重复，确认修改
sum(df.duplicated())
```
output：<br>0
## 3.3 数据类型的转化
使用pandas的astype函数将 `年龄`、 `教育`、 `工龄` 列转换为 `int` 型
```python
df['年龄'] = df['年龄'].astype(int)
df['教育'] = df['教育'].astype(int)
df['工龄'] = df['工龄'].astype(int)
# 用 info() 确认修改
df.info()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/21.png" width="20%" height="20%" /><br>
## 3.4 重命名列
在绘制图形时，多数不支持中文，因此可以重命名列，使用的是 `rename` 方法。
```python
df1 = df.rename(columns={'年龄':'age', '教育':'education', '工龄':'w_age', '收入':'income', 
                         '负债率':'debt_ratio', '信用卡负债':'cardloan', '其他负债':'o_loan', '违约':'default'})
df1.head()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/22.png" width="20%" height="20%" /><br>
# 4. 得出结论
由于此篇为基础版，因此没有涉及过多的数据分析、机器学习的方法，主要回答以下的几个问题得出分析的结论。<br>
1.30岁（含）以下人群的总收入与信用卡总负债情况<br>
2.30岁（含）以下人群、31岁到39岁及40岁（含）以上人群的总收入总比情况
```python
% matplotlib inline
# 探索数据，为所有的变量绘制柱状图，了解大概分布情况
df1.hist(figsize=(8, 8))
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/23.png" /><br>
```python
# 30岁（含）以下人群的总收入与信用卡总负债情况
df2 = df1[df1['age'] <= 30]
print("30岁（含）以下人群的总收入约为 %d 元。" % df2['income'].sum())
print("30岁（含）以下人群的信用卡总负债约为 %d 元。" % df2['cardloan'].sum())
```
output：<br>
30岁（含）以下人群的总收入约为 6806 元。<br>
30岁（含）以下人群的信用卡总负债约为 219 元。
```python
# 30岁（含）以下人群、31岁到39岁及40岁（含）以上人群的总收入总比情况
df3 = df1[(df1['age'] > 30) & (df1['age'] < 40)]
df4 = df1[df1['age'] >= 40]

total = df1['income'].sum()
print("0岁（含）以下人群总收入占比为 %.2f 。" % (df2['income'].sum() / total))
print("31岁到39岁人群总收入占比为 %.2f 。" % (df3['income'].sum() / total))
print("40岁（含）以上人群总收入占比为 %.2f 。" % (df4['income'].sum() / total))
```
output：<br>
0岁（含）以下人群总收入占比为 0.21 。<br>
31岁到39岁人群总收入占比为 0.34 。<br>
40岁（含）以上人群总收入占比为 0.45 。
# 5. 传达结果（创建具有适当标签、颜色和尺寸的图）
```python
import matplotlib.pyplot as plt

X=['total_income', 'total_cardloan']
Y=[df2['income'].sum(),df2['cardloan'].sum()] 
fig = plt.figure()
plt.bar(X,Y,0.4,color="blue")
plt.xlabel("Class")
plt.ylabel("Cash")
plt.title("income VS cardloan of age lower than 30")
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/24.png" width="50%" height="50%" /><br>
```python
# 30岁（含）以下人群、31岁到39岁及40岁（含）以上人群的总收入总比情况

labels = 'age<=30', '30<age<40', 'age>=40'
fracs = [df2['income'].sum() / total, df3['income'].sum() / total, df4['income'].sum() / total]
explode = [0, 0, 0.1] # 0.1 凸出这部分，
plt.axes(aspect=1)  # Figure is round, otherwise it is an ellipse
plt.pie(x=fracs, labels=labels, explode=explode,autopct='%3.1f %%',
        shadow=True, labeldistance=1.1, startangle = 90,pctdistance = 0.5)
plt.title("Percentage of total income in different age groups")
'''
labeldistance，文本的位置离远点有多远，1.1指1.1倍半径的位置
autopct，圆里面的文本格式，%3.1f%%表示小数有三位，整数有一位的浮点数
shadow，饼是否有阴影
startangle，起始角度，0，表示从0开始逆时针转，为第一块。一般选择从90度开始比较好看
pctdistance，百分比的text离圆心的距离
'''
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/25.png" width="50%" height="50%" /><br>
# 6. 简单可视化
```python
# 绘制收入与信用卡负债之间的关系图
df1.plot(x='income', y='cardloan', kind='scatter')
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/26.png" width="50%" height="50%" /><br>
```python
# 绘制学历分布图
df1['education'].hist()
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/27.png" width="50%" height="50%" /><br>
```python
# 绘制工龄变量的箱线图
df1['w_age'].plot(kind='box')
```
output：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/28.png" width="50%" height="50%" /><br>
