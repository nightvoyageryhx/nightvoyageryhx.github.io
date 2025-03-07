# 毕设《基于深度学习的音乐源分离》


&lt;!--more--&gt;

记录一下毕设的内容

搜了一下主要有两种分离方式，一种是用频谱图的，即用stft（短时傅里叶变化）对音频进行处理。这样特征图同时包含时域和频域的信息。另一种是直接基于波形在时域进行处理分离的，

评价指标：信号失真比 signal to distortion ratio(SDR)
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


因为之前搞cv大作业用了一下anaconda，所以有一些换镜像之类的操作就没有再次赘述，anaconda的使用就上网搜或者看我之前的[笔记](https://www.lostune.top/2025/01/11/anaconda、tensorflow和pytorch的配置和使用问题/)

conda输入以下命令，**注意有无gpu用的不一样，不要直接复制**。

```
conda env update -f environment-cpu.yml  # if you don&#39;t have GPUs
conda env update -f environment-cuda.yml # if you have GPUs
conda activate demucs
pip install -e .
```

装好以后就可以用了，用法在demucs的页面有，和上面的视频不太一样。具体如下：

```
demucs PATH_TO_AUDIO_FILE_1 [PATH_TO_AUDIO_FILE_2 ...]   # for Demucs
# If you used `pip install --user` you might need to replace demucs with python3 -m demucs
python3 -m demucs --mp3 --mp3-bitrate BITRATE PATH_TO_AUDIO_FILE_1  # output files saved as MP3
        # use --mp3-preset to change encoder preset, 2 for best quality, 7 for fastest
# If your filename contain spaces don&#39;t forget to quote it !!!
demucs &#34;my music/my favorite track.mp3&#34;
# You can select different models with `-n` mdx_q is the quantized model, smaller but maybe a bit less accurate.
demucs -n mdx_q myfile.mp3
# If you only want to separate vocals out of an audio, use `--two-stems=vocals` (You can also set to drums or bass)
demucs --two-stems=vocals myfile.mp3
```

详细的就不说了。我让deepseek总结了一下，需要注意的点是gpu加速且显存较小（小于7g）的时候命令行要添加一个segment，我的只有4g当然要加。

demucs的文档说

&gt; **GPU 加速的内存需求**
&gt; 如果你想使用 GPU 加速，至少需要 3GB 的 GPU 内存。但如果使用默认参数，大约需要 7GB 内存。使用 `--segment SEGMENT` 来更改每个分段的大小。如果只有 3GB 内存，将 `SEGMENT` 设置为 8（但如果该参数太小，质量可能会下降）。设置环境变量 `PYTORCH_NO_CUDA_MEMORY_CACHING=1` 可以帮助内存更小的用户（如 2GB），但这会使分离速度变慢。
&gt;
&gt; 如果 GPU 内存不足，只需在命令行中添加 `-d cpu` 以使用 CPU。使用 Demucs 时，处理时间大约为音轨时长的 1.5 倍。

但是 Hybrid Transformer 模型（如 `htdemucs`）的分段长度不能超过 **7.8 秒**，即使设置为 `8`，实际会被自动截断。

我一开始设置为8，就报错了。

实际我是这样用的：

```
demucs C:\桌面\temp\whnd.mp3 --segment 7
```

然后就报错了，有提示是

```
A module that was compiled using NumPy 1.x cannot be run in
NumPy 2.0.2 as it may crash. To support both 1.x and 2.x
versions of NumPy, modules must be compiled with NumPy 2.0.
Some module may need to rebuild instead e.g. with &#39;pybind11&gt;=2.12&#39;.

If you are a user of the module, the easiest solution will be to
downgrade to &#39;numpy&lt;2&#39; or try to upgrade the affected module.
We expect that some modules will need time to support NumPy 2.
...
...
RuntimeError: Numpy is not available
```

就是我的numpy版本太高了，我的是`2.0.2`，应该用`1.x`的才适用。之前做cv大作业也遇到过。可以输`conda list`看看自己的numpy版本

那么就重装一下numpy，如果你没有这个问题就不用搞了。

输入`conda search numpy --info`搜一下可安装版本。我之前cv用`1.24.3`可以用，就继续用了，实际上版本应该还是有对应关系的。

输入这个改一下

```
conda install numpy==1.24.3
```

然后版本应该就换好了，然后再执行

```
demucs C:\桌面\temp\whnd.mp3 --segment 7
```

就可以了，出来读条，正常运行啦~

![](https://s21.ax1x.com/2025/03/06/pEY0Mvt.png)

总时间大概是音频时长的1.5倍。和视频里cpu训练的速度接近，于是我怀疑我没有用上gpu加速。我又运行了一次，然后再cmd里看gpu的状态，显存占用基本是0，于是就可以确定是没有用上gpu了。

----------

然后我就执行了cuda训练的命令

```
demucs -d cuda C:\Users\zsw\OneDrive\桌面\temp\passby.mp3 --segment 7
```

果然报错了。

```
......
......
AssertionError: Torch not compiled with CUDA enabled
```

这个错误一般是torch用不了cuda，一般是版本问题，可以验证一下。键入以下命令

```
python
import torch
torch.__version__
torch.cuda.is_available()
```

返还了一个false，但是torch是可以import的，那么基本就确定是cudatoolkit和torch版本不对应的问题了，以前用tensorflow也会这样。

![](https://s21.ax1x.com/2025/03/06/pEY02G9.png)

去pytorch官网对照一下[Previous PyTorch Versions | PyTorch](https://pytorch.org/get-started/previous-versions/)，注意demucs的文档说了torchaudio版本不能大于2.1

然后就装了一个pytorch-cuda

```
&gt;conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.8 -c pytorch -c nvidia
```

装好之后，再验证就是true了

![](https://s21.ax1x.com/2025/03/06/pEYsb0P.png)

重新执行```demucs -d cuda C:\Users\zsw\OneDrive\桌面\temp\passby.mp3 --segment 7```，成功运行。

![](https://s21.ax1x.com/2025/03/06/pEYsOk8.png)

速度明显快了很多，是cpu的十几倍了。

键入``explorer separated``可以跳转到分离完成的音轨。

听了一下出来的结果，效果还是不错的，人声分离比较干净，鼓的效果也挺好，不过也视具体的音频，不同的音频出来效果不一样，有的分的就会有干扰不干净。一般人声清亮的音乐分的效果会好。

----

既然复现完毕下一步就是训练我的模型了。


---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/88b29af/  

