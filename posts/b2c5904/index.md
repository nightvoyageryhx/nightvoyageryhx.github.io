# 常用数学公式


&lt;!--more--&gt;

## 常用等价无穷小
{{&lt; raw &gt;}}
$$
\sin x\sim x\\
\tan x\sim x\\ \arcsin x\sim x\\ \arctan x\sim x\\ \ln(1&#43;x)\sim x\\e^{x}-1\sim x\\a^{x}-1\sim x\ln a\\ 1-\cos x\sim\frac{1}{2}x^{2} \\(1&#43;x)^{a}-1\sim ax
$$
{{&lt; /raw &gt;}}

----------

## 泰勒展开
{{&lt; raw &gt;}}
$$
f\left(x\right)=f\left(0\right)&#43;f&#39;\left(0\right)x&#43;\frac{f&#39;&#39;\left(0\right)}{2!}x^{2}&#43;\cdots&#43;\frac{f^{\left(n\right)}\left(0\right)}{n!}x^{n}&#43;o\left(x^{n}\right)
$$
{{&lt; /raw &gt;}}
如
{{&lt; raw &gt;}}
$$
\sin x=\sin0&#43;(\sin x)^{\prime}|_{x=0}\cdot x&#43;\frac{(\sin x)^{\prime\prime}|_{x=0}}{2!}x^{2}&#43;\frac{(\sin x)^{\prime\prime\prime}|_{x=0}}{3!}x^{3}&#43;o(x^{3})
$$
{{&lt; /raw &gt;}}
即
{{&lt; raw &gt;}}
$$
\sin x=x-\frac16x^{3}&#43;o(x^{3})
$$
{{&lt; /raw &gt;}}
再如
{{&lt; raw &gt;}}
$$
\sec x=\sec0&#43;(\sec x)^{\prime}|_{x=0}\cdot x&#43;\frac{(\sec x)^{\prime\prime}|_{x=0}}{2!}x^2&#43;\frac{(\sec x)^{\prime\prime\prime}|_{x=0}}{3!}x^3&#43;o(x^3)
$$
{{&lt; /raw &gt;}}
即
$$
\sec x=1&#43;\frac{x^2}2&#43;o(x^3)
$$

同理可得如下重要函数的泰勒公式。
{{&lt; raw &gt;}}
$$
\sin x=x-\frac{x^{3}}{3!}&#43;o\left(x^{3}\right)\\

\cos x=1-\frac{x^{2}}{2!}&#43;\frac{x^{4}}{4!}&#43;o\left(x^{4}\right)\\

\arcsin x=x&#43;\frac{x^{3}}{3!}&#43;o\left(x^{3}\right)\\

\tan x=x&#43;\frac{x^{3}}{3}&#43;o\left(x^{3}\right)\\
e^{x}=1&#43;x&#43;\frac{x^{2}}{2!}&#43;\frac{x^{3}}{3!}&#43;o\left(x^{3}\right)\\ \quad\left(1&#43;x\right)^{\alpha}=1&#43;\alpha x&#43;\frac{\alpha\left(\alpha-1\right)}{2!}x^{2}&#43;o\left(x^{2}\right)
$$
{{&lt; /raw &gt;}}

----------

## 基本求导公式
{{&lt; raw &gt;}}
$$
(x^{a})^{\prime}=\alpha x^{a-1}\left(\alpha\text{ 为常数 }\right)\\
\quad(a^{x})^{\prime}=a^{x}\ln a(a&gt;0,a\neq1)\\ \quad(e^{x})^{\prime}=e^{x}\\ 
\quad(\log_{a}x)^{\prime}=\frac{1}{x\ln a}\left(a&gt;0,a\neq1\right)\\

\begin{gathered}
\left(\ln\left|x\right|\right)^{\prime}=\frac{1}{x}\\ 
\left(\sin x\right)^{\prime}=\cos x\\ 
\left(\cos x\right)^{\prime}=-\sin x\\ 
\left(\arcsin x\right)^{\prime}=\frac{1}{\sqrt{1-x^{2}}} \\
(\arccos x)^{\prime}=-\frac{1}{\sqrt{1-x^{2}}}\\
\left(\tan x\right)^{\prime}=\sec^{2}x\\ 
\left(\cot x\right)^{\prime}=-\csc^{2}x\\ 
\left(\arctan x\right)^{\prime}=\frac{1}{1&#43;x^{2}} \\
\left(arccotx\right)^{\prime}=-\frac{1}{1&#43;x^{2}}\\ 
\left(\sec x\right)^{\prime}=\sec x\tan x \\ 
\left(\csc x\right)^{\prime}=-\csc x\cot x \\
\left[\ln\left(x&#43;\sqrt{x^{2}&#43;1}\right)\right]^{\prime}=\frac{1}{\sqrt{x^{2}&#43;1}} \\ 
\left[\ln\left(x&#43;\sqrt{x^{2}-1}\right)\right]^{\prime}=\frac{1}{\sqrt{x^{2}-1}} 
\end{gathered}
$$

{{&lt; /raw &gt;}}


---

> 作者: 夜航星  
> URL: http://localhost:7017/posts/b2c5904/  

