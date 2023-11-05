# esp32 接入腾讯云

该项目的修改基于`esp example mqtt tcp(IDF 5.1.1v)`

# 关键点1：

mqtt项目demo

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-14-22-50.png)

我这个简单，直接选用的tcp模式的mqtt。

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-14-23-20.png)

这个username和password的格式属实没想到，还是我想当然了，直接看的代码，想着代码注释中会给出接口的数据格式，大意了。

# 关键点2：

腾讯云的算法更新，之前我用三元组，先用`mqttFx`测试，然后将三元组直接写入代码中，没有问题。

但是这次测了两天一直报错 auth错误，查找 [文档](https://espressif-docs.readthedocs-hosted.com/projects/esp-faq/zh-cn/latest/software-framework/protocols/mqtt.html) 账号密码错误，当时我还坚信，fx能连上，账号密码肯定对，去云端复制新的三元组不经过fx测试， 直接烧录代码没有任何问题，成功接入平台。


![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-14-30-53.png)


# 关键点3：

设别连接如云平台后wifi断连后无法自动重连上wifi调整`ESP_ERROR_CHECK(example_connect());`位置，可以解决问题，但是不是最优解。

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-14-54-13.png)


成功运行输出

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-15-08-06.png)

----------

#### 侃侃空

`该项目的最终目的是手机控制开关灯具`

查找很多方案要门时间太久了，要么是使用的`arduino`, 不得不承认`arduino`的确很优秀。但是我还是不想用，个人感觉官方的idf的框架可玩性更高，借此机会也尝试下饱受吐槽的`idf`.

用了些时间把`vscode`的`idf`环境配置好感觉还是有些费劲，esp官方也提供了ide，基于eclipse开发的（eclips属实是被各大厂商整明白了，主要是ui和补全太差了）。环境配置好，代码阅读，太舒服了。但是有个问题，第一次编译时间太长了。。。。我的机器也有很大的缘故。

idf也是很贴心把接下来会用到的代码都在终端中输出，还是不错的。vscode中还有按钮，不过我还是喜欢命令行，释放右手鼠标。

常用的命令：

``` bash
idf.py build
idf.py -p COM1 flash
idf.py menuconfig
idf.py cleanfull
idf.py monitor
```
