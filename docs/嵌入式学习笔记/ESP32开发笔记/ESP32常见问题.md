## 仅输入引脚

GPIO 34-39是GPIO仅输入引脚。这些引脚没有内部上拉或下拉电阻。它们不能用作输出，因此只能将这些引脚用于输入

```
GPIO 34
GPIO 35
GPIO 36
GPIO 37
GPIO 38
GPIO 39
```
## 集成在ESP-WROOM-32上的SPI闪存

在一些ESP32开发板中，GPIO 6-GPIO 11是暴露的。但是，这些引脚连接到ESP-WROOM-32芯片上的集成SPI闪存，不建议用于其他用途。

```
GPIO 6 (SCK/CLK)
GPIO 7 (SDO/SD0)
GPIO 8 (SDI/SD1)
GPIO 9 (SHD/SD2)
GPIO 10 (SWP/SD3)
GPIO 11 (CSC/CMD)
```
## 电容式触摸GPIO
ESP32有10个内部电容式触摸传感器。这些传感器可以感知任何带有电荷的东西的变化，比如人的皮肤。因此，它们可以检测到用手指触摸GPIO时引起的变化。这些引脚可以很容易地集成到电容式焊盘中，并取代机械按钮。电容式触摸引脚还可以用来将ESP32从深度睡眠中唤醒。那些内部触摸传感器就连接到这些GPIO上：

```BASH
T0 (GPIO 4)
T1 (GPIO 0)
T2 (GPIO 2)
T3 (GPIO 15)
T4 (GPIO 13)
T5 (GPIO 12)
T6 (GPIO 14)
T7 (GPIO 27)
T8 (GPIO 33)
T9 (GPIO 32)
```

[ESP32-S3 引脚参考大全 – 凌顺实验室 (lingshunlab.com)](https://lingshunlab.com/book/esp32/esp32-s3-pin-reference#3x_UART)