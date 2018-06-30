# ThinkPHP5快速入门教程笔记

## 安装Composer，我将使用Composer安装ThinkPHP5

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

新开一个CMD命令行，使用composer -V命名，如果跳出版本信息即视为安装成功。

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
有个这一行 #!/usr/bin/env sh ，git-bash 会自动识别为可执行文件，不需要也不能使用 chmod 命令修改权限。