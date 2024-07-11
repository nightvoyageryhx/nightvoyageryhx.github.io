# Batch脚本编写


&lt;!--more--&gt;

## ping脚本的编写

由于之前没写过.bat脚本，先写一个简单的ping试试。注意ping的脚本不能命名为`ping.bat`，会出错。

```batch {title=&#34;pin.bat&#34;}
ping www.baidu.com
ping www.sina.com
```

双击是可以执行的，但是cmd窗口在实行完之后就自动关闭了，想到在push之后往往要查看结果，所以修改让cmd窗口执行完之后保留。

修改为

```batch {title=&#34;pin.bat&#34;}
ping www.baidu.com
ping www.sina.com
cmd /k
```

这样运行后cmd窗口不会关闭，但是会有一行

&gt; ```C:\Users\yhxgo\hugoblog&gt;cmd /k```

也就是对执行```cmd /k```的回显，要想不显示回显，在这一命令前面&#43;@，也就是

```batch {title=&#34;pin.bat&#34;}
ping www.baidu.com
ping www.sina.com
@cmd /k
```

这样就可以正常执行ping并且执行完后不关闭cmd窗口也没有额外的回显。

然后编写push的脚本，很简单，就是把用到的几个命令合一块就可以了。``hpush.bat``文件放在博客的根目录，也就是``public``的上一级目录

```batch {title=&#34;hpush.bat&#34;}
hugo
cd public
git add .
git commit -m update
git push
cd ..
@cmd /k
```

双击运行可以正常push，成功实现脚本push。

那么再写一个本地部署调试的脚本吧~

一开始我写的是

```batch {title=&#34;hsv.bat&#34;}
hugo
hugo server
start http://localhost:1313/
@cmd /k
```

按照平时逐条命令然后打开浏览器的顺序来写，结果发现不能打开浏览器。因为执行```hugo server```后，cmd就停在这个地方了，需要``ctrl&#43;c``停止后才能继续执行下一条打开网页，那么肯定是不行的，因为停止后在访问本地部署的地址就没东西了，所以改一下顺序

```batch {title=&#34;hsv.bat&#34;}
hugo
start http://localhost:1313/
hugo server
@cmd /k
```

然后双击运行就可以自动本地部署然后在浏览器打开本地网页了

---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/6db4cf1/  

