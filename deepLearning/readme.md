# 深度学习算法 The deeplearning algorithms includes (now):
- 逻辑回归 Logistic Regression   [logisticRegression.py](logisticRegression.py)
- 多层感知机 Multi-Layer Perceptron (MLP) [mlp.py](mlp.py)
- 卷积神经网络 Convolution Neural Network (CNN) [cnn.py](cnn.py)
- 自编码 Denoising Aotoencoder (DA) [da.py](da.py)
- 多层降噪自动编码机 Stacked Denoising Autoencoder (SDA) [sda.py](sda.py)
- 受限玻尔兹曼机Restricted Boltzmann Machine (RBM) [rbm.py](rbm.py)  [gbrbm.py](gbrbm.py)
- 深度信念网络Deep Belief Network (DBN) [dbn.py](dbn.py)

Note: the project aims at imitating the well-implemented algorithms in [Deep Learning Tutorials](http://www.deeplearning.net/tutorial/) (coded by [Theano](http://deeplearning.net/software/theano/index.html)).


[参考博客](https://github.com/DragonFive/myblog/blob/master/source/_posts/DL-learningNote1.md)

[吴恩达老师的深度学习课程笔记及资源](https://github.com/fengdu78/deeplearning_ai_books)

## 1. 神经网络的编程基础(Basics of Neural Network programming)
### 1.1 二分类(Binary Classification)
### 1.2 逻辑回归(Logistic Regression)
### 1.3 逻辑回归的代价函数（Logistic Regression Cost Function）
### 1.4 梯度下降（Gradient Descent）
### 1.5 导数（Derivatives）
### 1.6 计算图（Computation Graph）
### 1.7 逻辑回归的梯度下降（Logistic Regression Gradient Descent）
### 1.8 梯度下降的例子(Gradient Descent on m Examples)
### 1.9 向量化(Vectorization)
### 1.10 损失函数详解（cost/Loss function）softmax 交叉熵 Forc Loss

## 2. 浅层神经网络(Shallow neural networks)
### 2.1 计算一个神经网络的输出 （Computing a Neural Network's output）
### 2.2 多样本向量化（Vectorizing across multiple examples）
### 2.3 激活函数（Activation functions）
### 2.4 神经网络的梯度下降（Gradient descent for neural networks）
### 2.5 直观理解反向传播（Backpropagation intuition）
### 2.6 随机初始化（Random+Initialization）
### 2.7 

## 2. 深层神经网络(Deep Neural Networks)
### 3.1 前向传播和反向传播（Forward and backward propagation）
### 3.2 深层网络中的前向和反向传播（Forward propagation in a Deep Network）
### 3.3 核对矩阵的维数（Getting your matrix dimensions right）
### 3.4 参数VS超参数（Parameters vs Hyperparameters）
### 3.5 深度学习和大脑的关联性（What does this have to do with the brain?）

## 3. 改善深层神经网络：超参数调试、正则化以及优化
### 3.1 数据集 训练测试验证 均值方差
### 3.2 正则化 L1 L2 Dropout 随机失活 正则化 Regularization
### 3.3 标准化输入（Normalizing inputs）  BN GN
### 3.4 梯度消失/梯度爆炸（Vanishing / Exploding gradients）
### 3.5 神经网络的权重初始化（Weight Initialization for Deep NetworksVanishing /Exploding gradients）
### 3.6 梯度的数值逼近（Numerical approximation of gradients）& 梯度检验（Gradient checking）

## 4. 优化算法 (Optimization algorithms)
### 4.1 Mini-batch 梯度下降（Mini-batch gradient descent）
### 4.2 指数加权平均（Exponentially weighted averages）
### 4.3 momentum梯度下降（Gradient descent with momentum）
### 4.4 RMSprop——root mean square prop（RMSprop）
### 4.5 Adam优化算法（Adam optimization algorithm）
### 4.6 学习率衰减（Learning rate decay）
### 4.7 局部最优问题（The problem of local optima）



## 5. 卷积神经网络（Convolutional Neural Networks）
### 5.1 卷积层 普通卷积 DW逐通道卷积 分组卷积
### 5.2 池化层 最大值池化  均值池化
### 5.3 全连接层
### 5.4 Padding填充 步长Stride
### 5.5 经典网络（Classic networks）
### 5.6 残差网络（Residual Networks (ResNets)）
### 5.7 网络中的网络以及 1×1 卷积（Network in Network and 1×1 convolutions）通道重排ChannelShuffle
### 5.8 Inception  Xception ResXt DenseNet DectNet 网络
### 5.9 特征金字塔 FPN
### 5.10 目标检测OD 边框预测BBP 交并比IOU 非极大值抑制NMS 预设边框Anchor Boxes
### 5.11 YOLO v1 v2 v3 算法
### 5.12 轻量化模型 MobileNet SqueezeNet ShuffleNet

## 6. 人脸识别和神经风格转换（Special applications: Face recognition &Neural style transfer）
### 6.1 人脸识别
### 6.2 神经风格转换  neural style transfer
### 6.3 深度卷积网络学习 deep ConvNets learning
### 6.4 代价函数（Cost function） 内容代价函数（Content cost function） 风格代价函数（Style cost function）
### 6.5  一维到三维推广（1D and 3D generalizations of models）

## 7. 序列模型(Sequence Models)
### 7.1 循环神经网络模型 RNN（Recurrent Neural Network Model） 
#### 7.1.1 通过时间的反向传播（Backpropagation through time）
#### 7.1.2 不同类型的循环神经网络（Different types of RNNs）
#### 7.1.3 语言模型和序列生成（Language model and sequence generation）
#### 7.1.4 对新序列采样（Sampling novel sequences）
#### 7.1.5 循环神经网络的梯度消失（Vanishing gradients with RNNs）
### 7.2 GRU单元（Gated Recurrent Unit（GRU））
### 7.3 长短期记忆（LSTM（long short term memory）unit）
### 7.4 双向循环神经网络（Bidirectional RNN）
### 7.5 深层循环神经网络（Deep RNNs）

## 8. 自然语言处理与词嵌入（Natural Language Processing and Word Embeddings）
### 8.0 语音识别的传统方法 HMM隐马尔可夫模型 GMM高斯混合模型
### 8.1 词汇表征（Word Representation）
### 8.2 使用词嵌入（Using Word Embeddings） 词嵌入的特性（Properties of Word Embeddings） 词嵌入除偏（Debiasing Word Embeddings）
### 8.3 嵌入矩阵（Embedding Matrix）
### 8.4 Word2Vec
### 8.5 负采样（Negative Sampling） GloVe 词向量（GloVe Word Vectors） 
### 8.6 情绪分类（Sentiment Classification）
### 8.7 集束搜索（Beam Search）
### 8.8 注意力模型（Attention Model）
### 8.9 语音识别（Speech recognition）
### 9.10 触发字检测（Trigger Word Detection）

## 9. 视频分析 人体行为识别
### 9.1 传统方法 视觉词袋模型BoVW  FV高斯混合建模
### 9.2 双流模型
### 9.3 3D卷积模型
### 9.4 双流+3D卷积
### 9.5 LSTM模型
### 
### 

