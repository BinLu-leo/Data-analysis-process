# Python数据分析过程（基础版）
此篇文章介绍了如何用Python进行数据分析，主要目的是理清思路、梳理逻辑，涉及的也是一些常用的命令代码。
## 1. 读取 CSV 文件
先尝试读取一份学生成绩数据集（csv 文件）。使用的是 `read_csv()` 方法，用于将数据从 csv 文件加载到 Pandas 数据框中。只需要指定数据的文件路径。我已经将 `student_scores.csv` 存储在与这个 Jupyter notebook 相同的目录下，所以只需要提供文件名，否则需要添加 `绝对路径` 。
```python
import pandas as pd

df = pd.read_csv('student_scores.csv')
```
`head()` 是一个有用的功能，可以在数据框上调用，用于显示前几行。
```python
df.head() # 默认是显示前5行，也可以显示指定的行数，如df.head(2)只显示前2行
```
输出结果：<br>
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/1.png" width="50%" height="50%" /><br>
请记住，CSV 代表逗号分隔值，但这些值实际可用不同的字符、制表符、空格等分隔。例如，如果文件用逗号分隔，仍然可以将 `read_csv()` 与 `sep` 参数一起使用。
```python
df = pd.read_csv('student_scores.csv', sep=':')
df.head()
```
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/2.png" width="50%" height="50%" /><br>
明显没有成功，因为 CSV 文件是用逗号分隔的。由于没有冒号，没有被分隔的值，所有值都被读取到一个列！
### 1.1 标题
`read_csv` 的另一个功能是指定文件的哪一行作为标题，而标题指定了列标签。通常第一行是标题，但有时如果文件顶部有额外的元信息，我们希望指定另一行作为标题。可以这样操作。
```python
df = pd.read_csv('student_scores.csv', header=2)
df.head()
```
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/3.png" width="50%" height="50%" /><br>
```python
df = pd.read_csv('student_scores.csv', header=None)
df.head()
```
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/4.png" width="50%" height="50%" /><br>
还可以用以下方法自己指定列标签。
```python
labels = ['id', 'name', 'attendance', 'hw', 'test1', 'project1', 'test2', 'project2', 'final']
df = pd.read_csv('student_scores.csv', names=labels)
df.head()
```
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/5.png" width="50%" height="50%" /><br>
如果想告诉 pandas，正在替换的数据包含标题行，可以用以下方法指定这一行。
```python
labels = ['id', 'name', 'attendance', 'hw', 'test1', 'project1', 'test2', 'project2', 'final']
df = pd.read_csv('student_scores.csv', header=0, names=labels)
df.head()
```
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/6.png" width="50%" height="50%" /><br>
### 1.2 索引
除使用默认索引（从 0 递增 1 的整数）之外，还可以将一个或多个列指定为数据框的索引。
```python
df = pd.read_csv('student_scores.csv', index_col='Name')
df.head()
```
<img src="https://github.com/lubin007/Data-analysis-process/blob/master/img/7.png" width="50%" height="50%" /><br>
```python
df = pd.read_csv('student_scores.csv', index_col=['Name', 'ID'])
df.head()
```
这个功能可单独用于进行多种操作，例如解析日期、填充空值、跳行等。可以在  `read_csv()` 后面进行不同步骤，实现这些操作。可以在 [这里：pandas 读取csv文档](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html) 查看如何用这个功能进行操作。
