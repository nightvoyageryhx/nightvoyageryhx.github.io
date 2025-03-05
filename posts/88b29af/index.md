# 毕设《基于深度学习的音乐源分离》


&lt;!--more--&gt;

记录一下毕设的内容

搜了一下主要有两种分离方式，一种是用频谱图的，即用stft（短时傅里叶变化）对音频进行处理。这样特征图同时包含时域和频域的信息。另一种是直接基于波形在时域进行处理分离的，

评价指标：signal to distortion ratio(SDR)
$$
\mathrm{SDR}:=10\log_{10}\frac{\|s_{\mathrm{target}}\|^2}{\|e_{\mathrm{interf}}&#43;e_{\mathrm{artif}}\|^2}
$$

[facebookresearch/demucs: Code for the paper Hybrid Spectrogram and Waveform Source Separation](https://github.com/facebookresearch/demucs?tab=readme-ov-file)给出了各模型的性能，如下

| Model                                                        | Domain      | Extra data?       | Overall SDR | MOS Quality | MOS Contamination |
| ------------------------------------------------------------ | ----------- | ----------------- | ----------- | ----------- | ----------------- |
| [Wave-U-Net](https://github.com/f90/Wave-U-Net)              | waveform    | no                | 3.2         | -           | -                 |
| [Open-Unmix](https://github.com/sigsep/open-unmix-pytorch)   | spectrogram | no                | 5.3         | -           | -                 |
| [D3Net](https://arxiv.org/abs/2010.01733)                    | spectrogram | no                | 6.0         | -           | -                 |
| [Conv-Tasnet](https://github.com/facebookresearch/demucs/tree/v2) | waveform    | no                | 5.7         | -           |                   |
| [Demucs (v2)](https://github.com/facebookresearch/demucs/tree/v2) | waveform    | no                | 6.3         | 2.37        | 2.36              |
| [ResUNetDecouple&#43;](https://arxiv.org/abs/2109.05418)         | spectrogram | no                | 6.7         | -           | -                 |
| [KUIELAB-MDX-Net](https://github.com/kuielab/mdx-net-submission) | hybrid      | no                | 7.5         | **2.86**    | 2.55              |
| [Band-Spit RNN](https://arxiv.org/abs/2209.15174)            | spectrogram | no                | **8.2**     | -           | -                 |
| **Hybrid Demucs (v3)**                                       | hybrid      | no                | 7.7         | **2.83**    | **3.04**          |
| [MMDenseLSTM](https://arxiv.org/abs/1805.02410)              | spectrogram | 804 songs         | 6.0         | -           | -                 |
| [D3Net](https://arxiv.org/abs/2010.01733)                    | spectrogram | 1.5k songs        | 6.7         | -           | -                 |
| [Spleeter](https://github.com/deezer/spleeter)               | spectrogram | 25k songs         | 5.9         | -           | -                 |
| [Band-Spit RNN](https://arxiv.org/abs/2209.15174)            | spectrogram | 1.7k (mixes only) | **9.0**     | -           | -                 |
| **HT Demucs f.t. (v4)**                                      | hybrid      | 800 songs         | **9.0**     | -           | -                 |

先复现一下demucs的，已经是第四个版本了

----------

参考b站一位北交姐姐的视频和github上的文档安装了一下
{{&lt; bilibili BV1dz4y1X7ka 1 &gt;}}

先把github上的项目clone一下，然后去到项目的根目录，打开anaconda配一下环境。我这里装有gpu的版本。

```
conda env update -f environment-cuda.yml
```

因为之前搞cv大作业用了一下anaconda，所以有一些换镜像之类的操作就没有再次赘述，anaconda的使用就上网搜或者看我之前的[笔记](https://www.lostune.top/2025/01/11/anaconda、tensorflow和pytorch的配置和使用问题/)



---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/88b29af/  

