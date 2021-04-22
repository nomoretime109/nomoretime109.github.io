

# 一、模板渲染和参数

1.html5文件需要创建在templates文件夹中

2.要想在html界面中显示用户名而且用户名是会随着登入账号不同而不同，所以在html中的用户名是不固定的，而登入的账号只只有后台知道，因此需要借助后台的数据传递给前端

即模板传参

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HomePage</title>
</head>
<body>
    这里是首页
    <p>用户名:{{ username }}</p>
    <p>性别：{{ gender }}</p>
    <p>年龄：{{ age }}</p>
</body>
</html>
```

这里注意：因为flask语法的要求传到html的参数需要用两个花括号

{{}}

```python
@app.route('/')
def homepage():
    return render_template('homepage.html',username='韩立',gender='男',age=22)

```

然后在render_template（）括号的后面再添加对应的参数

但是如果传递的参数很多的时候，这时候就需要使用到字典

```python
@app.route('/')
def homepage():
    context={
        'username':'韩立',
        'gender':'男',
        'age':22
    }
    return render_template('homepage.html',**context)
```

要注意的是render_template（）中需要添加**context，表示将字字典打散成一个个关键字参数去填充到对应的星号中去

3.总结：

***a、***如何渲染模板：

​	******模板放在‘templates’文件夹下

​	******从‘flask’中导入‘render_template’

​	函数，渲染模板。注意：调用render_template()函数的时候括号	内只需填写模板的名字（而且名字被单引号包裹着），不需要填	写‘template’这个文件夹目	录，	但是有一个例外如下：

​	![image-20210412200022143](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210412200022143.png)

​	如果需要渲染的模板是存放在'template'文件目录下的另外一个文	件夹时，括号内需要写成：

![image-20210412200151386](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210412200151386.png)

​	即需要渲染的html前加上目录文件名

***b、***模板传参：

​	******如果只有一个或者少量参数，直接在‘render_template’函数中添加格式为“参数名=（等号的右边按照整型字符串型等类型来写）”，比如username=‘韩立’

​	******如果有多个参数的时候，那么可以先把所有的参数放在字典中，然后在'render_template'中，使用两个星号（格式为星星+字典名），把字典转换成关键字参·		数传递进去，这样的代码更方便管理和使用。

​	******复习字典：1.使用花括号包裹{}

​							2.等号的左边的对象使用单引号包裹着（前提是字符串对象）

​							3.字典中每一组要用逗号隔开

​	在那个视图函数中调用render_template函数，字典就在那个视图函数中



# 二、模板中访问属性和字典

#### 1.模板中使用变量

在模板当中要使用一变量的语法是（两个花括号括起来）：

```html
{{params}}
```

#### 2.如果想在模板当中访问一个模型的属性或者说返回字典的某一值的做法

##### [1].模板中访问一个模型的属性：

***a.***在模板中去访问视图函数中的类（途径与python访问类的对象的类似的）

首先创建类且初始化对象

![image-20210412211023009](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210412211023009.png)

复习类：**与命名函数类似，以class关键字开头，接上  类名（）：，千万别忘记了后面加上冒号

​				**类初始化对象的时候也别忘记了括号

***b.***将类对象加入到字典当中：

![image-20210412211748873](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210412211748873.png)

值得注意的是：图中元组‘person’：p中person的作用是在html模板当中要使用person加上成员选择符‘.’去访问类中的成员

***c.***在模板中访问类对象：

![image-20210412212014951](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210412212014951.png)

##### [2].模板中访问字典中的字典

***a.***可以给字典中的元素创建对应的字典对象

![image-20210414170028530](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210414170028530.png)

***b.***访问的语法：

![image-20210414170142241](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210414170142241.png)

语法与类对象去访问对象中的数据成员是一样的，同样使用成员选择符“.”

同样也可以使用如下的语法二者等价，只有方便与不方便的区分：

```html
{{ website['baidu'] }}
```

# 三、if判断语句

#### 1.语法

在py文件中if语句的语法与python的一样

在html中使用if语句时：

```html
{% if 判断条件 %}
	pass
{% else %}
	pass
{% endif %}#此行语法不可省略，必须要有，意思是结束if语句的判断
```

#### 2.实例

在后端中传入的参数为user=True，注：在本实例中传入的参数中等号的右边如果布尔值为True则在html中去执行if语句的内容，如果是False则就执行else的内容

复习：布尔值为false的表达式

​			False、整数值0、浮点数0.0、None、'' or ""	空字符串、[] or list()空列表、{}空字典、()空集合![image-20210414180319599](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210414180319599.png)

# 四、for循环语句

#### 1.在html中遍历字典：

**[1].**首先创建字典并且利用render_template（）将字典传递到html中

![image-20210419205515733](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419205515733.png)

注：这里的user=user，相当于将左边的user初始化成字典

**[2].**在html中for遍历的语法：

```html
{% for k,v in user.items() %}
	pass
{% endfor %}
```

![image-20210419210221550](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419210221550.png)

注:    a.这里的items（）指的是以列表返回可遍历的（键，值）元组数组

​		 ![image-20210419210046656](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419210046656.png)

既然返回的是元组数组，那么在遍历的时候需要2个参数去遍历，前者是对应键keys后者对应值values

​			b.除了函数items（）之外，还可以使用“keys（）”、“values（）”

#### 2.列表的遍历:

**[1].**创建列表：

![image-20210419211021152](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419211021152.png)

**[2].**遍历:

![image-20210419211054988](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419211054988.png)

#### 3.实例:

![image-20210419212305369](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419212305369.png)

![image-20210419212336269](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419212336269.png)效果图：

![image-20210419212409067](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210419212409067.png)

# 五、过滤器

#### 1.简介:

**[1].**作用的前提：作用的的对象是模板中的变量,模板中的变量形式为{{parmas}}

**[2].**原来的值-------->（经过过滤器）过滤后的值

#### 2.语法：

```html
{{ avatar|过滤器的名称('.........') }}
```

![image-20210420193650727](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420193650727.png)

#### 3.过滤器的种类:

**[1].default过滤器：**这个过滤器的作用是如果传过来的变量不存在，那么就调用默认值

![image-20210420193722204](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420193722204.png)

括号中赋予的就是默认值

default通常用于默认用户头像之类

比如当用户发表的时候没有定义头像，那么就使用服务器给的默认的头像

**[2].length过滤器：**这个过滤器的作用通常是去获取列表、字符串、字典、元组的长度

![image-20210420195013536](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420195013536.png)

![image-20210420195028922](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420195028922.png)

![image-20210420195045405](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420195045405.png)

**[注]:**通常想要将评论中的内容展示出来，那就需要结合到上节课的for变量列表

![image-20210420195201507](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420195201507.png)

其中<ul></ul>指的是一个无序的列表，通常与<li></li>连用，后者的作用是形成一个小圆点

```html
<ul>
    <li>
    </li>
