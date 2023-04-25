# 列出所有的虚拟环境 ： conda env list
# 创建最新的3.6版本的，名为env_name的虚拟环境 ：conda create -n py_39 python=3.9
# 激活名为env_name的虚拟环境 ： conda activate env_name
# 删除名为env_name的虚拟环境 ： conda remove -n env_name --all

#windows的cmd常用命令
dir是相当于list
切换路径直接d:
cls是清屏
进入E:/Anaconda3/envs/py39/Scripts中，pip list可以列出所有包

#pip 设置
pip3 config set global.cache-dir "no-cache-dir"           无缓存设置
pip3 config set global.index-url "https://mirrors.aliyun.com/pypi/simple"  配置阿里源
pip install pip -U  #升级pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple   配置清华源
pip3 config set global.timeout 8      8s内未完成就中断
pip3 config set install.trusted-host "mirrors.aliyun.com"   设置可信主机
pip cache purge：从 pip 的缓存中清除所有车轮文件

#装GPU版本的torch：
1. nvidia-smi查看显卡与支持的最高cuda版本
2.对应好torch版本与cuda版本，torchvision版本
3.到cuda官网下载cuda toolkeit， https://developer.nvidia.com/cuda-toolkit-archive  有历史版本
4. 安装cuda时选择自定义，CUDA中的Nsight VSE和Visual Studio Integration取消勾选，以及Other，Driver components也取消
5. nvcc -V查看是否成功
6. https://download.pytorch.org/whl/torch_stable.html 或是 pypi  ： 找到cu116，torch1.13.1， cp39 ---》torchvision0.14
7. 将它们都放到E:/Anaconda3/envs/py39/Scripts中， pip install 按下tab补全即可
8.检测时需要activate虚拟环境，再import torch， print(torch.cuda.is_available())
（默认外部python为base环境下的）

#mmlab
https://download.openmmlab.com/mmcv/dist/cu116/torch1.13.0/index.html
labelme做数据标注，github的项目里、example上有json转voc格式（https://blog.csdn.net/qq_21386397/article/details/123656072）的代码，改一下配置（在运行时需要加上的）就可以了

#unet++项目
1. parser相当于配置文件，可以读取里面的内容
2. 数据增强用的albumentations中的函数，图片与mask一起变换（已安装）
3. transpose是维度顺序的变换0,1,2
4. Dataset自己定义，只要有__getitem__(self, idx)就可以放到torch的dataloader里用
5. 不同维度的特征图如何拼接？对小的呢个进行上采样即可，torch.cat([, , , , ], 1)
6. 最终结果输出时不是fc，还是一个卷积层，类别数 * h * w

#docker安装
1.toolbox已经弃用，安装desktop版本
2.docker默认的安装位置是在："C:\Program Files\Docker"
  数据位置，包括镜像位置(wsl2)及docker桌面的位置是在：C:\Users\Administrator\AppData\Local\Docker
3. 默认使用wsl2引擎，（https://www.cnblogs.com/alunzuishuai/p/16344884.html）更新程序放在了阿里云盘，
4.  更改镜像存储位置： https://blog.csdn.net/qq_36835255/article/details/124908795
5. docker version 查看版本
   dockerhub（中心）可以下载各种东西（python库，，，，），但是速度慢，用阿里云镜像加速器：https://h8qk3iwr.mirror.aliyuncs.com，界面下面有配置教程！
   docker info 显示的warnning可以忽视
6. 启动docker后，在cmd里直接输入命令就行： 
	docker pull hello-world ： 拉取docker hub中的镜像, 
	docker images ：查看镜像 ， 
	docker run -it hello-world ：进入环境， 进入环境后，exit可以关闭运行
	删除容器： docker rm 容器id ----》删除镜像：docker rmi imageId
	cmd中docker ps列出所有在运行的，docker stop (ID) 关闭运行
 	进入运行的容器：docker exec -it 容器name/id
7. 上传文件
	在文件目录下，利用git bash here打开，输入：docker cp 当前文件目录 （ID）：目标文件目录
8. 提交镜像到阿里云仓库，按照镜像仓库页面的提示就行，去掉sudo就可以。拉取也有提示。

#sliming
1. run config ？sr s  ,args添加变量指明dest，可以用s与sr，args = parser.parser_args()
2. argparse配置文件，可以指定type，名称一定要加--！
3. Normalize一般选例如ImageNet这种大数据集算出来的均值，标准差，不用自己的！
4. 各层layers用list存储，提前将各卷积核的尺寸放到list中，按照这个list构建网络， list可以用+=运算，每个卷积后面都跟一个BN！
5. L1正则化要在BN之后自己加，BN层的weight.grad.data.add_(导数)，还有一个bias不用加导数！
6. 用gamma的绝对值排序，取一定百分比保存，将gamma小于阈值的设置为0，并保存新的卷积核通道数，重新构建网络，把gamma大于阈值的参数拿过来
    np.argwhere( np的array类型 ) ： 返回非0的数组元素的索引
