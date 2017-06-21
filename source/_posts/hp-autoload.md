---
title: php 自动加载
date: 2017-6-11 14:48
tags: [php,自动加载]
---

在开发过程中，每个类通常被放在单独的一个文件，而要使用这些类，就要将类文件加载进来。以前通常用include包含进来，但是如果有很多类文件，那就要写一长串的include，令人很烦恼。

在php5之后，有了更好的解决方法。那就是自动加载。自动加载有两个方法。分别是\__autoload()和spl_autoload_register()。
<span style="color:#d02727;">
尽管 \__autoload()函数也能自动加载类和接口，但是相对spl_autoload_register() 来说不够灵活，而且以后可能被淘汰，建议使用spl_autoload_register()来自动加载。
</span>

<!-- more -->
## \__autoload()

虽然不建议使用 \__autoload() 但还是要简单说一下。下面举一个简单的使用。
假设有两个类，A类和B类，分别在A.php和B.php中。
A类
``` php
<?php

class A {
    public function __construct() {
        echo '我是A类<br/>';
    }
}
```
B类
``` php
<?php

class B {
    public function __construct() {
        echo '我是B类<br/>';
    }
}
```
在index.php中调用A类和B类
``` php
<?php
function __autoload($class) {   
    $file = $class . '.class.php';   
    if (is_file($file)) {   
        require_once($file);   
    }   
}   
$a = new A();
$b = new B();

//我是A类
//我是B类
```

## spl_autoload_register()
(PHP 5 >= 5.1.2, PHP 7)
spl_autoload_register — 注册给定的函数作为 \__autoload 的实现
##### 说明
``` php
bool spl_autoload_register ([ callable $autoload_function [, bool $throw = true [, bool $prepend = false ]]] )
```
将函数注册到SPL \__autoload函数队列中。如果该队列中的函数尚未激活，则激活它们。

如果在你的程序中已经实现了\__autoload()函数，它必须显式注册到\__autoload()队列中。因为 spl_autoload_register()函数会将Zend Engine中的\__autoload()函数取代为spl_autoload()或spl_autoload_call()。

如果需要多条 autoload 函数，spl_autoload_register() 满足了此类需求。 它实际上创建了 autoload 函数的队列，按定义时的顺序逐个执行。相比之下， \__autoload() 只可以定义一次。

#### 参数
autoload_function
欲注册的自动装载函数。如果没有提供任何参数，则自动注册 autoload 的默认实现函数spl_autoload()。

throw
此参数设置了 autoload_function 无法成功注册时， spl_autoload_register()是否抛出异常。

prepend
如果是 true，spl_autoload_register() 会添加函数到队列之首，而不是队列尾部。

#### 返回值
成功时返回 TRUE， 或者在失败时返回 FALSE。

#### 例子
同样假设有两个类，A类和B类，分别在A.php和B.php中。
A类
``` php
<?php

class A {
    public function __construct() {
        echo '我是A类<br/>';
    }
}
```
B类
``` php
<?php

class B {
    public function __construct() {
        echo '我是B类<br/>';
    }
}
```
在index.php中调用A类和B类
``` php
<?php

spl_autoload_register();
$a = new A();
$b = new B();

//我是A类
//我是B类
```

这只是spl_autoload_register()简单的使用，这里调用的默认的spl \__autoload()方法来加载，当然我们也可以自己定义\__autoload()
``` php
<?php

function load($class) {   
    $file = $class . '.class.php';   
    if (is_file($file)) {   
        require_once($file);   
    }   
} 
spl_autoload_register('load');  
$a = new A();
$b = new B();

//我是A类
//我是B类
```
### 扩展
如果类文件的名字不是A.php，B.php，而是A.class.php，B.class.php,也不再同一目录下，而在class目录下，那么就会出现问题。
方法1 通过set_include_path()和spl_autoload_extensions()
``` php
<?php

set_include_path('class');
spl_autoload_extensions('.class.php');
spl_autoload_register(); 
$a = new A();
$b = new B();
```
方法二 通过自己定义\__autoload()
``` php
<?php

function load($class) {   
    $file = './class/'.$class . '.class.php';   
    if (is_file($file)) {   
        require_once($file);   
    }   
} 

spl_autoload_register('load'); 
$a = new A();
$b = new B();
```