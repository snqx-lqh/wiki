
以下笔记来自视频：https://www.bilibili.com/video/BV1Ay4y1W7xd/

# QML常用组件
<a name="c0cb9ba0"></a>
## 可视和不可视元素

不可视：如Column垂直分布器

可视：如Button、Label等元素

元素使用示例

```javascript
Button{
    id:btn
    text: "按钮"
}
```

元素使用例程可查看手册搜索Qt Quick Controls QML Types

<a name="c325c145"></a>
## 信号和槽操作

例如button，按键按下的槽函数，每个控件的元素改变也可以发出一个信号，有对应的槽处理函数。

```javascript
Button{
    text: "click"
	//元素改变时的槽处理函数
    onTextChanged: {
    	console.log("hello")
    }
    //按键按下时的槽处理函数
    onClicked: {
    	console.log("btn click")
    	text = "hello"
    }
}
```

<a name="54fb675d"></a>
## 方法的建立

建立方法，function无变量数据类型

```javascript
Button{
    text: "click"

    onClicked: {
        console.log(add(12,39))
    }

    function add(num1,num2){
        return num1 + num2;
    }
}
```

<a name="9f3df106"></a>
## 导入js文件中的方法

新建一个js文件,做一个类构造函数

```javascript
function OperatorNumber(number)
{
    this.number = number;

    this.add = function(num){
        return number + num;
    }

    this.sub = function(num){
        return number - num;
    }
}
```

导入文件后，做一个简单的使用

```javascript
Button{
    text: "click"

    onClicked: {
        let oper = new BJ.OperatorNumber(888);

        console.log('add',oper.add(100))
        console.log('sub',oper.sub(100))
    }
}
```

<a name="9518eb67"></a>
## XYZ属性和动态表达式绑定

以父对象为参考的坐标偏移，Z越大图层越高

动态表达式可以随着初始配置元素的变化而变化。

测试鼠标带动Button动

```javascript
MouseArea{
	anchors.fill: parent
	drag.target: btn
}
```

<a name="5aefca55"></a>
## 布局

<a name="d6b66c56"></a>
### 锚点布局

主要是anchors的使用。

先建立一个父对象，然后fill。然后该对象的子对象就可以根据父对象布局

```javascript
Item{
    anchors.fill:parent

    Rectangle{
        anchors.top: parent.top
        //anchors.verticalCenter: parent.top
        width: 100
        height: 100
        color: 'red'
    }
}
```

margin, border ,padding 布局实现

