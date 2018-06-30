# ThinkPHP5快速入门教程笔记

## 安装ThinkPHP5

### Composer方式安装ThinkPHP5。

1. 添加环境变量，使CMD命令行可以运行php命令。添加方法：右键打开"此电脑"->"属性"->"高级系统设置"->"环境变量"->"系统变量"->找到"Path"，双击它，选择"新建"，复制PHP的安装目录位置，比如我的是"D:\Wamp64\bin\php\php7.2.4"，复制黏贴完成后，点"确定就可以了"。到此,我们需要新开一个CMD命令行，运行"php -v"命令，如果成功跳出PHP的版本信息，即视为操作成功。

2. 打开命令行并依次执行下列命令安装最新版本的 Composer：
```php
php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
//执行此条命令下载下来的 composer-setup.php 脚本将简单地检测 php.ini 中的参数设置，如果某些参数未正确设置则会给出警告；然后下载最新版本的 composer.phar 文件到当前目录。
```
```php
php composer-setup.php
```
```php
php -r "unlink('composer-setup.php');"
```

3. 上述 3 条命令的作用依次是：
  1. 下载安装脚本 － composer-setup.php － 到当前目录。
  2. 执行安装过程。
  3. 删除安装脚本。