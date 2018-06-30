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