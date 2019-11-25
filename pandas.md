## pandas

#### 简介

 	numpy的特长并不是在于数据处理，而是在它能非常方便地实现科学计算，所以我们日常对数据进行处理时用的numpy情况并不是很多，我们需要处理的数据一般都是带有列标签和index索引的，而numpy并不支持这些，这时我们就需要pandas 了。

​	 **Pandas**是基于**Numpy**构建的库，在数据处理方面可以把它理解为numpy加强版，同时Pandas也是一项开源项目 。不同于numpy的是，pandas拥有种数据结构：**Series**和**DataFrame** 。

#### 数据结构

#### 1、Series

​	 Series是一种类似一维数组的数据结构，由一组数据和与之相关的index组成，这个结构一看似乎与dict字典差不多，我们知道字典是一种**无序**的数据结构，而pandas中的Series的数据结构不一样，它相当于**定长有序**的字典，并且它的**index和value**之间是**独立**的，两者的索引还是有区别的，Series的**index**是**可**变的，而**dict**字典的**key**值是不可变的。 

 **Series的两种生成方式：** 

```
In [1]: from pandas import Series,DataFrame
In [2]: import pandas as pd
In [3]: data = Series([1,2,3,4],index = ['a','b','c','d'])
In [4]: data
Out[4]:
a    1
b    2
c    3
d    4
dtype: int64
```

 这种形式的Series可以理解为numpy的array外面披了一件index的马甲，所以array的相关操作，Series同样也是支持的。 

```
In [2]: data = Series([222,'btc',234,'eos'])
In [3]: data
Out[3]:
0    222
1    btc
2    234
3    eos
dtype: object
```

 	虽然我们在生成的时候没有设置index值，但Series还是会自动帮我们生成index，这种方式生成的Series结构跟list列表差不多，可以把这种形式的Series理解为竖起来的list列表。 

​	结构非常相似的**dict字典同样也是可以转化为Series格式的：**

```
In [2]: dic = {'a':1,'b':2,'c':'as'}
In [3]: dicSeries = Series(dic)
```

**查看Series的相关信息：**

```
In [1]: data = Series([1,2,3,4],index = ['a','b','c','d'])
In [2]: data.index
Out[2]: Index(['a', 'b', 'c', 'd'], dtype='object')

In [3]: data.values
Out[3]: array([1, 2, 3, 4], dtype=int64)

In [4]: 'a' in data    #in方法默认判断的是index值
Out[4]: True
```

**Series的NaN生成：**

```
In [2]: index1 = [ 'a','b','c','d']
In [3]: dic = {'b':1,'c':1,'d':1}
In [4]: data2 = Series(dic,index=index1)
In [5]: data2
Out[5]:
a    NaN
b    1.0
c    1.0
d    1.0
dtype: float64
```

**NaN的相关查询：**

```
In [2]: data2.isnull()
Out[2]:
a     True
b    False
c    False
d    False
dtype: bool

In [3]: data2.notnull()
Out[3]:
a    False
b     True
c     True
d     True
dtype: bool

In [4]: data2[data2.isnull()==True]    #嵌套查询NaN
Out[4]:
a   NaN
dtype: float64

In [5]: data2.count()    #统计非NaN个数
Out[5]: 3
```

​	 查询NaN值切记不要使用np.nan==np.nan这种形式来作为判断条件，结果永远是False，==是用作**值判断**的，而NaN并没有值，如果你不想使用上方的判断方法，你可以使用is作为判断方法，**is**是**对象引用判断，np.nan is np.nan**，结果就是你要的True。 

**Series的name属性：**

```
In [1]: data = Series([1,2,3,4],index = ['a','b','c','d'])
In [2]: data.index.name = 'abc'
In [3]: data.name = 'test'
In [4]: data
Out[4]:
abc
a    1
b    2
c    3
d    4
Name: test, dtype: int64
```

​	Series**对象本身**及其**索引index**都有一个**name属性**，name属性主要发挥作用是在**DataFrame**中，当我们把一个Series对象放进DataFrame中，新的列将根据我们的name属性对该列进行命名，如果我们没有给Series命名，DataFrame则会自动帮我们命名为**0**。



#### 2、DataFrame

​	DataFrame这种数据结构我们可以把它看作是一张二维表，DataFrame长得跟我们平时使用的Excel表格差不多，DataFrame的横行称为**columns**，竖列和Series一样称为**index**，DataFrame每一列可以是不同类型的值集合，所以DataFrame你也可以把它视为不同数据类型同一index的Series集合。

 **DataFrame的生成：** 

```
In [2]:  data = {'name': ['BTC', 'ETH', 'EOS'], 'price':[50000, 4000, 150]}
In [3]: data = DataFrame(data)
In [4]: data
Out[4]:
  name  price
0  BTC  50000
1  ETH   4000
2  EOS    150
```

科学计算方面numpy是优势，但在数据处理方面DataFrame就更胜一筹了，事实上DataFrame已经覆盖了一部分的数据操作了，对于数据挖掘来说，工作可大概分为读取数据-数据清洗-分析建模-结果展示：

先说说读取数据，Pandas提供强大的IO读取工具，csv格式、Excel文件、数据库等都可以非常简便地读取，对于大数据，pandas也支持大文件的分块读取；

