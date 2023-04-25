1，
当我们使用GPT2Tokenizer将输入的文本分解成一系列token时，它会将文本分解成一系列单词、标点符号和其他符号。例如，如果我们将文本"I love NLP"输入到GPT2Tokenizer中进行编码，它将返回一个包含四个元素的list，
每个元素代表一个token，如下所示：['I', 'Ġlove', 'ĠN', 'LP']，在这个例子中，GPT2Tokenizer将文本分解成了四个token，分别是"I"、"love"、"NLP"。注意，在第二个token中，
它使用了一个特殊的字符"Ġ"来表示单词之间的空格。这是因为GPT2模型使用了一种称为Byte Pair Encoding(BPE)的技术来编码文本，
它可以将单词分解成子词，并使用特殊的字符来表示单词之间的空格。因此，在使用GPT2Tokenizer时，我们需要注意它返回的token中可能包含特殊字符。

2，https://www.cs.cornell.edu/~cristian/Chameleons_in_imagined_conversations.html 这才是对话数据下载的链接！

Sequence2Sequence模型：
1，标点符号也对应一个Word2index，句子结尾一定要加一个表示 结尾的token。
2，
embedding 在中文中的含义是“嵌入”，在机器学习和自然语言处理中，它通常指将高维稀疏的数据（如单词、词汇、类别等）映射为低维稠密的向量表示。
这个过程就称为嵌入（embedding）。
给定单词的idx数，函数自动将每个idx映射成一个n维的向量。

[1, 2, 3]------> [[ 0.1602, -0.6986, -0.7365, -1.3340, -1.0861],[ 0.2989, -0.4878, -0.2781,  0.1598, -0.0032],[ 0.6769,  1.0761, -0.1813, -0.4315, -0.6086]]

3，
Dropout 就是在每一次前向传播的过程中，以一定的概率随机地将某些神经元的输出置为零，
这些被置为零的神经元在这次前向传播中不会对后面的神经元产生任何影响。
在后面的反向传播中，这些被置为零的神经元也不会接收到任何梯度信号，因此也不会对其它神经元的梯度产生任何影响。

4,
[1, 2, 3] 和 [4, 5]---填充padding--->  [1, 2, 3, 0, 0] 和 [4, 5, 0, 0, 0]  ----pack_padded_sequence---->  [(1, 4), (2, 5), (3)]，batch_size是每一个时间步输入的特征数

5， 
gru的输出为 seq_len * batch_size * 2hidden_size (双向) ， 最终将两个hidden_size相加，encoder的输出为s*b*h

6，
每次gru的输入都是一个时间步，input的维度是 1*b*h

7,
decoder的output_size是（1，voc字典中词的个数）从中用max选择最大的一个即可
decoder的实现，就是结合所有的encoder数据与上个decoder隐层数据去做gru然后进行fc，变成上述的维度即可
8，
先将数据处理成一问一答，然后处理句子的格式（针对标点符号等），删除过长的句子，去除出现频率小于3的单词，为每个单词创建一个索引，索引用作后续的embedding
9，
list(itertools.zip_longest(*l, fillvalue=fillvalue)) ：将同一列数据打包（zip）并且进行填充，相当于进行了行列互换。
10，
训练时，
预处理句子中的词数最高为9 加上EOS_token最长为10，进行zip时相当于是进行了统一长度，并转置： （batch ， len） ————> (10, batch)
embedding : (10, batch, 500) 500为词向量长度
packed ： 将非0的词全拿出来，一行一个，并记录每一个时间步有多少个词
gru输出是对于每一个非0的词计算得到的输出：（非0数即packed的第一维，1000） 1000是因为双向
pad_packed再将0填充回去（10，batch，1000 ）
将双向的相加，得到encoder的输出为（10， bath，500）

decoder的输入初始化为sos_token：（1， batch）， decoder的初始hidden用的是encoder的最终hidden前两层（2，batch,500）
将其输入embeded（1,batch,500），gru使用decoder的输入与上一个的hidden，输出为（1,batch,500）
将gru的输出 与 encoder的输出进行点乘计算得到attention (batch, 1, 10) ,用attention成encoder输出的transpose（0,1），得到（batch，1，500）
将gru输出与encoder使用attention的输出concat ----》 （batch， 1000）
经过全连接转换成（batch，voc中词的总数），进行softmax后，作为decoder的输出。
用target的索引取出对应的概率值p 进行交叉熵即可 -log（p）（p越接近1损失越接近0）
（teaching force模式下decoder的输入为target，否则为上一次的输出中每一个batch选出最大p的呢个值）


torch-model-archiver --model-name seq2seq  --version 1.0  --serialized-file models/9000_checkpoint.tar  --extra-files Voc.py,Import.py,evaluate.py,network/Encoder.py,network/Decoder.py  --handler models/handler.py
docker run --rm -it -p 8080:8080 -p 8081:8081 -p 7070:7070 -p 7071:7071 -v /models:/home/model-server/model-store pytorch/torchserve:latest-gpu torchserve --start --model-store model-store --models my_s2s=seq2seq.mar
curl http://localhost:8081/models
curl http://localhost:8081/models/my_s2s
curl http://localhost:8080/predictions/my_s2s -T hello.txt
curl http://localhost:8082/metrics
torchserve --start --ncs --model-store models --models my_s2s=seq2seq.mar
torchserve --stop

docker ps ------> docker stop id
类的静态函数、类函数、普通函数、全局变量以及一些内置的属性都是放在类__dict__里的, 对象的__dict__中存储了一些self.xxx的一些东西
卸载java去系统应用里卸载！

