参考：[https://blog.csdn.net/weixin_58006135/article/details/142259451](https://blog.csdn.net/weixin_58006135/article/details/142259451)

windows下载

下载地址：[https://www.gyan.dev/ffmpeg/builds/ffmpeg-git-full.7z](https://www.gyan.dev/ffmpeg/builds/ffmpeg-git-full.7z)



官网：[https://ffmpeg.org/](https://ffmpeg.org/)



1、下载后解压+改名字为ffmpeg，然后复制到C:\Program Files下

2、配置环境变量

系统变量

![](assets/1764251043382-5fe5c328-857f-4801-b959-28dfcb735672.png)

3、任意路径打开cmd，输入ffmpeg，有回显表示正常

![](assets/1764251138534-19795647-17ba-4c53-a27e-01dedc10e1ea.png)

4、抓取视频

<font style="color:rgb(77, 77, 77);">输入代码：（input_file是下载的视频的以</font>[<font style="color:rgb(78, 161, 219);">.m3u8</font>](https://blog.csdn.net/2301_78894093/article/details/136238169?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22136238169%22%2C%22source%22%3A%222301_78894093%22%7D)<font style="color:rgb(77, 77, 77);">为后缀的视频）</font>

```bash
ffmpeg -i input_file -c copy /home/user/Downloads/video.mp4
```

举例：

打开cmd输入

```bash
ffmpeg -i "https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8" -c copy video.mp4
```

等待进程结束，此时就会在cmd的目录看到下载的视频

![](assets/1764252417138-0bc611d4-75a4-42a8-8bd1-a76e6dc4549a.png)







附加：

在线m3u8下载工具

[https://tools.thatwind.com/tool/m3u8downloader](https://tools.thatwind.com/tool/m3u8downloader)

