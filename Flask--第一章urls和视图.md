一、python虚拟环境的安装和介绍



1.python的虚拟环境目的是为了能够使不同版本的Flask能够共存而互不影响

2.python虚拟环境的安装：在Windows终端使用代码pip install virtualenv（前提是已经配置python的环境变量了才可使用pip）（***virtual*** ***env***ironment）

3.在任意的文件目录中使用virtualenv flask-env创建flask的环境

4.激活想选用的flask环境：进入到flask-env目录中的Script在终端输入activate激活

然后在终端目录前面会出现（flask-env）即说明已经激活，因此以后的操作均在此虚拟环境之下进行的

5.退出虚拟环境：使用指令deactivate

# 二、flask的安装（pip）

1.tips：如果不在虚拟环境中安装flask那么flask将会安装在全局环境之中

2.安装指令：pip install flask

3.查看flask版本：进入python，命令import flask导入flask，使用flask.version(version两边都需要两个下划线)即可查看

# 三、第一个flask程序

1.由于python2默认使用的是ASCII编码，中文不支持，所以在代码前需要加上#encoding:utf-8

![image-20210410201957532](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410201957532.png)

# 四、debug模式

1.当flask代码中出现了错误时在网页中将显示（而且网页并不会提示错误的地方，只能在终端中看到，如果能在网页上直接显示错误就很明了）

![image-20210410203002644](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410203002644.png)

2.所以用debug模式可以让错误显示在网页上，代码为：app.run(debug=True)，提示Pycharm需要手动去打开debug模式

![image-20210410204713561](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410204713561.png)

3.debug模式的另外一个优点是当代码文件中有地方被修改了，那么服务器将自动重启，而不需要手动重启（指的是不需要再回到Pycharm去重新运行代码，而直接刷新网页即可）

![image-20210410205143398](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410205143398.png)

4.总结：

​	*当程序出现问题的时候，可以在页面中看到错误信息和出错的位置

​	*只要修改了项目中的‘Python’文件，程序会自动加载，不需要手动重新启动服务器

5.tips：运行之后再打开debug

# 五、使用配置文件

![image-20210410212647847](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410212647847.png)

![image-20210410212705816](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410212705816.png)

1.新建一个‘config.py’文件

2.在主app文件中导入这个文件，并且配置到‘app’中，示例代码如下：

```python
import config
app.config.from_object(config)
```

值得注意的是括号里面是python的文件名，前面的代码固定的

3.后续中的其他参数，比如‘SECRET_KEY’个'SQLALCHEMY'都是放在配置文件中去

# 六、URL传参到视图中

1.理解URL传参

简书网站地址

![image-20210410214027767](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410214027767.png)

当点击一篇文章《三千字说废就废》的标题时

![image-20210410214110395](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410214110395.png)

会从数据库中传递出ID来响应该点击

![image-20210410214201337](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210410214201337.png)

此时网站地址中后面多出来的部分e2a551641506就相当于ID

2.

```python
@app.route('/article/<id>')
def art(id):
	return '你传过来的参数%s' % id
```

*****

在上面的代码中应该注意的点：

*****在url中所创建的目录名没有要求一定要与函数名一致

*****目录后面所接上的是参数需要使用<>括起来

*****id可以作为视图函数的参数，因此视图函数中需要放和url中的参数同名的参数

*****请注意字符串的格式化输出：当需要格式化输出多个时%（ ，）当只输出一个的时候可以将括号省略，但是前面的百分号不能省

*****而且<>中是自己起的名

3.参数的作用：可以在相同的URL，但是指定不同的参数，来加载不同的数据

# 七、反转URL

1.想要使用URL的反转前提是需要引入url_for

```python
from flask import Flask, url_for
```

2.理解url的反转：

利用url_for使得从视图函数到其对应url的转换

3.使用url_for的细节：

******url_for（）,()中函数名需要用字符串表示

![image-20210411134920734](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210411134920734.png)

******当需要反转的url中有参数时，需要使用id=进行标明

![image-20210411135049367](C:\Users\Kirito\AppData\Roaming\Typora\typora-user-images\image-20210411135049367.png)

4.反转url的用处：

******在页面重定向的时候，会使用url反转，比如

```python
flask.redirect(flask.url_for('detail',id=question_id))
```

可以理解为在页面重定向的时候直接使用url_for去获取你想要重定向的方向

******在模板中，也会使用url反转

```html
<a href="{{url_for('login')}}">登录</a>
```

同样是使用url_for去获取url然后当点击登录的时候根据这个url进行跳转

# 八、页面跳转和重定向

1.导入redirect：

```python
from flask import Flask,redirect
```

2.两种演示方法：

```python
@app.route('/')
def index():
    return redirect('/login/')
@app.route('/login/')
def login():
    return '这里是登录界面'
```

第一种使用redirect是直接在redirect()中用url作为参数

```python
@app.route('/')
def index():
    login_url=url_for('login')
    return redirect(login_url)
@app.route('/login/')
def login():
    return '这里是登录界面'
```

第二种是先使用url_for获取url储存到login_url中，然后再用login_url作为redirect的参数，这样做的好处是无论视图函数login的url怎么改变，redirect的参数不会随着其改变的改变，简而言之就是减少修改代码的次数

3.应用

```python
@app.route('/')
def first():
    return '这里是首页'


@app.route('/login/')
def login():
    return '这里是登录界面!!!'


@app.route('/publish/<is_login>/')
def publish(is_login):
    if is_login=='0':
        return redirect(url_for('login'))
    else:
        return '这里是发布页面'


if __name__ == '__main__':
    app.run(debug=True)
```

在用户访问一些需要登录的页面的时候，如果用户没有登录，那么可以让他重定向到登录页面去

******注意is_login=='0'中‘0’是字符串，不能写成整数0

