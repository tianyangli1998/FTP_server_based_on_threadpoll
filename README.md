# FTP_server_based_on_threadpoll
基于线程池的FTP服务器    技术栈： C++、libevent/socket、线程池、win/Linux跨平台、工厂模式
项目简介：实现了一个基于线程池和libevent的FTP服务器
项目内容：使用std::thread库实现了线程池的基本框架，用单例模式创建线程池，实现自定义任务和任务的线程分发。基于 libevent和线程池，用工厂模式创建根据FTP协议的各钟类型任务，分发给线程处理，实现了FTP服务器的登录、目录获取和切换、文件上传和下载功能。
