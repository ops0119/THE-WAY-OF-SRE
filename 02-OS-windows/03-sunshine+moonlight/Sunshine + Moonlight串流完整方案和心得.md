

目录

背景
--

前两年买了一台PC放在书房来办公和玩游戏，最初还仅限于坐在电脑前对着显示器使用，后来开始琢磨是否有办法把画面投屏到客厅的电视上，能享受更大的画面尺寸。另外随着生活中的种种变故，能长时间坐在书房的机会逐渐减少，因此在家里任意地方，乃至出门的时候也能用各种设备连接PC的需求越发旺盛。普通的远程连接通过各类远程桌面软件就可以办到，但是如果需要玩游戏和看视频，那普通远程桌面的帧率不能满足需求，需要更加低延迟的方案。

经过种种尝试，目前使用的是Sunshine + [Moonlight](https://zhida.zhihu.com/search?content_id=247798974&content_type=Article&match_order=1&q=Moonlight&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2NzM3ODUsInEiOiJNb29ubGlnaHQiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNDc3OTg5NzQsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.dEQFJH2qgN2nU722mOPVUM6PuP7jRn86KHhMNtuUU6w&zhida_source=entity)的串流解决方案，整体比较稳定，期间有了不少心得，在这里记录一下，全是干货，也算是最新的第一手资料，方便后人。

方案优点：

*   低延迟，更加适合看视频和玩游戏
*   最高可以支持4k + 120Hz + HDR的画质
*   服务端支持各种主流PC硬件
*   客户端支持各种设备

方案缺点：

*   因为定位是串流工具，所以缺乏远程桌面软件的各种便捷功能，例如傻瓜式地跨机器复制粘贴文本/文件
*   如果想实现出门的时候通过外网远程连接家里的电脑，对计算机网络知识和动手能力的要求较高，对计算机和电子设备小白并不友好

**注意：**本文记录的主要是整体方案，以及针对实践过程中遇到的各种问题的解决思路，具体实践中由于各人使用环境不尽相同，需要自行排查问题。

### 使用场景

通过各种设备连接家里的主力PC来远程办公或者玩游戏，包括：

*   躺在床上用手机或者平板操作PC
*   不用把电脑搬到电视旁边，就能用电视的大屏幕观看电脑画面（例如玩游戏，看视频网站）
*   外出时使用笔记本电脑连接家里的PC

我的软硬件环境：

PC端：Windows 11, Nvidia显卡

客户端：各种都有，包括Windows笔记本电脑, Macbook, 苹果手机/平板，安卓手机/平板/电视

原理
--

简单来说就是通过安装在电脑上的串流软件服务端将电脑屏幕画面实时发送到安装了串流软件客户端的设备上，本质上属于投屏，但是客户端同时可以通过键盘/鼠标/手柄等设备给服务端发送输入指令。为了能够高效地传输画面，需要借助电脑显卡对传输画面进行实时编码，并由客户端设备的硬件实时解码。

硬件要求：PC服务端需要有独立显卡，集成显卡是否可以用没有实际测试过，估计比较吃力，意义不大。客户端需要有硬件解码的能力，最好可以支持HEVC（也即H265）的硬解（只要不是太旧的设备，都能支持）。

服务端软件：Sunshine，支持各种主流的PC操作系统和显卡（包括Nvidia/AMD/Intel），适用于局域网和外网的串流场合。Nvidia有一个GeForceExperience软件虽然也可以支持串流，但是仅支持自家显卡，而且官方也不再继续开发维护串流功能，因此还是推荐Sunshine。

客户端软件：Moonlight，支持各种主流的PC和移动端操作系统，可以安装在PC/手机/平板/智能电视上面。

安装
--

### Sunshine

首先通过下面的链接在PC上安装Sunshine（打不开github的话自行百度），注意选择latest版本，不要使用pre-release版本，windows电脑的话选择sunshine-windows-installer.exe，推荐勾选开机自动启动。注意Sunshine必须要以windows服务的形式启动，不能直接去安装目录里点击sunshine.exe文件。所以启动Sunshine要么通过开机自动启动，要么点击开始菜单里的Sunshine快捷方式。

[https://github.com/LizardByte/Sunshine/releasesgithub.com/LizardByte/Sunshine/releases](https://link.zhihu.com/?target=https%3A//github.com/LizardByte/Sunshine/releases)

Sunshine的管理界面是网页形式，第一次进入的时候浏览器会警告非私密链接，选择忽略警告，继续访问。随后设置管理员用户名和密码，这个用户名密码是存储在本地的，必须牢记，如果忘了没办法找回，也就没法进入配置页面，但是不影响Sunshine服务的运行。

如果需要调整网页语言，去Configuration下面的General页面，在Locale里选择简体中文，点击Save和Apply，网页会重启。

![](assets/v2-221298097106d28dd619686c8adfd082_1440w.jpg)

这样基本的安装就结束了，其他配置项自行摸索，一般不怎么需要改。

**游戏管理**

Sunshine自带了基本的程序管理功能，在网页的Applications里面可以配置希望远程启动的各个程序的名称和执行路径，配合客户端使用。对于更复杂的游戏管理能力，Sunshine也自带了电脑桌面以及Steam启动器。

总的来说Sunshine自带的程序管理功能比较简陋，而且启动有些游戏会失败。如果需要更成熟的游戏管理平台，这里推荐一款叫Playnite的游戏管理软件，支持多个游戏平台的资料导入，自动下载游戏封面图片，也提供大屏模式，方便投屏到电视上用手柄操作。可以在Sunshine里配置启动Playnite，再从Playnite统一启动游戏。

[![](assets/v2-bd581c2bc6b418b3541525ff2edabb5b_180x120.jpg)Playnite - video game library managerplaynite.link/](https://link.zhihu.com/?target=https%3A//playnite.link/)

### Moonlight

客户端支持各种操作系统和设备，去以下链接下载。

[https://moonlight-stream.org/moonlight-stream.org/](https://link.zhihu.com/?target=https%3A//moonlight-stream.org/)

其中iOS设备直接去苹果App Store搜索安装，Android设备会被指向Google Play商店，国内无法访问，可以从以下链接下载apk安装包。

[Releases · moonlight-stream/moonlight-android · GitHubgithub.com/moonlight-stream/moonlight-android/releases](https://link.zhihu.com/?target=https%3A//github.com/moonlight-stream/moonlight-android/releases)

各个客户端的界面基本一样，如果客户端和Sunshine在同一个局域网内，则会自动搜索到服务端的机器，第一次连接的时候需要在服务端的Sunshine管理页面的PIN页面中输入pin码，与客户端进行配对。之后在客户端里设置串流的分辨率和帧数，就可以开始串流了。如果有其它需求，自行摸索配置，其中安卓客户端的配置选项最丰富。

建议客户端配对的时候与服务端在同一个局域网内，方便操作。尤其是iOS设备受到苹果的安全限制，仅能在局域网内配对。

**虚拟键盘**

对于iOS/Android设备，在串流的时候可以采用三个手指同时点击屏幕的手势，唤起设备自带的虚拟键盘。许多安卓设备的厂商带有自己的手势系统，导致这个虚拟键盘的手势失效，需要用户自行排查解决（例如关闭安卓厂商的手势功能，或者有些设备可以开启游戏模式）。

**常用快捷键**

在电脑上使用Moonlight的时候，有一些常用的快捷键：

*   Ctrl+Alt+Shift+Q - 退出串流，但是串流时启动的程序还在运行，需要在客户端里手动关闭
*   Ctrl+Alt+Shift+X - 客户端切换窗口/全屏模式
*   Ctrl+Alt+Shift+S - 实时显示串流状态，包括网络延迟、解码信息 (不支持Steam Link或者Raspberry Pi)
*   Ctrl+Alt+Shift+D - 最小化客户端窗口

完整文档见下。

[Setup Guide · moonlight-stream/moonlight-docs Wiki · GitHubgithub.com/moonlight-stream/moonlight-docs/wiki/Setup-Guide#keyboardmousegamepad-input-options](https://link.zhihu.com/?target=https%3A//github.com/moonlight-stream/moonlight-docs/wiki/Setup-Guide%23keyboardmousegamepad-input-options)

网络连接
----

网络质量分为网络带宽/平均延迟/网络抖动几个因素，带宽决定了画质的上限，平均延迟决定了用户输入的响应速度，网络抖动决定了音画卡顿出现的几率。

**网络带宽**

各种常用画质所需的网络带宽：

720p 60Hz: 10Mb/s

1080p 60Hz: 20Mb/s

1080p 120Hz: 30Mb/s

2k 60Hz: 40Mb/s

2k 120Hz: 60Mb/s

4k 60Hz: 80Mb/s

4k 120Hz: 115Mb/s

内网环境下网络带宽往往不是问题，主要看客户端设备支持什么画质。如果客户端支持的帧率高于服务端所连接的显示器，可以在游戏里关闭垂直同步，解除帧率上限，或者使用后面介绍的虚拟屏幕。

外网环境下网络带宽取决于服务端的上行带宽，国内普通家庭宽带的上行带宽比较有限，哪怕是千兆宽带，电信/联通的上行带宽往往也只有60Mb/s。相比之下，移动宽带的上行带宽给得大方一些，可能超过100Mb/s。具体上行带宽有多少可以自行测速，往往不会超过40Mb/s。因此外网环境下，更常用的是1080p 60Hz的画质，这个画质对于移动设备来说也足够了。

**网络延迟**

内网环境下网络延迟很低，如果服务端和客户端都是有线连接，那延迟能够稳定低于1ms。使用5GHz wifi的话，信号非常好的情况下延迟大约在5ms左右。

外网环境下网络延迟很容易受各种因素影响，根据经验，同城延迟大约在10ms，跨城延迟取决于地理距离/是否跨省/是否跨网络运营商，延迟从15ms（相距300公里时实测）到上百ms不等。跨省、尤其是跨网络运营商的时候，有可能因为串流流量较高，或者因为处于网络高峰期而被限流。被限流时可能网络平均延迟并不算高，但是丢包严重，导致剧烈抖动乃至无法串流。

因此服务端和客户端**优先选择相同运营商的网络**，网络延迟在几十ms的时候都是可以流畅串流的，更重要的是网络抖动。

**网络抖动**

串流对网络抖动很敏感，隔三岔五的抖动非常影响体验，因此服务端和客户端都**尽量使用有线网络**，这样网络抖动可以显著降低。尤其是服务端，如果网络不稳定，自然会影响所有客户端。

如果实在缺少有线网络的环境，或者客户端是移动设备，也**务必避免**使用2.4GHz的Wifi，因为很容易受到外部环境干扰，导致频繁抖动。尤其是蓝牙设备（耳机/手柄）和2.4GHz的Wifi在一个频段上，会相互干扰，出现不可忍受的网络延迟。

手柄连接
----

客户端设备连接上手柄后可以通过手柄直接操控Moonlight界面来启动或者退出串流，长按Start可以在串流中开启/关闭右摇杆的鼠标模拟功能。退出当前串流的按键组合是Select + Start + LB + RB。

由于客户端可能运行在多种操作系统环境下，因此推荐使用Xbox手柄，各个操作系统都能兼容，或者至少使用带有XInput的第三方手柄。

另外需要关注的是手柄的振动功能。我只测试过Xbox One/Series以及PS5 DualSense手柄，总结如下：

*   Windows对各种手柄的支持最全，可以有线或者无线连接，均支持振动。
*   MacOS支持Xbox/PS手柄的无线连接，有线连接的时候往往只是给手柄供电，并没有实际的数据交互，此时仍然需要通过开启无线连接来正常使用。支持振动。
*   iOS/iPadOS和MacOS类似，需要至少升级到14.5版本。
*   Android设备对手柄的支持力度最弱，一个是因为Android本身对振动的支持并不好（需要Android层面和Linux内核层面的双重支持），另一个是Android设备往往缺少长期支持，大量设备无法升级到较新的版本（这点iOS设备好很多），导致不能识别手柄。**推荐至少使用Android 12**，该版本中引入了基本的手柄振动功能。具体来说：

*   从Android 10开始（可能更早），Xbox手柄可以通过有线/无线连接，但是从Android 12开始才支持有线连接下的振动，无线连接目前不支持振动。预计基于Linux 6.6内核的Android 15新设备可以支持无线连接下的Xbox手柄振动。
*   从Android 12开始支持PS5 DualSense手柄的有线/无线连接，均支持振动。
*   部分安卓厂商可能会自带额外驱动或补丁，具体行为需要自测。
*   如果Android设备的版本太老，有线连接下无法识别Xbox手柄或者没有振动，可以在Moonlight客户端的配置中开启自带的Xbox手柄USB驱动，并且勾选“覆盖安卓手柄支持”，Moonlight客户端自带的Xbox手柄驱动支持有线振动。

需要注意的是，Moonlight仅能提供基本的振动，各个手柄特有的振动功能（例如Xbox手柄的扳机振动，PS5手柄的触觉反馈）是无法实现的。如果需要此类振动，只能将手柄直接连接到服务端电脑，Moonlight客户端仅用来传输画面。

关于PS手柄的建议：windows上的游戏普遍对xbox手柄的支持最好，对于PS系手柄的支持就参差不齐了：有些游戏仅支持xbox手柄，有些游戏虽然支持ps手柄，但是功能不全（比如没有振动）。Sunshine会自动识别手柄属于xbox还是ps系列，导致使用ps手柄的时候部分游戏支持不佳。为了最大的兼容性，可以考虑在Sunshine的Configuration -> Input页面，把模拟的手柄类型从自动改为xbox，这样即使使用ps系列的手柄连接moonlight，也会被游戏认作xbox手柄。

![](assets/v2-7776e0eb9ec41a9aafc3f33333a85c77_1440w.jpg)

电视串流
----

现在的电视基本都自带了安卓，所以也能在电视上安装Moonlight，再连接一个无线手柄，就可以直接在电视上游玩，无需将PC搬到电视旁边。而且现在哪怕是中端型号的电视也支持4k 120Hz和HDR，只要电脑能够带得动游戏的高画质，就可以在电视大屏幕上享受。

与手机平板不同，电视的更换频率低很多，因此硬件和安卓版本都可能比较老旧，可能遇到的障碍有：

*   电视的安卓版本太低，不支持手柄的无线连接。考虑加装一个高版本的电视盒子，在电视盒子上安装Moonlight；或者距离允许的话，用手柄直接无线连接服务端PC，这样还能有PC原生的手柄振动支持。
*   电视硬件太旧，运行Moonlight吃力。同样考虑加装电视盒子。
*   如果电脑和电视都支持4k 120Hz，那需要超过百兆的网络传输速率。此时如果想通过网线连接电视来保证最低的延迟，就会发现哪怕是目前的旗舰电视，网口也只有百兆。对于这种情况，要么退而求其次，使用无线网络，要么购买USB网卡，前提条件是电视至少是Android 10版本，并且有USB 3.0接口。推荐的USB网卡芯片是RTL8153或者RTL8153B，这两个型号的兼容性最好，其中后者的能耗和稳定性更佳。不过USB网卡往往不会标注使用的芯片，或者不同批次使用不同型号的芯片，拿不准的话可以在某宝上直接搜索芯片名称，和商家确认。

虚拟屏幕
----

Sunshine需要PC外接显示器才能投屏，也就是在PC的显示设置里能看到显示器。有些显示器哪怕关闭屏幕，只要和PC之间的线连着，PC重启或者唤醒后仍然可以识别到；而有些显示器如果关闭了屏幕，那么PC重启或者唤醒后，就识别不到显示器了，此时通过Moonlight去连接，会出现黑屏无法操作。考虑到作为串流服务端的PC所外接的显示器往往是关闭的（甚至没有显示器），因此需要确保PC总能找到一个显示设备。

一个方案是购买硬件欺骗器，类似于U盘，插在电脑的显示端口上，充当一个虚假的显示器。好处是方便操作，兼容性强；坏处是需要额外的成本，而且常规的欺骗器只支持到4k 60Hz，如果想要4k 120Hz HDR等更高的规格，需要花更高的价钱去定制。

更推荐的是软件模拟的方案。从windows 10开始自带了对虚拟屏幕的系统支持，通过安装自定义用户驱动就能随意创建任意分辨率和刷新率的虚拟屏幕。如果操作系统版本不低于windows 11 23H2，创建出来的虚拟屏幕还支持HDR。相关的软件不止一种，比如下面链接里的：

[![](https://pica.zhimg.com/v2-18495f34c746050863acd47200fa334e_ipico.jpg)GitHub - itsmikethetech/Virtual-Display-Driver: Add virtual monitors to your windows 10/11 device! Works with VR, OBS, Sunshine, and/or any desktop sharing software.github.com/itsmikethetech/Virtual-Display-Driver](https://link.zhihu.com/?target=https%3A//github.com/itsmikethetech/Virtual-Display-Driver)

具体使用方式参考软件说明。虽然这个软件比较简陋，但是完全够用了，只需要创建并编辑配置文件，就能随意指定虚拟屏幕的参数。

外网方案
----

如果想脱离局域网，能够出门在外的时候也能串流到家里的电脑上，就需要规划外网方案。由于每个人的网络环境千差万别，只能因地制宜，对网络知识要求不低，因此这是串流里最难的一个环节。整体思路还是推荐家里能有一个开放系统的路由器，否则许多定制化的网络操作难以实现。

小白用户推荐使用虚拟局域网方案，其他方案不适合新手。

### 虚拟局域网

一个相对简单的方案就是组建虚拟局域网，相当于将本来不在一个局域网里的设备划分到一个虚拟局域网中，这样就可以跨网络相互访问，仿佛大家都在同一个局域网内，常用于企业的远程办公系统。

这里推荐[ZeroTier](https://zhida.zhihu.com/search?content_id=247798974&content_type=Article&match_order=1&q=ZeroTier&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2NzM3ODUsInEiOiJaZXJvVGllciIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI0Nzc5ODk3NCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.qMYp-hB46WsLXZS-qe__mgEIuLRpJaKm65Fc5VX4veQ&zhida_source=entity)软件，支持各种主流操作系统，傻瓜式操作，具体教程自行搜索，在服务端和客户端设备上都安装后，将它们都配置在一个虚拟网络里，然后在Moonlight中添加机器，填写服务端在虚拟局域网里的ip地址，就可以跨网串流了。

虚拟局域网的质量取决于服务端和客户端的网络环境，如果双方网络的连通性较好，则可以做到点对点直连，不需要走第三方中转，网络延迟相对比较有保障，和其他方案相比延迟区别不大，只是稳定性稍低一些；如果连通性较差，则只能走第三方中转，网络质量比较恶劣，想提升的话需要更复杂的网络方案，投入产出比低很多。

只要不是奇葩的网络环境，通过ZeroTier组建虚拟局域网是一个对小白比较友好的方案，哪怕还有其他方案，这个方案也可以作为备用链路。

### 公网IP

本部分是写给对计算机网络知识比较了解、动手能力比较强的老司机的，只包含整体思路。如果不知道什么是公网IP，就不用继续看了。

注意：有好奇心的小白用户在尝试前务必弄清楚公网ip的概念。一个简单的判断方法是随便找一个ip地址归属地查询网站，查询一下自己准备使用的ip地址，如果查询网站没有给出这个ip对应的地理和运营商信息，而是告知这是内网/局域网ip，那就说明这个ip地址不是公网ip。

**IPv4**

理想状态是能分配到公网IPv4地址（不需要固定）。目前国内家庭宽带里电信/联通用户可能可以获取公网IPv4地址，移动用户比较困难。

有了公网IPv4地址后，需要将家里的光猫改造成桥接模式，用自己的路由器拨号上网，这样路由器就拥有了公网ip，可以从外网直接访问到这台路由器。随后开启路由器的[upnp](https://zhida.zhihu.com/search?content_id=247798974&content_type=Article&match_order=1&q=upnp&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2NzM3ODUsInEiOiJ1cG5wIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjQ3Nzk4OTc0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.iZPudySV274S4573xS2HHcddjCQLhlzsUqjwHZe50fM&zhida_source=entity)功能，同时在Sunshine的Configuration->Network下面也开启upnp，准备工作就完成了（一般不需要手动配置unpn转发规则）。最后一步是在外网环境下打开Moonlight，手动添加机器，直接填写路由器的公网IPv4地址，如果一切顺利的话就能识别到家里的PC，达到远程串流的目的。

![](assets/v2-441409dbef0da31744c50613f96faff5_1440w.jpg)

**[IPv6](https://zhida.zhihu.com/search?content_id=247798974&content_type=Article&match_order=1&q=IPv6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2NzM3ODUsInEiOiJJUHY2IiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjQ3Nzk4OTc0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.YCdLqsPFIUr1EzD7dLzA5bpaF5hD0_3thEMeECxKsQI&zhida_source=entity)**

如果没有公网IPv4地址，那就退而求其次，使用IPv6地址。目前国内的三大网络运营商（电信/联通/移动）都提供家庭宽带的公网IPv6地址，电信/联通/移动的公网IPv6地址分别以240e/2408/2409开头。只要在路由器中开启IPv6功能，家里所有设备就都拥有了公网IPv6地址。之后在Sunshine的Configuration->Network下面开启IPv4 + IPv6选项，就能让Sunshine跑在公网IPv6地址上。

但是做到这些还不够，路由器往往都有防火墙，默认阻止外网流量访问内网，需要在路由器上把串流使用到的端口加入防火墙的IPv6白名单，具体使用了哪些端口可以在Sunshine的Configuration->Network页面上查看。添加白名单的操作步骤因具体路由器而异，IPv4和IPv6各自有一套白名单，注意区分。

最后在Moonlight里添加机器的时候，填写Sunshine所在机器的公网IPv6地址就可以了。注意填写的格式为\[ipv6地址\]，有一对中括号。如果要指定端口，那就是\[ipv6地址\]:端口号。IPv6方案是客户端直连Sunshine机器，所以填写的是Sunshine机器的IPv6地址，而之前的IPv4方案是客户端去连接路由器的IPv4地址，依靠路由器的upnp功能转发IPv4流量到Sunshine机器。

此方案的局限性：

需要服务端和客户端都在IPv6环境下才可以连通，而可以使用IPv6的环境比较有限，一般只出现在家庭宽带，手机的移动网络，以及一些教育网环境。这就导致虽然家里有了IPv6公网，但是出门在外的网络环境因为不受自己控制，往往只有IPv4（例如商场、酒店等地方的Wifi热点）。外出时最有保障的IPv6网络环境其实是手机网络，但是串流的流量消耗很高，手机套餐的流量难以长时间支撑。

### [DDNS](https://zhida.zhihu.com/search?content_id=247798974&content_type=Article&match_order=1&q=DDNS&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NjY2NzM3ODUsInEiOiJERE5TIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjQ3Nzk4OTc0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.Pve4TVAmwWqrICWi4z-yb8JpHhCW6TNPh3H5djYLzxo&zhida_source=entity)

家庭宽带下，不论是IPv4还是IPv6的公网地址，都不是固定的，不定期会发生改变。所以需要将公网ip地址映射到一个固定的域名上，并且定时更新（例如每10分钟）。这样在Moonlight里就可以填写域名，而不用去关注具体的ip地址。

这种方式也即是DDNS方案。提供域名的服务商有很多，免费和收费的都有，串流的场合下免费域名就够了。IPv4场景下需要给路由器的公网IPv4申请域名；而IPv6的场景下需要给Sunshine机器的IPv6公网地址申请域名，也推荐同时给路由器的IPv6地址申请一个域名，这样在没有IPv4公网地址的情况下也可以从外部访问路由器，远程管理网络（需要在路由器上开启外网访问路由器管理页面的权限）。至于如何定时更新IP地址，需要自行摸索，比如有的路由器自带更新地址功能，有的域名服务商提供自动更新的软件。

### 远程唤醒

除非PC设置了永远不休眠，否则一段时间不操作，PC就进入休眠状态，不再响应串流请求。所以需要给PC开启WOL功能，可以在局域网内通过网络请求唤醒PC。为此要在BIOS以及网卡驱动里分别开启对应功能，如果是通过有线网络唤醒，那就更改有线网卡的配置，如果是通过无线网络唤醒，那就更改无线网卡的配置。通过有线网络的唤醒更可靠一些，不过实测无线网络唤醒也是可以正常工作的。参考如下教程：

[](https://link.zhihu.com/?target=https%3A//github.com/moonlight-stream/moonlight-docs/wiki/WOL-%28Wake-On-LAN%29)

如果是在局域网内，Moonlight启动后会自动发送唤醒请求，不用手动操作。如果在外网环境就麻烦一些了，因为唤醒请求只在局域网里有效，没法直接在公网上唤醒设备。整体的思路是要有一个不会休眠的设备，拥有公网ip，而且处于局域网内，由它代为发送唤醒请求。这个设备一般就是家里的路由器，而不少路由器系统（尤其是开放路由器系统）是提供相应的用户界面来手动唤醒局域网里的任意设备。这种场合下，只要通过路由器的公网IP登录路由器的管理页面，就可以在外网唤醒机器了。

另一个方案就是购买名为开机棒的智能硬件，通过开机棒提供的软件来远程唤醒，本质上还是WOL，不过比较省心省力。

### 断电处理

如果突然停电导致机器关机，那就无法通过网络唤醒了。需要在PC的BIOS里的电源管理部分，配置类似于“（非正常关机时）上电后自动开机”的功能，这样恢复供电后PC会自动开机。具体如何配置，各个BIOS并不相同，自行摸索。

市面上也有智能插座硬件，可以使用智能插座提供的软件远程开启或关闭PC的电源。通过这种远程控制供电的方式，配合上电自动开机的设置，也可以间接做到远程唤醒PC。

本文转自 <https://zhuanlan.zhihu.com/p/718510054>，如有侵权，请联系删除。