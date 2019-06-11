# DVBPR-GANS
使用GAN对抗网络和贝叶斯进行服装的个性化推荐
环境
该代码在Linux桌面下使用单个GTX-1080 Ti GPU进行测试。
要求：
•	TensorFlow 1.3
•	NumPy的
•	PIL
数据集
四个时尚数据集：
•	AmazonFashion（3.3GB）：64K用户，234K图像，0.5M动作
•	AmazonWomen（6.2GB）：97K用户，347K图像，0.8M动作
•	AmazonMen（2.1GB）：34K用户，110K图像，0.2M动作
•	Tradesy（3.4GB）：33K用户，326K图像，0.6M动作
可以通过下载
bash download_dataset.sh 
所有数据集都以.npy格式存储，每个项目都与JPG图像相关联。有关详细用法，请参阅DVBPR代码。对于图像生成，我们主要使用AmazonFashion数据集。
亚马逊数据集来自这里，tradesy数据集在这里介绍。如果您使用数据集，请引用相应的论文。
请注意，原始图像仅供学术使用。
模特训练
第1步：训练DVBPR：
cd DVBPR
python main.py
默认的超参数在main.py中定义，您可以相应地更改它们。AUC（在验证和测试集上）记录在DVBPR.log中。
第2步：训练GAN：
cd GAN
python main.py --train True
默认的超参数在main.py中定义，您可以相应地更改它们。没有'--train True'，它将加载一个训练有素的模型并为每个类别生成图像（在文件夹样本中加入）。
第3步：偏好最大化：
cd PM
python main.py
PM基于预训练的DVBPR和GAN模型。它将为每个类别随机选择一个用户，并通过优化过程显示生成的图像。
使用单个GTX-1080 Ti，训练DVBPR和GAN分别需要大约7个小时。
演示（带预训练模型）
使用我们模型的快捷方法是使用预训练模型，可以通过以下方式获取：
bash download_pretrained_models.sh 
通过预训练模型，您可以看到DVBPR的AUC结果，并运行GAN和PM代码来生成图像。
杂项
•	致谢：GAN代码大量借用DCGAN。从LSGAN修改GAN网络。
•	原则上，我们的框架可以适应任何GAN变体，我们期待使用先进的GAN以更高的分辨率实现更好的生成结果。