7. torch.save() 可以保存一个字典类型的数据，可以不只有state_dict

#mobile-unet
1. 用mobilenetv3的bneck进行编码部分
2. os.listdir(a) : 列出a下的所有文件与文件夹, os.glob(..) : 列出目录下所有符合要求的文件路径
3. 一般是继承Dataset,自己写好__getitem__（self, index）, 文件名对应即可
4. np.expand_dims(a, axis=1) 根据axis添加一维
5. PIL专门处理图形的库，Image.open(dir)
6. torch.utils.data.random_split(dataset, [,,,,,]) 将数据按列表数量要求随机分开
7. tqdm（）可以显示进度条， set_description() 动态设置前缀， set_postfix() 动态设置后缀， update(n) 更新多少 : 
	iterable: 可迭代的对象, 在⼿动更新时不需要进⾏设置
	desc: 字符串, 左边进度条描述⽂字
	total: 总的项⽬数
	leave: bool值, 迭代完成后是否保留进度条
	file: 输出指向位置, 默认是终端, ⼀般不需要设置
	ncols: 调整进度条宽度, 默认是根据环境⾃动调节长度, 如果设置为0, 就没有进度条, 只有输出的信息
	unit: 描述处理项⽬的⽂字, 默认是it, 例如: 100 it/s, 处理照⽚的话设置为img ,则为 100 img/s
	unit_scale: ⾃动根据国际标准进⾏项⽬处理速度单位的换算, 例如 100000 it/s >> 100k it/s
	colour: 进度条颜色
8. Conv2d的groups是分组卷积的意思，将输入输出通道分为几组进行普通全连接卷积，一个卷积核的通道数为input_c / groups
9. dice coefficient就等于iou分子分母各加了一个AB交集。
10. 网络输出要再经过softmax或sigmoid变化到0-1之间，然后取大于阈值的为1否则为0。输出的时候转成Image，每个像素要乘255
11.查看tensorboard ：cd  run   ----》 tensorboard --logdir=run_2，在浏览器中打开网址

#deeplabv3+ 
1， PascalVoc数据集可以应用在很多深度学习问题：
	文件夹JPEGImages存所有数据，
	Annotation是xml格式的标注信息，
	ImageSets/Segmentation存txt文件是所有的训练验证的 图像名 集合，
	SegmentationClassAug是所有的mask文件
2， pytorch的checkpoint : 主要用于节省训练模型过程中使用的内存，将模型或其部分的激活值的计算方法保存为一个checkpoint，
		          在前向传播中不保留激活值，而在反向传播中根据checkpoint重新计算一次获得激活值用于反向传播。
	checkpoint_sequential : 上者的基础上，将模型分为几部分分别计算，只需用最大呢一部分的显存即可。
3，loss_func = nn.CrossEntropyLoss()
这个交叉熵损失函数输入的两个参数的shape并不是相同的，一个是各个类别分别的概率的向量（类似于独热码），另一个是位置下标的阿拉伯数字
4，cv2.imread()是numpy格式，Image.open是tensor格式
5，tarfile.open() 打开压缩文件
6，Pytoch中dilation默认等于1，但是实际为不膨胀（没有间隔），也就是说设置dilation = 2时才会真正进行膨胀操作（dilation指隔取的点的坐标之差）
7，Dropout是为了防止过拟合而设置的，nn.Dropout(p = 0.3) # 表示每个神经元有0.3的可能性不被激活
8，类.__dict__[] 存储类的信息
9，OSError: [WinError 1455]页面文件太小，无法完成操作 : https://blog.csdn.net/weixin_46133643/article/details/125042903
10, png由8位表示一个像素，每个像素由调色板中的8位索引表示。jpg是24位。pascal voc的类正好是调色板里的索引值
11，选择gpu 还是 cpu 就是一个设置device， 还有一个model = nn.DataParallel(model)
12，图片在前后端传送，用cv2.imread() -->numpy-->list,  放入json-->读json得字典dict类型，np.asarray(list)  --> cv2.imwrite()
       救命的：https://zhuanlan.zhihu.com/p/27349847?utm_source=weibo&utm_medium=social
13，visdom用法：
   	 ConnectionRefusedError: [WinError 10061] 由于目标计算机积极拒绝，无法连接。
	在anaconda环境中，输入python -m visdom.server，下载慢就去github: https://github.com/fossasia/visdom
	启动后找不到css可以直接到lib/site-packages/visdom/static/index.html删去引用就行
	使用时，需要先python -m visdom.server，然后不要关闭！运行代码即可，注意port指定为默认的

#yolov3
1，由配置文件创建网络结构，就是将文本逐行读入字典中，然后再放到list中。根据这个list创建即可，shortcut 残差层，yolo层得到输出与损失，route 拼接层
2，