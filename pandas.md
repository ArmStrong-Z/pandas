# pandas

<a name="l9915"></a>
## DataFrame
DataFrame是一种用来进行数据处理的类

- 其中index为二维表行的属性，columns为为其列的属性

`df.index 表示的行的值,df.columns表示的是列的值` 

- df2.to_numpy()其中其index和columns值不会被写入numpy之中
- pd.date_range('一个字符化表示的日期',periods= 一个整数表示的时间跨度)

```cpp
dates = pd.date_range('20130101', periods=6)`
```
![image.png](https://cdn.nlark.com/yuque/0/2020/png/522673/1582208567476-127d2070-cab9-4794-b3f0-e9af1b0f93f4.png#align=left&display=inline&height=115&name=image.png&originHeight=115&originWidth=682&size=13406&status=done&style=none&width=682)

- df提供了很多和统计有关的功能，df.T可以实现DataFrame转置，df.describe()可以简单的描述整个数据的统计结构
- df.sort_index(axis=0，ascending=True,index=False,na_position=在尾部或者头部添加NA,no,ignore_index=可以在重新排列之后重新添加index)其中axis表示的是其默认是按照行进行排序

df.sort_index默认只是返回其排序之后的值，但是其原来的DataFrame结构并不会被破坏<br />**df.sort_index实际根据器标签的值进行排序，我的想法是，将标签单独拎出来，可以看到对于标签进行排序的使用情况也很多**

-  df.sort_values:按照值对DataFrame结构进行排序。具体可以看官方api，其中axis的意思是在某一个轴上，比如以列中的某一个columns为属性进行排序，这样axis需要设置为0.（by）这个参宿可以指定需要排列的某一个行或者说是列的名字。

```cpp
str_list = ['2013-01-04','2013-01-02']
df.sort_values(by=str_list,axis=1)
```

- 对df使用[]操作符可以yield一个series对象，如df['A'],但是对DataFrame结构做一个切片得到的又是某几个index下的DataFrame,注意的是[]操作符不能对index进行操作
- df.loc也可以产生Series，具体用法df.loc['2013-1-13'']

从以上两个我们可以看到区别，对df直接使用[]操作符可以返回以某一columns为轴的Series，使用df.loc可以返回某一个Index为轴的Series<br />df.loc也可以产生DataFrame如 `type( df.loc['20130102':'20130104', ['A', 'B']])` ，这里指定了对应的index，其中index:index,columns以列表的形式传入<br />df.loc可以通过指定两个维度来得到DataFrame中某一个特定的值，同df.at

- df.iloc:对DataFrame的切片仍然是切片，对Series的切片和列表很相似，对于DataFrame的切片操作其永远保留所有的columns，这样可以保持其DataFrame的完整性

df.iloc支持对DataFrame结构进行切片操作，而且可以传入tuple进行切片

- df[df['A']>0]可以选择出所有Acolumns中大于0的所有index
- df可以直接通过[]操作符来添加对应的columns， `df2['E'] = ['one', 'one', 'two', 'three', 'four', 'three']` 
- df可以更具df来进行切片 `df2[df2['E'].isin(['two','four'])]` 这样可以筛选出E中含有two和four的column
- len(df)返回的结果是其总的整个DataFrame中总共index的个数
- df[df>0]=-df其是对所有的df中值大于0的元素进行取反
- df.reindex可以将df按照需求保留想要的index,df.reindex(index=dates[0:4],columns=list(df.columns)+'E')
- df.dropna(how=any)可以丢弃所有含有nan的行
- df.fillna(values=5)可以填充所有值为nan的df中的值
- df.isna可以将所有含有nan的值的dataframe结构进行填充
- df.sub可以利用一个Series将一个所有的dataFrame减去这个DatFrame结构然后返回一个新的DataFrame的值
- df.apply可以将一个函数传递到df结构之中，这样就可以在DataFrame结构对每一个元素使用这个函数,对于默认的axis=0的值可以将其看作为以某一个column为单位将数据传入到某一个func中，这样可以输出所有的在某一个column为单位的添加过func之后的值
- <br />
<a name="agcci"></a>
## Series
DataFrame中含有Series类,每一个Series类都会带有一个dtype类型的数据

```cpp
#这样生成出来的Series的dtype为object,这里是通过list将字符串进行了解析
s=list('123456')
s=pd.Series(s)
```
<a name="dUSX1"></a>
## 分组操作

```python
df = pd.DataFrame({'A': ['foo', 'bar', 'foo', 'bar',
                         'foo', 'bar', 'foo', 'foo'],
                   'B': ['one', 'one', 'two', 'three',
                         'two', 'two', 'one', 'three'],
                   'C': np.random.randn(8),
                   'D': np.random.randn(8)})

```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/522673/1582268028929-c79d9c50-06ed-4764-9d6d-c986ee56e196.png#align=left&display=inline&height=157&name=image.png&originHeight=157&originWidth=310&size=9658&status=done&style=none&width=310)<br />图 将数据按照类别A进行分组

df.groupby(['A','B']).sum()<br />会将AB进行分组，可以看到这样分组可以最后得到一个有一个双层结构的表格，可以根据分组最后得到值。这里AB和CD之所以会出现高低差，是因为AB使用了MultiIndex而CD使用的column<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/522673/1582267923389-e854737b-e0e2-432d-b07a-01da25e9d7df.png#align=left&display=inline&height=230&name=image.png&originHeight=230&originWidth=331&size=17844&status=done&style=none&width=331)<br />df.stack可以将df转化为行索引的操作，而不需要columns'<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/522673/1582269583532-14aec8b1-1eaf-410e-91c9-30a9917a5f43.png#align=left&display=inline&height=395&name=image.png&originHeight=395&originWidth=400&size=33571&status=done&style=none&width=400)
<a name="bXqks"></a>
## 透视表
透视表可以方便人们对表的观察，其中透视表可以让人看到具体的行号和列号下的数据。<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/522673/1582377970286-db7f41ad-4531-4b56-8515-50b8bbfbc528.png#align=left&display=inline&height=363&name=image.png&originHeight=363&originWidth=521&size=38277&status=done&style=none&width=521)<br />原始表如图<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/522673/1582378014301-ebf34080-4f67-47ac-88f7-fdc13e0b0019.png#align=left&display=inline&height=387&name=image.png&originHeight=387&originWidth=634&size=36863&status=done&style=none&width=634)<br />透视表如图<br />透视表中C变为了行索引，DE变为了列索引，-0.810965由A=one,B=A,C=foo,D=-0.810965得到。其中E的直被拆为了另一列，A=ONE,B=A,C=foo,E=-2.016254
