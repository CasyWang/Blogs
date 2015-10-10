# 局部敏感哈希





 分层法即为在数据查询过程中使用哈希技术在中间添加一层，将数据划分到桶中；在查询时，先对query计算桶标号，找到与query处于同一个桶的所有样本，然后按照样本之间的相似度计算方法（比如欧氏距离、余弦距离等）使用原始数据计算相似度，按照相似度的顺序返回结果，在该方法中，通常是一组或一个哈希函数形成一个表，表内有若干个桶，可以使用多个表来提高查询的准确率，但通常这是以时间为代价的。分层法的示意图如图1所示。在图1中，H1、H2等代表哈希表，g1、g2等代表哈希映射函数。

 哈希码法则是使用哈希码来代替原始数据进行存储，在分层法中，原始数据仍然需要以在第二层被用来计算相似度，而哈希码法不需要，它使用LSH函数直接将原始数据转换为哈希码，在计算相似度的时候使用hamming距离来衡量。转换为哈希码之后的相似度计算非常之快，比如，可以使用64bit整数来存储哈希码，计算相似度只要使用同或操作就可以得到，唰唰唰，非常之快，忍不住用拟声词来表达我对这种速度的难言之喜，还望各位读者海涵。



 在对哈希函数的要求上，哈希码方法对哈希函数的要求更高，因为在分层法中，即便哈希没有计算的精确，后面还有原始数据直接计算相似度来保底，得到的结果总不会太差，而哈希码没有后备保底的，胜则胜败则败。

 在查询的时间复杂度上，分层法的时间复杂度主要在找到桶后的样本原始数据之间的相似度计算，而哈希码则主要在query的哈希码与所有样本的哈希码之间的hamming距离的相似计算。哈希码法没有太多其他的需要，但分层法中的各个桶之间相对较均衡方能使复杂度降到最低。按照我的经验，在100W的5000维数据中，KSH比E2LSH要快一个数量级。

 在哈希函数的使用上，两者使用的哈希函数往往可以互通，E2LSH使用的p-stable LSH函数可以用到哈希码方法上，而KSH等哈希方法也可以用到分层法上去。


 无监督，哈希函数是基于某种概率理论的，可以达到局部敏感效果。如E2LSH等。
 有监督，哈希函数是从数据中学习出来的，如KSH、Semantic Hashing等。




 对于一个点P来说，P=(x<sub>1</sub>,x<sub>2</sub>,…,x<sub>d</sub>)，d是数据的维度；
 将每一维x<sub>i</sub>转换为一个长度为C的0/1序列，其中序列的前x<sub>i</sub>个值为1，剩余的为0.
 然后将d个长度为C的序列连接起来，形成一个长度为Cd的序列.








> + 从[0,Cd]内取L个数，形成集合G，即组成了一个桶哈希函数g。
 对于一个向量P，得到一个L维哈希值，即P<sub>|G</sub>，其中L维中的每一维都对应一个哈希函数h。
 由于直接以上步中得到的L维哈希值做桶标号不方便，因而再进行第二步哈希，第二步哈希就是普通的哈希，将一个向量映射成一个实数。

















![data_structure_of_e2lsh](https://raw.githubusercontent.com/stdcoutzyx/Blogs/master/blogs/imgs/n3-6.png)




 使用k个哈希函数计算桶标号的k元组；
 对k元组计算h1和h2值，
 获取哈希表的h1位置的链表，
 在链表中查找h2值，
 获取h2值位置上存储的样本
 Query与上述样本计算精确的相似度，并排序
 按照顺序返回结果。





[3]. Kulis B, Grauman K. Kernelized locality-sensitive hashing[J]. Pattern Analysis and Machine Intelligence, IEEE Transactions on, 2012, 34(6): 1092-1104.
[4]. Salakhutdinov R, Hinton G. Semantic hashing[J]. International Journal of Approximate Reasoning, 2009, 50(7): 969-978.
[5]. Liu W, Wang J, Ji R, et al. Supervised hashing with kernels[C]//Computer Vision and Pattern Recognition (CVPR), 2012 IEEE Conference on. IEEE, 2012: 2074-2081.
[7]. http://web.mit.edu/andoni/www/LSH/