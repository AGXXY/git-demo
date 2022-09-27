# 1.列表

##### 1.列表(list)相当于不用自定义数据类型的数组

##### 2.命名方式：变量 = []

例：

```python
name_list = ['Tom', 'Lily', 'Rose']
print(name_list[0])  Tom
print(name_list[1]) # Lily
print(name_list[2]) # Rose
```

##### 3.使用方法

###### 	1.index()：返回指定数据所在位置的下标 

​		列表序列.index(数据, 开始位置下标, 结束位置下标)

###### 	2.count()：统计指定数据在当前列表中出现的次数

###### 	3.len()：访问列表⻓度，即列表中数据的个数

##### 4.用in和not in来判断数据是否在列表，如果在返回True，否则返回False

```python
name_list = ['Tom', 'Lily', 'Rose']

# 结果：True

print('Lily' in name_list)

# 结果：False

print('Lilys' in name_list)

name_list = ['Tom', 'Lily', 'Rose']

# 结果：False

print('Lily' not in name_list) 

# 结果：True

print('Lilys' not in name_list)
```



##### 5.追加，append(数据)

```python
name_list = ['Tom', 'Lily', 'Rose']
name_list.append('xiaoming')
# 结果：['Tom', 'Lily', 'Rose', 'xiaoming']
print(name_list)

#注意如果append()追加的数据是⼀个序列，则追加整个序列到列表
name_list = ['Tom', 'Lily', 'Rose']
name_list.append(['xiaoming', 'xiaohong'])
# 结果：['Tom', 'Lily', 'Rose', ['xiaoming', 'xiaohong']]
print(name_list)
```

##### 6.extend()：列表结尾追加数据，如果数据是⼀个序列，则将这个序列的数据逐⼀添加到列表

语法：列表序列.extend(数据)

###### 1.单个数据

```python
name_list = ['Tom', 'Lily', 'Rose']
name_list.extend('xiaoming')
# 结果：['Tom', 'Lily', 'Rose', 'x', 'i', 'a', 'o', 'm', 'i', 'n', 'g']
print(name_list)
```

###### 2.序列数据

```python
name_list = ['Tom', 'Lily', 'Rose']
name_list.extend(['xiaoming', 'xiaohong'])
# 结果：['Tom', 'Lily', 'Rose', 'xiaoming', 'xiaohong']
print(name_list)
```

##### 7.遍历列表

```python
name_list = ['Tom', 'Lily', 'Rose']
i = 0
while i < len(name_list):
 print(name_list[i])
 i += 1
```

结果：

![image-20220817140705368](assets/image-20220817140705368.png)

# 2.元组

***思考：如果想要存储多个数据，但是这些数据是不能修改的数据，怎么做？***

答：列表？列表可以⼀次性存储多个数据，但是列表中的数据允许更改。



**⼀个元组可以存储多个数据，元组内的数据是不能修改的。**

##### 1.定义元组（tuple）

元组特点：定义元组使⽤⼩括号，且逗号隔开各个数据，数据可以是不同的数据类型

```python
# 多个数据元组
t1 = (10, 20, 30)
# 单个数据元组
t2 = (10,)
```

**注意：如果定义的元组只有⼀个数据，那么这个数据后⾯也好添加逗号，否则数据类型为唯⼀的 这个数据的数据类型**

```python
t2 = (10,)
print(type(t2)) # tuple
t3 = (20)
print(type(t3)) # int
t4 = ('hello')
print(type(t4)) # str
```

##### 2.元组常见操作

```python
tuple1 = ('aa', 'bb', 'cc', 'bb')
print(tuple1[0]) # aa
```



###### 1.index()：查找某个数据，如果数据存在返回对应的下标，否则报错，语法和列表、字符串的index ⽅法相同

###### 2.count()：统计某个数据在当前元组出现的次数

###### 3.len()：统计元组中数据的个数

#### 小总结:

###### 1.列表可以⼀次性存储多个数据，且可以为不同数据类型--[]

###### 2.元组是可以存储不同数据类型,且数据不可变的序列-------()

