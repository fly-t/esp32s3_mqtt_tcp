# esp32 接入腾讯云

> 目前效果可以使用腾讯云进行接入设备，然后通过微信小程序进行控制开发板上的LED点亮关闭（GPIO 控制）

该项目的修改基于`esp example mqtt tcp(IDF 5.1.1v)`

# GIPO config
配置gpio

配置gpio打开example中的`blink_exampl`。

因为使用的是ws2812，然后看到所有的demo中这个blink_exapmle，代码比较简洁，然后就对着个项目进行cv。

## 1. 复制yml文件（关键）

复制yml文件将添加项目依赖

## 2. 编辑kongfig文件

参照blink进行配置kconfig文件，该文件用于执行图形界面配置选项。

提交的文件中没有修改kconfig文件，使用的是默认的mqtt_tcp的kconfig，所有没有的kcondig宏定义直接写死在代码中。

``` bash
menu "Example Configuration"

    # 把基础环境配置kconfig包含进来。
    orsource "$IDF_PATH/examples/common_components/env_caps/$IDF_TARGET/Kconfig.env_caps"

    # 配置LED类型是普通GPIO 还是RMT驱动的类型
    choice BLINK_LED
        prompt "Blink LED type"
        # default BLINK_LED_GPIO if IDF_TARGET_ESP32 || !SOC_RMT_SUPPORTED：这是默认选择的选项。它告诉系统，在某些条件下，如果用户没有明确选择任何选项，将选择 BLINK_LED_GPIO 作为默认选项。条件是 IDF_TARGET_ESP32 或 !SOC_RMT_SUPPORTED（如果 SOC_RMT_SUPPORTED 未定义）。
        default BLINK_LED_GPIO if IDF_TARGET_ESP32 || !SOC_RMT_SUPPORTED
        # default BLINK_LED_RMT：如果用户没有明确选择选项并且不符合上述条件，将选择 BLINK_LED_RMT 作为默认选项。
        default BLINK_LED_RMT
        help
            Defines the default peripheral for blink example

        config BLINK_LED_GPIO
            bool "GPIO"
        config BLINK_LED_RMT
            bool "RMT - Addressable LED"
    endchoice
        
    config BLINK_GPIO
        int "Blink GPIO number"
        range ENV_GPIO_RANGE_MIN ENV_GPIO_OUT_RANGE_MAX
        default 5 if IDF_TARGET_ESP32
        default 18 if IDF_TARGET_ESP32S2
        default 48 if IDF_TARGET_ESP32S3
        default 8
        help
            GPIO number (IOxx) to blink on and off or the RMT signal for the addressable LED.
            Some GPIOs are used for other purposes (flash connections, etc.) and cannot be used to blink.

    config BLINK_PERIOD
        int "Blink period in ms"
        range 10 3600000
        default 1000
        help
            Define the blinking period in milliseconds.

endmenu
```

## 3. CV

复制粘贴`blink_demo`复制到目标项目。

这一版的没有解耦，不够优雅。


----------


# 常用的命令

``` bash
idf.py build
idf.py -p COM1 flash
idf.py menuconfig
idf.py cleanfull
idf.py monitor
```

----------

#### 侃侃空

push的时候有个小插曲，忘记添加gitignore文件了

``` bash
# 删除已经commit缓存的文件
git rm -r --cached /path/to/folder/
```

发现vscode的红色波浪线的引起原因大多是因为compiler配置不对，配置参考：

`c_cpp_perperties.json`

``` bash
{
    "configurations": [
        {
            "name": "ESP-IDF",
            "compilerPath": "${config:idf.toolsPath}\\tools\\xtensa-esp32-elf\\esp-12.2.0_20230208\\xtensa-esp32-elf\\bin\\xtensa-esp32-elf-gcc.exe",
            "includePath": [
                "${config:idf.espIdfPath}/components/**",
                "${config:idf.espIdfPathWin}/components/**",
                "${config:idf.espAdfPath}/components/**",
                "${config:idf.espAdfPathWin}/components/**",
                "${workspaceFolder}/**"
            ],
            "browse": {
                "path": [
                    "${config:idf.espIdfPath}/components",
                    "${config:idf.espIdfPathWin}/components",
                    "${config:idf.espAdfPath}/components/**",
                    "${config:idf.espAdfPathWin}/components/**",
                    "${workspaceFolder}"
                ],
                "limitSymbolsToIncludedHeaders": false
            },
            "compileCommands": "${workspaceFolder}/build/compile_commands.json"
        }
    ],
    "version": 4
}
```
