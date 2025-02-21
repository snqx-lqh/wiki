## 联合体浮点数和无符号数的转换

```c
typedef union {
	uint8_t uchar_data[4];
	float   float_data;
}uchar_to_float_t;

uchar_to_float_t uchar_to_float;

uchar_to_float.uchar_data[0] = data_get[10];
uchar_to_float.uchar_data[1] = data_get[9];
uchar_to_float.uchar_data[2] = data_get[8];
uchar_to_float.uchar_data[3] = data_get[7];
Humidity1 = uchar_to_float.float_data;

uchar_to_float.float_data = Humidity1;
data_get[1] = uchar_to_float.uchar_data[0];
data_get[2] = uchar_to_float.uchar_data[1];
data_get[3] = uchar_to_float.uchar_data[2];
data_get[4] = uchar_to_float.uchar_data[3];



```

## volatile优化变量

有时候有些变量会被优化导致程序运行不了
