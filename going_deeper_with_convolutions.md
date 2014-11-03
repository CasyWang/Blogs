# Going Deeper with convolutions

 Improve utilization of the computing resources inside the network, which is achieved by carefully crafted design and allows for increasing the depth and width of the network while keeping the computational budget constant.

 Architecture decisions are based on the Hebbian principle and the intuition of multi-scale processing.
 A 22 layers deep network is assessed in the competition.

 本文提出的网络结构为Inception，得名于论文参考文献12（network in network）。
 Recent trend of CNN is to increase the number of layers and layer size, while using dropout to address the problem of overfitting。
 论文参考文献15使用不同尺度的Gabor过滤器来处理多尺度问题，同本文的Inception Model类似。
 本文借鉴参考论文12，使用了很多1×1的卷积核。卷积核在本文中的作用主要在于降维，以此来去除计算瓶颈。
 Detection task’s leading approach is Regions with Convolutional Neural Networks(R-CNN) (参考文献6)。该方法分为两步：


 More prone to overfitting.
 Dramatically increase use of computational resources. (for example, if most weights end up to be close to zero, then lots of computations is wasted.)

 The fundamental way would be by ultimately moving from fully connected to sparsely connected architectures.
 论文的参考文献2表明，考虑到统计相关性，一个稀疏网络结构可以重新构建出最优结构。并产生了Hebbian principle——neurons that fire together, wire together。
 从更底层考虑，现在的硬件在非一致稀疏数据结构上的计算非常不高效，尤其在这些数据上使用已经为密集矩阵优化过的库时。原来自论文参考文献9以来，都会使用随机稀疏的网络结构来打破对称性，提高学习率。但论文参考文献11中又重新使用全连接的结构，以图利用密集计算的高效性。
 所以，现在的问题是有没有一种方法，既能保有网络结构的稀疏性，又能利用密集矩阵的高计算性能。论文提出了一种Inception Module，可以达到此等效果。



 Using DistBelief distributed machine learning system with modest amount model and data parallelism.
 Training with asynchronous stochastic gradient descent with 0.9 momentum, fixed learning rate schedule(decreasing the learning rate by 4% every 8 epochs) and Polyak averaging(论文参考文献13）is used。
 Sampling of various sized patches of the image whose size is distributed evenly between 8% and 100% of the image area and whose aspect ratio is chosen randomly between 3/4 and 4/3.
 Photometric distortions
 Using random interpolation methods for resizing relatively late and in conjunction with other hyperparameter changes.
 Trained 7 versions of same GoogLeNet model, performed ensembel prediction with them. These models are trained with the same initialization, but differ in sampling methodologies and the random order in which they see input images.
 Testing: resize the image to 4 scales where the shorter dim is 256,288,320,352. Take the left, right, center square of these resized images, then take the 4 corners and the center 224×224 crop and the square resize 224×224, and their mirror version. Namely, 4×3×6×2=144 crops per image.
 Softmax probabilities are averaged over multiple crops and over all individual classifiers to obtain the final prediction. Simple averaged is the best.