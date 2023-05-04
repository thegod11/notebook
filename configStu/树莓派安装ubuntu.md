- 拔出tf卡，插入到读卡器

- 使用SD Card Formatter工具格式化

- 去官网https://www.raspberrypi.com/software，往下滑，找到Raspberry Pi Imager，找到适合你电脑版本的文件下载

- 选择Raspberry Pi Os Lite(64-bit)进行烧录，并配置好ssh登录与wifi连接

- whereis python 查看python位置

- uname -a 查看树莓派操作系统版本信息

- [pip · PyPI](https://pypi.org/project/pip/#files)安装pip

  - 拿pip9.0.1举例，

  - #解压压缩包
    tar xf pip-9.0.1.tar.gz

    #进入pip-9.0.1目录
    cd pip-9.0.1

    #进行安装
    python setup.py install

  - #下载setuptools
    wget https://pypi.python.org/packages/28/4f/889339f38da415e49cff15b21ab27becbf4c017c79fbfdeca663f5b33b36/setuptools-36.4.0.zip

    unzip setuptools-36.4.0.zip

    cd setuptools-36.4.0

    python setup.py build

    python setup.py install

- 安装torch：https://torch.kmtea.eu/whl/stable.html
  - 使用pip install future -i https://pypi.tuna.tsinghua.edu.cn/simple
  - 导入torch出现module compiled against API version 0xe but this version of numpy is 0xd -- > pip install -U numpy 升级numpy
  - print(torch.distributed.is_available())
  - print(torch.distributed.is_gloo_available())  测试全为True
  
- 运行run_server.py出现libGL.so.1: cannot open shared object file: No such file or directory

  - 解决：pip install opencv-python-headless

- http状态码 

  - 2xx 表示成功
  - 3xx 重定向，需要进一步操作
  - 4xx 是客户端请求的问题
  - 5xx 是服务器的问题