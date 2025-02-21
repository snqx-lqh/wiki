## 更新说明
### 更新日期 2024/3/7

该项目做了部分的优化，原来的我也没有删除，新的文件我在命名后面加了V2，新的设计将GUI控制和GUI的data操作分离了开来，使用注册操作函数的方式注册gui相关使用函数。

优化修改主要针对刷屏，以前用的中景园的代码，在使用OLED_Clear的时候，他会把全部刷新一遍，我修改的部分，只是让他将缓冲清零，但是不写入。

```C
void OLED_Clear_Buff(void)
{
    u8 i,n;
    for(i=0; i<8; i++)
    {
        for(n=0; n<128; n++)
        {
            OLED_GRAM[n][i]=0;//清除所有数据
        }
    }
}
```
然后在代码中删除了部分的冗余操作。现在的整体代码逻辑是将缓冲清零，写入想要显示的数据，刷新缓冲。这样的话，每次想要刷新新的数据的时候原来显示的缓冲就会被清零，不用像我原来写的那样通过给缓冲区写空擦除。这样的话，在定义字符串的时候就不用考虑必须15个字符了。而且我感觉每次先来一次全写空，对我这个菜单不做动画的话，刷新看着也还行。

具体的操作可以看我的代码，大体的按键控制的思路是没变的，只是刷新数据的方式那部分做了改变，以及其他的一些细小的改变。刷新数据的逻辑就只有下面这个了，其中p_gui_operations->userGuiDataDisplayRefresh(nowMenu);的用法是函数指针的用法，里面的userGuiDataDisplayRefresh函数会在gui_port.c里面注册。

```C

void DisplayRefreash(struct Menu_t *nowMenu)
{
	int i = 0;
	OLED_Clear_Buff();
	p_gui_operations->userGuiDataDisplayRefresh(nowMenu);
	if(strcmp((char*)nowMenu->name,p_gui_operations->main_ui_name) == 0)//回到主页面
	{
		MainUiSet();
	}else 
	{	
		OLED_ShowChar(0,selectItem*16,     '>',16,1);//画出当前选中菜单项在当前显示菜单页的索引
		//显示菜单项的内容
		for(i=0;i<(nowMenu->MenuProperty->MenuLen-nowMenu->MenuProperty->scrollBarLen);i++)
		{
			OLED_ShowString(8,i*16,nowMenu[i+scrollBar].displayString,16,1);
		}
	}
	OLED_Refresh();
}
```

## 以下是原文

## 硬件

### 硬件选型

- STM32F103C8T6最小核心板 

- 0.96寸四脚OLED屏幕IIC接口

- 普通按键5个

### 硬件连线

- SCL ---- PA1

- SDA ---- PA2

- KEY_UP ---- PA4

- KEY_DOWN ---- PA5

- KEY_LEFT ---- PA3

- KEY_RIGHT ---- PA6

- KEY_OK ---- PA7

## 代码开源链接

### 百度网盘

链接：https://pan.baidu.com/s/1W4dIgTYgQv7Pp4iX-QnwTg 
提取码：k7p4 

### Gitee

