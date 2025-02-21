>
> 韦东山FreeRTOS课程教程的学习笔记，主要记录API的使用方法和应用场景。
>
## ARM架构简明教程

汇编指令在干嘛，函数跳转做了什么

## 堆的概念

开辟的一块很大的数组，堆管理就是申请的时候会带有长度信息和链表相关。

## 栈的概念

后入先出，存储局部变量，函数返回地址等

FreeRTOS会给每个任务开辟自己的栈，任务切换的时候将所有寄存器信息保存在栈中。

## 内存管理

FreeRTOS有几种内存管理方式 1、2、3、4、5，常用4，因为可分配可释放还可以合并内存碎片。

申请内存和释放内存，申请内存定义的是unsigned int，所以申请的也应该是4个字节吧（这里需要再看看）

```c
uint8_t *buffer;

buffer=pvPortMalloc(30);

vPortFree(buffer);

```

## 任务

### 创建任务

有动态创建任务，静态创建任务

如何估算栈的大小，通过看会有多少次函数嵌套乘以最大的寄存器数量，以及计算数组量有多大。

使用任务参数，是传进去地址信息，然后再在任务中定义一个指针接收这个地址。

**创建动态任务**

```c
//任务优先级
#define TASK1_TASK_PRIO		3
//任务堆栈大小	
#define TASK1_STK_SIZE 		128  
//任务句柄
TaskHandle_t Task1Task_Handler;

xTaskCreate((TaskFunction_t )task1_task,             
			(const char*    )"task1_task",           
			(uint16_t       )TASK1_STK_SIZE,        
			(void*          )NULL,                  
			(UBaseType_t    )TASK1_TASK_PRIO,        
			(TaskHandle_t*  )&Task1Task_Handler); 

```


**创建静态任务**

```C


```

**创建任务传递参数**

该功能可以运用在比如建立两个一样的任务，只是部分任务参数不一样，传递参数可以执行两个想同任务的不同功能，比如这里就是想显示两个不同的OLED内容。

```c
struct  DisplayInfo {
    int x;
    int y;
    const char *str;
};

DisplayInfo info1 = {0,0,"hello"};
DisplayInfo info2 = {0,20,"world"};

TaskHandle_t Lcd1Task_Handler;
TaskHandle_t Lcd2Task_Handler;

xTaskCreate(lcd_task,"lcd1_task",128,&info1,3,&Lcd1Task_Handler);
xTaskCreate(lcd_task,"lcd2_task",128,&info2,3,&Lcd2Task_Handler);

void lcd_task( void *pvParameters )
{
	struct  DisplayInfo *info = pvParameters;
	//任务执行
	//···
}

```

### 删除任务

调用删除API就可以了，这个可以用在比如遥控器播放音乐，不想播放就删除，想播放就新建个任务。

```c
//在自己的任务函数中删除自己
vTaskDelete(NULL)
//删除其他的任务函数，放入它的句柄，也可以这样删除自己的
vTaskDelete(pvTaskCode)
```

### 优先级和阻塞

优先级高的任务会先执行

### 任务状态

将任务挂起和恢复，可以实现比如音乐任务的播放和暂停

```C
vTaskSuspend(Task1Task_Handler);//挂起任务

vTaskResume(Task1Task_Handler);	//恢复任务
```

### 任务管理与调度

执行状态是怎么变化的，什么时候，将什么句柄放到什么链表中去，就绪态链表，阻塞态链表等

### 空闲任务

处理删除任务的一些释放工作，任务创建时会分配栈，那么任务删除的时候，谁来释放这些栈呢，就是空闲任务可以做的事，当然，他还有其他功能。

### 两个Delay

一个是执行完延时那么长，一个是从任务开始算时间

```C
vTaskDelay(200);  //至少等待指定个数的Tick Interrupt才能变为就绪状态

xTaskDelayUntil(&Pre, 200) //等待到指定的绝对时刻，才能变为就绪态。
```

## 同步和互斥

### 环形Buffer

下一个写的位置不等于读的位置就是没满
### 队列

应用场景，比如红外遥控器键值控制游戏块运动，编码器值也想控制，就把他们同时写入一个队列中，然后让游戏块控制任务执行。

游戏任务，最后的获取

```c
QueueHandle_t g_xQueuePlatform; 
struct input_data idata;

//创建队列  动态创建 个数 单个内容的大小
g_xQueuePlatform = xQueueCreate(10, sizeof(struct input_data));
//读取队列值
pdPASS == xQueueReceive(g_xQueuePlatform, &idata, portMAX_DELAY)

```

红外遥控器中断

```c
xQueueSendToBackFromISR(g_xQueuePlatform, &idata, NULL);//将准备好的数据传入队列
```

编码器中断

```c
xQueueSendFromISR(g_xQueueRotary, &rdata, NULL);
```

编码器任务

