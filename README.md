# ThinkPHP5快速入门教程笔记

# 基础

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

**渲染视图时，需要导入Controller类并继承Controller类**。

渲染视图需要用到```think```命名空间中的```Controller```类中的方法，所以需要先导入```Controller```类。

**驼峰命名法命名的控制器和方法名，在创建模板文件夹和模板文件时，需要注意使用```_```间隔大写字母**。否则会报错：
```模板文件不存在:D:\Wamp64\www\tp5\public/../application/test4\view\test_con\say_age.html```

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

# URL和路由

## URL访问

>ThinkPHP采用单一入口模式访问应用，对应用的所有请求都定向到应用的入口文件，系统会从URL参数中解析当前请求的模块、控制器和操作(也就是方法)，下面是一个标准的URL访问格式：```http://domainName/index.php/模块/控制器/方法```。
>>index.php就是应用的入口文件，通过修改apache的```.htaccess```或者nginx的```nginx.conf```文件，入口文件可被隐藏。

>>无论URL是否开启大小写转换，模块名都会被强制小写。

驼峰命名的控制器，例如类名为```HelloWorld```，那么正确的访问方式为```http://tp5.com/index.php/index/hello_world/index```

关闭URL自动转换，即可支持驼峰命名法进行控制器访问，同时通过URL访问时需要严格区分大小写(模块强制小写)。

```
// 关闭URL自动转换（支持驼峰访问控制器）
'url_convert' => false,
```

## 参数传入(在URL中传递参数值)

我这里生成了一个模块test2，在test2下有一个Index控制器和index方法，还有个hello方法：
```php
namespace app\test2\controller;

// 默认控制器
Class Index
{
  // 默认方法(也叫操作)
  public function index()
  {
    return 'index';
  }

  public function hello($name='World',$city)
  {
    return 'Hello'.$name.'You come from'.$city;
  }
}
```

**直接访问入口文件，由于在URL中没有指定模块、控制器、方法，所以系统会去访问默认的index模块、Index控制器、index方法。**可以看下配置文件：
```php
    // 文件位置：\tp5\application\config.php
    // +----------------------------------------------------------------------
    // | 模块设置
    // +----------------------------------------------------------------------

    // 默认模块名
    'default_module'         => 'index',
    // 禁止访问模块
    'deny_module_list'       => ['common'],
    // 默认控制器名
    'default_controller'     => 'Index',
    // 默认操作名
    'default_action'         => 'index',
    // 默认验证器
    'default_validate'       => '',
    // 默认的空控制器名
    'empty_controller'       => 'Error',
    // 操作方法后缀
    'action_suffix'          => '',
    // 自动搜索控制器
    'controller_auto_search' => false,

```

在地址栏中通过输入tp5.test\index.php\test2\index\index可以访问默认的index方法(操作)。
```访问tp5.test\index.php\模块\控制器\方法```

>如果模块、控制器、操作都是默认的，并且做了入口文件隐藏，甚至只需要访问域名即可```tp5.test```或者```tp5.test\inedx.php```即可访问到index模块Index控制器index操作(也叫方法)。

可以通过访问tp5.test\index.php\test2\index\hello来访问hello方法(也叫操作)。

可以注意到hello操作中，有一个```$name```参数和一个```$city```参数，有两种方法可以在URL中给$name和$city**传递值**。
```php
  //···省略若干
  public function hello($name='World',$city)
  {
    return 'Hello'.$name.'You come from'.$city;
  }
```

方法1：tp5.test\index.php\test2\index\hello\name\thinkphp\city\Shanghai
>tp5.test\index.php\test2\index\hello\参数1\参数值1\参数2\参数值2

方法2：tp5.test\index.php\test2\index\hello?name=thinkphp&city=Shanghai
>tp5.test\index.php\test2\index\hello?参数1=参数值1&参数2=参数值2

可以看到hello方法会自动获取URL中的**同名**参数值作为方法的参数值，并且*不受顺序的影响*。

可以对URL地址做简化，简化后URL的参数传值将严格按照传入的参数的顺序来传值了，并且免去了参数名，仅需要参数值。
```php
// 按照参数顺序获取
'url_param_type' => 1,
```
URL地址简化后的访问方式：
```tp5.test/index.php/test2/index/hello/thinkphp/shanghai```

>按顺序绑定参数的话，操作方法的参数只能使用URL pathinfo变量，而不能使用get或者post变量。
>>PATHINFO 模式是一种url访问方式，如下：http://域名/项目名/入口文件/模块名/方法名/键1/值1/键2/值2。

## 定义路由

首先，官方建议导入Route类，并使用Route::rule()方法来定义路由

但是，数组的方式写路由也是被允许的，而且我觉得很方便。*Route::rule()也使用在Laravel框架中*。

定义路由的文件处于```tp5\application\route.php```我们来添加一些路由。
```php
return [
  'index3'=>'test2/Index/index',
  'hello'=>'test2/Index/hello',
];
```
```php
  //···省略若干
  public function hello($name='World')
  {
    return 'Hello'.$name;
  }
```
现在，我们在地址栏中输入```tp5.test/index3```，就会路由到test2模块下的Index控制器下的index方法。
同时，我们在地址栏中输入```tp5.test/hello```，就会路由到test2模块下的Index控制器下的hello方法。

