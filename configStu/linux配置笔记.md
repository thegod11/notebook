用软件ssh，复制链接方便点，比如putty直接右键
vim 进入后按i或a开始编辑，按下：wq保存后退出
主机没有ping通虚拟机，是因为开着vpn导致ip不是一个分段（设置虚拟机：桥接网卡）


本地安装虚拟机：
1. 下载vbox
2. 下载ubuntu server
3. 修改为清华源（ubuntu安装时会问）
4. ssh登录
    update
    install build-(按下tab自动补全)
    install python3.8
    miniconda ：wget+链接 下载， bash+安装文件， bash进入base环境
    install jupyter d2l torch torchvision -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
    git clone https://github.com/d2l-ai/d2l-zh.git
    git clone https://github.com.cnpmjs.org/d2l-ai/d2l-zh.git
    wget https://zh-v2.d2l.ai/d2l-zh.zip 
    pip install unzip 解压文件d2l-zh.zip

5. 进入jupyter ： jupyter notebook --port 8080 --ip=192.168.8.109
进不去可能：未关闭防火墙 ： systemctl status firewalld.service(查看防火墙开启还是关闭)
                                             systemctl stop firewalld.service(停止防火墙)

 jupyter使用：

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
用filezilla链接时，要选择用sftp协议，不能快速链接！

visdom 出现下载scripts的错误时，可以进入minicode/lib/python3.8/site-packages/visdom/server/run_server.py 注释掉download_scripts()

没有ifconfig命令：sudo apt-get update  ------>  sudo apt install net-tools
    