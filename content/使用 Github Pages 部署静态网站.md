``````` json
{
  "create_time": "2018-11-05T03:57:34.631679Z",
  "description": "DESCRIPTION",
  "id": "8128a900-3a66-4253-baaa-b2890d281e29",
  "meta": null,
  "tags": [],
  "target": "POST",
  "title": "使用 Github Pages 部署静态网站"
}
```````

之前网站部署在阿里云香港的 VPS 上，最近发现某些网络环境下HTTPS打不开网站，开始还以为是 GFW 的问题。但是测试发现 4G 网络访问正常，HTTP 也能正常访问。搜索了一番发现了很多相似的情况。问题是 SSL ClientHello 包无法获得响应导致的。使用 Wrieshick 抓包的结果也证实了这一点。这个锅先甩给运营商吧。

目前还不知道更换证书或更换 VPS 能不能解决这个问题（感觉即使能解决也不是持久之计，有条件的同学还是备案用境内的 VPS 吧）。由于域名转入备案太麻烦，而且正巧发现 Github Pages 对自定义域名支持 HTTPS 了，所以决定吧网站迁移到 Github Pages 上。

# 迁移文件

## 选择站点类型

Github Pages 站点的静态文件都保存在 Repository 中。Github Pages 提供了 User or organization site 和 Project site 两种方式托管静态文件。

两种方式基本没有什么区别。由于我要部署的是个人博客，因此我选择使用 User or organization site。

顺带说一句，Github Pages 是支持 Jekyll 模版语法。所以可以通过 Jekyll 来“动态”生成网站。

## 创建&配置Repository

首先需要创建一个以 username.github.io 命名的 Repository。**注意替换 `username`为你的用户名**。

创建好 Repository 后按正常的提交代码的流程将站点文件提交到 master 分支。

如果操作正确而且如果你不需要绑定域名的话，现在你就可以通过访问 username.github.io 访问你的网站了。

## 绑定域名&开启 HTTPS

首先在 Repository -> Settings -> GitHub Pages -> Custom domain 中填写需要绑定的域名。

下面开始解析域名 CNAME 和 A 记录二选一即可。

###  添加 CNAME

添加一条 CNAME 记录到 username.github.io 即可。

使用一下命令查看效果：

``` bash
$ dig mydomain.com +noall +answer
mydomain.com	3600	IN A	XXX.XXX.XXX.XXX
$ dig username.github.io +noall +answer
username.github.io	3600	IN A	XXX.XXX.XXX.XXX
```

XXX.XXX.XXX.XXX 为服务器 IP，两个命令的 IP 如果一致的话，说明已经生效了。

### 添加 A 记录

在域名的 DNS 设置中添加 4 条 A 记录分别对印到：

- 185.199.108.153
- 185.199.109.153
- 185.199.110.153
- 185.199.111.153

如果生效了的话，使用 `dig`命令应该可以看到 4 条和上面 IP 对印的结果。

最后需要确保 Repository -> Settings -> GitHub Pages -> Custom domain 中的 Enforce HTTPS 是勾选状态。

最后，访问 https://mydomain.com 应该可以看到绿色的小锁了。

# Tips

+ 注意上文中出现的 username 全部需要替换为也登陆使用的用户名。
+ Github Pages 支持一个站点同时绑定顶级域名和 www 子域名，但是据文档所说每个站点只能绑定一个非 www 的子域名。
+ 如果之前域名有解析到其他位置的话，在进行操作之前可以先将域名的 TTL 设置短一点，方便修改解析后快速生效。