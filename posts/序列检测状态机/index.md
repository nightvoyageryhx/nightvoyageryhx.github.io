# 序列检测状态机


&lt;!--more--&gt;

## **序列检测器**

序列检测器是一种数字电路，用于检测输入序列中是否出现预定义的特定序列。三段式序列检测器是一种常用的实现方式，它将检测过程分为三个阶段：复位状态、中间状态和检测状态。

1. 复位状态:

这是电路的初始状态，表示还没有检测到任何目标序列的一部分。

在此状态下，电路会忽略所有输入，直到接收到目标序列的第一个比特。

通常使用一个复位信号来初始化电路到复位状态。

2. 中间状态:

当电路接收到目标序列中的部分比特时，它会进入一个或多个中间状态。

中间状态的数量取决于目标序列的长度和复杂性。

每个中间状态都表示已经检测到目标序列的一部分，并等待接收后续比特。

电路根据当前状态和输入比特，跳转到下一个状态或返回复位状态。

3. 检测状态:

当电路接收到完整的目标序列时，它会进入检测状态。

在此状态下，电路会输出一个信号，表示已经成功检测到目标序列。

然后，电路可以根据设计选择返回复位状态或继续检测下一个目标序列。

三段式序列检测器可以使用多种逻辑电路来实现，如状态机:

使用状态转移图和状态表来描述电路的行为，并使用触发器和组合逻辑电路来实现状态转换和输出逻辑

4. 状态转移列表

| 输入 | 当前状态 | 下一个状态 | 输出 |
| :--: | :------: | :--------: | :--: |
|  0   |   IDLE   |    IDLE    |  0   |
|  1   |   IDLE   |     A      |  0   |
|  0   |    A     |     B      |  0   |
|  1   |    A     |     A      |  0   |
|  0   |    B     |     C      |  0   |
|  1   |    B     |     A      |  0   |
|  0   |    C     |    IDLE    |  0   |
|  1   |    C     |     D      |  0   |
|  0   |    D     |     B      |  0   |
|  1   |    D     |     E      |  1   |
|  0   |    E     |     B      |  0   |
|  1   |    E     |     A      |  0   |
|      |          |            |      |

5. 状态转移图

```mermaid
stateDiagram
IDLE--&gt;IDLE:0/0
IDLE--&gt;A:1/0
A--&gt;B:0/0
A--&gt;A:1/0
B--&gt;A:1/0
B--&gt;C:0/0
C--&gt;IDLE:0/0
C--&gt;D:1/0
D--&gt;B:0/0
D--&gt;E:1/1
E--&gt;B:0/0
E--&gt;A:1/0



```

6. verilog代码

   ztj.v

```verilog {title=&#34;ztj.v&#34;}
module ztj(D_out,D_in,rst_n,clk);
// 定义状态编码，每个状态用5位二进制表示
parameter IDLE=5&#39;b00000;//空闲状态
parameter A=5&#39;b00001;//A
parameter B=5&#39;b00010;//B
parameter C=5&#39;b00100;//C
parameter D=5&#39;b01000;//D
parameter E=5&#39;b10000;//E

// 定义输入输出端口
input D_in; // 数据输入
input clk;//时钟
input rst_n;//复位
output wire D_out;//检测输出

reg [4:0]  current_state,next_state;//设置当前状态和下个状态变量

assign D_out=(current_state==E)?1:0;//判断当前状态是否状态E，是则置输出为1，否则0

//状态转换
always @(posedge clk or negedge rst_n) 

begin
    if (!rst_n) 
        current_state &lt;= IDLE;
    else 
        current_state &lt;= next_state;
end

//根据当前状态和输入判断将要跳转的状态
always @(current_state or D_in)
    begin
        case (current_state)//通过状态转移图编写
           IDLE: 
                if(D_in==0)
                    next_state&lt;=IDLE;
                else
                    next_state&lt;=A;//1
            A:
                if(D_in==0)
                    next_state&lt;=B;//0
                else
                    next_state&lt;=A;
            B:
                if(D_in==0)
                    next_state&lt;=C;//0
                else
                    next_state&lt;=A;  
            C:
                if(D_in==0)
                    next_state&lt;=IDLE;
                else
                    next_state&lt;=D;//1
            D:
                if(D_in==0)
                    next_state&lt;=B;
                else
                    next_state&lt;=E;//1；检测到10011
            E:   
                if(D_in==0)
                    next_state&lt;=B;
                else
                    next_state&lt;=A;
            default: next_state&lt;=IDLE;//缺省
        endcase
    end

endmodule
```

7. testbench

   ztj_tb.v

```verilog {title=&#34;ztj_tb.v&#34;}
   `timescale 10ns / 1ns
   
   module ztj_tb;
   
   // 参数定义
   reg D_in;  //序列输入          
   wireztj.v D_out;  //状态检测输出    
   reg rst_n;//复位
   reg clk;//时钟
   integer k,d1;//循环变量与随机数值
   
   ztj ztj(
       .D_in(D_in),
       .D_out(D_out),
       .rst_n(rst_n),
       .clk(clk)
   );
   
   initial begin
       clk = 0;
       forever #5 clk = ~clk;//时钟配置T=10ns
   end
   
   initial begin//设置输入序列值
       #20;
       rst_n=1&#39;b0;
        #15;
        rst_n=1&#39;b1;//复位键
        #10;
        for(k=0;k&lt;=1000;k=k&#43;1)//循环多次
        begin
        d1={$random}%2;//每一个周期0 1变
        D_in&lt;=d1;
        #10;
        end
        
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b1;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b1;
        #10;
        D_in=1&#39;b1;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b1;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b1;
        #10;
        D_in=1&#39;b1;
        #10;
        D_in=1&#39;b0;
        #10;
        D_in=1&#39;b0;
        #10;
               // 结束仿真。
   end
   
   endmodule
   
```

   


---

> 作者: 夜航星  
> URL: http://localhost:7017/posts/%E5%BA%8F%E5%88%97%E6%A3%80%E6%B5%8B%E7%8A%B6%E6%80%81%E6%9C%BA/  

