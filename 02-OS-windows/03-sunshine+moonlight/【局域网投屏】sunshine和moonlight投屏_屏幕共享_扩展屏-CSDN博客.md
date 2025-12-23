 

主机是sunshine，客机是moonlight，一个太阳一个月光，两者真是太配啦！

**下载sunshine**  
sunshine是服务器端，去以下GitHub链接下载windows端的解压缩即用版

```
https://github.com/LizardByte/Sunshine/releases
```

![在这里插入图片描述](assets/c1ab5dc9b46942a88bda759cee5b589b.png)  
下载完毕解压，点击exe文件启动sunshine  
![在这里插入图片描述](assets/304a8ada1e594540b3951c4e8ab6cc82.png)  
启动后可以按住ctrl+鼠标左键点击这个链接从浏览器打开界面，也可以在浏览器中输入localhost:47990  
![在这里插入图片描述](assets/8521ded0210e4b928fd2a2fb446f8837.png)  
然后访问到以下sunshine界面  
ps:如果浏览器提示链接不安全，点高级，然后继续访问  
![在这里插入图片描述](assets/1d0e82f993c640978abd0365181f5b41.png)  
在以上界面password中输入密码和confirm password中输入确认密码，比如都填123，然后等待页面自动跳转到以下页面  
![在这里插入图片描述](assets/71a0ddcb49664ccb8235ef452b72303b.png)  
**客户机下载moonlight**  
在以下GitHub链接下载，这里第二台电脑也是Windows，就下载Windows版的了

```
https://github.com/moonlight-stream/moonlight-qt/releases
```

![在这里插入图片描述](assets/3104ec19e5eb434c9eede07eb2555219.png)  
下载完毕解压，启动此exe文件启动moonlight  
![在这里插入图片描述](assets/92c79955a44e4eea8a506092e27bd1be.png)  
启动后如图所示，如果没有自动识别到可以连接的主机，在右上角点击添加主机  
![在这里插入图片描述](assets/5a3db6ef63df454fadb3c99fb03caf5f.png)  
在sunshine启动的主机终端中输入ipconfg查询当前ip地址  
![在这里插入图片描述](assets/2beb36f9b41f4ceeb517dc25e561ca3c.png)  
在客户机moonlight中填入此ip地址点击确定  
![在这里插入图片描述](assets/3df400f7a5864bc290a350ca51479e71.png)  
在moonlight中添加到sunshine主机  
![在这里插入图片描述](assets/bdf0fdffd1bc455d882d40ed62bd69f5.png)  
弹出配对码  
![在这里插入图片描述](assets/03ec1f92c244458398a691cbe1762aae.png)  
在sunshine主机浏览器界面左上角点击pin打开配对界面  
![在这里插入图片描述](assets/4c1a1c3678c5473fa46783143c7fa789.png)  
输入此配对码，下面的device name可以随意填，比如123，点击send  
![在这里插入图片描述](assets/acfb12066bc44bc0b43a2d5c20594efa.png)  
显示配对成功  
![在这里插入图片描述](assets/b4882b7706aa439ea25cde0754136171.png)  
在客户机moonlight中重新点击连接的主机  
![在这里插入图片描述](assets/1dc73686cfa347e3a7b4bdf8672a3622.png)  
再点击  
![在这里插入图片描述](assets/cbe5c92e6a9d4ad587bda9a4fe936109.png)  
然后自动连接，实现以下局域网投屏的效果  
![在这里插入图片描述](assets/69adf90f84ef4f159c9b9a8cc59c21d6.png)  
**其他项**  
调节投屏的清晰度点击这里进行调节测试  
![在这里插入图片描述](assets/96e2fffaf9714fe09021d799281a7cd4.png)

**加入扩展屏**  
在以下github官网链接，下载ParsecVDisplay

```
https://github.com/nomi-san/parsec-vdd/releases
```

![在这里插入图片描述](assets/beececd5961d4dc29d2705cfc5dffe3e.png)  
下载完毕安装完成，点击这里设置虚拟屏幕分辨率和帧率  
![在这里插入图片描述](assets/6616921154e34e2ebfb5caf79df2ff3f.png)  
这里设置一块2.5k/75hz的屏幕，点击apply应用配置  
![在这里插入图片描述](assets/2bd5cd51f8804ce2a68a3589651e2cf2.png)  
点击add 增加这块屏幕  
![在这里插入图片描述](assets/978630dd747848cbb11f0a068b56a114.png)  
在屏幕上右键，查看到如下信息  
![在这里插入图片描述](assets/eb7df04e2ead40a8bfd47a7cffc79061.png)  
在sunshine的配置界面中填入上述信息，然后save保存，重启sunshine  
![在这里插入图片描述](assets/ed0d2b91b12546e9b61b3f21037c7cc1.png)  
sunshine默认用的是集显，在某些2K或者4K视频流推送场景下会带不起来，以下为更改独显工作流程。  
以下为官方教程链接设置gpu工作

```
https://support.parsec.app/hc/en-us/articles/4423615425293-VDD-Advanced-Configuration#parent_gpu
```

