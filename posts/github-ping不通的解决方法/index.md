# Github Ping不通的解决方法


&lt;!--more--&gt;

今天用 hugo 做博文，在 git push 时遇到了问题

以下的所有代码和报错信息都是在cmd中的

push之后有以下报错，没有成功

```bash
ssh: Could not resolve hostname github.com: Name or service not known
fatal: Could not read from remote repository.
```

  

之前我也出现过这种报错，一般是代理占用了端口导致不成功，然而这次没有使用代理，也失败了，推测也是网络问题。

于是 ping github.com，果然 ping 不通

```bash
C:\Users\hugoblog\public&gt;ping github.com
正在 Ping github.com [20.205.243.166] 具有 32 字节的数据:
请求超时。
请求超时。
请求超时。
请求超时。
```

上百度搜了一下，成功解决~



解决方法是修改` C:\Windows\System32\drivers\etc\hosts`文件，添加github的ip  



每次修改前需要先获取当前 github 的 ip，通过访问[https://sites.ipaddress.com/github.com/](https://sites.ipaddress.com/github.com/)

可以找到这样一句话



&gt; The website holds a global ranking of 47 and is associated with the network IP address 140.82.113.4



通过记事本打开`C:\Windows\System32\drivers\etc\hosts`，在文件最下方添加一行`140.82.113.4 github.com`

*注意上述这个 ip 地址是你从上面的网站查找到的，不是我这里贴出来这个，因为 github 的 IP 地址可能会变*

![修改host文件](/imgs/image-20240122121409451.png)

保存之后，重新 ping github.com

```bash
C:\Users\hugoblog\public&gt;ping github.com

正在 Ping github.com [140.82.113.4] 具有 32 字节的数据:
请求超时。
请求超时。
来自 140.82.113.4 的回复: 字节=32 时间=246ms TTL=50
来自 140.82.113.4 的回复: 字节=32 时间=248ms TTL=50

140.82.113.4 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 2，丢失 = 2 (50% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 246ms，最长 = 248ms，平均 = 247ms
```

成功了，git push 也可以正常运行。


---

> 作者: 夜航星  
> URL: http://localhost:7017/posts/github-ping%E4%B8%8D%E9%80%9A%E7%9A%84%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/  