```c
QueueHandle_t g_xQueueRotary;   /* 旋转编码器队列 */
static uint8_t g_ucQueueRotaryBuf[10*sizeof(struct rotary_data)];
static StaticQueue_t g_xQueueRotaryStaticStruct;

//静态创建队列 个数 单个内容的大小 队列BUFF大小 静态结构控制器
g_xQueueRotary   = xQueueCreateStatic(10, sizeof(struct rotary_data), g_ucQueueRotaryBuf, &g_xQueueRotaryStaticStruct);

/* 读旋转编码器队列 */
xQueueReceive(g_xQueueRotary, &rdata, portMAX_DELAY);
/* 写入获取任务中 */
xQueueSend(g_xQueuePlatform, &idata, 0);
```

### 队列集

加入控制设备有多个，方便移植，将所有队列加入队列集使用

game任务

```C
static QueueSetHandle_t g_xQueueSetInput; /* 输入设备的队列集 */
static QueueHandle_t g_xQueuePlatform; /* 挡球板队列 */
static QueueHandle_t g_xQueueIR;
static QueueHandle_t g_xQueueRotary;

/* 创建队列,队列集,创建输入任务InputTask */
g_xQueuePlatform = xQueueCreate(10, sizeof(struct input_data));
g_xQueueSetInput = xQueueCreateSet(IR_QUEUE_LEN + ROTARY_QUEUE_LEN);

//获得红外和编码器的队列
g_xQueueIR = GetQueueIR();
g_xQueueRotary = GetQueueRotary();

//将队列添加到队列集
xQueueAddToSet(g_xQueueIR, g_xQueueSetInput);
xQueueAddToSet(g_xQueueRotary, g_xQueueSetInput);

xTaskCreate(InputTask, "InputTask", 128, NULL, osPriorityNormal, NULL);
xTaskCreate(platform_task, "platform_task", 128, NULL, osPriorityNormal, NULL);

```

红外任务，中断中

```C
xQueueSendFromISR(g_xQueueIR, &data, NULL);
```

编码器任务，中断中

```C
xQueueSendFromISR(g_xQueueRotary, &rdata, NULL);
```


在`InputTask`任务

```C
/* 读队列集, 得到有数据的队列句柄 */
xQueueHandle = xQueueSelectFromSet(g_xQueueSetInput, portMAX_DELAY);
if (xQueueHandle)
{
	/* 读队列句柄得到数据,处理数据 */
	if (xQueueHandle == g_xQueueIR)
	{
		//ProcessIRData();
		xQueueReceive(g_xQueueIR, &idata, 0);
		//处理数据
		//。。。
		/* 写挡球板队列 */
		xQueueSend(g_xQueuePlatform, &input, 0);
	}
	else if (xQueueHandle == g_xQueueRotary)
	{
		//ProcessRotaryData();
		/* 读旋转编码器队列 */
		xQueueReceive(g_xQueueRotary, &rdata, 0);
		//处理数据
		//。。。
		/* 写挡球板队列 */
		xQueueSend(g_xQueuePlatform, &idata, 0);
	}
	
}
```

在`platform_task`任务

```C
xQueueReceive(g_xQueuePlatform, &idata, portMAX_DELAY);
```

### 分发数据

队列分发数据，可以用一个数组注册队列，每次发队列数量的队列数据，一个数据发三次，就能每个任务都能接收一次

比如要将红外的数据多发送几次

```C
//创建一个队列数组
static QueueHandle_t g_xQueues[10];
static int g_queue_cnt = 0;

//注册函数，其他的任务函数调用，就可以把想要接收数据的队列放进来
void RegisterQueueHandle(QueueHandle_t queueHandle)
{
	if (g_queue_cnt < 10)
	{
		g_xQueues[g_queue_cnt] = queueHandle;
		g_queue_cnt++;
	}
}

//分发数据，然后把相同的数据，每个队列发一个
static void DispatchKey(struct ir_data *pidata)
{
	int i;
	for (i = 0; i < g_queue_cnt; i++)
	{
		xQueueSendFromISR(g_xQueues[i], pidata, NULL);
	}
}

```

### 信号量

就是比如门票

**计数型信号量**

```C
//信号量
static SemaphoreHandle_t g_xSemTicks; 
// 创建信号量
g_xSemTicks = xSemaphoreCreateCounting(3, 2);
/* 获得信号量 */
xSemaphoreTake(g_xSemTicks, portMAX_DELAY);
/* 释放信号量 */
xSemaphoreGive(g_xSemTicks);
```

**二值信号量**

```C
static SemaphoreHandle_t g_xSemTicks; 
//创建二值信号量
g_xSemTicks = xSemaphoreCreateBinary();
/* 获得信号量 */
xSemaphoreTake(g_xSemTicks, portMAX_DELAY);
/* 释放信号量 */
xSemaphoreGive(g_xSemTicks);
```

### 互斥量

比如领导临时提升等级

```C
g_xSemTicks = xSemaphoreCreateMutex();
```

常用方案

```c
// 等待互斥量，确保在写入共享数据时不会被其他任务中断
if (xSemaphoreTake(rcDataMutexSemaphore, portMAX_DELAY) == pdTRUE) {
	// 更新共享数据
	rc_data = rc_data_temp;
	// 释放互斥量
	xSemaphoreGive(rcDataMutexSemaphore);
}
```
### 事件组

就像标志位




