# DeepID人脸识别算法之三代














![img1](https://raw.githubusercontent.com/stdcoutzyx/Blogs/master/blogs/imgs/n2-2.png) 


 使用multi-scale patches的convnet比只使用一个只有整张人脸的patch的效果要好。
+ DeepID自身的分类错误率在40%到60%之间震荡，虽然较高，但DeepID是用来学特征的，并不需要要关注自身分类错误率。
+ 使用DeepID神经网络的最后一层softmax层作为特征表示，效果很差。
+ 随着DeepID的训练集人数的增长，DeepID本身的分类正确率和LFW的验证正确率都在增加。













 对lambda进行调整，也即对识别信号和验证信号进行平衡，发现lambda在0.05的时候最好。使用LDA中计算类间方差和类内方差的方法进行计算。得到的结果如下：



 DeepID的训练集人数越多，最后的验证率越高。
 对不同的验证信号，包括L1，L2，cosin等分别进行了实验，发现L2 Norm最好。

 神经单元的适度稀疏性，该性质甚至可以保证即便经过二值化后，仍然可以达到较好的识别效果；
 高层的神经单元对人比较敏感，即对同一个人的头像来说，总有一些单元处于一直激活或者一直抑制的状态；
 DeepID2+的输出对遮挡非常鲁棒。


 DeepID层从160维提高到512维。
 训练集将CelebFaces+和WDRef数据集进行了融合，共有12000人，290000张图片。
 将DeepID层不仅和第四层和第三层的max-pooling层连接，还连接了第一层和第二层的max-pooling层。


























 [1] 	Sun Y, Wang X, Tang X. Deep learning face representation from predicting 10,000 classes[C]//Computer Vision and Pattern Recognition (CVPR), 2014 IEEE Conference on. IEEE, 2014: 1891-1898.
 [2] 	Sun Y, Chen Y, Wang X, et al. Deep learning face representation by joint identification-verification[C]//Advances in Neural Information Processing Systems. 2014: 1988-1996.
 [3] 	Sun Y, Wang X, Tang X. Deeply learned face representations are sparse, selective, and robust[J]. arXiv preprint arXiv:1412.1265, 2014.