**使用路由，我们可以最大程度的简化URL地址栏。**

如果路由这样写，我们访问```tp5.test/hello```时，会报错并提示```模块不存在```。
```php
return [
  'hello/:name'=>'test2/Index/hello',
];
```
我们可以把路由改为这样，使用```[]```将路由中的变量包围起来，使它变为可选参数，就可以正常访问了。
```php
return [
  'hello/[:name]'=>'test2/Index/hello',
]
```

### 动态定义路由规则

道理还是一样的，只不过书写方式变了而已。

原本这样的路由：
```php
return [
  'hello/[:name]'=>'test2/Index/hello',
]
```
动态定义路由规则的写法：
```php
  //tp5\application\route.php
  use think\Route;
  Route::rule('hello/[:name]','test2/Index/hello');
```

**路由的第一个参数是地址栏中输入的地址(路由后的名称)，第二个参数为被路由的模块、控制器、方法。**

```:name```代表方法中的```$name```参数。

**当路由规则以$结尾的时候就表示当前路由规则需要完整匹配(严格匹配)，即不接受其他非路由允许的数据传入。**

简单的说：以$结尾的路由，必须严格完整的输入路由名所带的参数。

*在路由中，[]中的参数为可选参数。*

如果定义了动态路由，但是提示模块不存在的解决办法，先来看几行代码：
```php
// tp5\application\route.php
   use think\Route;

   return[
    'index3','index/Index/index',
]
   Route::rule('hello/[:name]'，'test2/Index/hello');

// 这时我们去访问：tp5/hello/shanghai，就会报错并提示：模块不存在。

```

解决办法如下：调整代码位置
```php
// tp5\application\route.php
   use think\Route;
   Route::rule('hello/[:name]'，'test2/Index/hello');

   return[
    'index3','index/Index/index',
]


// 这时我们去访问：tp5/hello/shanghai，就会显示：Hello，shanghai

```

### 闭包定义，其实就是路由加匿名函数

*注意return后面要加分号，```return;```*。

```php
use think\Route;
// 闭包定义
// 方法1：
Route::rule('anonymous/[:city]',function($city = 'Shanghai'){return 'Hello ,'.$city;});

// 方法1等同于方法2，只不过官方在TP5.1中更推荐方法1的写法。
// 方法2：
return [
  'anonymous2/[:city]'=>function($city = 'Hangzhou'){return 'Hello ,'.$city;},
];
// 在浏览器中输入tp5.test/anonymous2，会输出Hello ,Hangzhou；输入tp5.test/anonymous2/Beijing，会输出Hello ,Beijing
// 
// 
```

*闭包定义结束*

在```application\config.php中的``````pathinfo_depr```参数，可以改变URL的分隔符。就可以这样访问了：```http://tp5.com/hello-thinkphp```

### 路由参数

可以约束*路由规则中的的请求类型，或者URL后缀等条件*。例子：

控制器：

```php
// 控制器文件tp5/application/test4/controller/TestCon.php
namespace app\test4\controller;

// 要渲染，先导入并继承Controller类
use think\Controller;

Class TestCon extends Controller
{
  public function sayName($name = '小明')
  {
    return 'My Name is: '.$name;
  }

  public function sayAge($age = 20)
  {
    $this->assign('myage',$age);
    return $this->fetch();
  }
}

// 使用tp5.test/test4/test_con/sayname来访问。会输出My Name is: 小明。
```

模板：

```html
<!-- 模板文件tp5/application/test4/view/test_con/say_age.html -->

<html>
<head>
  <title>{$myage}</title>
</head>
<body>
  <p>My Age is {$myage}</p>
</body>
</html>
```

路由：

```php
// 路由文件tp5/application/route.php
use think\Route;

// Route::rule('sayname/[:name]','test4/TestCon/sayname');

return [
  'sayage/[:age]'=>['test4/TestCon/sayage',['ext'=>'html']],
];
```

已经设定了**路由参数**。现在访问tp5.test/sayage，会提示*模块不存在*，并且设置了*ext*参数，现在URL中只接受```.html```结尾的地址，并且无法在地址栏中为方法传入参数值。

**写路由时，第一个参数可以是任意字符，比如：**
```php
use think\Route;

return [
      // 路由名称/[可选参数]=>模块/控制器/方法
     'myNameRoute'=>'test3/MyController/userinfo',
];
```


*路由参数完毕*


### 变量规则

路由规则的第二个参数的第二个参数叫做变量规则，变量规则可以限制网页访问方式，网页文件格式，正式表达式等。

可以通过正则表达式决定要显示的页面，或者说决定要使用的路由。

```php
use think\Route;

return [
  'blog2/:year/:month' => ['test5/blog/archive',['method'=>'get','year'=>'\d{4}','month'=>'\d{2}']],
  'blog2/:id'          => ['test5/blog/get', ['method' => 'get'], ['id' => '\d+']],
  'blog2/:name'        => ['test5/blog/read', ['method' => 'get'], ['name' => '\w+']],
];
```

