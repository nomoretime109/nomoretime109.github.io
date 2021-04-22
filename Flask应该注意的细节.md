# 一、命名类的细节

1.在一个flask文件夹中，命名的python文件不可与文件夹的名字一样

比如文件夹名为：template

而命名的python文件名为：template2.2

就连tem2.2也不行

以上命名会导致在运行template2.2的时候出现一下错误

![image-20210412205212280](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210412205212280.png)

# 二、渲染中的引用细节

***1.第一种***（字典的引用）

```python
#1.创建字典
user={
    'username':'韩立',
    'gender':'男'
    'age':22
}
#2.将字典中的元素传到html中
render_template('index.html',**user)
#3.在html中引用
<p>用户名:{{username}}</p>
<p>性别:{{gender}}</p>
<p>年龄:{{age}}<p>
"""
解释：使用**+字典名，传递字典中的元素
然后在html中引用的时候{{}}中直接是字典元素左边的部分的名
"""
```

***2.第二种***（字典的引用）

```python
#1.创建字典同上
#2.字典传递
render_template('index.html',use=user)
"""
将user赋给render_template的一个参数,字典名！！！
"""
#3.引用
<p>用户名:{{use.username}}</p>
<p>性别:{{use.gender}}</p>
<p>年龄:{{use.age}}<p>
"""
在这种方法中需要参数名加上成员选择符来去引用字典中的元组
"""
```

***3.第三种***（类的引用）

```python
#1.创建类
    class Student(object):
        name='历飞雨'
        gender='男'
        age=25
#2.类对象的传递
render_template('index.html',person=Student)
#3.引用
    <p>这个是{{ person.name }}</p>
    <p>他是{{ person.gender }}</p>
    <p>他多大{{ person.age }}</p>
"""
#4.解释：由于在render_template()中相当于初始化了一个person类对象
所以将这个对象传递进html中时，也要遵循着类对象去引用其数据成员和成员函数的规则
即对象名+成员选择符
"""
```

