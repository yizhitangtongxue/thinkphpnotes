# ThinkPHP5快速入门教程笔记

## 安装Composer，我将使用Composer安装ThinkPHP5

[Composer中文文档](https://www.kancloud.cn/thinkphp/composer/35671)

添加环境变量，使CMD命令行可以运行php命令。添加方法：右键打开"此电脑"->"属性"->"高级系统设置"->"环境变量"->"系统变量"->找到"Path"，双击它，选择"新建"，复制PHP的安装目录位置，比如我的是"D:\Wamp64\bin\php\php7.2.4"，复制黏贴完成后，点"确定就可以了"。到此，我们需要新开一个CMD命令行，运行"php -v"命令，如果成功跳出PHP的版本信息，即视为操作成功。

打开命令行并依次执行下列命令安装最新版本的 Composer：
```php
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
// 执行此条命令下载下来的 composer-setup.php 脚本将简单地检测 php.ini 中的参数设置，如果某些参数未正确设置则会给出警告；然后下载最新版本的 composer.phar 文件到当前目录。
// 下载安装脚本 － composer-setup.php － 到当前目录。
```
```php
php composer-setup.php
// 执行安装过程。
```
```php
php -r "unlink('composer-setup.php');"
// 删除安装脚本。
```
在PHP的**根目录**下使用CMD命令行(或者GitBash)执行命令，新建composer.bat文件。
```php
echo @php "%~dp0composer.phar" %*>composer.bat
```
下载[composer.phar](https://getcomposer.org/composer.phar)文件，将其复制到PHP的**根目录**下。

新开一个CMD命令行，执行composer -V命令，如果跳出版本信息即视为安装成功。

>>记得经常执行composer selfupdate以保持Composer一直是最新版本哦！

>>本Composer安装教程部分来自于[Packagist / Composer中国全量镜像](https://pkg.phpcomposer.com/#how-to-install-composer)

GitBash无法使用composer命令的解决办法，转自[即墨丹青](https://blog.csdn.net/qq_26282869/article/details/80054551)：

>>git-bash 不识别 composer.bat 文件，需要新建一个 composer (是没有后缀的文件)文件，输入如下内容：
```shell
#!/usr/bin/env sh

# php /path/to/composer.phar $*
php `dirname $0`/composer.phar $*
```
>**将该文件放到php.exe同目录下**
>>有个这一行 #!/usr/bin/env sh ，git-bash会自动识别为可执行文件，不需要也不能使用chmod命令修改权限。

![Composer安装成功](https://github.com/yizhitangtongxue/thinkphpnotes/blob/master/Files/composer.png)

### 至此，Composer安装完成。

*优化配置：*

为了避免出现各种连接问题，建议使用国内的Composer镜像。在GitBash或者CMD命令行中执行以下命令：
```composer
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

## 使用Composer安装ThinkPHP5

[Composer中文文档](https://www.kancloud.cn/thinkphp/composer/35671)

切换至Web根目录下，执行```composer create-project topthink/think=5.0.*  tp5  --prefer-dist```

>这行代码表示安装在5.0.0以上，5.1.0以下的最新ThinkPHP框架，它有可能是5.0.0甚至是5.0.9。
>并且会自动创建tp5文件夹。

>>--prefer-source: 下载包的方式有两种： source 和 dist。对于稳定版本 composer 将默认使用 dist 方式。而 source 表示版本控制源 。如果 --prefer-source 是被启用的，composer 将从 source 安装（如果有的话）。如果想要使用一个 bugfix 到你的项目，这是非常有用的。并且可以直接从本地的版本库直接获取依赖关系。

>>--prefer-dist: 与 --prefer-source 相反，composer 将尽可能的从 dist 获取，这将大幅度的加快在 build servers 上的安装。这也是一个回避 git 问题的途径，如果你不清楚如何正确的设置。

![tp5_installed](https://github.com/yizhitangtongxue/thinkphpnotes/blob/master/Files/tp5_installed.png)

### 至此，ThinkPHP5安装完成。

*优化配置：*

我们可以用Git来维护文件版本，方便对文件进行版本的切换修改。

在Github创建一个新的仓库，仓库名tp5。

进入ThinkPHP的根目录，执行以下命令：
```php
echo "# tp5" >> README.md
git init
git add -A
git commit -m "first commit"
git remote add origin git@github.com:yizhitangtongxue/tp5.git
git push -u origin master
```

## 目录结构

初始的目录结构如下：

~~~
www  WEB部署目录（或者子目录）
├─application           应用目录
│  ├─common             公共模块目录（可以更改）
│  ├─module_name        模块目录
│  │  ├─config.php      模块配置文件
│  │  ├─common.php      模块函数文件
│  │  ├─controller      控制器目录
│  │  ├─model           模型目录
│  │  ├─view            视图目录
│  │  └─ ...            更多类库目录
│  │
│  ├─command.php        命令行工具配置文件
│  ├─common.php         公共函数文件
│  ├─config.php         公共配置文件
│  ├─route.php          路由配置文件
│  ├─tags.php           应用行为扩展定义文件
│  └─database.php       数据库配置文件
│
├─public                WEB目录（对外访问目录）
│  ├─index.php          入口文件
│  ├─router.php         快速测试文件
│  └─.htaccess          用于apache的重写
│
├─thinkphp              框架系统目录
│  ├─lang               语言文件目录
│  ├─library            框架类库目录
│  │  ├─think           Think类库包目录
│  │  └─traits          系统Trait目录
│  │
│  ├─tpl                系统模板目录
│  ├─base.php           基础定义文件
│  ├─console.php        控制台入口文件
│  ├─convention.php     框架惯例配置文件
│  ├─helper.php         助手函数文件
│  ├─phpunit.xml        phpunit配置文件
│  └─start.php          框架入口文件
│
├─extend                扩展类库目录
├─runtime               应用的运行时目录（可写，可定制）
├─vendor                第三方类库目录（Composer依赖库）
├─build.php             自动生成定义文件（参考）
├─composer.json         composer 定义文件
├─LICENSE.txt           授权说明文件
├─README.md             README 文件
├─think                 命令行入口文件
~~~

有几个关键的路径先了解下：

目录	 | 说明 | 常量
---  | ---- | ----
tp5	 | 项目根目录 | ROOT_PATH
tp5/application | 应用目录 | APP_PATH
tp5/thinkphp | 框架核心目录 | THINK_PATH
tp5/extend | 应用扩展目录	 | EXTEND_PATH
tp5/vendor | Composer扩展目录 | VENDOR_PATH

## 入口文件

[PHP中的魔术常量](http://php.net/manual/zh/language.constants.predefined.php)

TP5的默认入口文件在public/index.php，来看下入口文件内容：
```php
// [ 应用入口文件 ]

// 定义应用目录
define('APP_PATH', __DIR__ . '/../application/');
// 加载框架引导文件
require __DIR__ . '/../thinkphp/start.php';
```

进到application(应用目录)目录下，可以看到只有一个index模块目录，可以使用控制台命令来添加新的模块目录。

使用GitBash或者CMD命令行进入tp5根目录，执行如下命令添加新的模块(目录)：

```
php think build --module demoName
```

此命令会生成一个demo模块，同时也会生成一个默认的Index控制器文件。

生成的模块目录在tp5\application\demo下。
```
controller是控制器文件
model是模型文件
view是视图文件
config.php是模块配置文件
common.php是模块公共文件
```

用户浏览网页->将操作传递给controller->控制器判断是否需要动态调用数据->需要->控制器将数据传递给model处理(数据库)->model将处理过的数据传递给view进行渲染->view渲染完毕呈现给用户。

用户浏览网页->将操作传递给controller->控制器判断是否需要动态调用数据->不需要->view渲染完毕呈现给用户。

\_\_DIR\_\_是PHP中的魔术常量，可以输出当前文件的绝对路径。

>>..的意思是向上一级目录

### 至此，入口文件介绍完毕。

## 域名配置

将```C:\Windows\System32\drivers\etc\hosts```文件复制到桌面上，增加一条```127.0.0.1      tp5.test```到host文件中，再将hosts文件复制**替换**到```C:\Windows\System32\drivers\etc\hosts```下。至此，hosts文件修改完成。

修改httpd-vhosts.conf(或vhosts.conf)文件，增加以下代码：
```
<VirtualHost *:80>
  ServerName tp5.test
  DocumentRoot "${INSTALL_DIR}/www/tp5/public"
  <Directory "${INSTALL_DIR}/www/tp5/public">
    Options +Indexes +Includes +FollowSymLinks +MultiViews
    AllowOverride All
    Require local
  </Directory>
</VirtualHost>
```

关于httpd-vhosts.conf的配置详情，可以查看[apache的AllowOverride以及Options使用详解](https://www.jb51.net/article/31721.htm)

重启apache服务，在浏览器中输入tp5.test看到相关页面，即视为域名配置成功。

### 至此，域名配置成功。

## 资源访问

网站的资源一般放在public目录下，如果访问的资源不存在，则会报模块不存在的错误。

以下是官方建议的目录规范：
~~~
public
├─index.php   应用入口文件
├─static	  静态资源目录   
│  ├─css      样式目录
│  ├─js       脚本目录
│  └─img      图像目录
~~~

同时，官方只建议将资源文件存放在public目录中，引用一下官方的原话：
>>记住，千万不要在public目录之外的任何位置放置资源文件，包括application目录。

### 至此，资源访问介绍完毕。

## 调试模式

ThinkPHP默认会开启调试模式，但是从5.0.10+版本开始，关闭了默认调试模式，需要自己开启。

调试模式对性能有一定的影响。

调试模式只能全局启用，不能对单独模块开启。

建议在开发中开启调试模式，并在正式部署时关闭调试模式。

>>为了安全考虑，避免泄露你的服务器WEB目录信息等资料，一定记得正式部署的时候关闭调试模式。

修改```application/config.php```文件中的```app_debug```参数，可以开启或关闭调试模式。如下：
```
// 关闭调试模式
'app_debug' =>  false,
```

### 至此，调试模式介绍完毕。

## 控制器Controller

*命名空间可以理解为存放各种类(属性、方法等)的容器。*

*在ThinkPHP5.0的规范里面，命名空间其实对应了文件的所在目录。*

*可以使用use关键字来导入类。*

>>根据类的命名空间可以快速定位文件位置，在ThinkPHP5.0的规范里面，命名空间其实对应了文件的所在目录；
>>app命名空间通常代表了文件的起始目录为application；
>>think命名空间则代表了文件的起始目录为thinkphp/library/think，后面的命名空间则表示从起始目录开始的子目录。

*用URL访问控制器的办法：*

先来看一下默认的控制器位置处于：```\tp5\application\index\controller\Index.php```

```\tp5\application\```后的第一个```index```是**模块名**

```\controller\```后的第一个```Index.php```是**控制器名**

打开Index.php控制器文件，可以看到如下代码：
```php
// 命名空间
namespace app\index\controller;

// 控制器类名
class index
{
	// 控制器方法名
    public function index($name = 'World')
    {
        return 'Hello,' .$name.'!';
    }
}
```

需要注意：**控制器文件名与控制器类名要求相同，不区分大小写**。

使用URL地址来访问默认控制器```\tp5\application\index\controller\Index.php```。
```
http://tp5.test/index/index/

http://tp5.test/模块名/控制器类名/
```
或者：
```
http://tp5.test/index.php/index/index/

http://tp5.test/入口文件/模块名/控制器类名/
```
也可以跟上方法名：
```
http://tp5.test/index.php/index/index/index

http://tp5.test/入口文件/模块名/控制器类名/方法名
```

需要注意：**控制器文件名与控制器类名要求相同，不区分大小写**。

>>可以通过在tp5根目录下使用CMD命令行或者GitBash执行```php think build --module 模块名```生成新模块。
>>生成的新模块位于```/tp5/application/```文件夹下。

*继承公用的控制器：*

```php
// Base控制器(\tp5\application\index\controller\Base.php)
namespace app\index\controller;

class Base
{
	public $name = 'Base';
}
```

```php
// Index控制器(\tp5\application\index\controller\Index.php)
namespace app\index\controller;
// 导入Base类(Base控制器)
// 导入区分大小写
use app\index\controller\Base;

class index extends Base
{
    public function index()
    {
        return 'Hello,' .$this->name.'!';
    }
}

```
>>use关键字导入时区分大小写。

*为控制器的方法定义参数：*
```php
// Func控制器(\tp5\application\index\controller\Func.php)
namespace app\index\controller;

class Func
{
	public function Func($world)
	{
		return 'Hello'. $world .'!';
	}
}

// 执行方法：在浏览器地址栏中输入http://tp5.test/index/Func/Func?world=ThinkPHP。
// 提示方法不存在:app\index\controller\Func->index()的原因是我们这里定义的方法名是Func。
// 而ThinkPHP默认设置的方法名是index，所以这里会报这样的错。
// 解决办法1：把Func方法名改为index。
// 解决办法2：在浏览器地址栏中加上完整的方法http://tp5.test/index/Func/Func?world=ThinkPHP
```
*在地址栏中传递```?参数名=参数值```可以向方法中传递参数值。*

**只有public类型的方法是可以通过URL访问的，如果方法类型为private或者protected则会报异常**。例子：
```php
namespace app\index\controller;

class Func
{
	public function Func($world)
	{
		return 'Hello'. $world .'!';
	}
	protected function Func2()
	{
		return '我是protected类型的方法';
	}
	private function Func3()
	{
		return '我是private类型的方法';
	}
	public function Func4()
	{
		return '我是public类型的方法';
	}
}
```

如果访问Func2和Func3方法，则会提示```方法不存在```，只有Func1和Func4可以访问。

>>如果关闭了*调试模式*，则会提示*页面错误！请稍后再试~*。

### 至此，控制器介绍完毕

## 视图View

>>think命名空间则代表了文件的起始目录为thinkphp/library/think，后面的命名空间则表示从起始目录开始的子目录。

>>think是Think类库包目录。

```use think\Controller;```即为导入```thinkphp/library/think```中的```Controller```类。

我们可以在```\tp5\application\index```模块中新建一个视图```view```文件夹。

文件结构解析：

~~~
application
├─index	        模块目录
│ ├─view        视图目录
│ ├─controller  控制器目录
│ └─model       模型目录
~~~

```\tp5\application\index\view```下的文件夹名称，与控制器绑定，举个例子：

比如控制器是```\tp5\application\index\controller\Index.php```

那么控制器绑定的视图就是```\tp5\application\index\view\index\hello.html```

其中hello.html是模板名，模板名与控制器中的方法名绑定。

*简单来说：控制器类名称与视图view下的文件夹名绑定，比如控制器为Index.php\Index类，那么视图就是view\index文件夹*

*简单来说：view\index\hello.html，hello就是模板名，与控制器Index.php中的方法名绑定*

控制器指定了如何渲染视图。

渲染视图需要用到```think```命名空间中的```Controller```类中的方法，所以需要先导入```Controller```类。

```php
namespace app\index\controller;

// 导入Controller类，如果不导入的话，下面继承就得用class Index extends \think\Controller了
use think\Controller;


class Index extends Controller
{
    public function hello($name = 'thinkphp')
    {
        $this->assign('name', $name);
        return $this->fetch();
    }
}
```

assign()方法可以为模板变量进行赋值。

fetch()方法可以渲染输出，fetch()可以接受一个模板名作为参数,比如fetch('hello')。

fetch()方法默认会输出控制器中**与方法名相同的模板**。

*官方原话：*

>>注意，Index控制器类继承了 think\Controller类之后，我们可以直接使用封装好的assign和fetch方法进行模板变量赋值和渲染输出。

>>fetch方法中我们没有指定任何模板，所以按照系统默认的规则（视图目录/控制器/操作方法）输出了view/index/hello.html模板文件。

模板中的变量需要用{}括起来。例子：```{$name}```。

### 至此，视图介绍完毕

## 读取数据

数据库的配置文件在```\tp5\database.php\```中。

有个很有意思的选项
```
   // 数据库表前缀
   'prefix'          => 'think_',
   // 如果数据表的名称为think_data
   // 在实际使用时可以省略为data
```

控制器需要继承```think\Controller;```才能对视图进行渲染。

控制器需要继承```think\Db;```才能对数据库进行操作。

```php
// Db::name();方法用于指定默认的数据表名
Db::name('tableName');

// find()；方法用于查询单条语句
Db::name('tableName')->find();
```

初步理解```Db::name('tableName')->find();```

将```Db::name('tableName')```得到的(数据表名)数据作为参数传入```find()```方法中进行查询。

```Db::name('tableName')->find();```也可以写成：
```Db::find(Db::name('data'));```

>>官方*读取数据*[文档](https://www.kancloud.cn/thinkphp/thinkphp5_quickstart/478277)

### 至此，读取数据介绍完毕