### 01-obsidian+gitee搭建云笔记

### 1、说明

| 软件名称      | 作用                                         |
| ------------- | -------------------------------------------- |
| git bash      | 初始化本地仓库，配置环境信息                 |
| obsidian+插件 | 构建“个人知识图谱”，可进行相互链接，同步到云 |
| typora        | 本地笔记编写，所见即所得                     |

### 2、搭建步骤

#### 2.1、gitee环境准备

创建一个或者是多个仓库，但是不需要初始化readme

![image-20251220081659195](assets/image-20251220081659195.png)

创建后会默认弹出以下信息，选择HTTPS，保留1、2内容，**2.2**中使用

![image-20251220082049187](assets/image-20251220082049187.png)

#### 2.2、git bash安装+初始化本地仓库

##### 2.2.1 下载

下载地址：https://git-scm.com/install/windows

![image-20251220082340006](assets/image-20251220082340006.png)

##### 2.2.2 安装

管理员身份运行后，一直选择下一步安装即可，请注意，如果你不熟悉每个选项的意思，请保持默认的选项

##### 2.2.3 初始化本地仓库

新建文件夹作为本地笔记存储路径，建议非C盘，我这里是D:\test001，进入文件夹，右击选择==open git bash here==

![image-20251220082639348](assets/image-20251220082639348.png)

打开后是

![image-20251220082744494](assets/image-20251220082744494.png)

依次执行以下命令

```bash
#1、初始化本地git环境
git init
#2、配置local级别的用户信息，默认保存的是global的，去除global的关键词
git config  user.name "Neil"
git config user.email "9853578+xdxghy@user.noreply.gitee.com"
#3、把gitee仓库clone到本地
git clone https://gitee.com/xdxghy/test001.git
```

![image-20251220083239567](assets/image-20251220083239567.png)

执行上述clone后，会自动弹出gitee的登录窗口，输入用户名密码登录即可

![image-20251220083346415](assets/image-20251220083346415.png)

如图，表示拉取成功

![image-20251220083332436](assets/image-20251220083332436.png)

#### 2.3、obsidian安装+插件安装配置

##### 2.3.1 下载

下载地址：https://obsidian.md/

![image-20251220083518338](assets/image-20251220083518338.png)

##### 2.3.2 安装

管理员身份运行后，一直选择下一步安装即可，请注意，如果你不熟悉每个选项的意思，请保持默认的选项

##### 2.3.3 打开仓库

安装软件后，打开，则会提示如下，选择打开本地仓库，选择D:\test002\test002

![image-20251220083725992](assets/image-20251220083725992.png)

此时在windows资源管理器打开这个目录，会发现在这个目录下多了一个**.obsidian**的目录，里边是obsidian的配置文件

![image-20251220084040991](assets/image-20251220084040991.png)

在这个目录，需要新建一个文件.gitignore，写入以下信息，表示不把obsidian缓存区的文件同步到gitee

```bash
.obsidian/workspace.json
.obsidian/workspace-mobile.json
```

##### 2.3.4 插件安装

关闭安全模式，点击社区插件市场，点击浏览

![image-20251220084257834](assets/image-20251220084257834.png)

![image-20251220084327515](assets/image-20251220084327515.png)

输入git，检索（==这里加载比较慢，因为是获取的github的资源，后续考虑魔法或者是pkmer==）

![image-20251220084506063](assets/image-20251220084506063.png)

![image-20251220084550189](assets/image-20251220084550189.png)

安装好了启用

![image-20251220084618106](assets/image-20251220084618106.png)

启用后配置选项

![image-20251220084641214](assets/image-20251220084641214.png)

![image-20251220084704092](assets/image-20251220084704092.png)

![image-20251220084735600](assets/image-20251220084735600.png)

保存后，直接关闭窗口即可

##### 2.3.5 同步到gitee测试

在本地创建几个文件

![image-20251220084841649](assets/image-20251220084841649.png)

关闭obsidian，再次开启，就可以看到自动同步了文件

![image-20251220085525856](assets/image-20251220085525856.png)

打开gitee仓库查看，就可以看到上传的文件

#### 2.4、typora安装

下载地址：https://typoraio.cn/

管理员身份运行后，一直选择下一步安装即可，请注意，如果你不熟悉每个选项的意思，请保持默认的选项



注意，这里可以把笔记目录固定下，后续打开typora就是笔记目录

![image-20251220090222314](assets/image-20251220090222314.png)