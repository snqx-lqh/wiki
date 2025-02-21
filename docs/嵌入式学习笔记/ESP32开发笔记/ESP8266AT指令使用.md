
```C
AT   测试
AT+CWMODE?  查看模式 Station/SoftAP/Station+SoftAP
AT+CWMODE=1 设置成为Station
AT+CWLAP  列出周围wifi  //写代码不要
AT+CWJAP="5A77C4","11223344" 连接到一个wifi
AT+CIPSTA?  查看本机IP和服务端IP
AT+CWQAP  断开与WIFI AP的连接


AT+CIPMUX? 查询传输模式 1: Wi-Fi 透传模式, 只有在TCP单连接模式
AT+CIPMODE=1 设置成透传
AT+CIPSTART="TCP","192.168.137.123",8888  TCP连接
AT+CIPSEND 进入透传
+++  退出透传 这个不发送新行
```

## TCP连接

### STA模式

```C
AT\r\n                            //测试
AT+CWMODE?\r\n                    //查看模式 Station/SoftAP/Station+SoftAP
AT+CWMODE=1\r\n                   //设置成为Station
AT+CWLAP\r\n                      //列出周围wifi  //写代码不要
AT+CWJAP="5A77C4","11223344"\r\n  //连接到一个wifi
AT+CIPSTA?\r\n                    //查看本机IP和服务端IP
AT+CWQAP\r\n                      //断开与WIFI AP的连接

AT+CIPMUX?\r\n                        //查询传输模式 1: Wi-Fi 透传模式, 只有在TCP单连接模式
AT+CIPMODE=1\r\n                      //设置成透传
AT+CIPSTART="TCP","192.168.137.123",8888\r\n  //TCP连接
AT+CIPSEND\r\n                        //进入透传
+++                               //退出透传 这个不发送新行
```