[屏幕使用-GUI设计: 一些单片机控制屏幕的项目，和一些GUI界面设计 stm32驱动的oled屏等等 (gitee.com)](https://gitee.com/snqx-lqh/screen-design-using-gui)

## 软件开发

### 基础功能-自己完成

#### Systick延时

我使用的是正点原子的代码，进行了简单的修改，但大体一样，可用正点原子代码。具体修改看我代码，不修改也可用。

#### 5ms定时器

使用的定时器3做的一个5ms的定时器中断。定时器中断中放置按键扫描函数。

#### 按键扫描

也是参考的正点原子的按键扫描函数，但是没有使用延时函数，使用的计数延时来达到按键消抖的目的。5ms扫描一次，大于2就延时10ms。该函数放在定时器5ms的中断函数中。这里按键扫描给一个按键按下的标志位，例如isKeyUp，按键按下就将它置1。

```c
/**
 * @brief 按键扫描函数
 * 
 * @param mode 模式为1就是连续扫描，为0就是单次
 */
void KeyScan(u8 mode)
{
    static int keyCount = 0;
    static int keyState = 0;
    if(mode == 1) keyState=0;
    if (keyState == 0 && (KEY_UP == 0||KEY_DOWN == 0||KEY_LEFT == 0||KEY_RIGHT == 0||KEY_OK == 0))
    {    
        keyCount++;
        if(keyCount>2)
        {
            keyState = 1;
            keyCount=0;
            if(KEY_UP == 0) KeyUp();
            else if(KEY_DOWN == 0) KeyDown();
            else if (KEY_LEFT == 0) KeyLeft();
            else if (KEY_RIGHT == 0) KeyRight();
            else if (KEY_OK == 0) KeyOk();
        }
    }else if (KEY_UP == 1 && KEY_DOWN == 1 && KEY_LEFT == 1 && KEY_RIGHT == 1 && KEY_OK == 1)
    {
        keyState = 0;
    }
}

void KeyUp()
{
	if(isKeyUp == 0)
		isKeyUp=1;
	LED=!LED;
}
```

#### oled的简单使用

oled使用的是中景园电子的代码，可以在项目代码中查看。

### 多级菜单设计

#### 定义菜单项结构体

```c
//菜单页参数结构体
struct MenuProperty_t
{
	u8 MenuLen;//当前菜单页菜单项总个数
	u8 scrollBarLen;//滚动条长度,由于都是用的16SIZE的字符，所以一个菜单页最多四个菜单项，五个菜单项滚动条就为1
};

//菜单项结构体
struct Menu_t{
	struct MenuProperty_t *MenuProperty;//当前菜单项所在菜单页的参数
	u8 displayString[15];//当前菜单项的字符
	void (*func1) (void);//当前菜单项的功能函数
	void (*func2) (void);//当前菜单项的功能函数
	struct Menu_t *fatherMenu;//当前菜单项的父级菜单项
	struct Menu_t *childrenMenu;//当前菜单项的子级菜单项	
};
```

#### 定义一个菜单页

主界面菜单，算一级菜单，主界面一般可以拿来画一些好玩的UI设计，我这个项目做的是一个时钟设计。这个菜单页就只有一个菜单项，滚动条为0。由于初始化不能先填入未初始化的数据，所以他的子菜单项初始化先设定为NULL。

```c
//主UI
struct MenuProperty_t MainUIProperty={1,0};
struct Menu_t MainUI=
{&MainUIProperty,"MainUI       " ,NULL,NULL,NULL};
```

主菜单，算二级菜单，拿来做我想要显示的数据项分类，父菜单就是MainUI,子菜单项初始化先设定为NULL。注意字符串尽量写15个字符，用空格也要占位，使得后面数据好刷新。这个菜单页就有四个菜单项，滚动条为0。

```c
//主菜单
struct MenuProperty_t menuMainProperty={4,0};
struct Menu_t menuMain[4]=
{
	{&menuMainProperty,"last menu     ", NULL,NULL, &MainUI,NULL},
	{&menuMainProperty,"Animal        ", NULL,NULL, &MainUI,NULL},
	{&menuMainProperty,"Pid           ", NULL,NULL, &MainUI,NULL},
	{&menuMainProperty,"Time set      ", NULL,TimeSetInit, &MainUI,NULL}
};
```

animal的子菜单，算3级菜单，这个就是真的想要显示的animal菜单项的数据。父菜单就是menuMain,子菜单项初始化先设定为NULL。这个菜单页就有六个菜单项，滚动条长度为2，因为一面最多显示4个，滚动一下往下移一个。

> 注意：要是你定义的是单个项，取地址就要加&，要是定义的数组，就可以用数组名取该数组首地址。

```c
//animal的子菜单
struct MenuProperty_t setMenu1Property={6,2};
struct Menu_t setMenu1[6]=
{
	{&setMenu1Property,"last menu     ",NULL,NULL,menuMain,NULL},
	{&setMenu1Property,"bull          ",NULL,NULL,menuMain,NULL},
	{&setMenu1Property,"bird          ",NULL,NULL,menuMain,NULL},
	{&setMenu1Property,"dog           ",NULL,NULL,menuMain,NULL},
	{&setMenu1Property,"bow           ",NULL,NULL,menuMain,NULL},
	{&setMenu1Property,"fish          ",NULL,NULL,menuMain,NULL}
};
```

#### OLED页面刷新函数

刷新页面信息，要是在主页面就清空一下在画图，要是没有在主页面，使用覆盖来达到刷新的效果。

```c
void DisplayRefreash(struct Menu_t *nowMenu,u8 selectItem,u8 scrollBar)
{
	int i = 0;
	static u8 lastSelectItem=0;//记录上次索引
	if(nowMenu==&MainUI)//当回到主菜单时，由于没有全占屏，所以全部清屏，再画
	{
		OLED_Clear();
		MainUiSet();
	}else 
	{	
		OLED_ShowChar(0,lastSelectItem*16, ' ',16,1);//清除上次索引箭头
		OLED_ShowChar(0,selectItem*16,     '>',16,1);//画出这次索引箭头
		for(i=0;i<(nowMenu->MenuProperty->MenuLen-nowMenu->MenuProperty->scrollBarLen);i++)
		{
			OLED_ShowString(8,i*16,nowMenu[i+scrollBar].displayString,16,1);
		}
	}
	OLED_Refresh();
	lastSelectItem = selectItem;
}
```

#### OLED数据刷新函数

当每个页面的数据要刷新时，就只需要把上一次的数据覆盖就行了，所以每次就要写满一行的字符，或者你每行的字符长度相同也可以达到相同的目的。

```c
void DisplayRefreashData(struct Menu_t *nowMenu,u8 selectItem,u8 scrollBar)
{
	int i = 0;
	for(i=0;i<(nowMenu->MenuProperty->MenuLen-nowMenu->MenuProperty->scrollBarLen);i++)
	{
		OLED_ShowString(8,i*16,nowMenu[i+scrollBar].displayString,16,1);
	}
	OLED_Refresh();
}
```

刷新数据前，数据的更改可以通过Sprintf函数来重新定义每个菜单项的显示字符串

```c
void GuiDataDisplayRefresh()
{
	if(menuPoint == setMenu1)
	{
		sprintf((char*)setMenu1[1].displayString,"bull  %3d     ",count1);
		sprintf((char*)setMenu1[2].displayString,"bird  %3d     ",count2);
		sprintf((char*)setMenu1[3].displayString,"dog   %3d     ",count3);
		sprintf((char*)setMenu1[4].displayString,"bow   %3d     ",count4);
		sprintf((char*)setMenu1[5].displayString,"fish  %3d     ",count5);
		DisplayRefreashData(menuPoint,selectItem,scrollBar);
	}
	else if(menuPoint==&MainUI)
	{
		MainUiSet();
		OLED_Refresh();
	}
}
```

#### 菜单初始化

主要拿来初始化一些菜单项的子菜单，以及当前菜单的指针指向。

全局变量

```
menuPoint    当前菜单指向地址
selectItem   当前索引 0-3
scrollBar    当前滚动条所在位置，最上处为0
```



```c
void GuiInit()
{
	MainUI.childrenMenu = menuMain;
	menuMain[1].childrenMenu = setMenu1;
	menuMain[2].childrenMenu = setMenu2;
	menuMain[3].childrenMenu = setMenu3;
	menuPoint = &MainUI;
	DisplayRefreash(menuPoint,selectItem,scrollBar);
}
```

#### 按键控制函数

上下按键主要拿来切换现在的索引和滚动条

左右键主要拿来实现功能函数

```c
void GuiControl()
{
	if(isKeyUp==1)//上键按下
	{
		isKeyUp=0;//标志位清零
		selectItem--;//当前菜单在当前菜单页的索引--
		if(selectItem<0&&scrollBar!=0)//小于0,但是滚动条不在0，就减滚动条
		{
			selectItem = 0;
			scrollBar--;
		}else if(selectItem<0&&scrollBar==0)//小于0,滚动条也在0，就将索引移到最后，滚动条到最大
		{
			selectItem = menuPoint->MenuProperty->MenuLen-1-menuPoint->MenuProperty->scrollBarLen;
			scrollBar  = menuPoint->MenuProperty->scrollBarLen;
		}
		DisplayRefreash(menuPoint,selectItem,scrollBar);//刷新显示
	}else if(isKeyDown==1)//和上键差不多
	{
		isKeyDown=0;
		selectItem++;
		//假如索引大于最大值，但是滚动条不在最大值，保持索引最大值，滚动条++
		if(selectItem>(menuPoint->MenuProperty->MenuLen-1-menuPoint->MenuProperty->scrollBarLen)&&scrollBar!=menuPoint->MenuProperty->scrollBarLen)
		{
			selectItem = menuPoint->MenuProperty->MenuLen-1-menuPoint->MenuProperty->scrollBarLen;
			scrollBar++;
		}
		//假如索引大于最大值，滚动条在最大值，移动到第一个位置
		else if(selectItem>(menuPoint->MenuProperty->MenuLen-1-menuPoint->MenuProperty->scrollBarLen)&&scrollBar==menuPoint->MenuProperty->scrollBarLen)
		{
			selectItem=0;
			scrollBar =0;
		}
		DisplayRefreash(menuPoint,selectItem,scrollBar);
	}else if(isKeyLeft==1)
	{
		//假如当前菜单的func1不为空，执行相关函数
		if(menuPoint[selectItem+scrollBar].func1!=NULL)
		{
			menuPoint[selectItem+scrollBar].func1();
		}
		isKeyLeft=0;
		DisplayRefreash(menuPoint,selectItem,scrollBar);
	}else if(isKeyRight==1)
	{
		if(selectItem==0 && scrollBar==0 && menuPoint[selectItem].fatherMenu!=NULL)//假如索引为零而且父菜单不为空，指向父指针
		{
			menuPoint = menuPoint[selectItem].fatherMenu;
		}
		else if(menuPoint[selectItem+scrollBar].childrenMenu!=NULL)//假如该索引子菜单页不为空，指向子菜单
		{
			if(menuPoint[selectItem+scrollBar].func2!=NULL)//假如当前菜单的func2不为空，执行相关函数
			{
				menuPoint[selectItem+scrollBar].func2();
			}
			menuPoint = menuPoint[selectItem+scrollBar].childrenMenu;
			selectItem = 0;
		}
		else if(menuPoint[selectItem+scrollBar].func2!=NULL)//假如当前菜单的func2不为空，执行相关函数
		{
			menuPoint[selectItem+scrollBar].func2();
		}
		isKeyRight=0;
		DisplayRefreash(menuPoint,selectItem,scrollBar);
	}else if(isKeyOk==1)
	{
		isKeyOk=0;
		DisplayRefreash(menuPoint,selectItem,scrollBar);
	}
	GuiDataDisplayRefresh();
}
```

## 项目参考

[STM32F1多级菜单代码讲解_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1zy4y1m7xA)

