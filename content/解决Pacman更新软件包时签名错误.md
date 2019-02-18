``````` json
{
  "create_time": "2019-02-18T15:00:52.219462Z",
  "description": "DESCRIPTION",
  "id": "1f602f9f-37ef-42d6-8742-c7a6a8f1f8e8",
  "meta": null,
  "tags": ["Linux"],
  "target": "POST",
  "title": "Pacman 更新软件包时提示签名错误的解决办法"
}
```````


我使用的发行版是Manjaro，在某次更新过程中强制重启。之后发现无法更新软件包。使用 `pacman -S `安装任何软件都提示如下错误：

```
 error: GPGME error: Invalid crypto engine
 error: ***: missing required signature
```

查找一些资料说可以通过重新安装 gpgme和 keyring解决，但是由于`pacman` 安装软件包时也会校验签名导致无法重新安装。

期间折腾了好久，突然想到 pacman 好像可以通过配置文件设置是否校验软件包，试了一下果然好了。

# 操作步骤

修改 SigLevel ：

``` shell
sudo vim /etc/pacman.conf
```

将 [core] 的 SigLevel 改为 Never并保存，如下：

```
[core]
SigLevel = Never
#SigLevel = PackageRequired
```

移除无效的 key：

``` shell
sudo rm -r /etc/pacman.d/gnupg 
```

重新安装 gnupg和 keyring：

``` shell
sudo pacman -Sy gnupg archlinux-keyring manjaro-keyring
```

初始化 keyring：

``` shell
sudo pacman-key --init 
```

加载签名：

``` shell
sudo pacman-key --populate archlinux manjaro 
```

刷新签名：

``` shell
sudo pacman-key --refresh-keys 
```

清除已下载的包缓存：

``` shell
sudo pacman -Sc
```

最后将 SigLevel 的值改回来就行了。