![](https://lqh-tc-1309688396.cos.ap-chongqing.myqcloud.com/img/image-20220519093029082.png#id=Afhs3&originHeight=204&originWidth=216&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

主要是margin的使用,下面这个例子，主要是rect1实现了一个间隔，整体效果是一个顶部、左部、右部的效果

```javascript
Item{
    anchors.fill:parent

    Rectangle{
        id: rect1
        anchors.fill: parent
        anchors.topMargin: 100
        anchors.leftMargin: 100
        color: 'red'
    }
    Rectangle{
        id: rect2
        anchors.top: parent.top
        anchors.right: parent.right
        anchors.left: parent.left
        height: rect1.anchors.topMargin
        color: 'orange'
    }
    Rectangle{
        id: rect3
        anchors.top: rect2.bottom
        anchors.left: parent.left
        anchors.right: rect1.left
        anchors.bottom: parent.bottom
        color: 'green'
    }
}
```

<a name="4ad30198"></a>
### 行列布局

主要是Row和Column的使用，还有ColumnLayer和RowLayer的使用，大致差不多，Column会将元素进行垂直布局，Row会将元素水平布局

```javascript
Column{
    RadioButton{
        checked: true
        text: '第一项'
    }
        RadioButton{
        text: '第二项'
    }
        RadioButton{
        text: '第三项'
    }
}
Row{
    RadioButton{
        checked: true
        text: '第一项'
    }
    RadioButton{
    	text: '第二项'
    }
    RadioButton{
        text: '第三项'
    }
}
```

<a name="36751864"></a>
### Flow布局

超过该布局设定区域后会自动换行，例如

```javascript
Flow{
    anchors.left: parent.left
    anchors.right: parent.right
    //            flow: Flow.TopToBottom

    Rectangle{
        height: 100
        width: parent.width/2
        color: 'orange'
    }
    Rectangle{
        height: 100
        width: parent.width/2
        color: 'red'
    }
    Rectangle{
        height: 100
        width: parent.width/2
        color: 'green'
    }
}
```

<a name="a8f71195"></a>
### 网格布局

Grid的使用，类似流布局

```javascript
Grid {
    id: grid
    columns: 3 //选择为3列
    spacing: 2 //元素间间隔为2

    Text { text: "Three"; font.bold: true; }
    Text { text: "words"; color: "red" }
    Text { text: "in"; font.underline: true }
    Text { text: "a"; font.pixelSize: 20 }
    Text { text: "row"; font.strikeout: true }
}
```

<a name="5d961e79"></a>
## 基本类型

<a name="2cb472ff"></a>
### 初始化

组件初始化运行

```javascript
Component.onCompleted:{
    parent.number = parent.width
}
```

<a name="173ec062"></a>
### 基本的数据类型、别名、子元素

bool、double、enumeration、int、list、real、string、url、var

基本使用

```javascript
property int number: parent.width
```

起别名,给一些对象起别名

```javascript
property alias WindowWidth: window.width
```

子元素children，children只是可以读一级元素的参数

```javascript
Column{
    id:column
    Rectangle{
        id:rect1
        width: 100
        height: 100
        color: 'black'
    }
    Rectangle{
        id:rect2
        width: 100
        height: 100
        color: 'green'
    }
    Component.onCompleted: {
    	console.log(column.children.length)
        console.log(column.children[0].color)
        console.log(column.children[1].width)
    }
}
```

<a name="31dec312"></a>
## 基础控件

<a name="CheckBox"></a>
### CheckBox

基础使用

```javascript
CheckBox {
    checked: true
    text: qsTr("First")
    onCheckedChanged: {
        if(checked)
        	console.log('First')
    }
}
```

<a name="Calendar"></a>
### Calendar

该控件在QtQuick.Controls 1.4中所以需要import QtQuick.Controls 1.4 as Ctr1

基础使用

```javascript
Column{
    Ctr1.Calendar{
        minimumDate: new Data(2017,0,1)
        maximumDate: new Data()

        onSelectedDateChanged: {
            let strData = Qt.formatDateTime(selectedDate,"yyyy-MM-dd hh:mm:ss")
            console.log(strData)
        }
    }
}
```

<a name="ComboBox"></a>
### ComboBox

当model文件为数组字符串类型时

```javascript
ComboBox{
   id:combo
   model: ['first','second','third']

   onActivated: {
       console.log(index)
       console.log(textAt(index))
       console.log(model[index])
   }
}
```

当model文件为项目组时,ListModel建立一个组，ListElement就是具体的项目类型，注意加一个textRole确定Combo显示数据的类型

```javascript
ComboBox{
   id:combo
   textRole: 'text'
   model: ListModel{
       ListElement{
           text:'first'
           value:100
       }
       ListElement{
           text:'second'
           value:200
       }
       ListElement{
           text:'third'
           value:300
       }
   }

   onActivated: {
       let obj = model.get(index)
       console.log(obj.text,obj.value)
   }
}
```

<a name="ApplicationWindow"></a>
## ApplicationWindow

该窗口主要包含四个常用空间，菜单栏、工具条、操作空间、状态栏

菜单栏简单使用，MenuSeparator为分割符，加上&后会在名称最前面的那个字母加一个下划线,Ttiggered为点击该项后的功能

```javascript
MenuBar {
    Menu {
        title: qsTr("&File")
        Action { text: qsTr("&New...") }
        Action { text: qsTr("&Open...") }
        Action { text: qsTr("&Save") }
        Action { text: qsTr("Save &As...") }
        MenuSeparator { }
        Action {
            text: qsTr("&Quit")
            onTriggered: {
                close()
            }
        }
	}
}
```

状态栏的使用,注意StatusBar为1.4版本中的

```javascript
footer: Ctr1.StatusBar{
    Row{
        Label{
            text: 'Menu ' + ApplicationWindow.menuBar.count + ' count'
            font.italic: true
        }
    }
}
```

工具栏的使用,和菜单栏使用基本差不多

```javascript
header: ToolBar{
    Row{
        ToolButton{
            text: '打开'
        }
        ToolButton{
            text: '退出'
            onClicked: {
                close()
            }
        }
    }
}
```

<a name="259e698b"></a>
## 附加属性

可以附加到其他组件上面去的属性，如

```javascript
Component.onCompleted:{
}
Component.onDestruction: {
    console.log('关闭')
}
```

<a name="ListView"></a>
## ListView

<a name="c86c7a2c"></a>
### 基础生成

model为项目值，delegate项目代理，像对他的属性做一个添加修改

```javascript
ListView{
    id:lqhList
    anchors.fill: parent
    model: ['第一个项目','第二个项目']

    delegate: ItemDelegate {
        width:lqhList.width
        text: modelData

        background:Rectangle{
            color: getColor()
            function getColor(){
                var colorObject = BJ.getColorRandom()
                return Qt.rgba(colorObject.red,colorObject.green,colorObject.blue)
            }
        }
        onClicked: console.log(modelData)
    }
    ScrollBar.vertical: ScrollBar{}
}
```

<a name="d0eeaa0c"></a>
### 封装颜色生成器

在js文件中封装一个颜色生成器

```javascript
function getRangeRandom(max)
{
    return Math.floor(Math.random() * max)
}

function getColorRandom()
{
    var red = getRangeRandom(256)
    var green = getRangeRandom(256)
    var blue = getRangeRandom(256)
    return {red:red/255,green:green/255,blue:blue/255}
}

function getColorBulider(rgbFunction){
    return function(){
        var color = getColorRandom()
        return rgbFunction(color.red,color.green,color.blue)
    }
}
```

然后再在QML文件中，做一个控制器

```javascript
property var myColorBulider: Bj.getColorBulider(Qt.rgba)

ListView{
    id:lqhList
    anchors.fill: parent
    model: ['第一个项目','第二个项目']

    delegate: ItemDelegate {
        width:lqhList.width
        text: modelData

        background:Rectangle{
            color: myColorBulider()
        }
        onClicked: console.log(modelData)
    }
    ScrollBar.vertical: ScrollBar{}

}
```

<a name="f2f8c781"></a>
### 添加组件

```javascript
Component.onCompleted: {
    var modelArray = lqhList.model
    modelArray.push('WangWu')
    lqhList.model = modelArray
}
```

<a name="59b4ffa6"></a>
### Index的使用

该实例为查看选中状态的Index和实际的Index是否相同

```javascript
ListView{
    id:lqhList
    anchors.fill: parent
    model:[
        {
            name:'ZhangSan',
            age : 26
        },
        {
            name:'LiSi',
            age : 36
        }
    ]
    delegate: ItemDelegate {
        width:lqhList.width
        text: modelData.name + modelData.age + (lqhList.currentIndex === index ? ' Select':'')

        background:Rectangle{
            color: myColorBulider()
        }
        onClicked: {
            console.log(JSON.stringify(modelData))
            lqhList.currentIndex = index;
        }
    }
}
```

<a name="ListModel"></a>
### ListModel

在model中使用

简单使用

```javascript
ListView{
    id:lqhList
    anchors.fill:parent

    model: ListModel{
        ListElement{
            name:'zhangsan'
            age:67
        }
        ListElement{
            name:'lisi'
            age:88
        }
    }

    delegate: Rectangle{
        width:lqhList.width
        height: lqhList.height*0.1
        color: lqhItem.myColorBulider()

        Text {
            anchors.fill: parent
            text: qsTr("name "+name+" age "+age)
            font.pointSize: 16
            verticalAlignment: Text.AlignVCenter
            horizontalAlignment: Text.AlignHCenter
        }
    }
}
```

初始化时添加model

```javascript
Component.onCompleted: {
    lqhList.model.append({'name':'wangwu','age':77})
    myListModel.append({'name':'wemz','age':22})
}
```

MouseArea的基础使用

```javascript
 MouseArea{
     anchors.fill: parent

     onClicked: {
         var data = myListModel.get(index)
         console.log(data.name,data.age)
         lqhList.currentIndex = index
     }
 }
```

<a name="section"></a>
### section

先选择列表对象中的一个数据，然后可直接可以对选择对象做一个代理

```javascript
section.property: 'group'
section.delegate: Rectangle{
    width:lqhList.width
    height: 30
    color: lqhItem.myColorBulider()
    Text {
        anchors.centerIn: parent
        text: section
        font.pointSize: 20
    }
}
```

<a name="259e698b-1"></a>
### 附加属性

例如

```javascript
color: ListView.isCurrentItem ? "blue" : "red"
```

<a name="Timer"></a>
## Timer

做一个定时器

```javascript
Timer{
    interval:1000//定时时间
    repeat:true//是否重复
    running:true//是否运行
    triggeredOnStart:true//开启前触发一次
    onTriggered: {
        parent.text = Qt.formatDateTime(new Date(),'yyyy-MM-dd hh:mm:ss')
    }
}
```

<a name="Component"></a>
## Component

先建立一个Component组件，给他一个ID，这里的示例给的ID是dateLabelTime

```javascript
Component.onCompleted: {
    for(var i=0; i < 10 ; i++)
    {
        dateLabelTime.createObject(this,{text:'lqh Example'})
        console.log('建立成功')
    }
}
```

<a name="ce31e5bb"></a>
## 自定义一个控件

建立一个自定义控件名相同的qml文件，然后将上一个示例中Component的内容写法加到该文件中

然后文件中直接调用该控件就可以了

```javascript
LqhDateTimeLabel{
    text: 'LQH'
    colorBulider: lqhItem.myColorBulider
}
```

<a name="70af5cf6"></a>
## JS异步加载（失败）

```javascript
function createDateTimeLabelToColumn(nums, cmp, date){
    if(cmp && (cmp.status === Component.Ready)){
        for(var i = 0;i < nums; i++)
            cmp.createObject(this,date)
    }
}

Component.onCompleted: {
    var cmp = Qt.createComponent('./LqhDateTimeLabel.qml',Component.PreferSynchronous,lqhItem)
    createDateTimeLabelToColumn(5,cmp,{colorBuilder:lqhItem.myColorBulider})
}
```

<a name="Loader"></a>
## Loader

普通加载组件

```javascript
Loader{
    sourceComponent: Component{
        LqhDateTimeLabel{
            text: 'lqh'
        }
    }
}
```

加载带参数的路径文件

```javascript
Loader{id:loader}

Component.onCompleted: { 				                                    					loader.setSource('./LqhDateTimeLabel.qml'{text:'hello',colorBulider:myCo
lorBulider})
}
```

<a name="MouseArea"></a>
## MouseArea

mouse.x，mouse.y可以获取鼠标参数

propageteComposedEvents:true鼠标穿透属性

mouse.accepted = false鼠标穿透属性

```javascript
Rectangle{
    width: 100
    height: 100
    color: 'orange'

    MouseArea{
        anchors.fill:parent

        onClicked: {
            console.log(mouse.x,mouse.y)
        }
    }

    Text {
        id: myText
        anchors.centerIn:parent
        text: qsTr("Hello")

        MouseArea{
            anchors.fill:parent
            propagateComposedEvents: true
            onClicked: {
                console.log('Hello')
                mouse.accepted = false
            }
        }
    }
}
```

<a name="StackView"></a>
## StackView

实现页面跳转，一个压栈和出栈的操作

```javascript
Component{
    id:rect1
    Rectangle{
        anchors.fill:parent
        MouseArea{
            anchors.fill: parent
            onClicked: {
                stack.pop()
            }
        }
    }
}

StackView{
    id:stack
    anchors.fill:parent

    initialItem: Component{
        Rectangle{
            anchors.fill: parent
            color: 'red'

            MouseArea{
                anchors.fill: parent

                onClicked: {
                    stack.push(rect1,{color:lqhItem.myColorBulider()})
                }
            }
        }
    }
}
```

<a name="StackLayout"></a>
## StackLayout

栈布局，可以提前设定几个空间，然后通过改变他的currentIndex来改变现在的空间

示例代码

```javascript
Row{
    id:btn
    Button{
        text: 'Home'
        onClicked: {
            stack.currentIndex = 0
        }
    }
    Button{
        text: 'Text'
        onClicked: {
            stack.currentIndex = 1
        }
    }
}
StackLayout{
    id:stack
    anchors.top: btn.bottom
    anchors.left: parent.left
    anchors.right: parent.right
    anchors.bottom: parent.bottom
    currentIndex: 0
    Rectangle{
        anchors.fill: parent
        color: 'red'
    }
    Rectangle{
        anchors.fill: parent
        color: 'green'
    }

}
```

对上面的代码做一个简单封装，例如按钮段

```javascript
Row{
    id:header
    Component{
        id:headerButton
        Button{
            property string menuText
            property var    layout
            property int    index

            text: menuText
            onClicked: layout.currentIndex = index
        }
    }

    Component.onCompleted: {
        var menus = ['Home','Test']
        menus.forEach(function(item,index){
          headerButton.createObject(header{layout:stack,menuText:item,index:index})
        })
    }
}
```

对stackLayout做一个简单封装

```javascript
StackLayout{
    id:stack
    anchors.top: header.bottom
    anchors.left: parent.left
    anchors.right: parent.right
    anchors.bottom: parent.bottom
    currentIndex: 0

    Component{
        id: pageComponent
        Rectangle{
        }
    }
    Component.onCompleted: {
        for(var i=0;i<2;i++){
            pageComponent.createObject(stack,{color: lqhItem.myColorBulider()})
        }
    }
}
```

<a name="cd4610e5"></a>
## Repeater中继器

在其中创建的对象父对象为中继器上一级

```javascript
Row{
    objectName:'lqh obj'
    Repeater{
        model:3
        Button{
            text: index+''
            onClicked: {
                console.log(parent.objectName)
            }
        }
    }
}
```

<a name="3fe2b7c6"></a>
## SwipeView和TabBar

SwipeView是可以滑动的视图控件，常和PageIndicator共用

```javascript
TabBar{
    id:headerButton
    anchors.left: parent.left
    anchors.right: parent.right
    currentIndex: view.currentIndex
    Repeater{
        model:lqhItem.page
        TabButton{
            text: modelData
            onClicked: {
                view.currentIndex = index
            }
        }
    }
}
SwipeView{
    id: view
    currentIndex: 1
    anchors.left: parent.left
    anchors.right: parent.right
    anchors.top: headerButton.bottom
    anchors.bottom: parent.bottom

    Repeater{
        model:lqhItem.page.length
        Rectangle {
            color: lqhItem.myColorBulider()
        }
    }
}
PageIndicator {
    id: indicator

    count: view.count
    currentIndex: view.currentIndex

    anchors.bottom: view.bottom
    anchors.horizontalCenter: parent.horizontalCenter
}
```

<a name="c27512c8"></a>
## Button的用户自定义

在帮助文档中，找Qt 5.12<br />Qt Quick Controls<br />Customizing Qt Quick Controls 2

示例代码,记得要给按钮加ID才能自己修改

```javascript
Button {
    id: control
    text: qsTr("Button")

    contentItem: Text {
        text: control.text
        font: control.font
        opacity: enabled ? 1.0 : 0.3
        color: control.down ? "#17a81a" : "#21be2b"
        horizontalAlignment: Text.AlignHCenter
        verticalAlignment: Text.AlignVCenter
        elide: Text.ElideRight
    }

    background: Rectangle {
        implicitWidth: 100
        implicitHeight: 40
        opacity: enabled ? 1 : 0.3
        color: control.down ? 'red':'white'
        border.color: control.down ? "#17a81a" : "#21be2b"
        border.width: 1
        radius: 2
    }
}
```

<a name="4a06b950"></a>
## State状态

给一个对象定义一个状态实现，主要是要给状态定义一个目标

```javascript
Rectangle{
    id:rect
    implicitHeight: 200
    implicitWidth: 200
    color: 'red'

    states: State {
        name: "myState"
        PropertyChanges {
            target: rect
            color: lqhItem.myColorBulider()
        }
    }
    MouseArea{
        anchors.fill:parent
        onClicked: {
            rect.state = rect.state === 'myState'?' ':'myState'
        }
    }
}
```

还可以使用when来触发状态

```javascript
states: State {
    name: "myState"
    when:myMouse.pressed
    PropertyChanges {
        target: rect
        color: lqhItem.myColorBulider()
    }
}
```

<a name="2c7971a0"></a>
## 状态机的实现

其实就是不同的状态下，控件或者其他元素有着不同的实现

```javascript
SwipeView{
    id: view
    currentIndex: 1
    anchors.top: parent.top
    anchors.bottom: btn.top
    anchors.left: parent.left
    anchors.right: parent.right
    Rectangle {
        id: firstPage
        color: 'red'
    }
    Rectangle {
        id: secondPage
        color: 'blue'
    }
    Rectangle {
        id: thirdPage
        color: 'green'
    }
}
PageIndicator {
    id: indicator

    count: view.count
    currentIndex: view.currentIndex

    anchors.bottom: view.bottom
    anchors.horizontalCenter: parent.horizontalCenter
}
Button{
    id:btn
    text: 'button'
    anchors.bottom: parent.bottom
    anchors.horizontalCenter: parent.horizontalCenter

    states:[
        State {
        when: view.currentIndex === 0
        PropertyChanges {
        target: btn
        text: 'home'
        }
},
    State {
        when: view.currentIndex === 1
        PropertyChanges {
            target: btn
            text: 'help'
        }
    },
        State {
            when: view.currentIndex === 2
            PropertyChanges {
                target: btn
                text: 'about'
            }
        }
]


}
```

<a name="84102831"></a>
## 对话框

创建一个对话框,注意需要import QtQuick.Dialogs 1.2 as Old

```javascript
Button{
    text: 'click'
    onClicked: {
        myDialog.open()
    }
}

Old.Dialog{
    id:myDialog
}
```

<a name="Behavior"></a>
## Behavior

动画效果，选择需要改变的元素对象，duration，变化时间

```javascript
Behavior on width {
    NumberAnimation{
        duration: 1000
    }
}

MouseArea{
    anchors.fill: parent
    onClicked: {
        if(rect.width === 100)
        {
            rect.width = 300
        }else
        {
            rect.width = 100
        }
    }
}
```

<a name="ebe08230"></a>
## 动画顺序执行和并行

顺序执行,组件为SequentialAnimation

```javascript
SequentialAnimation{
    running: true
    loops: Animation.Infinite

    NumberAnimation{
        property: 'x'
        target: rect
        to: 300
        duration: 1000
    }

    NumberAnimation{
        property: 'x'
        target: rect
        to: 0
        duration: 1000
    }

}
```

并行执行，两个动画任务将同时执行

```javascript
ParallelAnimation{
    running: true
    NumberAnimation{
        property: 'x'
        target: rect
        to: 300
        duration: 1000
    }
    ColorAnimation {
        property: 'color'
        target: rect
        to: lqhItem.myColorBulider()
        duration: 1000
    }
}
```

使用状态来过渡

```javascript
transitions: Transition {
    NumberAnimation{
        property: 'x'
        target: rect
        to: 300
        duration: 1000
    }
    ColorAnimation {
        property: 'color'
        target: rect
        to: lqhItem.myColorBulider()
        duration: 1000
    }
}
states: State {
    name: "demo"
    PropertyChanges {
        target: rect
        x: 300
        color:'orange'
    }
}
MouseArea{
    anchors.fill: parent
    onClicked: {
        rect.state = rect.state ? '':'demo'
    }
}
```

<a name="abb38f5f"></a>
# QML和CPP

<a name="3999c52b"></a>
## 从QML中获取数据到CPP

先在main.qml中定义一个Label控件

```javascript
Label{
    objectName: 'labelTest'
    text: '这是一个测试标签'

    function getText(info){
        return text + info
    }
}
```

现在想要在cpp文件中调用，注意头文件

```c
#include <QMetaObject>
#include <QDebug>
```

```c
//获取所有的对象
auto root = engine.rootObjects();
//找到对象中的子对象文件
auto myLabelTest = root.first()->findChild<QObject*>("labelTest");
//定义一个可以获取值的数据类型
QVariant ret;
//调用子对象的方法
QMetaObject::invokeMethod(myLabelTest,"getText",Q_RETURN_ARG(QVariant,ret),Q_ARG(QVariant,"1"));

qDebug()<<ret;
```

<a name="a1bb30f0"></a>
## 从CPP中注册数据到QML

上下文注册，Person类似注册的组件名，注意包含头文件

```c
#include <QQmlContext>
```

```c
engine.rootContext()->setContextProperty("Person",&p1);  

auto context = engine.rootContext();
context->setContextProperty("Person",&p1);
```

注册单例文件

<a name="054fe748"></a>
## 连接QML的信号和CPP的槽

在QML中建立一个Button信号

然后在CPP文件中

```javascript
//获取所有的对象
auto root = engine.rootObjects();
//找到对象中的子对象文件
auto myButton = root.first()->findChild<QObject*>("myBtn");
//连接信号和槽
QObject::connect(myButton,SIGNAL(clicked()),&p1,SLOT(clickDemo()));
```

在QML中建立信号，然后CPP中接收

```javascript
Button{
    property int count: 0
    objectName: 'myBtn'
    text: 'click'
    signal demo_click(int _count)
    onClicked: {
        count++
        if(count>10)
        {
            demo_click(count)
            count = 0
        }
    }
}
```

带参数的信号和槽

```
QObject::connect(myButton,SIGNAL(demo_click(int)),&p1,SLOT(clickCountDemo(int)));
```

<a name="7ee9773a"></a>
## C++的信号在QML中使用

5.12版本，Person是建立的类型名

```javascript
Connections{
    target: Person
    onQmlTest:{
        console.log("shidgigd")
        console.log(age)
    }
}
```

<a name="3cba486b"></a>
## 在QML中注入一个C++类

先创建一个C++类，记得继承QQuickItem才能嵌套，因为有个描点，Q_PROPERTY是将变量可以在QML中调用，int只能调用32位

在改变时，发送信号

```c
class Info: public QObject{
    Q_OBJECT
    Q_PROPERTY(QString name READ getName WRITE setName NOTIFY nameChanged)
    Q_PROPERTY(qint32 age   READ getAge  WRITE setAge  NOTIFY ageChanged)
public:

    Info();
    void setName(QString _name);
    QString getName();
    void setAge(qint32 _age);
    qint32 getAge();

signals:
    void nameChanged();
    void ageChanged();

private:
    QString name;
    qint32  age;

};
```

然后注入类型

```
//import lqhModule 1.0
qmlRegisterType<Info>("lqhModule",1,0,"MyInfo");
```

然后在QML中建立一个组件,便可以直接使用了

```javascript
MyInfo{
    id:firstInfo
    name: 'lqh'
    age: 55
}
```

<a name="071bd131"></a>
## 元数据注入（失败）64

QML_ELEMENT

<a name="f5af2335"></a>
## 函数的附加属性(失败）65

可注册但是不可以有实例信息，可使用附加信息，但是不允许创建

```
qmlRegisterUncreatableType<person>("lqhModule",1,0,"MyInfo","lqh:not allowed");
```

<a name="ca79f5e0"></a>
## 画家API继承

paint

<a name="f687602e"></a>
## C++中创建列表类 68需要重看

<a name="Model"></a>
# Model

c++中建立模型然后QML中调用

如何自建一个listModel和列表项

<a name="b7a177ad"></a>
# Web 暂时跳过

<a name="87893286"></a>
# Material样式

材质包

需要包含

```
import QtQuick.Controls.Material 2.12
```

在cpp中

```
#include <QQuickStyle>
```

main函数中

```
QQuickStyle::setStyle("Material");
```

在pro文件中

```
QT += quick quickcontrols2
```

<a name="408598cd"></a>
# Canvas元素

自己绘制,requestPaint()是重新画图

```javascript
Column{
    Button{
        text: 'click'
        onClicked: {
            myCanvas.rectColor = 'green'
        }
    }
    Canvas{
        id:myCanvas
        width: 200
        height: 200
        property color rectColor: Qt.rgba(1, 0, 0, 1)
        onPaint: {
            var ctx = getContext("2d");
            ctx.fillStyle = rectColor
            ctx.fillRect(0, 0, width, height);
        }
        onRectColorChanged:{
            requestPaint()
        }
    }
}
```

<a name="TableView"></a>
# TableView

简单示例,记得包含以下文件

```javascript
import QtQuick.Controls 1.4
import QtQuick.Controls.Styles 1.4
```

```javascript
TableView{
    anchors.fill: parent
    model:ListModel{
        ListElement{
            title: 'shuihu'
            author:'lisi'
        }
        ListElement{
            title: 'sanguo'
            author:'zhangshan'
        }
    }

    TableViewColumn {
        role: "title"
        title: "Title"
        width: 100
    }
    TableViewColumn {
        role: "author"
        title: "Author"
        width: 200
    }
    style: TabViewStyle{
        rowDelegate:Rectangle{

        }
        itemDelegate:Rectangle{

        }
    }
}
```

<a name="0b6bc039"></a>
# TreeView （失败）

卡在实例注册

<a name="ed351dad"></a>
# 第三方库涛哥qmake
