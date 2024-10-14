# Java简单输入输出


&lt;!--more--&gt;

今晚做实验，用了一点java，是一个简单的输入名字和学号，还有温度，然后回显。

其实感觉下来和c有很多相似的地方。

```java{title=&#34;hello.java&#34;}
package helloworld;
import java.util.Scanner;

public class Helloworld {

	public static void main(String[] args) {
		Scanner sc =new Scanner(System.in);
		System.out.println(&#34;输入姓名和学号和温度:&#34;);
		// TODO Auto-generated method stub
		
		String name = sc.next(); //输入字符串
		
		String id = sc.next(); //sc.next可以以空格为分隔
		
		int temp = sc.nextInt(); //输入整数
		
		System.out.printf(&#34;你好，%s！ 你的学号是：%s；当前温度值为:%d度。%n&#34;,name,id,temp);

		
	}

}

```

直接在IDE可以运行，也可以生成可执行文件，在文件目录下用命令行。

用命令```java -jar hello.jar```来执行。


---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/a3d125f/  

