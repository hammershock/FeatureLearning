# FeatureLearning

特征学习，PCA&amp;AutoEncoder

本实验先展示传统的线性降维方法主成分分析PCA以及使用了核技巧的KernelPCA变种。
然后介绍`自动编码机（AutoEncoder，AEs）`及其变种`稀疏自动编码机（SparseAutoEncoder， SAE）`和`降噪自动编码机（DenoisingAutoEncoder， DAE）`的训练和使用场景。

## 任务1: PCA 或 Kernel PCA

### 实验目的和方法

本实验旨在通过使用主成分分析（PCA）或核主成分分析（Kernel PCA）对手写数字数据集MNIST进行降维，以观察在二维特征空间中数据的分布情况。通过将数据嵌入到二维空间（R2），我们可以分析和评价这些降维后的特征是否足够用于对输入的分类。
### 实验步骤

1. **数据准备**: 加载MNIST数据集，将图像展平为一维向量。
2. **降维处理**: 使用PCA或Kernel PCA对数据进行降维，保留前两个主要特征向量。
3. **数据可视化**: 在二维空间中绘制降维后的数据点，使用不同颜色代表不同的数字类别。
4. **结果分析**: 观察二维空间中的数据分布，分析不同数字类别是否能够被明显区分。
### 结果与分析

- **可视化结果**: 降维后的数据点在二维空间中表现出一定程度的类别聚集性。不同数字类别的数据点形成了不同的簇。
![Pasted image 20231218180436.png](assets%2FPasted%20image%2020231218180436.png)
![Pasted image 20231218180451.png](assets%2FPasted%20image%2020231218180451.png)

- **分类能力分析**: 尽管在二维空间中某些数字类别之间存在重叠，但多数类别能够被相对清晰地区分。这表明两个主要特征向量在一定程度上能够捕捉到区分不同数字的关键特征。

使用SVM对降维后的向量进行分类，准确率如下：
```
accuracy PCA 0.4696
accuracy Kernel PCA 0.11716666666666667
```

PCA降维后的方差贡献率,加在一起不到20%：
```
array([0.09704664, 0.07095924])
```

- **评价**: 虽然PCA或Kernel PCA在降维过程中捕获了数据的主要变化趋势，但完全依赖这两个特征进行分类可能不足以达到很高的分类准确率。对于一些相似的数字类别，可能需要更多的特征来实现更精确的分类。

## 任务2: Autoencoder及其变种

### 实验目的和方法

本实验的目的是使用自动编码器学习MNIST数据集中手写数字的特征表示。实验中将尝试设计全连接前馈神经网络或卷积神经网络，并探索不同的损失函数和正则化方法对学习特征表示的影响。

### 实验步骤

1. **网络设计**: 设计一个自动编码器网络，可以选择使用全连接层或卷积层。
2. **损失函数和正则化**: 尝试使用不同的损失函数（如MSELoss、CrossEntropyLoss、HuberLoss等）和正则化方法（如Dropout、L1/L2正则化）。
3. **训练和验证**: 在MNIST数据集上训练模型，并在验证集上评估其性能。

### 结果与分析
![Pasted image 20231218180232.png](assets%2FPasted%20image%2020231218180232.png)
![Pasted image 20231218181748.png](assets%2FPasted%20image%2020231218181748.png)

- **性能评估**: 根据不同的损失函数和正则化方法，自动编码器的重建性能有所不同。MSE损失通常能够提供较好的像素级重建精度。
- **特征表示学习**: 自动编码器成功学习到了数据的低维表示，能够在编码器部分捕捉到关键特征，在解码器部分重建图像。
- **评价**: 模型的性能受损失函数的选择和网络结构的影响较大。全连接网络适用于较为简单的特征提取任务，而卷积网络在处理图像数据时表现更佳。

## 附加题: 模型性能提升尝试

### 实验方法

为了提升模型性能，尝试了以下方法：
1. **网络结构优化**: 设计多个隐藏层，探索不同的激活函数。
2. **引入降噪自动编码器**: 对输入数据添加噪声，训练模型以重建原始无噪声图像。
3. **超参数调整**: 调整学习率、批量大小等超参数。
4. **正则化**:引入正则项提高编码空间的稀疏性

### 结果与分析

- **性能提升**: 引入降噪自动编码器和优化网络结构有助于提高模型的泛化能力和重建质量。
- **损失计算**: 在训练集和测试集上的损失表明模型在避免过拟合的同时保持了良好的重建性能。
- **评价**: 通过这些方法，模型的性能得到了显著提升，特别是在处理具有噪声的数据时更加稳健。

### DAE用于图像降噪效果展示
- 本实验在训练过程中使用Huber损失，不使用正则化
- 使用卷积和全连接的自动编码器效果对比

![Pasted image 20231218180259.png](assets%2FPasted%20image%2020231218180259.png)
![Pasted image 20231218181432.png](assets%2FPasted%20image%2020231218181432.png)

### 总结
自编码机在特征学习领域是一类强大的工具，它通常可以分为编码器和解码器两个部分，通过学习恒等变换来以无监督方式训练，自动提取原始特征的表示。
如果将编码器输出维度设置为比输入维度小，网络整体结构呈现沙漏形状⌛️，这种结构能够迫使网络忽略不必要的信息，学习到更加低维的特征表示，保留更加关键的信息。
这种方法利用神经网络强大的非线性表征能力将输入特征映射到稠密且连续的编码空间，这种方法常常用于特征提取和降维
此外，我们还尝试使用不同的训练方法，实现`稀疏自动编码机（SparseAutoEncoder， SAE）`和`降噪自动编码机（DenoisingAutoEncoder， DAE）`, 
在原始自编码器结构基础上，使用参数正则化可以提高编码空间的稀疏性，在训练时引入噪声可以鼓励模型将原始数据从噪声中恢复出来、使得模型更好地提取特征，实现更好的鲁棒性。
在下一个章节里，我将继续实现自动编码机的其他变种，比如著名的生成模型：`变分自动编码机（VariationalAutoEncoder， VAE）`
