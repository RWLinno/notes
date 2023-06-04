最近花了好多时间，废了好多U盘才把这东西搞明白了。

用这篇博客总结一下自己，然后附上详细的流程以免大家踩坑。







# Windows to Go

这个比较容易上手

### 1. 准备工作

- 下载Windows10安装程序：https://www.microsoft.com/en-au/software-download/windows10

![image-20230529104825257](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230529104825257.png)

- 下载Rufus，我这这边用的版本是3.21
- 准备一个U盘，3.0版64G以上最好。



### 2. 设置流程

- 打开windows10 安装程序
- 选择**为另一台电脑创建安装介质**
- 选择语言(中文)、版本(windows 10)，体系结构(64位)
- 选择**ISO文件(不要选择U盘!)**，先将ISO放在本地再用Rufus去装。

- 使用Rufus格式化并装入镜像，设置如下：

![image-20230529104718854](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230529104718854.png)

### 3. BIOS设置

这里我以防万一用的是实验室的机子，开机按F10进入。各种牌子的电脑金Bios方式不一样，大家自己搜索一下。

- 进入BIOS，在**安全->安全引导配置**一栏开启旧支持，关闭安全引导

![8bbff78a3cbd92b7fe2890c1125b14d](C:\Users\lenovo\AppData\Local\Temp\WeChat Files\8bbff78a3cbd92b7fe2890c1125b14d.jpg)

- 在**存储->引导顺序**一栏设置成USB先运行

![image-20230529113137823](C:\Users\lenovo\AppData\Roaming\Typora\typora-user-images\image-20230529113137823.png)

- 保存并退出

设置完成后重启，稍等就能进入Windows初始化界面了，大功告成。



# Linux to Go





# 总结

这里附上网盘链接，大家可以直接打包带走所有东西，有不懂的可以私信交流一下。



# 致谢

如何将Win10和Linux系统安装进U盘？实现即插即用！| 零度解说

Ubuntu To Go 制作教程 | [Nofated's Blog](https://blog.nofated.win/)

以及踩坑时解决我问题的ChatGPT、知乎、Bing