</ul>
```

![image-20210420195430549](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420195430549.png)

![image-20210420195446397](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420195446397.png)

#### 4.其他的过滤器:

##### [1]abs(value):

返回一个数值的绝对值，比如-1|abs

##### [2]default(value,default_value,Boolean=False):

如果当前变量没有值，则会使用参数中的值来代替，比如name|default（‘xiaotuo’）--如果name不存在，则会使用xiaotuo来代替。Boolean=False默认是在只有这个变量undefined的时候才会使用default中的值，如果想使用python的形式判断是否为False，则可以传递Boolean=true。也可以使用or来代替

##### [3]escape(value)或者e：

转义字符，会将<、>等符号转义成HTML中的符号，比如content|escape或者content|e

##### [4]first(value)：

返回一个序列的第一个元素，比如names|first

##### [5]format(value,*arags,**kwargs):

格式化字符串，比如：

```html
{{ "%s"-"%s"|format('hello?','Foo!') }}
将输出：Hello？-Foo！
```

##### [6]last（value）：

返回一个序列的最后一个元素，比如names|last

##### [7]length（value）：

返回一个序列或者字典的长度，比如names|length

##### [8]join(value,d=u''):

将一个列表用d这个参数的值拼接成字符串

##### [9]safe（value）：

如果开启了全局转义，那么safe过滤器会将变量关掉转义，比如content_html|safe

##### [10]int (value):

将值转换为int类型

##### [11]float(value):

将值转换成float类型

##### [12]lower(value):

将字符串转换为小写

##### [13]upper(value):

将字符串转换成小写

##### [14]replace(value,old,new):

替换，将old替换为new的字符串

##### [15]truncate(value,length=225,killwords=False):

截取length长度的字符串

##### [16]striptags(value):

删除字符串中所有HTML标签，如果出现多个空格，将替换一个空格

##### [17]trim:

截取字符串前面和后面的空白字符

##### [18]string(value):

将变量转换成字符串

##### [19]wordcount(s):

计算一个长字符串中单词的个数

# 六、继承和使用block

#### 1.继承通常的用处：

继承通常同于让页面在跳转的时候保持某一部分改变，而让另外一部分保持不变，就相当于继承原来页面的基础上修改

#### 2.课程实例代码解读之创建导航条:

![image-20210420203708087](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420203708087.png)

![image-20210420203721312](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420203721312.png)

![image-20210420203813280](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420203813280.png)

##### [1]首先创建div标签，而且创建class=‘nav’，相当于这个标签的ID

##### [2]标签属性的添加：

```html
<style>
.......
</style>
```

上面代码放置的位置是在<head></head>之间

##### [3]对div标签添加属性：

代码**.nav**，注意前还有一个小圆点，后面紧跟的是花括号，花括号里面放置的是属性

其中background是背景颜色，height是导航条的高度

##### [4]对标签无序列表ul添加属性：

因为是对标签ul，所以添加属性的时候语法直接是ul+花括号

overflow的作用是消除浮动

##### [5]对标签li添加属性：

因为li标签是在ul标签之内的，因此语法应该是ul li+花括号，就相当于是一个路径

float：left表示在左边浮动，同时可以使原本分行的变量并行，list-style：none作用是消除li标签左边的小圆点

padding：0 10 px，前面的数字0是指的是高度，10px指的是宽度，这个作用是标签li中变量之间的间隔line-height与.nav中height相等的时候可以使变量高度居中

##### [6]对标签a添加属性：

同样因为标签a是在标签li之内，而li标签又是在ul之内的，所以同样是像写路径一样

color就是改变标签对应变量的字体的颜色

#### 3.创建父模板

并且将之前index页面中创建的导航条的代码copy到父模板中

![image-20210420205709557](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420205709557.png)

剩余标签同上

#### 4.继承的语法

##### [1].继承网页的所有内容：

```html
{% extends '父模板的名称' %}
```

注意：别忘记了单引号

#### 5.继承的基础上增添新的属性

要想在子模板中增添新的属性，那么就需要在父模板中定义一个接口

##### [1]在父模板中定义接口:

![image-20210420210946358](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420210946358.png)

值得注意的是block之后的main是一个命名，在子模板中要使用哪一个接口，就要对准那个一个命名

##### [2]在子模板中使用这个接口:

![image-20210420211037538](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420211037538.png)

![image-20210420211054055](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420211054055.png)

注意：当使用接口的时候，需要添加的属性只能写在block之间，写在block之外是无			法接入该入口的

#### 6.其他用法：

##### [1].同样也可以用在title标签中

![image-20210420211507798](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420211507798.png)

![image-20210420211731455](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420211731455.png)

##### [2].同于head标签中

![image-20210420212006987](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420212006987.png)

![image-20210420212124137](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420212124137.png)

# 七、URL链接和加载静态文件

#### 1.URL链接

##### [1]方式①(在同一个服务器下运行)：

![image-20210420213050100](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420213050100.png)

##### [2]方式②(在同一个服务器下运行):

![image-20210420213209160](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420213209160.png)

本质上这两种方式其实都是一样的，因为在本地5000的端口可以简写成斜杠

![image-20210420213326946](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420213326946.png)

因此login前面的第一个斜杠实际上代表的就是5000的那个端口的本地服务器

##### [3]方式③url的反转：

![image-20210420213545690](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420213545690.png)

#### 2.加载静态文件(比如CSS文件、图像、js文件)

注：创建的静态文件需要创建在static文件目录之下

##### [1]加载CSS文件

![image-20210420213755859](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420213755859.png)

本来这些样式可以和变量写在同一个文件中的

但为了简洁明了，实际上都放在css文件当中

![image-20210420225444564](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420225444564.png)

![image-20210420225457771](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420225457771.png)

![image-20210420225506662](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420225506662.png)

使用link链接css文件，因为静态文件中通常都是放置在static中的，因此在url_for中第一个参数是static，第二个参数是filename，但是如果静态文件还在其他文件夹中，那么就需要像写路径一样将该文件夹名字也添加上，而且别忘记了静态文件的后缀，这里css文件的后缀是css，如果忘记写这个的话是无法加载该静态文件的

##### [2]加载图片

![image-20210420230117753](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420230117753.png)

![image-20210420230127273](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420230127273.png)

![image-20210420230137761](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420230137761.png)

##### [3]加载js文件

![image-20210420230526690](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420230526690.png)

该js文件的作用是在加载模板网页的时候会弹出警告框

![image-20210420230621334](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420230621334.png)

同样的在写filename文件的时候别忘记了静态文件名的后缀

![image-20210420230713142](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210420230713142.png)