1，新建一个仓库，在仓库的setting设置里，general向下翻找到danger zone就有
2，打开git，输入ssh-keygen -o获得ssh公钥，cat .ssh/id_rsa.pub 可以看到公钥
pwd 命令查看当前所在路径
3，在github的总的个人setting中，找到ssh and gpg keys，添加一个ssh ley
4，使用git clone git@github.com:thegod11/notebook.git（ssh地址）
5，cd 到仓库目录下，使用mkdir进行创建新的目录，touch 新文件名创建新的文件
6，将notebook文件夹直接拖进typora即可操作
7，git status查看当前的仓库状态，git add .   ---> git commit -m "标记信息" ----> git push 增加所有新增加的文件
8，每次上传前先进入本地仓库文件中，再git pull（上传记得开VPN，而且多试几次）