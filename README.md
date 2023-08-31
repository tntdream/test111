# 基于Java反序列化的自定义gadget链

## 1. **概述**

在对目标网站进行安全测试过程中，我们识别到Java反序列化漏洞。攻击者可以利用该漏洞通过反序列化操作注入SQL语句并进一步获取敏感数据。

## 2. **漏洞详情**

### 2.1 漏洞发现

- 当用户登录后，会话cookie包含一个序列化的Java对象。
- 从站点地图中可以发现，该网站引用了一个名为`/backup/AccessTokenUser.java`的文件。
  
- 进一步探索`/backup`目录，发现还有另一个文件`ProductTemplate.java`。
- 通过源代码审查，发现`ProductTemplate.readObject()`方法将模板的id属性传递到一个SQL语句中。

### 2.2 漏洞利用

我们开发了一个简单的Java程序，该程序可以实例化带有任意ID的`ProductTemplate`，然后序列化它，并进行Base64编码。通过这种方式，我们成功地将id设置为一个单引号，从而引发了SQL错误，这表明该网站存在Postgres基础的SQL注入漏洞。

![漏洞证明](https://github.com/tntdream/test111/blob/main/7711693501139_.pic.jpg)
![漏洞证明](https://github.com/tntdream/test111/blob/main/7701693499601_.pic.jpg)
![漏洞证明](https://github.com/tntdream/test111/blob/main/7721693501535_.pic.jpg)
![漏洞证明](https://github.com/tntdream/test111/blob/main/7731693501651_.pic.jpg)

