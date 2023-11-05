# esp32 接入腾讯云

该项目的修改基于`esp example mqtt tcp(IDF 5.1.1v)`

# 1. 配置idf在vscode中的环境

下载好`idf.exe`之后，进入到安装目录

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-13-48-12.png)

接下来进入到`Espressif\tools\idf-python\3.11.2`目录和`Espressif\python_env\idf5.1_py3.11_env\Scripts`中安装pip工具，命令如下

``` python
python -m ensurepip --upgrade
```

# 2.安装vscode esp插件

安装完pip后 安装vscode插件

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-13-53-24.png)

圈起来的下面的那个选项，`esp-idf configure ESP-IDF!!!!!!!`
![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-13-55-00.png)

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-13-56-52.png)

![](https://raw.githubusercontent.com/fly-t/images/main/blog/README-2023-11-05-13-57-47.png)

接下完成后安装python的包，选择左上角本地安装即可， 因为下载的idf中包含了python工具包。

# 总结： 

选择一个example中的项目编译根据官方文档操作即可。主要是python缺少的pip工具，安装好之后就没问题了。
