## 基于线程池和libevent的FTP服务端



### 代码结构

主程序

`main.cpp ` 

命令工厂

`XFtpFactory.class` `单例`

`XFtpUSER.class ： XFtpTask`

`XFtpPORT.class ： XFtpTask`

`XFtpLIST.class ： XFtpTask`

`XFtpRETR.class ： XFtpTask`

`XFtpSTOR.class ： XFtpTask`

命令任务

`XFtpTask.class : XTask`

`XFtpServerCMD.class : XFtpTask` 

线程池(基于libevent)

`XTask.h`

`XThread.class`

`XThreadPool.class` `单例`

主要的技术栈：线程池、libevent  工厂设计模式  单例设计模式  多线程  网络编程

文件服务器  FTP协议



### 代码流程

1.线程池初始化为10个线程

2.使用`evconnlistener_new_bind`监听21端口

3.有用户连接通过`XFtpFactory::CreateTask`创建一个任务，这个任务注册了`USER,PORT,PWD,LIST,CWD,CDUP,RETR,STOR` 命令，然后把这个任务丢给线程池处理

```c++
XTask *XFtpFactory::CreateTask() {

  testout("At XFtpFactory::CreateTask");

  XFtpServerCMD *x = new XFtpServerCMD();

  x->Reg("USER", new XFtpUSER());

  x->Reg("PORT", new XFtpPORT());

  XFtpTask *list = new XFtpLIST();

  x->Reg("PWD", list);

  x->Reg("LIST", list);

  x->Reg("CWD", list);

  x->Reg("CDUP", list);

  x->Reg("RETR", new XFtpRETR());

  x->Reg("STOR", new XFtpSTOR());

  

  return x;

}
```