###### 3.集合中可以存储任意类型的数据;集合可以去掉重复数据;集合数据是⽆序的，故不⽀持下标{};

###### 4.字典数据为键值对形式出现,各个键值对之间⽤逗号隔开{}，{'key' = value}，value类型可以任意



##### for循环

###### for 变量 in 序列(序列、列表、元组、集合)

若要生成整数序列，则使用range()函数

```python
# range(数字)
range(10) #输出0到9

# range(start， stop)

range(1,10) #输出1到9， 输出的最后一个数是stop-1


```



# 3.集合

##### 1.集合(set)的创建

###### 创建集合使⽤ {} 或 set() ， 但是如果要创建空集合只能使⽤ set() ，因为 {} ⽤来创建空字典

```python
s1 = {10, 20, 30, 40, 50}
print(s1)
s2 = {10, 30, 20, 10, 30, 40, 30, 50}
print(s2)
s3 = set('abcdefg')
print(s3)
s4 = set()
print(type(s4)) # set
s5 = {}
print(type(s5)) # dict
```

![image-20220818141326402](assets/image-20220818141326402.png)

**注意：**

1. ###### 集合可以去掉重复数据； 

2. ###### 集合数据是⽆序的，故不⽀持下标

##### 2.集合常用方法

1.添加add()

```python
s1 = {10, 20}
s1.add(100)
s1.add(10)
print(s1) # {100, 10, 20}
```

###### 集合有自动去重功能，添加集合已有元素时，只会保留一个

2.1删除remove(),**删除集合中的指定数据，如果数据不存在则报错**

```python
s1 = {10, 20}
s1.remove(10)
print(s1)
s1.remove(10) # 报错
print(s1)
```

2.2删除discard()

```python
s1 = {10, 20}
s1.discard(10)
print(s1)
s1.discard(10)
print(s1)
```

##### 3.遍历集合

```python
test_set = {1,2,3,4,'xx',5.2}
for x in test_set:
    print(x)
```

###### 结果：

![image-20220818142456187](assets/image-20220818142456187.png)



# 4.字典

##### 1.创建字典(dict)

```python
# 有数据字典
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
# 空字典
dict2 = {}
dict3 = dict()
```

{}当中，key都是被单引号修饰，对应符号是：而不是=

##### 2.字典常用操作

1.增

语法：**字典序列[key] = 值**

​	*注意：如果key存在则修改这个key对应的值；如果key不存在则新增此键值对。*

```python
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
dict1['name'] = 'Rose'
# 结果：{'name': 'Rose', 'age': 20, 'gender': '男'}
print(dict1)
dict1['id'] = 110
# {'name': 'Rose', 'age': 20, 'gender': '男', 'id': 110}
print(dict1)
```



2.删，del：删除字典或删除字典中指定键值对

```python
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
del dict1['gender']
# 结果：{'name': 'Tom', 'age': 20}
print(dict1)
```



3.改

语法：**字典序列[key] = 值**

```python
dict1['name'] = 'Rose'
```



4.查

**通过key取出value，使用get()函数**

*注意：如果当前查找的key不存在则返回第⼆个参数(默认值)，如果省略第⼆个参数，则返回 None。*

```python
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
print(dict1.get('name')) # Tom
print(dict1.get('id', 110)) # 110
print(dict1.get('id')) # None
```



5.其他函数

​	1.keys()，获得所有字典中的key

​	2.values(), 获得所有字典中的value

​	3.items(), 获得所有字典中的键值对

```python
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
print(dict1.keys()) # dict_keys(['name', 'age', 'gender'])


dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
print(dict1.values()) # dict_values(['Tom', 20, '男'])


dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
print(dict1.items()) # dict_items([('name', 'Tom'), ('age', 20), ('gender',
'男')])
```



6.遍历字典

1.遍历所有元素

```python
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
for item in dict1.items():
 print(item)
```

###### 结果：

![image-20220818145949895](assets/image-20220818145949895.png)



2.遍历所有键值对的方法

```python
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}
for key, value in dict1.items():
 print(f'{key} = {value}')
```

###### 结果：

![image-20220818150105062](assets/image-20220818150105062.png)













