---
isChild: true
---

## Composer and Packagist

Composer是一个**出色**的PHP依赖管理器，把项目的依赖列在`composer.json`文件中，然后通过一些简单的命令，Composer就会
自动的帮你下载这些依赖，并配置好自动加载路径。

现在已经有很多PHP库支持Composer，可以在项目中使用它们，具体列表可以[点击查看][1]，这是官方支持的Composer兼容的PHP库。

### 如何安装Composer

Composer可以安装在本地(在当前工作目录，不推荐这种方式)，也可以安装在系统中(如/usr/local/bin)。假设你要在本地安装，在
项目的根目录执行：

    curl -s https://getcomposer.org/installer | php

它会下载`composer.phar`(PHP二进制文档)，然后你就可以用`php`运行它来完成项目依赖的管理。 <strong>请注意:</strong>如果
你通过管道直接把下载的代码传给PHP解释器，请先在线阅读代码以确保该代码是安全的。

### 如何手动安装Composer

手动安装composer有点麻烦，不过很多开发者可能更喜欢这种安装方式。使用交互式安装程序，它会检查你安装的PHP:

- PHP版本满足要求
- `.phar`文件可以正确执行
- 相关目录的权限设置正确
- 没有加载某些不兼容的扩展
- 相应的`php.ini`设置正确

而手动安装则需要你自己做这些事情，你必须自己权衡利弊，以决定是否手动安装。下面是手动获取Composer的方法:

    curl -s https://getcomposer.org/composer.phar -o $HOME/local/bin/composer
    chmod +x $HOME/local/bin/composer

目录`$HOME/local/bin`(或你自己选择其它目录)应该在你的`$PATH`环境变量中，从而可以直接运行`composer`命令。

这样文档中描述的运行Composer的命令`php composer.phar install`，就可以用如下命令替代：

    composer install
    
下面默认你已经在PATH路径下安装Composer。

### 如何定义和安装依赖

Composer通过文件`composer.json`跟踪项目的依赖。这个文件可以手工维护，也可以通过Composer管理，命令`php composer require`用于添加项目的依赖，如果项目下还没有`composer.json`文件，则会自动创建一个。下面是一个依赖[Twig][2]例子，在项目的根目录执行：

	composer require twig/twig:~1.8

或者通过`composer init`命令也可以一步步地引导你创建项目所需的`composer.json`文件。无论使用哪种方式创建了`composer.json`文件后，就可以通过Composer下载和安装项目依赖到目录`vendors/`:

    composer install

最后在应用的PHP入口文件添加下面代码，告诉PHP使用Composer自动加载器加载项目的依赖库：

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

现在你就可以使用项目依赖的库了，它们会在需要的时候自动加载。

### 更新依赖

当你首次执行`php composer.phar install`命令时，Composer会创建一个叫做`composer.lock`的文件，其中保存了所下载的每个依赖库
确切的版本信息。 如果你把`composer.lock`作为项目的组成部分共享给其他程序，当他们执行`php composer.phar install`命令时，他
们将会得到与你版本相同依赖库。如果要更新你的依赖库，执行`php composer.phar update`即可。

当你想要灵活的指定所需依赖库的版本时，这将会非常有用。比如~1.8的版本要求表示“任何比1.8.0新，但不超过2.0.x-dev的版本”。你
也可以使用`*`通配符如`1.8.*`。这样Composer的`php composer.phar update`命令将会把所有的依赖库升级到满足你所指定条件的最新
版本。

### 更新通知

想要收到依赖库新版发布的通知，你可以注册一个[VersionEye][3]账号，这是一个监控你GitHub和BitBucket上的`composer.json`文件并
通过邮件发送依赖库更新通知的web服务。

### 检查依赖包的安全问题

[Security Advisories Checker][4] 包括一个web服务和一个命令行工具, 两者都会检查你的`composer.lock`文件并告知你是否需要
对所用到的依赖包进行升级。

* [学习Composer][5]

[1]: http://packagist.org/
[2]: http://twig.sensiolabs.org
[3]: https://www.versioneye.com/
[4]: https://security.sensiolabs.org/
[5]: http://getcomposer.org/doc/00-intro.md
