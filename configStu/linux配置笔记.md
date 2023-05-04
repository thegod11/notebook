- 用软件ssh，复制链接方便点，比如putty直接右键
  vim 进入后按i或a开始编辑，按下：wq保存后退出
  主机没有ping通虚拟机，是因为开着vpn导致ip不是一个分段（设置虚拟机：桥接网卡）

- 本地安装虚拟机：

1. 下载vbox
2. 下载ubuntu server
3. 修改为清华源（ubuntu安装时会问）
4.  ==ssh连接后配置linux==
    - sudo apt update
      pip install build-(按下tab自动补全)
      pip install python3.8
    - anaconda ：（apt-get install -y wget 可以安装wget）wget+链接（https://repo.anaconda.com/archive/ 中找对对应的） 下载， bash+安装文件， bash进入base环境
    - 要是中途选了no，自己添加环境变量：.bashrc中最后一行加上   `export PATH=/home/anaconda3/bin:$PATH`，然后 source ~/.bashrc
    - conda -V 查看版本
    - source activate py39  启动环境与windows不同
    - nvidia-smi查看显卡与支持的最高cuda版本，[Start Locally | PyTorch](https://pytorch.org/get-started/locally/) 官网选择好对应的设置后，将命令复制过去即可
    - print(torch.cuda.is_available())、print(torch.cuda.device_count())、print(torch.cuda.get_device_name())、print(torch.cuda.get_device_capability(0))
    - visdom在这：/home/ubuntu/anaconda3/envs/py39/lib/python3.9/site-packages/visdom
    - pip install jupyter d2l torch torchvision -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
      git clone https://github.com/d2l-ai/d2l-zh.git
      git clone https://github.com.cnpmjs.org/d2l-ai/d2l-zh.git
      wget https://zh-v2.d2l.ai/d2l-zh.zip 
      pip install unzip 解压文件d2l-zh.zip
5. sudo apt-get install vim , :*q*!表示强制退出,不保存
6. 进入jupyter ： jupyter notebook --port 8080 --ip=192.168.8.109
   进不去可能：（sudo apt-get install -y firewalld firewall-config）
   - 未关闭防火墙 ： systemctl status firewalld.service(查看防火墙开启还是关闭)
                                  systemctl stop firewalld.service(停止防火墙)

-  jupyter使用：

ESC进入命令模式， enter进入编辑模式
编辑模式：
ctrl+回车 运行
shift+enter 运行后再插入一个单元
shift+tab 提示补全
ctrl + / : 注释
ctrl + U : 撤销
一个单元只能输出最后一个
、、、、、、、、、、、、、、、
命令模式：
A 上方插入新单元
B 下方插入新单元
D 删除选中单元

#############

- 用filezilla链接时，要选择用sftp协议，不能快速链接！

- visdom 出现下载scripts的错误时，可以进入minicode/lib/python3.8/site-packages/visdom/server/run_server.py 注释掉download_scripts()

- 没有ifconfig命令：sudo apt-get update  ------>  sudo apt install net-tools

-   本地PyCharm连接云服务器

[Pycharm远程连接云服务器训练模型教程_远程训练模型_ZHW_AI课题组的博客-CSDN博客](https://blog.csdn.net/m0_37758063/article/details/118191228) 

[本地Pycharm连接远程服务器训练模型教程-yolov5为例_耿鬼喝椰汁的博客-CSDN博客](https://blog.csdn.net/m0_57787115/article/details/130273543)

不需要先配置deployment，直接配置ssh的编译器就有文件映射！