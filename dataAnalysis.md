# 数据清洗和准备
## 处理缺失值
对于数值类型数据pandas 用NaN(NOt a number)表示缺失值 NaN为容易检测到的缺失值

缺失值或不可观察数据表示为NA,python 内建的none值也被当做NA值处理

### 过滤缺失值

用dropna()过滤  当处理DataFrame对象时，dropna默认会删除包含趋势值的行

参数how='all', 删除所有值均为NA的行；

axis=1删除列。

保留包含一定数量的观察值的行，用thresh参数
``` python
df.dropna(thresh=2)
#存在两个及以上的观察值的行会被保留
```
NA处理方法：dropna,fillna,isnull,notnull

### 补全缺失值
用fillna来补全缺失值，可用常数来代替缺失值
``` python
df.fillan(0)
#将缺失值全都替换为0
df.fillan({0:0.5, 2:0})
#将第0列的缺失值替换为0.5，第2列的缺失值替换为0
#更多参数见百度，包含value,method,axis,inplace,limit
```
## 数据转换
### 删除重复值



