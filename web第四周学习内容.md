1.目前owasp的十大web安全漏洞是哪些？这些漏洞排名是按照漏洞的严重程度排序的还是按照漏洞的常见程度排序的？（2分）  
```
1.Injection->注入(SQL, NoSQL, OS, and LDAP injection)  
2.Broken Authentication->中断身份验证
3.Sensitive Date Exposure->敏感数据泄露
4.XML External Entities(XXE)->XML 外部处理器漏洞
5.Broken Aceess Control->中断访问控制
6.Security Misconfiguration->安全配置错误
7.Cross-Site-Scripting(XXS)->跨站脚本攻击
8.Insecure Deserialization->不安全的反序列化
9.Using Components with Known Vulnerabilities->使用含有已知漏洞的组件
10.Insufficient Logging & Monitoring->不足的记录和监控漏洞
```
```
严重程度
```
2.请翻译一下credential stuffing（1分）
```
撞库
```
3.为什么说不充分的日志记录(insufficient logging)也算owasp十大漏洞的一种？他的危害性如何（2分）
```
因为一个日志系统不完善的服务器很容易遭受攻击并且在遭受攻击后无法判断攻击来源，这样就无法做出相应的防御，很可能再次遭受同样的攻击。
```
4.请翻阅一下owasp testing guide，以及owasp testing guide check-list，视频说怎么结合这两个文档来学习渗透测试？ 结合你平时渗透过程中的经验，谈谈你的感想。（3分）
```

```
5.you are only as good as you notes
   you are only as good as things you can refer to
结合这两句话谈谈你的感想。（2分）
```
我们平时学习要多记笔记，要多举一反三
```
