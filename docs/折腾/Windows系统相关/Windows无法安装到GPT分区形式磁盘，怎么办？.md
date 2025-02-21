
在系统提示无法安装的那一步，按住“shift+f10”，呼出“cmd”命令符

输入：diskpart，回车

进入diskpart。

输入：list disk，回车

显示磁盘信息

输入：select disk 0，回车

选择第0个磁盘（电脑的硬盘编号是从0开始的）

输入：clean，回车

删除磁盘分区&格式化

输入：convert mbr，回车

将当前磁盘分区设置为Mbr形式