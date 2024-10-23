0.abstract：  
首先表明了transformer模型单独也可以在计算机视觉方面有很好的表现，即本文的vit。同之前transformer架构通常用于NLP(自然语言处理)且attention机制需要靠CNN来处理视觉有所突破。 
1.introduction：  
思路：同transformer在文本处理类似，将图像分成很多小块类似token，将这些块的线性嵌入作为输入，然后以监督方式在图像分类任务中训练  
不足与优点：在中等数据集上训练表现不如ResNet。但在大规模数据集训练下，vit的表现达到了最先进的水平。  
2.Related work：  
之前对图像应用self-attention由于图像的大规模输入cost非常大。于是对self-attention有所改进。vit与之前提取2x2图块的方法相近  
模型概述：将图片切成固定大小图块，对每个图块进行线性嵌入，添加位置嵌入，并将得到的向量放入transformer编码器中。  
3.method：  
尽量遵循原有的transformer框架  
3.1ViT：架构分析：   
首先将图片HxWxC(heights,weights,channels)转化为Nx(PxPxC),即每个块是PxP的，N是数量且是向量输入的长度，由(HxW)/(PxP)得到。并将其映射到D维上。称为块嵌入。  
随后添加位置嵌入，保留位置信息。(patch and position embedding)还存在一个class token(cls)，类似于一个分类层,和其他patch一样。最后根据cls的输出判断结果。Transformer编码器由MSA，MLP等组成。在编码器中，首先进行组归一化或层归一化，随后进入Multi-Head Attention部分，这种自注意力使每个图像块能对其他块产生交互，从而达到对于全局特征的抓取。然后有一个类似残差的操作，随后进入MLP，中间使用的是GELU激活函数。  
3.2微调和更高的分辨率：  
在更高分辨率下进行微调是有益的  
4.实验部分：  
数据集选择。  
对于模型的变体：有Base， Large和Huge。使用ResNet，但将批归一化替换为组归一化。使用Adam优化器。  
训练和微调  
5.关键技术创新：  
把transformer这种本应用于NLP的模型用于图形，视觉。原因在于可以把图片分成图块，就像语言中的token一样。并且含有attention机制，对于图像全局特征的感知较好。并且加入了位置嵌入。整个模型也引入了混合架构，比如加入了残差网络  
6.ViT与CNN比较：  
ViT是分成一个个图块并应用attention机制，对于图片的全局特征提取更好。CNN则是使用卷积核对图片提取特征，更多是局部的特征。  
归纳偏差：ViT的归纳偏差较弱，跟attention机制有关，因此需要较大规模的数据。CNN对局部特征提取较好，所以归纳偏差更强，小规模数据表现更好。  
ViT处理高分辨率的计算量更大，CNN相对较小。