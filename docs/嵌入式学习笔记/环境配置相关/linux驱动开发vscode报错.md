
按f1 ->  输入 C/C++:Edit Configuration(JSON)搜索 -> 打开该文件 -> 输入以下内容

关键是头文件目录和defines

```json
{
    "configurations": [
        {
            "name": "Linux",
            "includePath": [
                "${workspaceFolder}/**",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/arch/arm/include",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/arch/arm/include/generated",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/include",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/include",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/arch/arm/include/generated",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/arch/um/include/asm",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/include/uapi",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/arch/arm/include/generated/uapi",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/arch/um/include",
                "/home/lqh/linux/imx6ull/linux-imx-alientek/tools/virtio"
            ],
            "defines": [
                "__GNUC__",
                "__KERNEL__",
                "MODULE"
            ],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c17",
            "cppStandard": "c++14",
            "intelliSenseMode": "linux-clang-x64"
        }
    ],
    "version": 4
}
```