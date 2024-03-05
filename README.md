## 基于线程池和libevent的FTP服务端
- 技术栈： C++、libevent/socket、线程池、win/Linux跨平台、工厂模式
- 项目简介：实现了一个基于线程池和libevent的FTP服务器
- 项目内容：使用std::thread库实现了线程池的基本框架，用单例模式创建线程池，实现自定义任务和任务的线程分发。基于 libevent和线程池，用工厂模式创建根据FTP协议的各钟类型任务，分发给线程处理，实现了FTP服务器的登录、目录获取和切换、文件上传和下载功能。



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

