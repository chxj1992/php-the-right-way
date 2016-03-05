---
isChild: true
---

## 基础概念 {#basic_concept_title}

我们将会通过一个简单，甚至有点幼稚的例子来说明这个概念。

这里我们有一个`Database`类需要一个适配器(adapter)与数据库连接。我们在构造器中实例化这个适配器而产生了一个硬编码的依赖关系。
这样的做法让测试变得困难并且意味着`Database`类与适配器之间产生了紧密的耦合。

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct()
    {
        $this->adapter = new MySqlAdapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

这样的代码可以通过依赖注入的方式重构来减轻类之间的依赖。

{% highlight php %}
<?php
namespace Database;

class Database
{
    protected $adapter;

    public function __construct(MySqlAdapter $adapter)
    {
        $this->adapter = $adapter;
    }
}

class MysqlAdapter {}
{% endhighlight %}

现在我们通过将依赖关系传递给`Database`类的方式代替了自己创建一个依赖。我们甚至可以创建一个接收参数是所要依赖类的方法并
通过它来设置依赖关系，或者假如`$adapter`是一个公开的(`public`)属性我们也可以直接设置它的值。
