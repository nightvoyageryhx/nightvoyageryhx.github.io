# ESP32C3频率响应LED阵列


&lt;!--more--&gt;

# ESP32c3_FFT_LEDmatrix

{{&lt; bilibili BV1hhh9egEjB &gt;}}

## 条件

需要的元件：Arduino ESP32c3、MAX9814麦克风模块、MAX72198*8LED阵列模块

环境：Windows10家庭版、Arduino IDE v2.0.4

使用的库函数：LedControl v1.0.6、ArduinoFFT v1.6.1

## 原理图

![原理图](https://pic1.zhimg.com/80/v2-6809b4db1b569730b8444572bcfb7ea3_720w.png?source=d16d100b)

**********

**如果使用的是ESP32C3，在编译前请进行以下操作** 

将```C:文档\Arduino\libraries\LedControl\src``` 路径（也就是LEDControl库的安装路径）下的```LedControl.h```文件中的

```
#include &lt;avr/pgmspace.h&gt;
```

注释掉或者删除（大约在第30行），**否则会报错**

如果不知道路径在哪，把鼠标指针放到代码里```#include &lt;LedControl.h&gt;```这一行IDE就会显示出来

*****************

## 连线

参考原理图

| ESP32c3 | MAX9814 | MAX7219 |
| :-----: | :-----: | :-----: |
|  GPIO9  |    \    |   CLK   |
| GPIO10  |    \    |   CS    |
| GPIO12  |    \    |   DIN   |
|  GPIO1  |   OUT   |    \    |
|   5V    |   VCC   |   VCC   |
|   GND   |   GND   |   GND   |



&gt; ESP32c3**不推荐使用GPIO11**，如果要使用需要请先进行解锁 

********

代码([GitHub - nightvoyageryhx/ESP32c3_FFT_LEDmatrix](https://github.com/nightvoyageryhx/ESP32c3_FFT_LEDmatrix))



---

> 作者: 夜航星  
> URL: http://localhost:1313/posts/esp32c3%E9%A2%91%E7%8E%87%E5%93%8D%E5%BA%94led%E9%98%B5%E5%88%97/  

