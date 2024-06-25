# 主题使用记录


<!--more-->

## 拓展 shortcode

### 音乐插入

自定义音乐平台

有以下命名参数来使用自定义音乐平台： 

- **server** _[必需]_（**第一个**位置参数） [`netease`, `tencent`, `kugou`, `xiami`, `baidu`] 

  音乐平台。 

- **type** _[必需]_（**第二个**位置参数） [`song`, `playlist`, `album`, `search`, `artist`] 

  音乐类型。 

- **id** _[必需]_（**第三个**位置参数） 

  歌曲 ID, 或者播放列表 ID, 或者专辑 ID, 或者搜索关键词，或者创作者 ID。 


  一个使用自定义音乐平台的 `music` 示例：

``` go-html-template {title="插入音乐"}
{{</* music server="netease" type="song" id="65334" */>}}
或者 
{{</* music netease song 65334 */>}}
```
呈现的输出效果如下：

{{< music server="netease" type="song" id="65334" >}}


---

> 作者: 夜航星  
> URL: https://nightvoyageryhx.github.io/posts/%E4%B8%BB%E9%A2%98%E4%BD%BF%E7%94%A8%E8%AE%B0%E5%BD%95/  

