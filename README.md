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

\_\_DIR\_\_是PHP中的魔术常量，可以输出当前文件的绝对路径。

>>..的意思是向上一级目录

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