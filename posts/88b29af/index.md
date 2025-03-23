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

# 复现

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
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.8 -c pytorch -c nvidia
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

# 训练

## librosa

[Librosa](https://github.com/librosa/librosa)是一个Python库，专门用于音频和音乐信号分析。它提供了一系列功能，包括音频特征提取、音频可视化、节奏分析、音频处理等等。Librosa库是开源的，广泛用于音乐信息检索、音频信号处理、机器学习等领域。

## 构建数据集

我打算用demucs来训练，数据集格式和musDB类似，结构如下

```
my_dataset/
  ├── train/               # 训练集
  │   ├── song1/           # 每首歌曲一个文件夹
  │   │   ├── mixture.wav  # 混合音频
  │   │   ├── vocals.wav   # 干声音轨（必须）
  │   │   ├── drums.wav    # 其他可选音轨
  │   │   └── ...
  ├── valid/               # 验证集（结构同train）
  └── test/                # 测试集（结构同train）
```

复制了一个demucs的环境，以免后续修改把能用的搞坏了

```
conda create -n demucs_train --clone demucs
```

让deepseek帮我写了一个构建数据集的python脚本和重命名文件的bat脚本

```python {title=&#34;generate_mixes.py&#34;}
# generate_mixes.py
from pathlib import Path
import librosa
import soundfile as sf
import numpy as np
import random


def generate_mixes(input_dir, output_dir, num_mixes=1000):
    input_dir = Path(input_dir)
    output_dir = Path(output_dir)
    output_dir.mkdir(exist_ok=True)

    instruments = [&#34;dizi&#34;, &#34;yangqin&#34;, &#34;pipa&#34;, &#34;other&#34;]

    for i in range(num_mixes):
        mix_id = f&#34;mix_{i:04d}&#34;
        (output_dir / mix_id).mkdir(exist_ok=True)
        print(&#34;generate mix_%d&#34; % i)

        mix = None
        # 为每个音轨选择随机片段
        for instr in instruments:
            # 随机选择一个干声文件
            files = list((input_dir / instr).glob(&#34;*.wav&#34;))
            if not files:
                continue
            file = random.choice(files)

            # 加载并标准化
            audio, sr = librosa.load(file, sr=44100)
            audio /= np.max(np.abs(audio))  # 归一化

            # 随机增益
            gain = 10 ** (random.uniform(-6, 3) / 20)
            audio *= gain

            # 保存单独音轨
            sf.write(output_dir / mix_id / f&#34;{instr}.wav&#34;, audio, sr)

            # 混合到总音轨
            if mix is None:
                mix = audio.copy()
            else:
                if len(audio) &gt; len(mix):
                    mix = np.pad(mix, (0, len(audio) - len(mix)))
                mix[:len(audio)] &#43;= audio

        # 标准化总音轨
        peak = np.max(np.abs(mix))
        if peak &gt; 1:
            mix /= peak
            # 重新保存各音轨
            for instr in instruments:
                track_file = output_dir / mix_id / f&#34;{instr}.wav&#34;
                if track_file.exists():
                    track_audio, _ = librosa.load(track_file, sr=44100)
                    track_audio /= peak
                    sf.write(track_file, track_audio, 44100)

        # 保存混合音频
        sf.write(output_dir / mix_id / &#34;mixture.wav&#34;, mix, 44100)


if __name__ == &#34;__main__&#34;:
    generate_mixes(
        input_dir=&#34;path/to/your/instruments&#34;, # 这里放音轨的路径
        output_dir=&#34;custom_dataset/musdb_train&#34;, #在python项目的根目录创建一个这个路径的文件夹，不然会报错
        num_mixes=5000
    )
```

```bash {title=&#34;rename.bat&#34;}
@echo off
setlocal enabledelayedexpansion

for /l %%i in (1,1,999) do (
    if exist &#34;%%i.wav&#34; (
        set &#34;num=00%%i&#34;
        ren &#34;%%i.wav&#34; &#34;!num:~-3!.wav&#34;
    )
)
endlocal
```

python脚本要用librosa所以在conda里面装一个

````bash
pip install librosa
````

然后就可以执行脚本里，注意解释器要用克隆后的环境。

## demucs训练准备

前面准备了数据集，后面就在demucs训练

训练遇到了点问题，demucs这个训练文档比较模糊，还有里面提到的文件在源码里也没有。

# 论文笔记

卷积神经网络[41]中的特征块包含了三个维度信息：宽度、高度以及通道。其结构主要由输入层、卷积层（convolution layer）、池化层（pooling layer）、 全连接层（full-connection layer）、激活层（activation layer）和输出层等结构堆叠构成

卷积层和池化层实现了对输入特征的提取功能，网络中需要学习的权重值和偏置值主要来自卷积层，而权重值和偏置值的数量反应了卷积神经网络的计算量。

注意力机制是目前深度学习领域中非常重要的技术，它所关注的问题其实就是处理输入特征信息数据的权重分配问题

目前用于评估分离后的音乐源信号质量指标主要是使用盲源分离评估工具包 [40]（Blind Source Separation Evaluation toolbox, BSS_EVAL toolbox）来进行分析

工具包官方使用了四个量化指标来计算分离后的音源信号的全局性能，分别是信源失真比（Source to Distortion Ratio, SDR），信源干扰比（Source to Interferences Ratio, SIR），信源噪声比（Sources to Noise Ratio, SNR）和信源伪影比（Sources to Artifacts Ratio, SAR）。这几个指标的值越高，则说明被评估的信号有着越好的鲁棒性与低噪声性能，算法的分离效果越好，它们的具体计算公式定义如下
$$
SDR=10log_{10}\frac{\left\|s_{target}(t)\right\|^2}{\left\|e_{interf}(t)&#43;e_{noise}(t)&#43;e_{artif}(t)\right\|^2}
$$
$$
SIR=10log_{10}\frac{\left\|s_{target}(t)\right\|^{2}}{\left\|e_{artif}(t)\right\|^{2}} \\
$$
$$
SNR=10log_{10}\frac{\left\|s_{target}(t)&#43;e_{interf}(t)\right\|^2}{\|e_{noise}(t)\|^2} \\
$$
$$
SAR=10log_{10}\frac{\left\|s_{target}(t)&#43;e_{interf}(t)&#43;e_{noise}(t)\right\|^{2}}{\left\|e_{artif}\right\|^{2}}
$$

--------



进去先换一个路径

```bash
cd ~/open-unmix-pytorch/scripts
conda activate umx1 #umx1是我的环境名
```

记得要激活环境

```bash
conda activate umx1 #umx1是我的环境名
```

用open-unmix训练，这是训练的命令，各参数具体含义看文档

```bash
python train.py --output &#34;models/test_model1&#34; --batch-size 16 --seq-dur 2 --epochs 1 --lr 0.001 --nb-workers 8
```

开始训练后会有提示，如下

```
(umx1) root@DESKTOP:~/open-unmix-pytorch/scripts# python train.py --output &#34;~/open-unmix-pytorch/test_model2&#34; --batch-size 16 --seq-dur 2 --epochs 5 --lr 0.001 --nb-workers 4                                                          Using GPU: True                                                                                                         Compute dataset statistics: 100%|███████████████████████████████████████████████████████|  80/80 [00:24&lt;00:00,  3.26it/s]
Training epoch:   0%|                                                                             | 0/5 [00:00&lt;?, ?it/s]
Training batch:   4%|█▉                                                   | 12/320 [00:34&lt;08:32,  1.66s/it, loss=25.842]
```

![](https://dlink.host/wx2.sinaimg.cn/large/0075RS9aly8hzf93llejlj30x8069jt6.jpg)

![](https://dlink.host/wx2.sinaimg.cn/large/0075RS9aly8hzf9hdjhupj30xd06qac0.jpg)

一个epoch的损失是4.32，基本用不了，先试试能不能分离

```
umx &#34;/mnt/c/Users/zsw/OneDrive/桌面/temp/雨天.flac&#34;     --model &#34;test_model1&#34;  --targets vocals --residual residual 
```

wsl不能分离，但是导出到win10就可以，也不知道为什么，效果比较差，因为loss还很高。

open-unmix是每个分离轨道单独训练的，再使用`seperator.json`分别分离，所以每个轨道都需要单独训练。

再训练一个bass的。

```
python train.py --target bass --output &#34;models/test_bass&#34; --batch-size 16 --seq-dur 2 --epochs 2 --lr 0.001 --nb-workers 8 
```

![](https://dlink.host/wx1.sinaimg.cn/large/0075RS9aly8hzfbzeykx8j30x904zjt9.jpg)

跑了两个epoch，效果比人声好一点，但是还是很差，不过可以听出来对bass有强化的表现，对其他音轨有衰减。

![image-20250313154943573](../../static/imgs/image-20250313154943573.png)

![](https://dlink.host/wx1.sinaimg.cn/large/0075RS9aly8hzfe65jhpcj30xh0b077e.jpg)

![](https://dlink.host/wx3.sinaimg.cn/large/0075RS9aly8hzfegxs169j30xa06zgon.jpg)

每次epoch还是验证损失还是有下降的，就是跟训练损失差别比较大，可能有过拟合风险

又15个epoch后

![](https://dlink.host/wx4.sinaimg.cn/large/0075RS9aly8hzfgvx7jcvj30xe0duwkp.jpg)

验证损失下降到1.4

贝斯的分离效果大概30个epoch后居然已经比较好了，可能是因为频率比较低，好提取特征。

然后我就训练drums的提取，我换了一个数据集，因为之前训练bass的是默认的musdb18的7秒片段，我还删了一部分来加快训练速度

这次用musdb18hq的部分数据来训练，试试效果

```
python train.py --root /root/MUSDB18/musdbhq --target drums  --output &#34;models/HQ_bass1&#34; --batch-size 16 --seq-dur 6 --epochs 1 --lr 0.003 --nb-workers 6 --is-wav
```

```
python train.py --root /root/MUSDB18/musdbhq --output &#34;models/HQ_drums1&#34; --batch-size 16 --seq-dur 6 --epochs 20 --lr 0.003 --nb-workers 6 --dataset trackfolder_fix --target-file drums.wav --interferer-files bass.wav vocals.wav other.wav
```

```
umx &#34;C:\Users\zsw\OneDrive\桌面\temp\开始懂了.flac&#34; --model &#34;F:\openunmix\open-unmix-pytorch\models\test_drums&#34;  --targets drums --residual residual
```

![image-20250313214947977](../../static/imgs/image-20250313214947977.png)

继续训练的命令
```
python train.py --target drums --output &#34;models/test_drums&#34; --batch-size 8 --seq-dur 2 --epochs 15 --lr 0.001 --nb-workers 6 --checkpoint &#34;models/test_drums&#34;
```

![image-20250314153727912](../../static/imgs/image-20250314153727912.png)

bass大概45个epoch之后，loss已经下降的很缓慢了，也比较符合umx给出的训练曲线，大概就是类似指数函数的倒数，后面的下降应该会逐渐更缓慢。

```
python train.py --target vocals --output &#34;models/test_vocals&#34; --batch-size 8 --seq-dur 2 --epochs 10 --lr 0.001 --nb-workers 6    
```

```
umx &#34;C:\Users\zsw\OneDrive\桌面\temp\开始懂了.flac&#34; --model &#34;F:\openunmix\open-unmix-pytorch\hq_drums2&#34;  --targets drums --residual residual
```

aligned训练

成功地用我自己的数据集训练出来了

```
python train.py --output &#34;F:\openunmix\open-unmix-pytorch\scripts\models\temp&#34; --root &#34;F:\dataset\test2&#34; --dataset aligned --input-file mixture.wav --output-file dizi.wav --batch-size 1 --epoch 10 --checkpoint &#34;F:\openunmix\open-unmix-pytorch\scripts\models\temp&#34;
```

```
python train.py --output &#34;models\temp&#34; --root &#34;root\MUSDB18\chwav&#34; --dataset aligned --input-file mixture.wav --output-file dizi.wav --batch-size 1 --epoch 1 
```

```
python train.py --output &#34;F:\openunmix\open-unmix-pytorch\scripts\models\temp&#34; --root &#34;F:\dataset\stemtest&#34; --batch-size 4 --epoch 1 --checkpoint &#34;F:\openunmix\open-unmix-pytorch\scripts\models\temp&#34; --target drums
```

```
python train.py --output &#34;F:\openunmix\open-unmix-pytorch\scripts\models\temp&#34; --root &#34;F:\dataset\ch2mus_aug_stem&#34; --batch-size 4 --epoch 1 --checkpoint &#34;F:\openunmix\open-unmix-pytorch\scripts\models\temp&#34; --target drums --nb-workers 4
```

```
umx &#34;C:\Users\zsw\OneDrive\桌面\temp\mixtest1.wav&#34; --model &#34;F:\openunmix\open-unmix-pytorch\scripts\models\pipa_1&#34;  --targets drums --residual residual --outdir &#34;F:\openunmix\open-unmix-pytorch\scripts\umxoutput&#34;
```

```
python train.py --output &#34;./models/yangqin_e03&#34; --root &#34;/root/MUSDB18/ch2mus_augment_wav&#34; --batch-size 1 --epoch 120 --target vocals --nb-workers 6 --input-file mixture.wav --output-file vocals.wav --dataset aligned --checkpoint &#34;./models/yangqin_e03&#34; 
```

```bash
umx &#34;C:\Users\zsw\OneDrive\桌面\temp\mixtest2.wav&#34; --model &#34;F:\openunmix\open-unmix-pytorch\scripts\models\yangqin_e04&#34;  --targets vocals --residual residual
```



```mermaid
graph TD
    A[混合音频输入] --&gt; B[短时傅里叶变换 STFT]
    B --&gt; C[标准化处理]
    C --&gt; D[频率-通道压缩]
    D --&gt; E[双向LSTM处理]
    E --&gt; F[频率-通道扩展]
    F --&gt; G[掩码估计 Sigmoid]
    G --&gt; H[应用掩码相乘]
    H --&gt; I[逆STFT重建]
    I --&gt; J[分离音轨输出]

    subgraph 核心处理模块
        B --&gt;|n_fft=4096&lt;br&gt;n_hop=1024| C
        D --&gt;|1x1卷积&lt;br&gt;512通道| E
        E --&gt;|3层双向LSTM&lt;br&gt;hidden_size=512| F
        G --&gt;|逐元素相乘| H
    end
    
    subgraph 关键技术点
        C --&gt;|全局均值方差标准化&lt;br&gt;通道独立处理| D
        F --&gt;|1x1反卷积&lt;br&gt;恢复原始维度| G
        I --&gt;|保留混合音频相位| J
    end

    style A fill:#f9f,stroke:#333
    style J fill:#f9f,stroke:#333
    style E fill:#bbf,stroke:#333
    style G fill:#fbb,stroke:#333
```



---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/88b29af/  

