#Joomla对象注入漏洞
@(WebPEN)[Joomla, infosec, PHP Object Injection]
##原理
1. session反序列化：
	PHP函数重写read()方法
2. 字符截断
	出现4字节字符时会发生截断
	截断方法：字符串中插入“|”
3. 漏洞分析
	注入点：user-agent, x-forwarded-for
	未过滤，完全可控

##利用
1. 将恶意代码放在 User-Agent 或 X-Forwarded-For 中发送给网站
2. 将网站返回的cookie值带入第二个请求中，即可触发漏洞。