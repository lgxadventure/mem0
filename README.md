<p align="center">
  <img src="docs/images/mem0-bg.png" width="500px" alt="Mem0 Logo">
</p>

<p align="center">
  <a href="https://embedchain.ai/slack">
    <img src="https://img.shields.io/badge/slack-embedchain-brightgreen.svg?logo=slack" alt="Slack">
  </a>
  <a href="https://embedchain.ai/discord">
    <img src="https://dcbadge.vercel.app/api/server/6PzXDgEjG5?style=flat" alt="Discord">
  </a>
  <a href="https://twitter.com/mem0ai">
    <img src="https://img.shields.io/twitter/follow/mem0ai" alt="Twitter">
  </a>
</p>

# Mem0: The Memory Layer for Personalized AI

Mem0 provides a smart, self-improving memory layer for Large Language Models, enabling personalized AI experiences across applications.

> Note: The Mem0 repository now also includes the Embedchain project. We continue to maintain and support Embedchain ❤️. You can find the Embedchain codebase in the [embedchain](https://github.com/mem0ai/mem0/tree/main/embedchain) directory.
## 🚀 Quick Start

### Installation
```bash
pip install mem0ai
```

### Basic Usage

```python
import os
from mem0 import Memory

os.environ["OPENAI_API_KEY"] = "xxx"

# Initialize Mem0
m = Memory()

# Store a memory from any unstructured text
result = m.add("I am working on improving my tennis skills. Suggest some online courses.", user_id="alice", metadata={"category": "hobbies"})
print(result)
# Created memory: Improving her tennis skills. Looking for online suggestions.

# Retrieve memories
all_memories = m.get_all()
print(all_memories)

# Search memories
related_memories = m.search(query="What are Alice's hobbies?", user_id="alice")
print(related_memories)

# Update a memory
result = m.update(memory_id="m1", data="Likes to play tennis on weekends")
print(result)

# Get memory history
history = m.history(memory_id="m1")
print(history)
```

## 🔑 Core Features

- **Multi-Level Memory**: User, Session, and AI Agent memory retention
- **Adaptive Personalization**: Continuous improvement based on interactions
- **Developer-Friendly API**: Simple integration into various applications
- **Cross-Platform Consistency**: Uniform behavior across devices
- **Managed Service**: Hassle-free hosted solution

## 📖 Documentation

For detailed usage instructions and API reference, visit our documentation at [docs.mem0.ai](https://docs.mem0.ai).

## 🔧 Advanced Usage

For production environments, you can use Qdrant as a vector store:

```python
from mem0 import Memory

config = {
    "vector_store": {
        "provider": "qdrant",
        "config": {
            "host": "localhost",
            "port": 6333,
        }
    },
}

m = Memory.from_config(config)
```

## 🗺️ Roadmap

- Integration with various LLM providers
- Support for LLM frameworks
- Integration with AI Agents frameworks
- Customizable memory creation/update rules
- Hosted platform support

## 🙋‍♂️ Support
Join our Slack or Discord community for support and discussions.
If you have any questions, feel free to reach out to us using one of the following methods:

- [Join our Discord](https://embedchain.ai/discord)
- [Join our Slack](https://embedchain.ai/slack)
- [Follow us on Twitter](https://twitter.com/mem0ai)
- [Email us](mailto:founders@mem0.ai)
Reviewer 1：
1、	In this study, the authors first measured the category similarity, but you used an autoencoder system for feature extraction. Why? We know that an autoencoder system can uncover the intrinsic features of images, but can these features fully reflect the discriminative nature of the categories? If they can, would it be better to use these features directly for scene classification? If they cannot, is it accurate to use such features for measuring category similarity?
感谢精彩的评论。众所周知，原始数据中包含大量的噪声和冗余信息，因此，使用Autoencoder通过最小化重构误差，提取遥感场景的降维后的表征。此特征因未使用标签训练和未提取多尺度、多深度特征。因此该特征判别性不强。但是，由最小化重构误差提取的特征，数据的特征分布逼近原始数据分布。使用其特征构造的类别分布更符合自然规律，因此，使用其分布计算出的类别差异性是较为合理并充分的。
2、	Using multiple models for scene classification has been a commonly used approach in the past, and it can indeed improve the performance of the model to a certain extent. However, according to the principles of ensemble learning, ensemble methods are built upon the basis of complementary differences in information. In the research, many convolutional models are used for feature extraction and then integrated. How can the complementary differences between these models be ensured, and how can their effectiveness be proven?
感谢精彩的评论。集成学习确实建立在基分类器具有多样性和互补性的基础上。在本文中，由于遥感场景的复杂性和场景的大小异性，因此需要多尺度的卷积核去提取多尺度的信息。由文章[]证明了，多层小卷积的神经网络，等效于一个大卷积的神经网络，并从中引入了更多的非线性信息。因此本文使用多个深度不同的小卷积神经网络提取多尺度信息，其符合集成学习的基分类器的多样性。而为什么卷积层的深度最多是到13层，此内容已在文章的[]说明，因为如果继续加深神经网络的深度，可能会出现梯度爆炸的情形。
而同时为了体现集成学习的互补性，本文应用特征选择的方式去除融合多个基分类器软标签表征的冗余性信息，而选择后的软标签结果是相互补充的，会促进判别性结果的。同时，通过消融实验[],验证了特征提取的有效性。同时，场景分类过程中使用的基分类器中效果最好的是13层卷积+3层全连接的VGG16模型，参照表[].[],[]实验结果,可以发现本文的模型优于VGG-16模型。
3、	In the analysis of the results, there was only a general comparison of the accuracy differences with other models, but no in-depth analysis of whether the model addressed the specific issues raised in this study, such as whether it improved the discrimination ability for similar categories and what impact it had on easily distinguishable categories.
感谢精彩的评论。由于论文篇幅有限，未在正文中表述模型对相似类别的分类的促进结果。在附录[],的混淆矩阵实验中，说明了本文模型对相似类别的判别能力强于表[],[],[]的最优、次优算法T-CNN，在对中等小区和高等小区的判别误差下降[]%。
4、	Based on the results of this study, using such a complex system actually did not achieve higher accuracy compared to current advanced single network models. Then, what are the advantages of this method? It is recommended to test this method on more complex datasets or larger datasets to highlight its advantages over other models.
感谢精彩的评论。本文所应用的数据集均是近几年常见的遥感场景标准，而更复杂的遥感数据集未在网上找到，因为经费、设备有限，无法采集更为复杂的遥感数据。若reviewer有更好的数据集，欢迎分享。同时，众所周知，对于场景分类任务，精度提升的难度与精度的大小成正相关关系，在如今的遥感场景分类精度难以提升的情况下，需要更复杂的模型实现精度的提升。
Reviewer 2：
1、	While the paper mentions the application of ensemble learning and deep learning in remote sensing scene classification in the introduction, it can further elaborate on the specific shortcomings of existing methods and how this study specifically improves upon these deficiencies. For instance, the paper mentions that existing deep models often overlook the intrinsic connections between difficult-to-distinguish categories when dealing with class distributions, but the literature review section is insufficient. Additional related works could be included to enhance the persuasiveness of the article and the perceived difficulty of the work presented.
感谢你的评论。


2、	The paper proposes a deep ensemble learning model based on class distribution information, which includes two main modules: class distribution information extraction and scene classification. The article employs 13 convolutional layers and 3 fully connected layers. In ensemble learning, this is a variable hyperparameter, and the authors should demonstrate the optimal parameters of the method from theoretical and experimental perspectives or based on relevant literature.
感谢精彩的评论。具体的可以参照Reviewer1的相关内容。


3、	The paper introduces α as a balancing factor. When α reaches a certain threshold, the model tends to focus more on challenging instances, neglecting the rest and leading to a decline in overall performance. Figure 8 in the experimental results validates this point. However, is there a positive correlation between this factor and the amount of training data? As a hyperparameter, could the potential mechanism between the factor and the experiment be further explored from both theoretical and experimental standpoints?
感谢精彩的评论。本研究在未来会考虑这个进行这个实验。


Reviewer 3：
1、	The base learners all employ 13 CNNs with similar structures. However, if the base classifiers are highly correlated, ensemble learning may fail to improve performance.
感谢精彩的评论。论文[]已经说明了使用的是13个深度不同的神经网络，结构并不相似，并相关的解释可参照Reviewer1.3。
2、	I think the classification problem is relatively low difficulty and does not need to be handled with such a large model. And the method is not sufficiently correlated with geoscience and remote sensing.
感谢你的评论。请问遥感场景分类任务是否属于遥感领域。
3、	The modules in this paper such as autoencoder and CNN are basic backbones without optimization, and the authors did not discuss the layer and parameter selection for base learners.
感谢你的评论。参考method部分，已经说明了基分类器和自编码器的层数和结构参数。
4、	The experiment uses four evaluation metrics. For AA and KC, the authors only present the result of this paper without comparison with other methods. And there is no related statement in the text.
感谢你的评论。作为对比算法，很多并未开源，其过程中并未使用AA和KC，本文应用AA，KC是证明本文的算法……………
5、 It is confusing that the analysis of CM is presented in the appendix.
感谢你的评论。大部分的遥感场景分类任务均需要混淆矩阵的实验，并对其进行分析。
5、	The writing needs improvement. There are many spelling and grammar mistakes. For example, - Page 2 of 14, column left, line 31, “a” should be “an”. - Page 2 of 14, column left, line 55, “Ours” shouldn’t be capitalized. - Page 7 of 14, column left, line 59, “are” should be “is”.
感谢你的评论。我们会仔细校准初稿，并改正上述错误。
Reviewer 4：
1、 The technique novelties of this work are incremental and modest. The proposed Category Distribution Association learning scheme is in fact a variation of ensemble learning with adaboost, which has been extensively studied in the machine learning community for decades. Besides, the general idea to use ensemble learning to process the deep features for remote sensing scenes, have also been extensively studied in the past decade [1-2]
[1] Li, Erzhu, et al. "Integrating multilayer features of convolutional neural networks for remote sensing scene classification." IEEE Transactions on Geoscience and Remote Sensing 55.10 (2017): 5653-5665. [2] Hu, Fan, et al. "Transferring deep convolutional neural networks for the scene classification of high-resolution remote sensing imagery." Remote Sensing 7.11 (2015): 14680-14707.
感谢精彩的评论。本文是整体使用了集成学习的框架，并首次将类别分布的差异信息引入至boosting框架中，缓解了boosting过程中，错分类样本提升的程度一样的问题。具体贡献点可参照文章[]

2、The compared references are old and out-of-date. More recent remote sensing scene classification methods in 2022-2023 [3-4] should be involved for comparison. [3] Wang, Junjie, et al. "Remote sensing scene classification via multi-stage self-guided separation network." IEEE Transactions on Geoscience and Remote Sensing (2023). [4] Yang, Yuqun, et al. "SAGN: Semantic-aware graph network for remote sensing scene classification." IEEE Transactions on Image Processing 32 (2023): 1011-1025.
感谢精彩的评论。本文将引入较新的参考文献和比较方法。具体参照新论文[]。

3. The proposed method shows a clearly inferior performance when compared with the recent state-of-the-art. Besides, a naïve use of vision Transformer model (e.g., ViT [5], Swin-T [6]), without using an additional complicated machine learning techniques, would show a significant better performance than the proposed method. [5] Vaswani, Ashish, et al. "Attention is all you need." Advances in neural information processing systems 30 (2017). [6] Liu, Ze, et al. "Swin transformer: Hierarchical vision transformer using shifted windows." Proceedings of the IEEE/CVF international conference on computer vision. 2021.
感谢精彩的评论。尽管近些年来，transformer模型得到了长足的发展。但是在遥感领域中依旧有很多SOTA是基于CNN的方法。

4. Could the authors make some clarification: whether the proposed method is end-to-end, or still need to first extract deep features from deep learning models and then fine-tune the extracted deep features on a classifier? If the later paradigm, it is relatively old and out-of-date. Again, devising the propose method into the deep models in a learnable and endto-end manner would be much more significant and practical, yielding better performance.
感谢精彩的评论。本文是一个two-stage的模型，是因为使用Autoencoder提取类别分布表征，其和stacking过程的基分类器神经网络，以及元分类器无法构建到一个端对端的网络中。尽管本文的模型不是端对端的网络，但是其性能不逊于端对端的神经网络架构。同时未来的工作中，也考虑将网络进行改进，修改为端对端的神经网络架构。

5. The ablation study lacks in some important aspect. Since the authors extract the deep features from each layer of the deep model, then an ablation on the impact from each layer deep features would be preferred.
感谢精彩的评论。此可参考Reviewer1.3.本文应用了特征选择的方式，同时也做了对特征选择过程的消融实验。

6. From the reviewer’s view, the performance of hand-crafted features in the tables could be removed, and again, to add more recent methods’ performance in 2022-2023.
感谢精彩的评论。具体请参照上述第2条。
7. After computing Eq.2-Eq.7, is there any visualization on the feature distribution and the weigh matrix for evaluation?
感谢精彩的评论。由于使用自编码器提取的特征处于较高维情况并未引入标签约束其判别性信息，使用T-SNE等可视化方法，会出现拥挤的情况，因此并未对特征分布进行可视化。
Other comments for improvement: 
8. Please provide more heatmaps of the proposed method? Can the features properly activate the key local regions of a remote sensing scene? 
9. Fig.5, 6 and 7 seems to be not necessary. 
10. Fig.8. Please provide higher-resolution figure, and enlarge the text font size
感谢上述的意见。上述内容会在未来发展中继续研究。
Reviewer 5：
1. One potential criticism is the lack of a thorough comparison with the most recent state-of-the-art methods. While the paper claims superiority over existing approaches, it would be beneficial to see a more detailed and direct comparison.
感谢精彩的评论。本文将引入较新的参考文献和比较方法。具体参照新论文[]。
2.The complexity of the proposed model might be a drawback in terms of computational efficiency. The paper does not address the computational cost and feasibility of applying this model to large-scale datasets.
感谢精彩的评论。由于本文是stacking架构，其中各个基分类器和类别分布提取模块是可以并行运算的，适用于目前遥感分类领域的大规模数据集。具体的计算成本，本文在3090中运算
4. The paper could also benefit from a deeper discussion of the model’s limitations and potential failure cases. Understanding where the model might not perform well is crucial for practical applications. Additionally, the paper does not provide insights into how the model might be adapted or extended to other classification tasks beyond remote sensing scene classification." 
感谢精彩的评论。在常用的自然图像分类数据集中，其也存在类别之间存在挑战性类对的问题。因此其也适用于自然图像分类任务。

These criticisms are meant to be constructive.
5. Some Visualization Results could be provided to show how the deep feature learning could distinguish between categories with many similarities. 
6. Some cofusion matrix results can be be made to show the performance。
感谢上述的意见。上述内容会在未来发展中继续研究。