可以看到上面的路由名称都是一样的，路由是如何分辨你是需要查看id的内容呢，还是name的内容呢？
这就由正则表达式决定了：
```php
// \d+就是数字
// \w+就是非数字
// \d{4}就是4位的数字
```

*变量规则完毕*


### 路由分组

如果路由名称一样，参数不一样，那么可以将路由作为一个二维数组来使用;

将原来的路由名称作为第一维的数据，记得在路由名称周围加上```[]```;

```php
use think\Route;

return [
  'blog2/:year/:month' => ['test5/blog/archive',['method'=>'get','year'=>'\d{4}','month'=>'\d{2}']],
  'blog2/:id'          => ['test5/blog/get', ['method' => 'get'], ['id' => '\d+']],
  'blog2/:name'        => ['test5/blog/read', ['method' => 'get'], ['name' => '\w+']],
];
```

变成了以下内容

```php
use think\Route;

return [
  '[blog2]'=>[
    ':year/:month' => ['test5/blog/archive',['method'=>'get','year'=>'\d{4}','month'=>'\d{2}']],
    ':id'          => ['test5/blog/get', ['method' => 'get'], ['id' => '\d+']],
    ':name'        => ['test5/blog/read', ['method' => 'get'], ['name' => '\w+']],
  ],
];
```

>>路由分组一定程度上可以提高路由检测的效率。

*路由分组完毕*

### 复杂路由

局部变量规则会覆盖全局变量规则。

```php
use think\Route;

return [
  // 全局变量规则
  '__pattern__' =>[
    'year' => '\d{4}',
    'month' => '\d{2}',
  ],
  // 局部变量
  '[blog2]'=>[
    ':year/:month' => 'test5/blog/archive',
    ':id'          => ['test5/blog/get', ['method' => 'get'], ['id' => '\d+']],
    ':name'        => ['test5/blog/read', ['method' => 'get'], ['name' => '\w+']],
  ],
];
```

*复杂路由完毕*

*官方在TP5.1中建议使用```Route::rule()方法定义路由```*。

## URL生成
路由参数中的```'ext' => 'html'```参数，此参数会强制我们必填.html后缀，就不能偷懒了。

URL伪静态后缀会影响到url()方法自动识别的后缀。
```
    //application\config.php
    // URL伪静态后缀
    'url_html_suffix'        => 'html',
```

来看个例子：
```php
// 路由
use think\Route;

return [
  'today/[:year]/[:month]/[:day]'=>['request/Index/today',['method'=>'get'],['year'=>'\d{4}']],
];
```
```php
// 控制器
namespace app\request\controller;

Class Index
{
    public function today($year = 2018,$month = 7,$day=7)
    {
      echo '今天是：'.$year.'年'.$month.'月'.$day.'日。';
      return url('request/Index/today',['year'=>$year,'month'=>$month,'day'=>$day]);
    }
}
```
```php
// 输出
// 当我们访问http://tp5.test/today就会输出
// 今天是：2018年7月7日。/today-2018-7-7.html
```
为什么中间是-而不是/呢，因为我在config.php中设置了    
```php
// pathinfo分隔符
    'pathinfo_depr'          => '-',
// pathinfo分隔符可以改变地址栏中的分隔符
```
**SEO优化时，越多的```/```就会被百度识别成一级又一级的目录，而百度最多只抓到第三级目录，所以使用pathinfo分隔符对SEO比较好。**

Url类可以简化路由规则，使用url()方法可以生成URL地址，以后配置改变也无需改动URL地址，用法：

>>url('方法名','参数值');
>>url方法的第一个参数是路由规则中的第二个参数

*一般url用于模板中的各种链接中。*
*推荐使用url()方法，而不是Url::build()*
*官方文档介绍的很清晰，可以看[文档](https://www.kancloud.cn/thinkphp/thinkphp5_quickstart/478283)*
### 至此，URL和路由介绍完毕

# 请求和响应

*先来看一下**链式调用***：
```php
class abcd
{
  public function A()
  {
    echo 'Hello ';
    return $this;
  }

  public function B()
  {
    echo 'World';
    return $this;
  }
  public function C()
  {
    echo ' !';
    return $this;
  }
  public function D()
  {
    return $this->A()->B()->C();
  }
}

$abcd = new abcd();
$abcd->D();
// 链式调用就是同时调用几个方法，相比这种:
// $abcd->A();
// $abcd->B();
// $abcd->C();
// 更简单，也更好书写代码，更方便
// 链式调用没那么复杂，就是一次性调用ABC方法而已。
// 当然，前提是，方法中都return $this和它们都是一个类的方法。
```

## 请求和响应

>>本章主要了解如何获取当前的请求信息，以及进行不同的输出响应、跳转和页面重定向。

>>ThinkPHP5的架构设计和之前版本的主要区别之一就在于增加了Request请求对象和Response响应对象的概念，了解了这两个对象的作用和用法对你的应用开发非常关键。



