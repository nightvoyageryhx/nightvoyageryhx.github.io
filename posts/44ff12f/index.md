# Batch自动ping并写入日志脚本


&lt;!--more--&gt;

今天做睿联的笔试时，考了一个编写自动ping并写入日志的脚本的题，我才发现我对bat的传参等用法不太会，于是学了一下怎么写。

----------

先上代码

```batch{title=&#34;pinglog.bat&#34;}
@echo off
echo --------------------------------------
echo ping自动写入log
echo.
echo written by nightvoyager
echo.
echo 2024年9月27日20:01
echo --------------------------------------
echo 按CTRL&#43;C以中止执行
set /p host=请输入需要ping的对象~

set logfile=SUCC_%host%.txt
set failfile=FAIL_%host%.txt

@echo 目标地址为%host%
ping %host% -n 1
echo %ERRORLEVEL%

if %ERRORLEVEL%==0 GOTO SUCC
if %ERRORLEVEL%==1 GOTO FAIL

:SUCC
echo Target Host=%host% &gt;&gt;%logfile%
for /f &#34;tokens=*&#34; %%A in (&#39;ping %host% -n 1 &#39;) do (echo %%A &amp;&amp; echo %%A&gt;&gt;%logfile% &amp;&amp; GOTO Ping)

:Ping
for /f &#34;tokens=* skip=2&#34; %%A in (&#39;ping %host% -n 1 &#39;) do (
    echo %date% %time:~0,2%:%time:~3,2%:%time:~6,2% %%A&gt;&gt;%logfile%
    echo %date% %time:~0,2%:%time:~3,2%:%time:~6,2% %%A
    timeout 1 &gt;nul
    GOTO Ping)

:FAIL
echo Target Host=%host% &gt;&gt;%failfile%
for /f &#34;tokens=*&#34; %%B in (&#39;ping %host% -n 1 &#39;) do (echo %%B &amp;&amp; echo %%B&gt;&gt;%failfile% &amp;&amp; GOTO Faping)

:Faping
for /f &#34;tokens=* skip=2&#34; %%B in (&#39;ping %host% -n 1 &#39;) do (
    echo %date% %time:~0,2%:%time:~3,2%:%time:~6,2% %%B&gt;&gt;%failfile%
    echo %date% %time:~0,2%:%time:~3,2%:%time:~6,2% %%B
    timeout 1 &gt;nul
    GOTO Faping)

@cmd /k
```

---------------

在window系统的batch中，设置变量使用``set`` ,``\p``表示让用户输入值，`=`后的值是输入提示。

-----------------

使用``%变量名称%``来使用变量的值

--------------

``%ERRORLEVEL%``表示上一个命令的状态，判断上一个命令是否执行成果，成功则为   0 ，失败则为 1 。在这个脚本中，当ping通的时候，其值为1，否则为0。这个变量不需要提前声明，可以直接使用。依此，我们可以分别设定``ping``成功与失败时的操作。

----------------

``echo Target Host=%host% &gt;&gt;%failfile%``

这一句用来将信息写入日志。其中``%failfle%``在前面已经声明了。如果工作目录下没有该文件，则batch会自动创建文件。**注意前面声明的时候要加上文件类型的后缀。**

-------------

``GOTO``就是跳转到声明为``:名称``的地方并执行后续的命令。

-------------

对于``for /f &#34;tokens=*&#34; %%B in (&#39;ping %host% -n 1 &#39;) do (echo %%B &amp;&amp; echo %%B&gt;&gt;%failfile% &amp;&amp; GOTO Faping)``

看起来是一个很复杂的命令，把它分为几个部分

基本格式： ``for %%i in (command1) do command2``

类似于C语言的for命令，可用作循环语句。

``%%i in (command1)``中，`i`可以是任意字母，区分大小写，表示`command1`的结果，也有点像linux的管道符`|`。

`do command2`表示执行后续的命令，可以几个命令连用，就像上面的代码，同时执行`ping`和`echo`，并且可以使用`%%i`

`&#34;token=*&#34;`是可选项，表示忽略开头的空格，它的值还可以是数字。具体用法请上网搜索。

--------

``&#34;skip=n&#34;``也是可选项，其中``n``为整数。表示``%%i``的值为``command1``的结果的第n行。

------

``%time:~0,2%``表示取时间从左往右的0-2位。

使用``echo %time%``命令可以查看时间的默认格式，是hh:mm:ss.ss


---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/44ff12f/  