接下来就是数据清洗，面对数据集，我们遇到最多的情况就是存在缺失值，Pandas把各种类型数据类型的缺失值统一称为NaN（这里要多说几句，None==None这个结果是true，但np.nan==np.nan这个结果是false，NaN在官方文档中定义的是float类型）,Pandas提供许多方便快捷的方法来处理这些缺失值NaN。

最重要的分析建模阶段，Pandas自动且明确的数据对齐特性，非常方便地使新的对象可以正确地与一组标签对齐，有了这个特性，Pandas就可以非常方便地将数据集进行拆分-重组操作。

最后就是结果展示阶段了，我们都知道Matplotlib是个数据视图化的好工具，Pandas与Matplotlib搭配，不用复杂的代码，就可以生成多种多样的数据视图。

**查看DataFrame的相关信息：**

```
In [2]: data.index
Out[2]: RangeIndex(start=0, stop=3, step=1)

In [3]: data.values
Out[3]:
array([['BTC', 50000],
       ['ETH', 4000],
       ['EOS', 150]], dtype=object)

In [4]: data.columns    #DataFrame的列标签
Out[4]: Index(['name', 'price'], dtype='object')
```

**DataFrame的索引：**

```
In [2]: data.name
Out[2]:
0    BTC
1    ETH
2    EOS
Name: name, dtype: object

In [3]: data['name']
Out[3]:
0    BTC
1    ETH
2    EOS
Name: name, dtype: object

In [4]: data.iloc[1]    #loc['name']查询的是行标签
Out[4]:
name      ETH
price    4000
Name: 1, dtype: object

#loc——通过行标签索引行数据
#iloc——通过行号索引行数据
#ix——通过行标签或者行号索引行数据（基于loc和iloc 的混合）
```

**简单地增加行、列：**

```
In [2]: data['type'] = 'token'    #增加列
In [3]: data
Out[3]:
  name  price   type
0  BTC  50000  token
1  ETH   4000  token
2  EOS    150  token

In [4]: data.loc['3'] = ['ae',200,'token']    #增加行
In [5]: data
Out[5]:
  name  price   type
0  BTC  50000  token
1  ETH   4000  token
2  EOS    150  token
3   ae    200  token
```

**删除行、列操作：**

```
In [2]: del data['type']    #删除列
In [2]: data
Out[2]:
  name  price
0  BTC  50000
1  ETH   4000
2  EOS    150
3   ae    200

In [3]: data.drop([2])    #删除行
Out[3]:
  name  price
0  BTC  50000
1  ETH   4000
3   ae    200

In [4]: data
Out[4]:
  name  price
0  BTC  50000
1  ETH   4000
2  EOS    150
3   ae    200
```

这里需要注意的是，使用**drop（）方法**返回的是**Copy**而不是**视图**，要想真正在原数据里删除行，就要设置***inplace=True***，pandas 中 inplace 参数在很多函数中都会有，它的作用是：是否在原对象基础上进行修改 **inplace = True**：不创建新的对象，直接对原始对象进行修改；**inplace = False**：对数据进行修改，创建并返回新的对象承载其修改结果。默认是False，即创建新的对象进行修改，原对象不变，和深复制和浅复制有些类似。


**设置某一列为index:**

```
In [2]: data.set_index(['name'],inplace=True)
In [3]: data
Out[3]:
      price
name
BTC   50000
ETH    4000
ae      200

In [4]: data.reset_index(inplace=True)    #将index返回回dataframe中
In [5]: data
Out[5]:
  name  price
0  BTC  50000
1  ETH   4000
2   ae    200
```

**处理缺失值:**

```
In [1]: data = {'name': ['BTC', 'ETH', 'ae',], 'price':[50000, 4000]}
In [2]: data = DataFrame(data)
In [3]: data
Out[3]:
  name    price
0  BTC  50000.0
1  ETH   4000.0
2   ae      NaN

In [4]: data.dropna()    #丢弃含有缺失值的行
Out[4]:
  name    price
0  BTC  50000.0
1  ETH   4000.0

In [5]: data.fillna(0)    #填充缺失值数据为0
Out[5]:
  name    price
0  BTC  50000.0
1  ETH   4000.0
2   ae      0.0
```

**数据合并：**

```
In [2]: data
Out[2]:
  name    price
0  BTC  50000.0
1  ETH   4000.0
2   ae      NaN

In [3]: data1
Out[3]:
  name  other
0  BTC  50000
1  BTC   4000
2  EOS    150

In [162]: pd.merge(data,data1,on='name',how='left')    #以name为key进行左连接
Out[162]:
  name    price    other
0  BTC  50000.0  50000.0
1  BTC  50000.0   4000.0
2  ETH   4000.0      NaN
3   ae      NaN      NaN

```

​	平时进行数据合并操作，更多的会出一种情况，那就是出现**重复值**，DataFrame也为我们提供了简便的方法: 

**data.drop_duplicates(inplace=True)**

**数据的简单保存与读取：**

```
In [2]: data.to_csv('test.csv')
In [3]: pd.read_csv('test.csv')
Out[3]:
   Unnamed: 0 name    price
0           0  BTC  50000.0
1           1  ETH   4000.0
2           2   ae    	NaN
```

```
In [4]: data.to_csv('test.csv',index=None)    #不保存行索引
In [5]: pd.read_csv('test.csv')
Out[5]:
  name    price
0  BTC  50000.0
1  ETH   4000.0
2   ae    200.0
3  eos      NaN
```