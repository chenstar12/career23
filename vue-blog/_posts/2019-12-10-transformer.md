---
layout: post
title:  Transformer知识点汇总
date:   2019-12-10 16:52:00
categories: 深度学习 
tags: 深度学习 NLP Transformer BERT GPT Attention BeamSearch seq2seq 杨植麟 XLNet 循环智能
excerpt: Attention is all you need!
mathjax: true
permalink: /transformer
---

* content
{:toc}

# Transformer学习笔记

- [The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html),Harvard NLP出品，含pytorch版代码实现
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
- [Transformer模型的PyTorch实现](https://luozhouyang.github.io/transformer/),[A PyTorch implementation of the Transformer model in "Attention is All You Need"](https://github.com/jadore801120/attention-is-all-you-need-pytorch)
- 【2021-1-21】[The Transformer Family](https://lilianweng.github.io/lil-log/2020/04/07/the-transformer-family.html)
  - ![](https://lilianweng.github.io/lil-log/assets/images/transformer.png)

## 总结

- 针对rnn和cnn的缺陷，怎么解决这些问题呢？
   - 并行化
   - 提升长程依赖的学习能力
   - 层次化建模

## 卷积

各类卷积讲解:[A Comprehensive Introduction to Different Types of Convolutions in Deep Learning](https://towardsdatascience.com/a-comprehensive-introduction-to-different-types-of-convolutions-in-deep-learning-669281e58215)
- 卷积与互相关（信号处理）
- 深度学习中的卷积（单通道/多通道）
- 3D卷积1 x 1卷积卷积运算（Convolution Arithmetic）
- 转置卷积（反卷积，checkerboard artifacts）
- 扩张卷积（空洞卷积）
- 可分离卷积（空间可分离卷积，深度卷积）
- 扁平卷积（Flattened Convolution）
- 分组卷积（Grouped Convolution）
- 随机分组卷积（Shuffled Grouped Convolution）
- 逐点分组卷积（Pointwise Grouped Convolution）

作者：[初识CV](https://www.zhihu.com/question/54149221/answer/1850592489)

![](https://pic1.zhimg.com/50/v2-0411ccbcb5529b2855478d619ac78d9d_hd.webp?source=1940ef5c)


空洞卷积 diolation

![](https://pic1.zhimg.com/50/v2-9c531569460c694db396a7530d8e5ffc_hd.webp?source=1940ef5c)


内部卷积 involution

[CVPR 2021 | Involution：超越 Convolution 和 Self-attention 的神经网络新算子](https://blog.csdn.net/BAAIBeijing/article/details/115222970), [论文地址](http://arxiv.org/abs/2103.06255)
![](https://img-blog.csdnimg.cn/img_convert/0f8c8ff1aa63b079025990418c20ea68.png)


![](https://img-blog.csdn.net/20170730100057611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTGVmdF9UaGluaw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![](https://img-blog.csdnimg.cn/img_convert/b670881b8e5cd7b52b4ebe69ace1654b.png)



## Seq2seq结构

编码-解码结构

### Seq2Seq 模型

参考：[Seq2Seq 模型详解](https://www.jianshu.com/p/80436483b13b)

- Seq2Seq 是一种RNN模型，使用的是 **Encoder-Decoder**结构，可以理解为一种 N * M 的模型
  - 即 **Encoder** 长度可以为N，**Decoder** 长度可以为M。多对多的序列结构
  - **Encoder** 用于编码序列的信息，将任意长度的序列信息编码到一个向量 **c** 里。
  - 而 **Decoder** 是解码器，解码器得到上下文信息向量 **c** 之后可以将信息解码，并输出为序列。

### 常见的 Seq2Seq 结构

Seq2Seq的模型结构也有很多种，常见的有下面几种，**Encoder**都相同，区别主要在于**Decoder**

#### 第一种Seq2Seq

Encoder & Decoder：

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-f3b85bfbaae7d8b1.png)

单看 Decoder：

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-6f461e520d31cb27.png)

这种**Decoder**比较简单：

- 将上下文向量 **c** 当成是 RNN 的初始隐藏状态输入到 RNN 中
- 后续只接受上一个神经元的隐藏层状态 **h'** 而不接收其他的输入 **x**


#### 第二种Seq2Seq

Encoder & Decoder：

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-479f2b802293c8c5.png)

单看 Decoder：

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-177f158afe2216bf.png)

-  Decoder 结构有了自己的初始隐藏层状态 **h'0**
-  不再把上下文向量 **c** 当成是 RNN 的初始隐藏状态，而是当成 RNN 每一个神经元的输入

#### 第三种Seq2Seq

Encoder & Decoder：

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-98c17bbce81a14ba.png)

单看Decoder：

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-840078e115f43250.png)

- Decoder 结构和第二种类似，但是在输入的部分多了上一个神经元的输出 **y'**。
- 即每一个神经元的输入包括：上一个神经元的隐藏层向量 **h'**，上一个神经元的输出 **y'**，当前的输入 **c** (Encoder 编码的上下文向量)。
- 对于第一个神经元的输入 **y'**0，通常是句子起始标志位的 embedding 向量。

### seq2seq实现

TensorFlow版本：[直观理解并使用Tensorflow实现Seq2Seq模型的注意机制](https://www.toutiao.com/i6846902952590836235/), 在Tensorflow中实现、训练和测试一个英语到印地语机器翻译模型

资料：
- [Sequence to sequence Architecture with Attention](https://arxiv.org/abs/1409.0473)
- Google神经机器翻译[官方讲解](https://www.tensorflow.org/tutorials/text/nmtwithattention)


#### 数据读入及预处理

数据源：Kaggle里一个名为"HindiEnglishTruncatedCorpus"的文件
- [Dataset](https://www.kaggle.com/aiswaryaramachandran/hindienglish-corpora)
- [代码](https://github.com/ayushjain19/NMT-Sequence-to-sequence-model-with-Attention-mechanism-for-English-to-Hindi-Translation)
- 格式：[english \\t hindi]

![](https://p6-tt.byteimg.com/origin/pgc-image/e7fbaa0b1da44938b8cacbbe572fcec2?from=pc)

预处理步骤如下:
- 在单词和标点符号之间插入空格
- 如果手头上的句子是英语，我们就用空格替换除了(a-z, A-Z, ".", "?", "!", ",")
- 句子中去掉多余的空格，关键字"sentencestart" (<SOS>)和"sentenceend"(<EOS>)分别添加到句子的前面和后面，让我们的模型明确地知道句子开始和结束。
- 每个句子的以上三个任务都是使用preprocess_sentence()函数实现的。我们还在开始时初始化了所有的超参数和全局变量。请阅读下面的超参数和全局变量。我们将在需要时使用它们。


```python
import numpy as np
import pandas as pd
from tensorflow.keras.preprocessing.text import Tokenizer
from tensorflow.keras.preprocessing.sequence import pad_sequences
import tensorflow as tf
from sklearn.model_selection import train_test_split
import time
from matplotlib import pyplot as plt
import os
import re

data = pd.read_csv("./Hindi_English_Truncated_Corpus.csv")
english_sentences = data["english_sentence"]
hindi_sentences = data["hindi_sentence"]

# Global variables and Hyperparameters
num_words = 10000
oov_token = '<UNK>'
english_vocab_size = num_words + 1
hindi_vocab_size = num_words + 1
MAX_WORDS_IN_A_SENTENCE = 16
test_ratio = 0.2
BATCH_SIZE = 512
embedding_dim = 64
hidden_units = 1024
learning_rate = 0.006
epochs = 100

def preprocess_sentence(sen, is_english):
	if (type(sen) != str):
		return ''
	sen = sen.strip('.')
	
	# insert space between words and punctuations
	sen = re.sub(r"([?.!,¿;।])", r" \1 ", sen)
	sen = re.sub(r'[" "]+', " ", sen)
	
	# For english, replacing everything with space except (a-z, A-Z, ".", "?", "!", ",", "'")
	if(is_english == True):
		sen = re.sub(r"[^a-zA-Z?.!,¿']+", " ", sen)
		sen = sen.lower()
	
	sen = sen.strip()
	sen = 'sentencestart ' + sen + ' sentenceend'
	
	sen = ' '.join(sen.split())
	return sen

# Loop through each datapoint having english and hindi sentence
# 对包含英语句子和印地语句子的每个数据点进行循环，确保不考虑带有空字符串的句子，并且句子中的最大单词数不大于MAXWORDSINASENTENCE的值。这一步是为了避免我们的矩阵是稀疏的。
processed_e_sentences = []
processed_h_sentences = []
for (e_sen, h_sen) in zip(english_sentences, hindi_sentences):
	processed_e_sen = preprocess_sentence(e_sen, True)
	processed_h_sen = preprocess_sentence(h_sen, False)
	if(processed_e_sen == '' or processed_h_sen == '' or processed_e_sen.count(' ') >  (MAX_WORDS_IN_A_SENTENCE-1) or processed_h_sen.count(' ') > (MAX_WORDS_IN_A_SENTENCE-1)):
		continue
	processed_e_sentences.append(processed_e_sen)
	processed_h_sentences.append(processed_h_sen)

print("Sentence examples: ")
print(processed_e_sentences[0])
print(processed_h_sentences[0])
print("Length of English processed sentences: " + str(len(processed_e_sentences)))
print("Length of Hindi processed sentences: " + str(len(processed_h_sentences)))
# 文本语料库进行向量化
def tokenize_sentences(processed_sentences, num_words, oov_token):
	tokenizer = Tokenizer(num_words = num_words, oov_token = oov_token)
	tokenizer.fit_on_texts(processed_sentences) # 为每个单词分配一个唯一的索引
	word_index = tokenizer.word_index
    # # 将一个文本句子转换为一个数字列表或一个向量，其中数字对应于单词的唯一索引
	sequences = tokenizer.texts_to_sequences(processed_sentences) 
	# 通过添加刚好足够数量的oovtoken(从vocab token中提取)来确保所有这些向量最后都具有相同的长度，使每个向量具有相同的长度。
    sequences = pad_sequences(sequences, padding = 'post')
	return word_index, sequences, tokenizer

english_word_index, english_sequences, english_tokenizer = tokenize_sentences(processed_e_sentences, num_words, oov_token)
hindi_word_index, hindi_sequences, hindi_tokenizer = tokenize_sentences(processed_h_sentences, num_words, oov_token)

# split into traning and validation set
english_train_sequences, english_val_sequences, hindi_train_sequences, hindi_val_sequences = train_test_split(english_sequences, hindi_sequences, test_size = test_ratio)
BUFFER_SIZE = len(english_train_sequences)

# Batching the training set
dataset = tf.data.Dataset.from_tensor_slices((english_train_sequences, hindi_train_sequences)).shuffle(BUFFER_SIZE)
dataset = dataset.batch(BATCH_SIZE, drop_remainder = True)
print("No. of batches: " + str(len(list(dataset.as_numpy_iterator()))))
```

### 编码器Encoder

Seq2seq架构在原论文中涉及到两个长短期内存(LSTM)。一个用于编码器，另一个用于解码器。请注意，在编码器和解码器中，我们将使用GRU(门控周期性单元)来代替LSTM，因为GRU的计算能力更少，但结果与LSTM几乎相同。Encoder涉及的步骤:

输入句子中的每个单词都被嵌入并表示在具有embeddingdim(超参数)维数的不同空间中。换句话说，您可以说，在具有embeddingdim维数的空间中，词汇表中的单词的数量被投影到其中。这一步确保类似的单词(例如。boat & ship, man & boy, run & walk等)都位于这个空间附近。这意味着"男人"这个词和"男孩"这个词被预测的几率几乎一样(不是完全一样)，而且这两个词的意思也差不多。

接下来，嵌入的句子被输入GRU。编码器GRU的最终隐藏状态成为解码器GRU的初始隐藏状态。编码器中最后的GRU隐藏状态包含源句的编码或信息。源句的编码也可以通过所有编码器隐藏状态的组合来提供[我们很快就会发现，这一事实对于注意力的概念的存在至关重要]。

![](https://p1-tt.byteimg.com/origin/pgc-image/3e473e6bde084bf6988f8c80bc0cabc7?from=pc)

```python
class Encoder(tf.keras.Model):
	
	def __init__(self, english_vocab_size, embedding_dim, hidden_units):
		super(Encoder, self).__init__()
		self.embedding = tf.keras.layers.Embedding(english_vocab_size, embedding_dim)
		self.gru = tf.keras.layers.GRU(hidden_units, return_sequences = True, return_state = True)
		
	def call(self, input_sequence):
		x = self.embedding(input_sequence)
		encoder_sequence_output, final_encoder_state = self.gru(x)
		#	Dimensions of encoder_sequence_output => (BATCH_SIZE, MAX_WORDS_IN_A_SENTENCE, hidden_units)
		#	Dimensions of final_encoder_state => (BATCH_SIZE, hidden_units)
		return encoder_sequence_output, final_encoder_state

# initialize our encoder
encoder = Encoder(english_vocab_size, embedding_dim, hidden_units)
```

### 解码器Decoder ( 未使用注意力机制)

注意:在本节中，我们将了解解码器的情况下，不涉及注意力机制。这对于理解稍后与解码器一起使用的注意力的作用非常重要。

解码器GRU网络是生成目标句的语言模型。最终的编码器隐藏状态作为解码器GRU的初始隐藏状态。第一个给解码器GRU单元来预测下一个的单词是一个像"sentencestart"这样的开始标记。这个标记用于预测所有num_words数量的单词出现的概率。训练时使用预测的概率张量和实际单词的一热编码来计算损失。这种损失被反向传播以优化编码器和解码器的参数。同时，概率最大的单词成为下一个GRU单元的输入。重复上述步骤，直到出现像"sentenceend"这样的结束标记。

![](https://p1-tt.byteimg.com/origin/pgc-image/0b2d43e2c52047e48ab4eead9f3f6b84?from=pc)

这种方法的问题是:
- **信息瓶颈**: 编码器的最终隐藏状态成为解码器的初始隐藏状态。这就造成了信息瓶颈，因为源句的所有信息都需要压缩到最后的状态，这也可能会偏向于句子末尾的信息，而不是句子中很久以前看到的信息。
- 解决方案:我们解决了上述问题，不仅依靠编码器的最终状态来获取源句的信息，还使用了编码器所有输出的加权和。那么，哪个编码器的输出比另一个更重要?注意力机制就是为了解决这个问题。

### 注意力机制

注意力不仅为瓶颈问题提供了解决方案，还为句子中的每个单词赋予了权重(相当字面意义)。源序列在编码器输出中有它自己的的信息，在解码器中被预测的字在相应的解码器隐藏状态中有它自己的的信息。我们需要知道哪个编码器的输出拥有类似的信息，我们需要知道在解码器隐藏状态下，哪个编码器输出的信息与解码器隐藏状态下的相似。

因此，这些编码器输出和解码器的隐藏状态被用作一个数学函数的输入，从而得到一个注意力向量分数。当一个单词被预测时（在解码器中的每个GRU单元），这个注意力分数向量在每一步都被计算出来。该向量确定每个编码器输出的权重，以找到加权和。

注意力的一般定义:给定一组向量"值"和一个向量"查询"，注意力是一种计算基于查询的加权值和的技术。

几种方法可以找到注意力得分(相似度)。主要有以下几点:
- 基本点积注意力（Dot Product Attention）: 最容易掌握
- 乘法注意力(multiplicative attention)
- 加性注意力(additive attention)

基本的点积注意有一个假设。
- 假设两个输入矩阵的维数在轴上要做点积的地方必须是相同的，这样才能做点积。在我们的实现中，这个维度是由超参数hidden_units给出的，对于编码器和解码器都是一样的。

![](https://p1-tt.byteimg.com/origin/pgc-image/10e0c1ac79004071a1a230bf607e0126?from=pc)

将编码器输出张量与解码器隐藏状态进行点积，得到注意值。这是通过Tensorflow的matmul()函数实现的。我们取上一步得到的注意力分数的softmax。这样做是为了规范化分数并在区间[0,1]内获取值。编码器输出与相应的注意分数相乘，然后相加得到一个张量。这基本上是编码器输出的加权和，通过reduce_sum()函数实现。

```python
class BasicDotProductAttention(tf.keras.layers.Layer):
	def __init__(self):
		super(BasicDotProductAttention, self).__init__()
		
	def call(self, decoder_hidden_state, encoder_outputs):
		#	Dimensions of decoder_hidden_state => (BATCH_SIZE, hidden_units)
		#	Dimensions of encoder_outputs => (BATCH_SIZE, MAX_WORDS_IN_A_SENTENCE, hidden_units)

		decoder_hidden_state_with_time_axis = tf.expand_dims(decoder_hidden_state, 2)
		#	Dimensions of decoder_hidden_state_with_time_axis => (BATCH_SIZE, hidden_units, 1)
		attention_scores = tf.matmul(encoder_outputs, decoder_hidden_state_with_time_axis)
		#	Dimensions of attention_scores => (BATCH_SIZE, MAX_WORDS_IN_A_SENTENCE, 1)
		attention_scores = tf.nn.softmax(attention_scores, axis = 1)
		weighted_sum_of_encoder_outputs = tf.reduce_sum(encoder_outputs * attention_scores, axis = 1)
		#	Dimensions of weighted_sum_of_encoder_outputs => (BATCH_SIZE, hidden_units)

		return weighted_sum_of_encoder_outputs, attention_scores
```

### 解码器Decoder ( 增加注意力机制)

代码在decoder类中增加了以下步骤。
- 就像编码器一样，我们在这里也有一个嵌入层用于目标语言中的序列。序列中的每一个单词都在具有相似意义的相似单词的嵌入空间中表示。
- 我们也得到的加权和编码器输出通过使用当前解码隐藏状态和编码器输出。这是通过调用我们的注意力层来实现的。

我们将以上两步得到的结果(嵌入空间序列的表示和编码器输出的加权和)串联起来。这个串联张量被发送到我们的解码器的GRU层。

这个GRU层的输出被发送到一个稠密层，这个稠密层给出所有hindivocabsize的单词出现的概率。具有高概率的单词意味着模型认为这个单词应该是下一个单词。

![](https://p6-tt.byteimg.com/origin/pgc-image/ec5880f5f7fd4f85b7e1fb457f9f9ae8?from=pc)

```python
class Decoder(tf.keras.Model):
	def __init__(self, hindi_vocab_size, embedding_dim, hidden_units):
		super(Decoder, self).__init__()
		self.embedding = tf.keras.layers.Embedding(hindi_vocab_size, embedding_dim)
		self.gru = tf.keras.layers.GRU(hidden_units, return_state = True)
		self.word_probability_layer = tf.keras.layers.Dense(hindi_vocab_size, activation = 'softmax')
		self.attention_layer = BasicDotProductAttention()
		
	def call(self, decoder_input, decoder_hidden, encoder_sequence_output):
		
		x = self.embedding(decoder_input)
		#	Dimensions of x => (BATCH_SIZE, embedding_dim)
		weighted_sum_of_encoder_outputs, attention_scores = self.attention_layer(decoder_hidden, encoder_sequence_output)
		#	Dimensions of weighted_sum_of_encoder_outputs => (BATCH_SIZE, hidden_units)
		x = tf.concat([weighted_sum_of_encoder_outputs, x], axis = -1)
		x = tf.expand_dims(x, 1)
		#	Dimensions of x => (BATCH_SIZE, 1, hidden_units + embedding_dim)
		decoder_output, decoder_state = self.gru(x)
		#	Dimensions of decoder_output => (BATCH_SIZE, hidden_units)
		word_probability = self.word_probability_layer(decoder_output)
		#	Dimensions of word_probability => (BATCH_SIZE, hindi_vocab_size)
		return word_probability, decoder_state, attention_scores

# initialize our decoder
decoder = Decoder(hindi_vocab_size, embedding_dim, hidden_units)
```

### 训练

定义我们的损失函数和优化器。选择了稀疏分类交叉熵和Adam优化器。每个训练步骤如下：
1. 从编码器对象获取编码器序列输出和编码器最终隐藏状态。编码器序列输出用于查找注意力分数，编码器最终隐藏状态将成为解码器的初始隐藏状态。
1. 对于目标语言中预测的每个单词，我们将输入单词、前一个解码器隐藏状态和编码器序列输出作为解码器对象的参数。返回单词预测概率和当前解码器隐藏状态。
1. 将概率最大的字作为下一个解码器GRU单元(解码器对象)的输入，当前解码器隐藏状态成为下一个解码器GRU单元的输入隐藏状态。
1. 损失通过单词预测概率和目标句中的实际单词计算，并向后传播

在每个epoch中，每次调用上述训练步骤，最后存储并绘制每个epoch对应的损失。

附注:在第1步，为什么我们仍然使用编码器的最终隐藏状态作为我们的解码器的第一个隐藏状态?

这是因为，如果我们这样做，seq2seq模型将被优化为一个单一系统。反向传播是端到端进行的。我们不想分别优化编码器和解码器。并且，没有必要通过这个隐藏状态来获取源序列信息，因为我们已经有注意力机制了:)

```python
optimizer = tf.keras.optimizers.Adam(learning_rate = learning_rate)
loss_object = tf.keras.losses.SparseCategoricalCrossentropy(reduction='none')
def loss_function(actual_words, predicted_words_probability):
	loss = loss_object(actual_words, predicted_words_probability)
	mask = tf.where(actual_words > 0, 1.0, 0.0)
	return tf.reduce_mean(mask * loss)

def train_step(english_sequences, hindi_sequences):
	loss = 0
	with tf.GradientTape() as tape:
		encoder_sequence_output, encoder_hidden = encoder(english_sequences)
		decoder_hidden = encoder_hidden
		decoder_input = hindi_sequences[:, 0]
		for i in range(1, hindi_sequences.shape[1]):
			predicted_words_probability, decoder_hidden, _ = decoder(decoder_input, decoder_hidden, encoder_sequence_output)
			actual_words = hindi_sequences[:, i]
			# if all the sentences in batch are completed
			if np.count_nonzero(actual_words) == 0:
				break
			loss += loss_function(actual_words, predicted_words_probability)

			decoder_input = actual_words

	variables = encoder.trainable_variables + decoder.trainable_variables
	gradients = tape.gradient(loss, variables)
	optimizer.apply_gradients(zip(gradients, variables))
	return loss.numpy()

all_epoch_losses = []
training_start_time = time.time()
for epoch in range(epochs):
	epoch_loss = []
	start_time = time.time()
	for(batch, (english_sequences, hindi_sequences)) in enumerate(dataset):
		batch_loss = train_step(english_sequences, hindi_sequences)
		epoch_loss.append(batch_loss)

	all_epoch_losses.append(sum(epoch_loss)/len(epoch_loss))
	print("Epoch No.: " + str(epoch) + " Time: " + str(time.time()-start_time))

print("All Epoch Losses: " + str(all_epoch_losses))
print("Total time in training: " + str(time.time() - training_start_time))

plt.plot(all_epoch_losses)
plt.xlabel("Epochs")
plt.ylabel("Epoch Loss")
plt.show()
```

### 测试

定义了一个函数，该函数接受一个英语句子，并按照模型的预测返回一个印地语句子。让我们实现这个函数，我们将在下一节中看到结果的好坏。
1. 我们接受英语句子，对其进行预处理，并将其转换为长度为MAXWORDSINASENTENCE的序列或向量，如开头的"预处理数据"部分所述。
1. 这个序列被输入到我们训练好的编码器，编码器返回编码器序列输出和编码器的最终隐藏状态。
1. 编码器的最终隐藏状态是译码器的第一个隐藏状态，译码器的第一个输入字是一个开始标记"sentencestart"。
1. 解码器返回预测的字概率。概率最大的单词成为我们预测的单词，并被附加到最后的印地语句子中。这个字作为输入进入下一个解码器层。
1. 预测单词的循环将继续下去，直到解码器预测结束标记"sentenceend"或单词数量超过某个限制(我们将这个限制保持为MAXWORDSINASENTENCE的两倍)。

```python
def get_sentence_from_sequences(sequences, tokenizer):
	return tokenizer.sequences_to_texts(sequences)

# Testing
def translate_sentence(sentence):
	sentence = preprocess_sentence(sentence, True)
	sequence = english_tokenizer.texts_to_sequences([sentence])[0]
	sequence = tf.keras.preprocessing.sequence.pad_sequences([sequence], maxlen = MAX_WORDS_IN_A_SENTENCE, padding = 'post')
	encoder_input = tf.convert_to_tensor(sequence)
	encoder_sequence_output, encoder_hidden = encoder(encoder_input)
	decoder_input = tf.convert_to_tensor([hindi_word_index['sentencestart']])
	decoder_hidden = encoder_hidden
	
	sentence_end_word_id = hindi_word_index['sentenceend']
	hindi_sequence = []
	for i in range(MAX_WORDS_IN_A_SENTENCE*2):
		predicted_words_probability, decoder_hidden, _ = decoder(decoder_input, decoder_hidden, encoder_sequence_output)
		# taking the word with maximum probability
		predicted_word_id = tf.argmax(predicted_words_probability[0]).numpy()
		hindi_sequence.append(predicted_word_id)
		# if the word 'sentenceend' is predicted, exit the loop
		if predicted_word_id == sentence_end_word_id:
			break
		decoder_input = tf.convert_to_tensor([predicted_word_id])
	print(sentence)
	return get_sentence_from_sequences([hindi_sequence], hindi_tokenizer)

# print translated sentence
print(translate_sentence("Try multiple sentences here to check how good model is working!"))
```

### 结果

NVidia K80 GPU Kaggle，在上面的代码。100个epoch，需要70分钟的训练。损失与epoch图如下所示。

![](https://p1-tt.byteimg.com/origin/pgc-image/2f46b1f2f52f440a9a47c9841290e98c?from=pc)

经过35个epoch的训练后，我尝试向我们的translate_sentence()函数中添加随机的英语句子，结果有些令人满意，但也有一定的问题。超参数并不是与实际翻译有一些偏差的唯一原因。

改进点：
- 使用堆叠GRU编码器和解码器
- 使用不同形式的注意力机制
- 使用不同的优化器
- 增加数据集的大小
- 采用Beam Search代替Greedy decoding：Beam Search解码从单词概率分布中考虑最高k个可能的单词，并检查所有的可能性

### Seq2Seq的优化技巧

#### 1、Teacher Forcing

Teacher Forcing 用于训练阶段，预测过程是都是一样的。

训练时：如果不使用 Teacher Forcing，输入包括了上一个神经元的输出 **y'**。如果上一个神经元的输出是错误的，则下一个神经元的输出也很容易错误，导致错误会一直传递下去。

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-a7cf394b2d40a052.png)

使用 Teacher Forcing，神经元直接使用正确的输出作为当前神经元的输入。

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-1ed6e410da784c5e.png)

#### 2、Attention

- 上面的Seq2Seq 模型中，**Encoder** 总是将源句子的所有信息编码到一个固定长度的上下文向量 **c** 中，然后在 **Decoder** 解码的过程中向量 **c** 都是不变的。这存在着不少缺陷：
  - 对于比较长的句子，很难用一个定长的向量 **c** 完全表示其意义。
  - RNN 存在长序列梯度消失的问题，只使用最后一个神经元得到的向量 **c** 效果不理想。
  - 人在阅读文章的时候，会把注意力放在当前的句子上，这种结构没有利用这点。
- 引入attention 机制，就是一种将模型的注意力放在当前翻译单词上的一种机制。
  - 例如翻译 "I have a cat"，翻译到 "我" 时，要将注意力放在源句子的 "I" 上，翻译到 "猫" 时要将注意力放在源句子的 "cat" 上。

使用了 Attention 后：

- **Decoder** 的输入就不是固定的上下文向量 **c** 了
- 而是会根据当前翻译的信息，计算当前的 **c**

![img](https://gitee.com/summerrat/images/raw/master/img/20030902-3caa6122e9b613c5.png)

- Attention 需要保留 Encoder 每一个神经元的隐藏层向量 **h**
- 然后 Decoder 的第 t 个神经元要根据上一个神经元的隐藏层向量 **h'**t-1 计算出当前状态与 Encoder 每一个神经元的相关性 **e**t
- **e**t 是一个 N 维的向量 (Encoder 神经元个数为 N)，若 **e**t 的第 i 维越大，则说明当前节点与 Encoder 第 i 个神经元的相关性越大。**e**t 的计算方法有很多种，即相关性系数的计算函数 a 有很多种：
  - ![img](https://gitee.com/summerrat/images/raw/master/img/20030902-d6ef2d1879205693.png)
- 上面得到相关性向量 **e**t 后，需要进行归一化，使用 softmax 归一化。然后用归一化后的系数融合 Encoder 的多个隐藏层向量得到 Decoder 当前神经元的上下文向量 **c**t：
  - ![img](https://gitee.com/summerrat/images/raw/master/img/20030902-0b435754763ef850.png)

#### 3、beam search

- beam search 方法不用于训练的过程，而是用在测试的。训练的时候因为知道正确答案，不需要再进行搜索。
  - 在每一个神经元中，我们都选取当前输出概率值最大的 **top k** 个输出传递到下一个神经元。
  - 下一个神经元分别用这 k 个输出，计算出 L 个单词的概率 (L 为词汇表大小)，然后在 kL 个结果中得到 **top k** 个最大的输出，重复这一步骤。后面会不断重复这个过程，直到遇到结束符或者达到最大长度为止。最终输出得分最高的2个序列。
- 例如
  - 预测的时候，假设词表大小为3，内容为a，b，c。beam size是2，decoder解码的时候：
    - 1： 生成第1个词的时候，选择概率最大的2个词，假设为a,c,那么当前的2个序列就是a和c。
    - 2：生成第2个词的时候，我们将当前序列a和c，分别与词表中的所有词进行组合，得到新的6个序列aa ab ac ca cb cc，计算每个序列的得分并选择得分最高2个序列，作为新的当前序列，假如为aa cb。
    - 3：后面会不断重复这个过程，直到遇到结束符或者达到最大长度为止。最终输出得分最高的2个序列。

#### 前缀树解码

【2021-12-17】
- 苏剑林：[Seq2Seq+前缀树：检索任务新范式（以KgCLUE为例）](https://spaces.ac.cn/archives/7115)
- ICLR2021上，Facebook的论文《[Autoregressive Entity Retrieval](https://arxiv.org/abs/2010.00904)》同样利用“Seq2Seq+前缀树”的组合，在实体链接和文档检索上做到了效果与效率的“双赢”。
- “Seq2Seq+前缀树”的组合理论上可以用到任意检索型任务中，堪称是检索任务的“新范式”。

前缀树来约束解码

语料：

```shell
明月几时有
明天会更好
明天下雨
明天下午开会
明天下午放假
明年见
今夕是何年
今天去哪里玩
```

前缀树来存储这些句子：
- ![](https://spaces.ac.cn/usr/uploads/2021/12/2908323201.png)
- 从左往右地把相同位置的相同token聚合起来，树上的每一条完整路径（以\[BOS]开头、\[EOS]结尾）都代表数据库中的一个句子。之所以叫“前缀树”，是因为利用这种树状结构，我们可以很快地查找到以某个前缀开头的字/句有哪些。比如从上图中我们可以看到，第一个字只可能是“明”或“今”，“明”后面只能接“月”、“天”或“年”，“明天”后面只能接“会”或“下”，等等。
- 第一个字只能是“明”或“今”，那么在预测第一个字的时候，我们可以把模型预测其他字的概率都置零，这样模型只可能从这两个字中二选一；如果已经确定了第一个字，比如“明”，那么我们在预测第二个字的时候，同样可以将“月”、“天”或“年”以外的字的概率都置零，这样模型只可能从这三个字中选一个，结果必然是“明月”、“明天”、“明年”之一；依此类推，保证解码过程只走前缀树的分支，而且必须走到最后，这样解码出来的结果必然是数据库中已有的句子。
- 相比常规的向量检索方案，“Seq2Seq+前缀树”的方案用前缀树代替了要储存的检索向量，而前缀树本质上是原始句子的一种“压缩表示”，所以不难想象前缀树所需要的储存空间要比稠密的检索向量要少得多。

Python实现前缀树比较简单的方案就是利用字典结构来实现嵌套
- [KgCLUE前缀树代码](https://github.com/bojone/KgCLUE-bert4keras)

效果：排行榜第二
- ![](https://spaces.ac.cn/usr/uploads/2021/12/1938819031.png)

“Seq2Seq+前缀树”或许可以在评测指标上取得不错的效果，但它对于工程来说，有一个不大有好的特点，就是修正bad case会变得比较困难，因为传统方法修正bad case，你只需要不断加样本就行了，而“Seq2Seq+前缀树”则需要你修改解码过程，这通常困难得多。


## RNN系列

- **机器翻译**是从RNN开始跨入神经网络机器翻译时代的，几个比较重要的阶段分别是: **Simple RNN**, **Contextualize RNN**, **Contextualized RNN with attention**, **Transformer**(2017)

### Simple RNN

- encoder-decoder模型结构中，encoder将整个源端序列(不论长度)压缩成一个向量(encoder output)，源端信息和decoder之间唯一的联系只是: encoder output会作为decoder的initial states的输入。这样带来一个显而易见的问题就是，随着decoder长度的增加，encoder output的信息会衰减。

![](https://pic3.zhimg.com/80/v2-b27fc5ee5d17d7954dc0c2b211482165_720w.jpg)

这种模型有2个主要的问题:
- 源端序列不论长短，都被统一压缩成一个**固定维度**的向量，并且显而易见的是这个向量中包含的信息中，关于源端序列末尾的token的信息更多，而如果序列很长，最终可能基本上<font color='red'>“遗忘”了序列开头的token的信息。</font>
- 第二个问题同样由RNN的特性造成: 随着decoder timestep的信息的增加，initial hidden states中包含的<font color='blue'>encoder output相关信息也会衰减</font>，decoder会逐渐“遗忘”源端序列的信息，而更多地关注目标序列中在该timestep之前的token的信息。

### Contextualized RNN

- 为了解决上述第二个问题，即**encoder output随着decoder timestep增加而信息衰减**的问题，有人提出了一种加了context的RNN sequence to sequence模型：
    - decoder在每个timestep的input上都会加上一个context。
- 为了方便理解，我们可以把这看作是encoded source sentence。这样就可以在decoder的每一步，都把源端的整个句子的信息和target端当前的token一起输入到RNN中，防止源端的context信息随着timestep的增长而衰减。

![](https://pic4.zhimg.com/80/v2-305acd420c4192c43954aaa430f7910b_720w.jpg)

- 但是这样依然有一个问题: context对于每个timestep都是静态的(encoder端的final hidden states，或者是所有timestep的output的平均值)。但是每个decoder端的token在解码时用到的context真的应该是一样的吗？在这样的背景下，Attention就应运而生了

### Contextualized RNN with soft align (Attention)

- Attention在机器翻译领域的应用最早的提出来自于2014年的一篇论文 [Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/pdf/1409.0473.pdf)
- ![](https://pic3.zhimg.com/80/v2-0137ab38a12f427925541ada8fd9f94f_720w.jpg)
- 在每个timestep输入到decoder RNN结构中之前，会用当前的输入token的vector与encoder output中的每一个position的vector作一个"attention"操作，这个"attention"操作的目的就是计算当前token与每个position之间的"相关度"，从而决定每个position的vector在最终该timestep的context中占的比重有多少。最终的context即encoder output每个位置vector表达的加权平均。
- ![](https://pic1.zhimg.com/80/v2-d0db8ab72a9319b5fad32acf32d86db7_720w.png)


## 序列解码

- 生成式任务比普通的分类、tagging等NLP任务复杂不少。生成时，模型的输出是一个时间步一个时间步依次获得，前面时间步的结果影响后面时间步的结果。即每一个时间步，模型给出的都是<font color='blue'>基于历史生成结果的条件概率</font>。
- 生成完整的句子，需要一个称为`解码`的额外动作来融合模型多个时间步的输出，使得最终序列的每一步条件概率连乘起来最大。
- 分析
    - 每一个时间步可能的输出种类称为`字典大小`(vocabulary size，我们用v表示)
    - 进行T步随机的生成可能获得的结果总共有vT种。
    - 拿中文文本生成来说，v的值大约是5000-6000，即常用汉字的个数。
- 在如此大的基数下，遍历整个生成空间是不现实的。


### 贪心 Greedy Search

- `贪心搜索`，即每一个时间步都取条件概率最大的输出，再将从开始到当前步的结果作为输入去获得下一个时间步的输出，直到模型给出生成结束的标志。
- 示例，生成序列[A,B,C]
    - ![](http://www.wuyuanhao.com/wp-content/uploads/2020/03/greedy.png)
- 分析
    - 优点
        - 原来指数级别的求解空间直接压缩到了与长度线性相关的大小。（指数级→线性级）
    - 缺点
        - 由于丢弃了绝大多数的可能解，这种关注当下的策略<font color='red'>无法保证最终序列概率是最优的</font>。

### 集束搜索 Beam Search

- Beam search是对贪心策略一个改进。
- 思路：稍微放宽一些考察的范围。
    - 在每一个时间步，不再只保留当前分数最高的1个输出，而是保留num_beams个。
    - 当num_beams=1时集束搜索就退化成了贪心搜索。
- 示例
    - 每个时间步有ABCDE共5种可能的输出，即v=5v=5，图中的num_beams=2，也就是说每个时间步都会保留到当前步为止条件概率最优的2个序列
    - ![](http://www.wuyuanhao.com/wp-content/uploads/2020/03/beam-search.png)
- 分析
    - beam search在每一步需要考察的候选人数量是贪心搜索的num_beams倍
    - BS是一种时间换性能的方法。
    - 缺点
        - 会遇到诸如词语重复问题

#### 序列扩展

- 序列扩展是beam search的核心过程
- ![](http://www.wuyuanhao.com/wp-content/uploads/2020/03/seqextend-1024x695.png)

#### 代码解析

- NLP界著名Python包[Transformers](https://github.com/huggingface/transformers)
- 解析过程见：[解读Beam Search (1/2)](http://www.wuyuanhao.com/2020/03/20/%e8%a7%a3%e8%af%bbbeam-search-1-2/)


### Beam Search改进

- Beam Search虽然比贪心有所改进，但还是会生成出空洞、重复、前后矛盾的文本。
- 试图最大化序列条件概率的解码策略从根上就有问题
- 人类选择的词（橙线）并不是像机器选择的（蓝线）那样总是那些条件概率最大的词。
    - 总是选择概率大的词会发生正反馈从而陷入重复，从生成的结果也可以看出，机器生成的结果有大量重复。
    - ![](http://www.wuyuanhao.com/wp-content/uploads/2020/03/probability.png)
- 参考：[解读Beam Search (2/2)](http://www.wuyuanhao.com/2020/03/23/%e8%a7%a3%e8%af%bbbeam-search-2/)
- 以下思路主要源自ICLR 2020论文：[《The Curious Case of Neural Text Degeneration》](https://arxiv.org/abs/1904.09751)

#### 随机采样(sampling)

- 随机采样：根据解码器输出的词典中每个词的概率分布随机抽样。
    - 相比于按概率“掐尖”，这样会增大所选词的范围，引入更多的随机性。
- 采样的时候有一个可以控制的超参数，称为**温度**(temperature, T)。
    - 模型蒸馏里用到
- 解码器的输出层后面通常会跟一个softmax函数来将输出概率归一化，通过改变T可以控制概率的形貌。
- softmax的公式如下
    - 当T大的时候，概率分布趋向平均，随机性增大；
    - 当T小的时候，概率密度趋向于集中，即强者俞强，随机性降低，会更多地采样出“放之四海而皆准”的词汇。

- 谷歌开放式聊天机器人Meena采用的方式，论文结论是：
    - 这种随机采样的方法远好于Beam Search。
    - 但这其实也是有条件的，随机采样容易产生前后不一致的问题。
    - 而在开放闲聊领域，生成文本的 长度都比较短 ，这种问题就被自然的淡化了。

#### top-k采样

- 采样前将输出的概率分布截断，取出概率最大的k个词构成一个集合，然后将这个子集词的概率再归一化，最后重新的概率分布中采样词汇。
- 据说可以获得比Beam Search好很多的效果，但有个问题，就是这个**k不太好选**。
    - 概率分布变化比较大，有时候可能很**均匀**(flat)，有的时候比较**集中**(peaked)。
    - [图](http://www.wuyuanhao.com/wp-content/uploads/2020/03/distribution.png) ![图](http://www.wuyuanhao.com/wp-content/uploads/2020/03/distribution.png)
    - 对于集中的情况还好说，当分布均匀时，一个较小的k容易丢掉很多优质候选词。
    - 但如果k定的太大，这个方法又会退化回普通采样。

#### 核采样（Nucleus sampling) —— Top-p采样

- 不再取一个固定的k，而是固定候选集合的概率密度和在整个概率分布中的比例
- 选出来这个集合之后也和top-k采样一样，重新归一化集合内词的概率，并把集合外词的概率设为0。
- 这种方式也称为top-p采样。

#### 惩罚重复

- 为了解决重复问题，还有可以通过惩罚因子将出现过词的概率变小或者强制不使用重复词来解决。

#### 代码实现

- 上述各种采样方式在HuggingFace的库里都已经实现了

```python
# 代码输入的是logits，而且考虑很周全（我感觉漏了考虑k和p都给了的情况，这应该是不合适的）
# 巧妙地使用了torch.cumsum
# 避免了一个词都选不出来的尴尬情况
def top_k_top_p_filtering(logits, top_k=0, top_p=1.0, filter_value=-float("Inf"), min_tokens_to_keep=1):
    """ Filter a distribution of logits using top-k and/or nucleus (top-p) filtering
        Args:
            logits: logits distribution shape (batch size, vocabulary size)
            if top_k > 0: keep only top k tokens with highest probability (top-k filtering).
            if top_p < 1.0: keep the top tokens with cumulative probability >= top_p (nucleus filtering).
                Nucleus filtering is described in Holtzman et al. (http://arxiv.org/abs/1904.09751)
            Make sure we keep at least min_tokens_to_keep per batch example in the output
        From: https://gist.github.com/thomwolf/1a5a29f6962089e871b94cbd09daf317
    """
    if top_k > 0:
        top_k = min(max(top_k, min_tokens_to_keep), logits.size(-1))  # Safety check
        # Remove all tokens with a probability less than the last token of the top-k
        indices_to_remove = logits < torch.topk(logits, top_k)[0][..., -1, None]
        logits[indices_to_remove] = filter_value

    if top_p < 1.0:
        sorted_logits, sorted_indices = torch.sort(logits, descending=True)
        cumulative_probs = torch.cumsum(F.softmax(sorted_logits, dim=-1), dim=-1)

        # Remove tokens with cumulative probability above the threshold (token with 0 are kept)
        sorted_indices_to_remove = cumulative_probs > top_p
        if min_tokens_to_keep > 1:
            # Keep at least min_tokens_to_keep (set to min_tokens_to_keep-1 because we add the first one below)
            sorted_indices_to_remove[..., :min_tokens_to_keep] = 0
        # Shift the indices to the right to keep also the first token above the threshold
        sorted_indices_to_remove[..., 1:] = sorted_indices_to_remove[..., :-1].clone()
        sorted_indices_to_remove[..., 0] = 0

        # scatter sorted tensors to original indexing
        indices_to_remove = sorted_indices_to_remove.scatter(1, sorted_indices, sorted_indices_to_remove)
        logits[indices_to_remove] = filter_value
    return logits
```


## Attention的细节
 
### 2.1. 点积attention
 
介绍一下attention的具体计算方式。attention可以有很多种计算方式: 
- 加性attention
- 点积attention
- 还有带参数的计算方式

着重介绍一下点积attention的公式:
 
![[公式]](https://www.zhihu.com/equation?tex=%5Ctext+%7B+Attention+%7D%28Q%2C+K%2C+V%29%3D%5Coperatorname%7Bsoftmax%7D%5Cleft%28%5Cfrac%7BQ+K%5E%7BT%7D%7D%7B%5Csqrt%7Bd_%7Bk%7D%7D%7D%5Cright%29+V)
 
![](https://pic2.zhimg.com/80/v2-dc8921bfabcdf2515472b88a0808d046_720w.jpg)
 
- Attention中(Q^T)*K矩阵计算，query和key的维度要保持一致
 
如上图所示， ![[公式]](https://www.zhihu.com/equation?tex=Q_%7BM%5Ctimes+d%7D) , ![[公式]](https://www.zhihu.com/equation?tex=K_%7BN%5Ctimes+d%7D) 分别是query和key，其中，query可以看作M个维度为d的向量(长度为M的sequence的向量表达)拼接而成，key可以看作N个维度为d的向量(长度为N的sequence的向量表达)拼接而成。
*   【一个小问题】为什么有缩放因子 ![[公式]](https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7B%5Csqrt%7Bd_k%7D%7D) ?
*   先一句话回答这个问题: 缩放因子的作用是归一化。
*   假设![[公式]](https://www.zhihu.com/equation?tex=Q) , ![[公式]](https://www.zhihu.com/equation?tex=K)里的元素的均值为0，方差为1，那么 ![[公式]](https://www.zhihu.com/equation?tex=A%5ET%3DQ%5ETK) 中元素的均值为0，方差为d. 当d变得很大时， ![[公式]](https://www.zhihu.com/equation?tex=A) 中的元素的方差也会变得很大，如果 ![[公式]](https://www.zhihu.com/equation?tex=A) 中的元素方差很大，那么![[公式]](https://www.zhihu.com/equation?tex=%5Coperatorname%7Bsoftmax%7D%5Cleft%28A%5Cright%29) 的分布会趋于陡峭(分布的方差大，分布集中在绝对值大的区域)。总结一下就是![[公式]](https://www.zhihu.com/equation?tex=%5Coperatorname%7Bsoftmax%7D%5Cleft%28A%5Cright%29)的分布会和d有关。因此![[公式]](https://www.zhihu.com/equation?tex=A) 中每一个元素乘上 ![[公式]](https://www.zhihu.com/equation?tex=%5Cfrac%7B1%7D%7B%5Csqrt%7Bd_k%7D%7D) 后，方差又变为1。这使得![[公式]](https://www.zhihu.com/equation?tex=%5Coperatorname%7Bsoftmax%7D%5Cleft%28A%5Cright%29) 的分布“陡峭”程度与d解耦，从而使得训练过程中梯度值保持稳定。
    
 
### 2.2. Attention机制涉及到的参数
 
一个完整的attention层涉及到的参数有:
*   把![[公式]](https://www.zhihu.com/equation?tex=q) , ![[公式]](https://www.zhihu.com/equation?tex=k) , ![[公式]](https://www.zhihu.com/equation?tex=v)分别映射到![[公式]](https://www.zhihu.com/equation?tex=Q) , ![[公式]](https://www.zhihu.com/equation?tex=K) , ![[公式]](https://www.zhihu.com/equation?tex=V)的线性变换矩阵 ![[公式]](https://www.zhihu.com/equation?tex=W%5EQ) ( ![[公式]](https://www.zhihu.com/equation?tex=d_%7Bmodel%7D+%5Ctimes+d_k+) ), ![[公式]](https://www.zhihu.com/equation?tex=W%5EK)( ![[公式]](https://www.zhihu.com/equation?tex=d_%7Bmodel%7D+%5Ctimes+d_k) ), ![[公式]](https://www.zhihu.com/equation?tex=W%5EV) ( ![[公式]](https://www.zhihu.com/equation?tex=d_%7Bmodel%7D+%5Ctimes+d_v) )
*   把输出的表达 ![[公式]](https://www.zhihu.com/equation?tex=O) 映射为最终输出 ![[公式]](https://www.zhihu.com/equation?tex=o) 的线性变换矩阵 ![[公式]](https://www.zhihu.com/equation?tex=W%5EO) ( ![[公式]](https://www.zhihu.com/equation?tex=d_v+%5Ctimes+d_%7Bmodel%7D+) )
    
 
### 2.3. Query, Key, Value
 
Query和Key作用得到的attention权值作用到Value上。因此它们之间的关系是:
1.  Query![[公式]](https://www.zhihu.com/equation?tex=%28M%5Ctimes+d_%7Bqk%7D%29) 和Key![[公式]](https://www.zhihu.com/equation?tex=%28N%5Ctimes+d_%7Bqk%7D%29)的维度必须一致，Value ![[公式]](https://www.zhihu.com/equation?tex=%28N%5Ctimes+d_%7Bv%7D%29) 和Query/Key的维度可以不一致。
2.  Key![[公式]](https://www.zhihu.com/equation?tex=%28N%5Ctimes+d_%7Bqk%7D%29)和Value ![[公式]](https://www.zhihu.com/equation?tex=%28N%5Ctimes+d_%7Bv%7D%29)的长度必须一致。Key和Value本质上对应了同一个Sequence在不同空间的表达。
3.  Attention得到的Output ![[公式]](https://www.zhihu.com/equation?tex=%28M%5Ctimes+d_%7Bv%7D%29) 的维度和Value的维度一致，长度和Query一致。
4.  Output每个位置 i 是由value的所有位置的vector加权平均之后的向量；而其权值是由位置为i 的query和key的所有位置经过attention计算得到的 ，权值的个数等于key/value的长度。
 
![](https://pic4.zhimg.com/80/v2-7e7fcf5895d3cfc3f9f97b5c19069bbb_720w.jpg)
 
- Attention示意图
 
在经典的Transformer结构中，我们记线性映射之前的Query, Key, Value为q, k, v，映射之后为Q, K, V。那么:
1.  self-attention的q, k, v都是同一个输入, 即当前序列由上一层输出的高维表达。
2.  cross-attention的q代表当前序列，k,v是同一个输入，对应的是encoder最后一层的输出结果(对decoder端的每一层来说，保持不变)

而每一层线性映射参数矩阵都是独立的，所以经过映射后的Q, K, V各不相同，模型参数优化的目标在于将q, k, v被映射到新的高维空间，使得每层的Q, K, V在不同抽象层面上捕获到q, k, v之间的关系。一般来说，底层layer捕获到的更多是lexical-level的关系，而高层layer捕获到的更多是semantic-level的关系。
 
### 2.4. Attention的作用
 
下面这段我会以机器翻译为例，用通俗的语言阐释一下attention的作用，以及query, key, value的含义。
 
![](https://pic4.zhimg.com/80/v2-cca6e1f0dd02f08cc554d731362a08af_720w.jpg)
 
Transformer模型Encoder, Decoder的细节图（省去了FFN部分）
 
query对应的是需要被表达的序列(称为序列A)，key和value对应的是用来表达A的序列(称为序列B)。其中key和query是在同一高维空间中的(否则无法用来计算相似程度)，value不必在同一高维空间中，最终生成的output和value在同一高维空间中。上面这段巨绕的话用一句更绕的话来描述一下就是:
 
> 序列A和序列B在高维空间 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha) 中的高维表达 ![[公式]](https://www.zhihu.com/equation?tex=A_%7B%5Calpha%7D) 的每个位置分别和 ![[公式]](https://www.zhihu.com/equation?tex=B_%7B%5Calpha%7D) 计算相似度，产生的权重作用于序列B在高维空间 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) 中的高维表达 ![[公式]](https://www.zhihu.com/equation?tex=B_%7B%5Cbeta%7D) ，获得序列A在高维空间 ![[公式]](https://www.zhihu.com/equation?tex=%5Cbeta) 中的高维表达 ![[公式]](https://www.zhihu.com/equation?tex=A_%7B%5Cbeta%7D)
 
Encoder部分中只存在self-attention，而Decoder部分中存在self-attention和cross-attention
- 【self-attention】encoder中的self-attention的query, key, value都对应了源端序列(即A和B是同一序列)，decoder中的self-attention的query, key, value都对应了目标端序列。
- 【cross-attention】decoder中的cross-attention的query对应了目标端序列，key, value对应了源端序列(每一层中的cross-attention用的都是encoder的最终输出)
 
### 2.5. Decoder端的Mask
 
Transformer模型属于自回归模型（p.s. 非自回归的翻译模型我会专门写一篇文章来介绍），也就是说后面的token的推断是基于前面的token的。Decoder端的Mask的功能是为了保证训练阶段和推理阶段的一致性。
 
论文原文中关于这一点的段落如下：
 
> We also modify the self-attention sub-layer in the decoder stack to prevent from attending to subsequent positions. This masking, combined with the fact that the output embeddings are offset by one position, ensures that the predictions for position i can depend only on the known outputs at positions less than i.
 
在推理阶段，token是按照从左往右的顺序推理的。也就是说，在推理timestep=T的token时，decoder只能“看到”timestep < T的 T-1 个Token, 不能和timestep大于它自身的token做attention（因为根本还不知道后面的token是什么）。为了保证训练时和推理时的一致性，所以，训练时要同样防止token与它之后的token去做attention。
 
### 2.6. 多头Attention (Multi-head Attention)
 
Attention是将query和key映射到同一高维空间中去计算相似度，而对应的multi-head attention把query和key映射到高维空间 ![[公式]](https://www.zhihu.com/equation?tex=%5Calpha) 的不同子空间 ![[公式]](https://www.zhihu.com/equation?tex=%28%5Calpha_1%2C+%5Calpha_2%2C+...%2C%5Calpha_h%29) 中去计算相似度。
 
为什么要做multi-head attention？论文原文里是这么说的:
 
> Multi-head attention allows the model to jointly attend to information from different representation subspaces at different positions. With a single attention head, averaging inhibits this.
 
也就是说，这样可以在不改变参数量的情况下增强每一层attention的表现力。
 
![](https://pic3.zhimg.com/80/v2-3f8c3c102404c9b61398b63e06ffd80b_720w.jpg)
 
Multi-head Attention示意图
 
Multi-head Attention的本质是，在参数总量保持不变的情况下，将同样的query, key, value映射到原来的高维空间的不同子空间中进行attention的计算，在最后一步再合并不同子空间中的attention信息。这样降低了计算每个head的attention时每个向量的维度，在某种意义上防止了过拟合；由于Attention在不同子空间中有不同的分布，Multi-head Attention实际上是寻找了序列之间不同角度的关联关系，并在最后concat这一步骤中，将不同子空间中捕获到的关联关系再综合起来。
 
从上图可以看出， ![[公式]](https://www.zhihu.com/equation?tex=q_i) 和 ![[公式]](https://www.zhihu.com/equation?tex=k_j) 之间的attention score从1个变成了h个，这就对应了h个子空间中它们的关联度。
 
3. Transformer模型架构中的其他部分
 
### 3.1. Feed Forward Network
 
每一层经过attention之后，还会有一个FFN，这个FFN的作用就是空间变换。FFN包含了2层linear transformation层，中间的激活函数是ReLu。
 
曾经我在这里有一个百思不得其解的问题：attention层的output最后会和 ![[公式]](https://www.zhihu.com/equation?tex=W_O) 相乘，为什么这里又要增加一个2层的FFN网络？
 
其实，FFN的加入引入了非线性(ReLu激活函数)，变换了attention output的空间, 从而增加了模型的表现能力。把FFN去掉模型也是可以用的，但是效果差了很多。
 
### 3.2. Positional Encoding
 
位置编码层只在encoder端和decoder端的embedding之后，第一个block之前出现，它非常重要，没有这部分，Transformer模型就无法用。位置编码是Transformer框架中特有的组成部分，补充了Attention机制本身不能捕捉位置信息的缺陷。
 
![](https://pic4.zhimg.com/80/v2-42d5035562aca2c6136a2c8abaafc565_720w.jpg)
 
- position encoding
 
Positional Embedding的成分直接叠加于Embedding之上，使得每个token的位置信息和它的语义信息(embedding)充分融合，并被传递到后续所有经过复杂变换的序列表达中去。
 
论文中使用的Positional Encoding(PE)是正余弦函数，位置(pos)越小，波长越长，每一个位置对应的PE都是唯一的。同时作者也提到，之所以选用正余弦函数作为PE，是因为这可以使得模型学习到token之间的相对位置关系：因为对于任意的偏移量k， ![[公式]](https://www.zhihu.com/equation?tex=PE_%7Bpos%2Bk%7D) 可以由 ![[公式]](https://www.zhihu.com/equation?tex=PE_%7Bpos%7D) 的线性表示：
 
![[公式]](https://www.zhihu.com/equation?tex=PE_%7B%28pos%2Bk%2C2i%29%7D%3Dsin%5B%28pos%2Bk%29%2F10000%5E%7B2i%2Fd_%7Bmodel%7D%7D%5D)
 
![[公式]](https://www.zhihu.com/equation?tex=PE_%7B%28pos%2Bk%2C2i%2B1%29%7D%3Dcos%5B%28pos%2Bk%29%2F10000%5E%7B2i%2Fd_%7Bmodel%7D%7D%5D)
 
上面两个公式可以由 ![[公式]](https://www.zhihu.com/equation?tex=sin%5B%28pos%29%2F10000%5E%7B2i%2Fd_%7Bmodel%7D%7D%5D) 和![[公式]](https://www.zhihu.com/equation?tex=cos%5B%28pos%29%2F10000%5E%7B2i%2Fd_%7Bmodel%7D%7D%5D)的线性组合得到。也就是 ![[公式]](https://www.zhihu.com/equation?tex=PE_%7Bpos%7D)乘上某个线性变换矩阵就得到了![[公式]](https://www.zhihu.com/equation?tex=PE_%7Bpos%2Bk%7D)
 
p.s. 后续有一个工作在attention中使用了“相对位置表示” ([Self-Attention with Relative Position Representations](https://link.zhihu.com/?target=https%3A//www.aclweb.org/anthology/N18-2074.pdf)) ，有兴趣可以看看。
 
### 3.3. Layer Normalization
 
在每个block中，最后出现的是Layer Normalization。Layer Normalization是一个通用的技术，其本质是规范优化空间，加速收敛。
 
当我们使用梯度下降法做优化时，随着网络深度的增加，数据的分布会不断发生变化，假设feature只有二维，那么用示意图来表示一下就是：
 
![](https://pic3.zhimg.com/80/v2-59e1dc490d55d7b908f4e12c38cb80f8_720w.jpg)
 
数据的分布发生变化，左图比较规范，右图变得不规范
 
为了保证数据特征分布的稳定性（如左图），我们加入Layer Normalization，这样可以加速模型的优化速度。

- 以上内容摘自：[Transformer模型深度解读](https://zhuanlan.zhihu.com/p/104393915)


## Transformer模型

![](https://picb.zhimg.com/80/v2-6c292e2a4ed43894fc954ee625372c67_720w.jpg)

在上图的下面部分，训练用的输入和输出数据的embedding，都会先加上一个position encoding来补充一下位置信息。
- `Encoder`
   - 途中左侧部分是encoder块，encoder中6层相同结构堆叠而成，在每层中又可以分为2个子层，底下一层是multihead self-attention层，上面是一个FC feed-forward层，每一个子层都有residual connection，，然后在进行Layer Normalization. 为了引入redisual connenction简化计算，每个层的输入维数和embedding层保持一致。
- `Decoder`
   - 同样是一个6层的堆叠，每层有三个子层，其中底下两层都是multihead self-attention层，最底下一层是有mask的，只有当前位置之前的输入有效，中间层是encode和decode的连接层，输出的self-attention层和输入的encoder输出同时作为MSA的输入，实现encoder和decoder的连接，最上层和encoder的最上层是一样的，不在单说，每个子层都有有residual connection，和Layer Normalization

【2021-8-25】Transformer结构中，左边叫做**编码端**(Encoder)，右边叫做**解码端**(Decoder)。不要小看这两个部分，其中左边的编码端最后演化成了最后鼎鼎大名的**Bert**，右边的解码端在最近变成了无人不知的**GPT**模型。

### 亮点

- `Self Attention`
   - 传统的编解码结构中，将输入输入编码为一个定长语义编码，然后通过这个编码在生成对应的输出序列。它存在的一个问题在于：输入序列不论长短都会被编码成一个固定长度的向量表示，而解码则受限于该固定长度的向量表示
   - attention机制: encoder的输出不是一个语义向量，是一个语义向量的序列
   ![](https://upload-images.jianshu.io/upload_images/14911967-cadfa37d31342857.png?imageMogr2/auto-orient/strip|imageView2/2/w/568/format/webp)
   - Transformer的Attenion函数称为scaled dot-Product Attention
   ![](https://upload-images.jianshu.io/upload_images/14911967-9fb3d576399e53e5.png?imageMogr2/auto-orient/strip|imageView2/2/w/455/format/webp)
- `MultiHead Attention`
   - self attention计算时会分为两个阶段，第一个阶段计算出softmax部分,第二部分是在乘以 Value部分，这样还是串行化的，并行化不够。
   - MultiHeadAttention，对query，key，value各自进行一次不同的线性变换，然后在执行一次softmax操作，这样可以提升并行度，论文中的head数是8个

![](https://upload-images.jianshu.io/upload_images/14911967-b31aa04d8628b8da.png?imageMogr2/auto-orient/strip|imageView2/2/w/600/format/webp)
- position Encoding
   - 语言是有序的，在cnn中，卷积的形状包含了位置信息，在rnn中，位置的先后顺序其实是通过送入模型的先后来保证。transformer抛弃了cnn和rnn，那么数据的位置信息怎么提供呢？
   - Transformer通过position Encoding来额外的提供位置信息，每一个位置对应一个向量，这个向量和word embedding求和后作为 encoder和decoder的输入。这样，对于同一个词语来说，在不同的位置，他们送入encoder和decoder的向量不同。


### 总结

- 结构
![](https://upload-images.jianshu.io/upload_images/14911967-dec395c8d1d19f18.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
- 训练过程
![](https://upload-images.jianshu.io/upload_images/14911967-ca45ad4ea6c91e77.gif?imageMogr2/auto-orient/strip|imageView2/2/w/640/format/webp)

作者：[Transformer模型学习](https://www.jianshu.com/p/04b6dd396d62)

## 图解Transformer
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/),中文翻译：[BERT大火却不懂Transformer？](https://zhuanlan.zhihu.com/p/54523019)
- [jalammar github repo](https://github.com/jalammar/jalammar.github.io/blob/master/_posts/2018-06-27-illustrated-transformer.md)
![](https://jalammar.github.io/images/t/transformer_resideual_layer_norm_3.png)

![](https://camo.githubusercontent.com/88e8f36ce61dedfd2491885b8df2f68c4d1f92f5/687474703a2f2f696d6775722e636f6d2f316b72463252362e706e67)


## [Transformer模型的PyTorch实现](https://luozhouyang.github.io/transformer/)

- Google 2017年的论文 [Attention is all you need](https://arxiv.org/abs/1706.03762) 阐释了什么叫做大道至简！该论文提出了**Transformer**模型，完全基于**Attention mechanism**，抛弃了传统的**RNN**和**CNN**。
- 我们根据论文的结构图，一步一步使用 [PyTorch](https://github.com/pytoch/pytorch) 实现这个**Transformer**模型。

## Transformer架构

- 首先看一下transformer的结构图：  
![transformer_architecture](http://blog.stupidme.me/wp-content/uploads/2018/09/transformer.jpg)  

解释一下这个结构图。首先，**Transformer**模型也是使用经典的**encoer-decoder**架构，由encoder和decoder两部分组成。
- 上图的左半边用`Nx`框出来的，就是我们的encoder的一层。encoder一共有6层这样的结构。
- 上图的右半边用`Nx`框出来的，就是我们的decoder的一层。decoder一共有6层这样的结构。
- 输入序列经过**word embedding**和**positional encoding**相加后，输入到encoder。
- 输出序列经过**word embedding**和**positional encoding**相加后，输入到decoder。
- 最后，decoder输出的结果，经过一个线性层，然后计算softmax。

**word embedding**和**positional encoding**我后面会解释。我们首先详细地分析一下encoder和decoder的每一层是怎么样的。

## Encoder

encoder由6层相同的层组成，每一层分别由两部分组成：
- * 第一部分是一个**multi-head self-attention mechanism**
- * 第二部分是一个**position-wise feed-forward network**，是一个全连接层
两个部分，都有一个　**残差连接(residual connection)**，然后接着一个**Layer Normalization**。

如果你是一个新手，你可能会问：
- * multi-head self-attention 是什么呢？
- * 参差结构是什么呢？
- * Layer Normalization又是什么？

这些问题我们在后面会一一解答。

## Decoder

和encoder类似，decoder由6个相同的层组成，每一个层包括以下3个部分：

* 第一个部分是**multi-head self-attention mechanism**
* 第二部分是**multi-head context-attention mechanism**
* 第三部分是一个**position-wise feed-forward network**

还是和encoder类似，上面三个部分的每一个部分，都有一个**残差连接**，后接一个**Layer Normalization**。

但是，decoder出现了一个新的东西**multi-head context-attention mechanism**。这个东西其实也不复杂，理解了**multi-head self-attention**你就可以理解**multi-head context-attention**。这个我们后面会讲解。

## Attention机制

在讲清楚各种attention之前，我们得先把attention机制说清楚。

通俗来说，**attention**是指，对于某个时刻的输出`y`，它在输入`x`上各个部分的注意力。这个注意力实际上可以理解为**权重**。

attention机制也可以分成很多种。[Attention? Attention!](https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html) 一文有一张比较全面的表格：  
![attention_mechanism](http://blog.stupidme.me/wp-content/uploads/2018/09/attention_mechanism_table.png)  
*Figure 2. a summary table of several popular attention mechanisms.*  

上面第一种**additive attention**你可能听过。以前我们的seq2seq模型里面，使用attention机制，这种**加性注意力(additive attention)**用的很多。Google的项目 [tensorflow/nmt](https://github.com/tensorflow/nmt) 里面这两种attention机制都有实现。

为什么这种attention叫做**additive attention**呢？很简单，对于输入序列隐状态$h_i$和输出序列的隐状态$s_t$，它的处理方式很简单，直接**合并**，变成$[s_t;h_i]$

但是我们的transformer模型使用的不是这种attention机制，使用的是另一种，叫做**乘性注意力(multiplicative attention)**。

那么这种**乘性注意力机制**是怎么样的呢？从上表中的公式也可以看出来：**两个隐状态进行点积**！

### Self-attention是什么？
到这里就可以解释什么是**self-attention**了。

上面我们说attention机制的时候，都会说到两个隐状态，分别是$h_i$和$s_t$，前者是输入序列第i个位置产生的隐状态，后者是输出序列在第t个位置产生的隐状态。

所谓**self-attention**实际上就是，**输出序列**就是**输入序列**！因此，计算自己的attention得分，就叫做**self-attention**！

### Context-attention是什么？
知道了**self-attention**，那你肯定猜到了**context-attention**是什么了：**它是encoder和decoder之间的attention**！所以，你也可以称之为**encoder-decoder attention**!

**context-attention**一词并不是本人原创，有些文章或者代码会这样描述，我觉得挺形象的，所以在此沿用这个称呼。其他文章可能会有其他名称，但是不要紧，我们抓住了重点即可，那就是**两个不同序列之间的attention**，与**self-attention**相区别。

不管是**self-attention**还是**context-attention**，它们计算attention分数的时候，可以选择很多方式，比如上面表中提到的：

* additive attention
* local-base
* general
* dot-product
* scaled dot-product

那么我们的Transformer模型，采用的是哪种呢？答案是：**scaled dot-product attention**。

### Scaled dot-product attention是什么？

论文[Attention is all you need](https://arxiv.org/abs/1706.03762)里面对于attention机制的描述是这样的：
> An attention function can be described as a query and a set of key-value pairs to an output, where the query, keys, values, and output are all vectors. The output is computed as a weighted sum of the values, where the weight assigned to each value is computed by a compatibility of the query with the corresponding key.

这句话描述得很清楚了。翻译过来就是：**通过确定Q和K之间的相似程度来选择V**！

用公式来描述更加清晰：

$$ \text{Attention}(Q,K,V)=softmax(\frac{QK^T}{\sqrt d_k})V $$

**scaled dot-product attention**和**dot-product attention**唯一的区别就是，**scaled dot-product attention**有一个缩放因子 $ \frac{1}{\sqrt d_k} $。

上面公式中的$d_k$表示的是K的维度，在论文里面，默认是`64`。

那么为什么需要加上这个缩放因子呢？论文里给出了解释：对于$d_k$很大的时候，点积得到的结果维度很大，使得结果处于softmax函数梯度很小的区域。

我们知道，梯度很小的情况，这对反向传播不利。为了克服这个负面影响，除以一个缩放因子，可以一定程度上减缓这种情况。

为什么是$\frac{1}{\sqrt d_k}$呢？论文没有进一步说明。个人觉得你可以使用其他缩放因子，看看模型效果有没有提升。

论文也提供了一张很清晰的结构图，供大家参考：  
- ![scaled_dot_product_attention_arch](http://blog.stupidme.me/wp-content/uploads/2018/09/scaled_dot_product_attention_arch.png)  
*Figure 3. Scaled dot-product attention architecture.*  

首先说明一下我们的K、Q、V是什么：
* 在encoder的self-attention中，Q、K、V都来自同一个地方（相等），他们是上一层encoder的输出。对于第一层encoder，它们就是word embedding和positional encoding相加得到的输入。
* 在decoder的self-attention中，Q、K、V都来自于同一个地方（相等），它们是上一层decoder的输出。对于第一层decoder，它们就是word embedding和positional encoding相加得到的输入。但是对于decoder，我们不希望它能获得下一个time step（即将来的信息），因此我们需要进行**sequence masking**。
* 在encoder-decoder attention中，Q来自于decoder的上一层的输出，K和V来自于encoder的输出，K和V是一样的。
* Q、K、V三者的维度一样，即 $d_q=d_k=d_v$。

上面scaled dot-product attention和decoder的self-attention都出现了**masking**这样一个东西。那么这个mask到底是什么呢？这两处的mask操作是一样的吗？这个问题在后面会有详细解释。

### Scaled dot-product attention的实现

咱们先把scaled dot-product attention实现了吧。代码如下：

```python
import torch
import torch.nn as nn

class ScaledDotProductAttention(nn.Module):
    """Scaled dot-product attention mechanism."""

    def __init__(self, attention_dropout=0.0):
        super(ScaledDotProductAttention, self).__init__()
        self.dropout = nn.Dropout(attention_dropout)
        self.softmax = nn.Softmax(dim=2)

    def forward(self, q, k, v, scale=None, attn_mask=None):
        """前向传播.

        Args:
        	q: Queries张量，形状为[B, L_q, D_q]
        	k: Keys张量，形状为[B, L_k, D_k]
        	v: Values张量，形状为[B, L_v, D_v]，一般来说就是k
        	scale: 缩放因子，一个浮点标量
        	attn_mask: Masking张量，形状为[B, L_q, L_k]

        Returns:
        	上下文张量和attetention张量
        """
        attention = torch.bmm(q, k.transpose(1, 2))
        if scale:
        	attention = attention * scale
        if attn_mask:
        	# 给需要mask的地方设置一个负无穷
        	attention = attention.masked_fill_(attn_mask, -np.inf)
		# 计算softmax
        attention = self.softmax(attention)
		# 添加dropout
        attention = self.dropout(attention)
		# 和V做点积
        context = torch.bmm(attention, v)
        return context, attention
```

### Multi-head attention又是什么呢？

理解了Scaled dot-product attention，Multi-head attention也很简单了。论文提到，他们发现将Q、K、V通过一个线性映射之后，分成 $h$ 份，对每一份进行**scaled dot-product attention**效果更好。然后，把各个部分的结果合并起来，再次经过线性映射，得到最终的输出。这就是所谓的**multi-head attention**。上面的超参数 $$h$$ 就是**heads**数量。论文默认是`8`。

下面是multi-head attention的结构图：  
- ![multi-head attention_architecture](http://blog.stupidme.me/wp-content/uploads/2018/09/multi_head_attention_arch.png)  
*Figure 4: Multi-head attention architecture.*  

值得注意的是，上面所说的**分成 $h$ 份**是在 $d_k、d_q、d_v$ 维度上面进行切分的。因此，进入到scaled dot-product attention的 $d_k$ 实际上等于未进入之前的 $D_K/h$ 。

Multi-head attention允许模型加入不同位置的表示子空间的信息。

Multi-head attention的公式如下：
- $$\text{MultiHead}(Q,K,V) = \text{Concat}(\text{head}_ 1,\dots,\text{head}_ h)W^O$$

其中，$\text{head}_ i = \text{Attention}(QW_i^Q,KW_i^K,VW_i^V)$

论文里面，$d_{model}=512$，$h=8$。所以在scaled dot-product attention里面的 $d_q = d_k = d_v = d_{model}/h = 512/8 = 64$

### Multi-head attention的实现

相信大家已经理清楚了multi-head attention，那么我们来实现它吧。代码如下：

```python
import torch
import torch.nn as nn

class MultiHeadAttention(nn.Module):

    def __init__(self, model_dim=512, num_heads=8, dropout=0.0):
        super(MultiHeadAttention, self).__init__()

        self.dim_per_head = model_dim // num_heads
        self.num_heads = num_heads
        self.linear_k = nn.Linear(model_dim, self.dim_per_head * num_heads)
        self.linear_v = nn.Linear(model_dim, self.dim_per_head * num_heads)
        self.linear_q = nn.Linear(model_dim, self.dim_per_head * num_heads)

        self.dot_product_attention = ScaledDotProductAttention(dropout)
        self.linear_final = nn.Linear(model_dim, model_dim)
        self.dropout = nn.Dropout(dropout)
		# multi-head attention之后需要做layer norm
        self.layer_norm = nn.LayerNorm(model_dim)

    def forward(self, key, value, query, attn_mask=None):
		# 残差连接
        residual = query

        dim_per_head = self.dim_per_head
        num_heads = self.num_heads
        batch_size = key.size(0)

        # linear projection
        key = self.linear_k(key)
        value = self.linear_v(value)
        query = self.linear_q(query)

        # split by heads
        key = key.view(batch_size * num_heads, -1, dim_per_head)
        value = value.view(batch_size * num_heads, -1, dim_per_head)
        query = query.view(batch_size * num_heads, -1, dim_per_head)

        if attn_mask:
            attn_mask = attn_mask.repeat(num_heads, 1, 1)
        # scaled dot product attention
        scale = (key.size(-1) // num_heads) ** -0.5
        context, attention = self.dot_product_attention(
          query, key, value, scale, attn_mask)

        # concat heads
        context = context.view(batch_size, -1, dim_per_head * num_heads)

        # final linear projection
        output = self.linear_final(context)

        # dropout
        output = self.dropout(output)

        # add residual and norm layer
        output = self.layer_norm(residual + output)

        return output, attention

```

上面的代码终于出现了**Residual connection**和**Layer normalization**。我们现在来解释它们。

## Residual connection是什么？

残差连接其实很简单！给你看一张示意图你就明白了：  
- ![residual_conn](http://blog.stupidme.me/wp-content/uploads/2018/09/residual_connection.png)  
*Figure 5. Residual connection.*  

假设网络中某个层对输入`x`作用后的输出是$F(x)$，那么增加**residual connection**之后，就变成了：$F(x)+x$

这个`+x`操作就是一个**shortcut**。那么**残差结构**有什么好处呢？显而易见：因为增加了一项$x$，那么该层网络对x求偏导的时候，多了一个常数项$1$！所以在反向传播过程中，梯度连乘，也不会造成**梯度消失**！

所以，代码实现residual connection很非常简单：

```python
def residual(sublayer_fn,x):
	return sublayer_fn(x)+x
```

文章开始的transformer架构图中的`Add & Norm`中的`Add`也就是指的这个**shortcut**。

至此，**residual connection**的问题理清楚了。更多关于残差网络的介绍可以看文末的参考文献。

## Layer normalization是什么？

[GRADIENTS, BATCH NORMALIZATION AND LAYER NORMALIZATION](https://theneuralperspective.com/2016/10/27/gradient-topics/)一文对normalization有很好的解释：
> Normalization有很多种，但是它们都有一个共同的目的，那就是把输入转化成均值为0方差为1的数据。我们在把数据送入激活函数之前进行normalization（归一化），因为我们不希望输入数据落在激活函数的饱和区。

说到normalization，那就肯定得提到**Batch Normalization**。BN在CNN等地方用得很多。

BN的主要思想就是：在每一层的每一批数据上进行归一化。

我们可能会对输入数据进行归一化，但是经过该网络层的作用后，我们的的数据已经不再是归一化的了。随着这种情况的发展，数据的偏差越来越大，我的反向传播需要考虑到这些大的偏差，这就迫使我们只能使用较小的学习率来防止梯度消失或者梯度爆炸。

BN的具体做法就是对每一小批数据，在批这个方向上做归一化。如下图所示：  
- ![batch_normalization](http://blog.stupidme.me/wp-content/uploads/2018/09/batch_normalization.png)  
*Figure 6. Batch normalization example.(From [theneuralperspective.com](https://theneuralperspective.com/2016/10/27/gradient-topics/))*  

可以看到，右半边求均值是**沿着数据批量N的方向进行的**！

Batch normalization的计算公式如下：
- $$BN(x_i)=\alpha\times\frac{x_i-u_B}{\sqrt{\sigma_B^2+\epsilon}}+\beta$$

具体的实现可以查看上图的链接文章。

说完Batch normalization，就该说说咱们今天的主角**Layer normalization**。

那么什么是Layer normalization呢？:它也是归一化数据的一种方式，不过LN是**在每一个样本上计算均值和方差，而不是BN那种在批方向计算均值和方差**！

下面是LN的示意图：  
- ![layer_normalization](http://blog.stupidme.me/wp-content/uploads/2018/09/layer_normalization.png)  
*Figure 7. Layer normalization example.*  

和上面的BN示意图一比较就可以看出二者的区别啦！

下面看一下LN的公式，也BN十分相似：
- $$LN(x_i)=\alpha\times\frac{x_i-u_L}{\sqrt{\sigma_L^2+\epsilon}}+\beta$$

### Layer normalization的实现

上述两个参数$\alpha$和$\beta$都是可学习参数。下面我们自己来实现Layer normalization(PyTorch已经实现啦！)。代码如下：

```python
import torch
import torch.nn as nn


class LayerNorm(nn.Module):
    """实现LayerNorm。其实PyTorch已经实现啦，见nn.LayerNorm。"""

    def __init__(self, features, epsilon=1e-6):
        """Init.

        Args:
            features: 就是模型的维度。论文默认512
            epsilon: 一个很小的数，防止数值计算的除0错误
        """
        super(LayerNorm, self).__init__()
        # alpha
        self.gamma = nn.Parameter(torch.ones(features))
        # beta
        self.beta = nn.Parameter(torch.zeros(features))
        self.epsilon = epsilon

    def forward(self, x):
        """前向传播.

        Args:
            x: 输入序列张量，形状为[B, L, D]
        """
        # 根据公式进行归一化
        # 在X的最后一个维度求均值，最后一个维度就是模型的维度
        mean = x.mean(-1, keepdim=True)
        # 在X的最后一个维度求方差，最后一个维度就是模型的维度
        std = x.std(-1, keepdim=True)
        return self.gamma * (x - mean) / (std + self.epsilon) + self.beta
```

顺便提一句，**Layer normalization**多用于RNN这种结构。

## Mask是什么？

现在终于轮到讲解mask了!mask顾名思义就是**掩码**，在我们这里的意思大概就是**对某些值进行掩盖，使其不产生效果**。

需要说明的是，我们的Transformer模型里面涉及两种mask。分别是**padding mask**和**sequence mask**。其中后者我们已经在decoder的self-attention里面见过啦！
- **padding mask**在所有的scaled dot-product attention里面都需要用到
- **sequence mask**只有在decoder的self-attention里面用到。

所以，我们之前**ScaledDotProductAttention**的`forward`方法里面的参数`attn_mask`在不同的地方会有不同的含义。这一点我们会在后面说明。

### Padding mask

什么是**padding mask**呢？回想一下，我们的每个批次输入序列长度是不一样的！也就是说，我们要对输入序列进行**对齐**！具体来说，就是给在较短的序列后面填充`0`。因为这些填充的位置，其实是没什么意义的，所以我们的attention机制**不应该把注意力放在这些位置上**，所以我们需要进行一些处理。

具体的做法是，**把这些位置的值加上一个非常大的负数(可以是负无穷)，这样的话，经过softmax，这些位置的概率就会接近0**！

而我们的padding mask实际上是一个张量，每个值都是一个**Boolen**，值为`False`的地方就是我们要进行处理的地方。

下面是实现：

```python
def padding_mask(seq_k, seq_q):
	# seq_k和seq_q的形状都是[B,L]
    len_q = seq_q.size(1)
    # `PAD` is 0
    pad_mask = seq_k.eq(0)
    pad_mask = pad_mask.unsqueeze(1).expand(-1, len_q, -1)  # shape [B, L_q, L_k]
    return pad_mask
```

### Sequence mask

文章前面也提到，sequence mask是为了使得decoder不能看见未来的信息。也就是对于一个序列，在time_step为t的时刻，我们的解码输出应该只能依赖于t时刻之前的输出，而不能依赖t之后的输出。因此我们需要想一个办法，把t之后的信息给隐藏起来。

那么具体怎么做呢？也很简单：**产生一个上三角矩阵，上三角的值全为1，下三角的值权威0，对角线也是0**。把这个矩阵作用在每一个序列上，就可以达到我们的目的啦。

具体的代码实现如下：

```python
def sequence_mask(seq):
    batch_size, seq_len = seq.size()
    mask = torch.triu(torch.ones((seq_len, seq_len), dtype=torch.uint8),
                    diagonal=1)
    mask = mask.unsqueeze(0).expand(batch_size, -1, -1)  # [B, L, L]
    return mask
```

哈佛大学的文章[The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html)有一张效果图:
- ![sequence_mask](http://blog.stupidme.me/wp-content/uploads/2018/09/sequence_mask.png)  
*Figure 8. Sequence mask.*

值得注意的是，本来mask只需要二维的矩阵即可，但是考虑到我们的输入序列都是批量的，所以我们要把原本二维的矩阵扩张成3维的张量。上面的代码可以看出，我们已经进行了处理。

回到本小结开始的问题，`attn_mask`参数有几种情况？分别是什么意思？
* 对于decoder的self-attention，里面使用到的scaled dot-product attention，同时需要`padding mask`和`sequence mask`作为`attn_mask`，具体实现就是两个mask相加作为attn_mask。
* 其他情况，`attn_mask`一律等于`padding mask`。

至此，mask相关的问题解决了。

## Positional encoding是什么？

【2021-8-25】[面经：什么是Transformer位置编码？](https://blog.csdn.net/Datawhale/article/details/119582757)

好了，终于要解释**位置编码**了，那就是文字开始的结构图提到的**Positional encoding**。

就目前而言，我们的Transformer架构似乎少了点什么东西。没错，就是**它对序列的顺序没有约束**！序列的顺序是一个很重要的信息，如果缺失了这个信息，可能我们的结果就是：所有词语都对了，但是无法组成有意义的语句！

Self-attention可以一次性的将所有的字都当做输入。但是NLP的输入是有特点的，其特点是输入的文本要按照一定的顺序才可以。不同的语序就有不同的语义。
- 句子1：我喜欢吃洋葱
- 句子2：洋葱喜欢吃我

对于Transformer结构而言，为了更好的发挥并行输入的特点，首先要解决的问题就是要让输入的内容具有一定的位置信息。论文提出了**Positional encoding**。一句话概括就是：**对序列中的词语出现的位置进行编码**！如果对位置进行编码，那模型就可以捕捉顺序信息！

***位置编码分类***

总的来说，位置编码分为两个类型：`函数型`和`表格型`
- `函数型`：通过输入token位置信息，得到相应的位置编码；
  - 方法①：使用[ 0, 1 ]范围分配。第一个token分配0，最后一个token分配去1，其余的token按照文章的长度平均分配。
    - 示例：
        - 我喜欢吃洋葱 【0 0.16 0.32.....1】
        - 我真的不喜欢吃洋葱【0 0.125 0.25.....1】
    - 问题：如果句子长度不同，那么位置编码是不一样，所以无法表示句子之间有什么相似性。
  - 方法②：1-n正整数范围分配
    - 这个方法比较直观，就是按照输入的顺序，一次分配给token所在的索引位置。具体形式如下：
      - 我喜欢吃洋葱 【1，2，3，4，5，6】
      - 我真的不喜欢吃洋葱【1，2，3，4，5，6，7】
    - 问题：往往句子越长，后面的值越大，数字越大说明这个位置占的权重也越大，这样的方式无法凸显每个位置的真实的权重。
  - 总结：过去的方法总有这样或者那样的不好，所以Transformer对于位置信息的编码做了改进
- `表格型`：建立一个长度为L的词表，按词表的长度来分配位置id
  - 相对位置编码的特点，关注一个token与另一个token距离的**相对位置**(距离差几个token)。位置1和位置2的距离比位置3和位置10的距离更近，位置1和位置2与位置3和位置4都只相差1。这种方法可以清晰的知道单词之间的距离远近的关系。
  - ![图示](https://img-blog.csdnimg.cn/img_convert/ef2c7618ee3451e8c16c2e7fa21fbd71.png)
  - 问题：这种方式虽说可以表示出相对的距离关系，但是也是有局限的。其中一个比较大的问题是：只能的到相对关系，无法得到**方向关系**。所谓的方向关系就是，对于两个token谁在谁的前面，或者谁在谁的后面是无法判断的。

transformer位置编码采用函数型，GPT-3论文给出的公式：
- ![公式](https://img-blog.csdnimg.cn/img_convert/0eed794d556ddb9a75bb2e39cf2791b7.png)
- 注意：每一个Token的位置信息编码不是数字，而是一个不同频率分割出来，和文本一样维度的向量。不同频率是通过Wn来表示。
- 得到位置向量P之后，将和模型的embedding向量相加，得到进入Transformer模型的最终表示 ![公式](https://img-blog.csdnimg.cn/img_convert/c096e564bb2b7b833c96769511a704a5.png), 其中，$w_i=1/10000^{2i/d_{model}}$,  t是每个token的位置，比如说是位置1，位置2，以及位置n

transformer怎么做呢？论文的实现很有意思，使用正余弦函数。公式如下：
- $$PE(pos,2i) = sin(pos/10000^{2i/d_{model}}) $$
- $$PE(pos,2i+1) = cos(pos/10000^{2i/d_{model}})$$

其中，`pos`是指词语在序列中的位置。可以看出，在**偶数位置，使用正弦编码，在奇数位置，使用余弦编码**。

上面公式中的$d_{model}$是模型的维度，论文默认是`512`。

这个编码公式的意思就是：给定词语的位置$\text{pos}$，我们可以把它编码成$d_{model}$维的向量！也就是说，位置编码的每一个维度对应正弦曲线，波长构成了从$2\pi$$到$$10000*2\pi$的等比序列。

上面的位置编码是**绝对位置编码**。但是词语的**相对位置**也非常重要。这就是论文为什么要使用三角函数的原因！

正弦函数能够表达相对位置信息。主要数学依据是以下两个公式：
- $$sin(\alpha+\beta) = sin\alpha cos\beta + cos\alpha sin\beta$$
- $$cos(\alpha+\beta) = cos\alpha cos\beta - sin\alpha sin\beta$$

上面的公式说明，对于词汇之间的位置偏移`k`，$PE(pos+k)$可以表示成$PE(pos)$和$PE(k)$的组合形式，这就是表达相对位置的能力！

以上就是$PE$的所有秘密。说完了positional encoding，那么我们还有一个与之处于同一地位的**word embedding**。

**Word embedding**大家都很熟悉了，它是对序列中的词汇的编码，把每一个词汇编码成$d_{model}$维的向量！看到没有，**Postional encoding是对词汇的位置编码，word embedding是对词汇本身编码**！

所以，我更喜欢positional encoding的另外一个名字**Positional embedding**！

### Positional encoding的实现

PE的实现也不难，按照论文的公式即可。代码如下：

```python
import torch
import torch.nn as nn


class PositionalEncoding(nn.Module):
    
    def __init__(self, d_model, max_seq_len):
        """初始化。
        
        Args:
            d_model: 一个标量。模型的维度，论文默认是512
            max_seq_len: 一个标量。文本序列的最大长度
        """
        super(PositionalEncoding, self).__init__()
        
        # 根据论文给的公式，构造出PE矩阵
        position_encoding = np.array([
          [pos / np.pow(10000, 2.0 * (j // 2) / d_model) for j in range(d_model)]
          for pos in range(max_seq_len)])
        # 偶数列使用sin，奇数列使用cos
        position_encoding[:, 0::2] = np.sin(position_encoding[:, 0::2])
        position_encoding[:, 1::2] = np.cos(position_encoding[:, 1::2])

        # 在PE矩阵的第一行，加上一行全是0的向量，代表这`PAD`的positional encoding
        # 在word embedding中也经常会加上`UNK`，代表位置单词的word embedding，两者十分类似
        # 那么为什么需要这个额外的PAD的编码呢？很简单，因为文本序列的长度不一，我们需要对齐，
        # 短的序列我们使用0在结尾补全，我们也需要这些补全位置的编码，也就是`PAD`对应的位置编码
        pad_row = torch.zeros([1, d_model])
        position_encoding = torch.cat((pad_row, position_encoding))
        
        # 嵌入操作，+1是因为增加了`PAD`这个补全位置的编码，
        # Word embedding中如果词典增加`UNK`，我们也需要+1。看吧，两者十分相似
        self.position_encoding = nn.Embedding(max_seq_len + 1, d_model)
        self.position_encoding.weight = nn.Parameter(position_encoding,
                                                     requires_grad=False)
    def forward(self, input_len):
        """神经网络的前向传播。

        Args:
          input_len: 一个张量，形状为[BATCH_SIZE, 1]。每一个张量的值代表这一批文本序列中对应的长度。

        Returns:
          返回这一批序列的位置编码，进行了对齐。
        """
        
        # 找出这一批序列的最大长度
        max_len = torch.max(input_len)
        tensor = torch.cuda.LongTensor if input_len.is_cuda else torch.LongTensor
        # 对每一个序列的位置进行对齐，在原序列位置的后面补上0
        # 这里range从1开始也是因为要避开PAD(0)的位置
        input_pos = tensor(
          [list(range(1, len + 1)) + [0] * (max_len - len) for len in input_len])
        return self.position_encoding(input_pos)
    
```

### Word embedding的实现

Word embedding应该是老生常谈了，它实际上就是一个二维浮点矩阵，里面的权重是可训练参数，我们只需要把这个矩阵构建出来就完成了word embedding的工作。

所以，具体的实现很简单：

```python
import torch.nn as nn


embedding = nn.Embedding(vocab_size, embedding_size, padding_idx=0)
# 获得输入的词嵌入编码
seq_embedding = seq_embedding(inputs)*np.sqrt(d_model)
```

上面`vocab_size`就是词典的大小，`embedding_size`就是词嵌入的维度大小，论文里面就是等于$d_{model}=512$。所以word embedding矩阵就是一个`vocab_size`*`embedding_size`的二维张量。

如果你想获取更详细的关于word embedding的信息，可以看我的另外一个文章[word2vec的笔记和实现](https://github.com/luozhouyang/machine-learning-notes/blob/master/word2vec.ipynb)。

## Position-wise Feed-Forward network是什么？
这就是一个全连接网络，包含两个线性变换和一个非线性函数（实际上就是ReLU）。公式如下：

$$FFN(x)=max(0,xW_1+b_1)W_2+b_2$$

这个线性变换在不同的位置都表现地一样，并且在不同的层之间使用不同的参数。

论文提到，这个公式还可以用两个核大小为1的一维卷积来解释，卷积的输入输出都是$d_{model}=512$，中间层的维度是$d_{ff}=2048$。

实现如下：

```python
import torch
import torch.nn as nn


class PositionalWiseFeedForward(nn.Module):

    def __init__(self, model_dim=512, ffn_dim=2048, dropout=0.0):
        super(PositionalWiseFeedForward, self).__init__()
        self.w1 = nn.Conv1d(model_dim, ffn_dim, 1)
        self.w2 = nn.Conv1d(model_dim, ffn_dim, 1)
        self.dropout = nn.Dropout(dropout)
        self.layer_norm = nn.LayerNorm(model_dim)

    def forward(self, x):
        output = x.transpose(1, 2)
        output = self.w2(F.relu(self.w1(output)))
        output = self.dropout(output.transpose(1, 2))

        # add residual and norm layer
        output = self.layer_norm(x + output)
        return output
```

## Transformer的实现

所有的细节都已经解释完了。现在来完成我们Transformer模型的代码。

首先，需要实现6层的encoder和decoder。

encoder代码实现如下：

```python
import torch
import torch.nn as nn


class EncoderLayer(nn.Module):
	"""Encoder的一层。"""

    def __init__(self, model_dim=512, num_heads=8, ffn_dim=2018, dropout=0.0):
        super(EncoderLayer, self).__init__()

        self.attention = MultiHeadAttention(model_dim, num_heads, dropout)
        self.feed_forward = PositionalWiseFeedForward(model_dim, ffn_dim, dropout)

    def forward(self, inputs, attn_mask=None):

        # self attention
        context, attention = self.attention(inputs, inputs, inputs, padding_mask)

        # feed forward network
        output = self.feed_forward(context)

        return output, attention


class Encoder(nn.Module):
	"""多层EncoderLayer组成Encoder。"""

    def __init__(self,
               vocab_size,
               max_seq_len,
               num_layers=6,
               model_dim=512,
               num_heads=8,
               ffn_dim=2048,
               dropout=0.0):
        super(Encoder, self).__init__()

        self.encoder_layers = nn.ModuleList(
          [EncoderLayer(model_dim, num_heads, ffn_dim, dropout) for _ in
           range(num_layers)])

        self.seq_embedding = nn.Embedding(vocab_size + 1, model_dim, padding_idx=0)
        self.pos_embedding = PositionalEncoding(model_dim, max_seq_len)

    def forward(self, inputs, inputs_len):
        output = self.seq_embedding(inputs)
        output += self.pos_embedding(inputs_len)

        self_attention_mask = padding_mask(inputs, inputs)

        attentions = []
        for encoder in self.encoder_layers:
            output, attention = encoder(output, self_attention_mask)
            attentions.append(attention)

        return output, attentions

```

通过文章前面的分析，代码不需要更多解释了。同样的，我们的decoder代码如下：

```python
import torch
import torch.nn as nn


class DecoderLayer(nn.Module):

    def __init__(self, model_dim, num_heads=8, ffn_dim=2048, dropout=0.0):
        super(DecoderLayer, self).__init__()

        self.attention = MultiHeadAttention(model_dim, num_heads, dropout)
        self.feed_forward = PositionalWiseFeedForward(model_dim, ffn_dim, dropout)

    def forward(self,
              dec_inputs,
              enc_outputs,
              self_attn_mask=None,
              context_attn_mask=None):
        # self attention, all inputs are decoder inputs
        dec_output, self_attention = self.attention(
          dec_inputs, dec_inputs, dec_inputs, self_attn_mask)

        # context attention
        # query is decoder's outputs, key and value are encoder's inputs
        dec_output, context_attention = self.attention(
          enc_outputs, enc_outputs, dec_output, context_attn_mask)

        # decoder's output, or context
        dec_output = self.feed_forward(dec_output)

        return dec_output, self_attention, context_attention


class Decoder(nn.Module):

    def __init__(self,
               vocab_size,
               max_seq_len,
               num_layers=6,
               model_dim=512,
               num_heads=8,
               ffn_dim=2048,
               dropout=0.0):
        super(Decoder, self).__init__()

        self.num_layers = num_layers

        self.decoder_layers = nn.ModuleList(
          [DecoderLayer(model_dim, num_heads, ffn_dim, dropout) for _ in
           range(num_layers)])

        self.seq_embedding = nn.Embedding(vocab_size + 1, model_dim, padding_idx=0)
        self.pos_embedding = PositionalEncoding(model_dim, max_seq_len)

    def forward(self, inputs, inputs_len, enc_output, context_attn_mask=None):
        output = self.seq_embedding(inputs)
        output += self.pos_embedding(inputs_len)

        self_attention_padding_mask = padding_mask(inputs, inputs)
        seq_mask = sequence_mask(inputs)
        self_attn_mask = torch.gt((self_attention_padding_mask + seq_mask), 0)

        self_attentions = []
        context_attentions = []
        for decoder in self.decoder_layers:
            output, self_attn, context_attn = decoder(
            output, enc_output, self_attn_mask, context_attn_mask)
            self_attentions.append(self_attn)
            context_attentions.append(context_attn)

        return output, self_attentions, context_attentions
```

最后，我们把encoder和decoder组成Transformer模型！

代码如下：

```python
import torch
import torch.nn as nn


class Transformer(nn.Module):

    def __init__(self,
               src_vocab_size,
               src_max_len,
               tgt_vocab_size,
               tgt_max_len,
               num_layers=6,
               model_dim=512,
               num_heads=8,
               ffn_dim=2048,
               dropout=0.2):
        super(Transformer, self).__init__()

        self.encoder = Encoder(src_vocab_size, src_max_len, num_layers, model_dim,
                               num_heads, ffn_dim, dropout)
        self.decoder = Decoder(tgt_vocab_size, tgt_max_len, num_layers, model_dim,
                               num_heads, ffn_dim, dropout)

        self.linear = nn.Linear(model_dim, tgt_vocab_size, bias=False)
        self.softmax = nn.Softmax(dim=2)

    def forward(self, src_seq, src_len, tgt_seq, tgt_len):
        context_attn_mask = padding_mask(tgt_seq, src_seq)

        output, enc_self_attn = self.encoder(src_seq, src_len)

        output, dec_self_attn, ctx_attn = self.decoder(
          tgt_seq, tgt_len, output, context_attn_mask)

        output = self.linear(output)
        output = self.softmax(output)

        return output, enc_self_attn, dec_self_attn, ctx_attn

```

至此，Transformer模型已经实现了！

### pytorch代码

【2021-11-1】
- [熬了一晚上，我从零实现了Transformer模型，把代码讲给你听](https://zhuanlan.zhihu.com/p/411311520)
- 理论讲解：[Transformer - Attention is all you need](https://zhuanlan.zhihu.com/p/311156298)
- 模型结构图
  - ![](https://pic1.zhimg.com/80/v2-dad8a00603dc120dee165c06ae8b44d0_720w.jpg)

完整代码

```python
# @Author:Yifx
# @Contact: Xxuyifan1999@163.com
# @Time:2021/9/16 20:02
# @Software: PyCharm

"""
文件说明：
"""

import torch
import torch.nn as nn
import numpy as np
import math


class Config(object):
    # 模型超参类
    def __init__(self):
        self.vocab_size = 6

        self.d_model = 20
        self.n_heads = 2

        assert self.d_model % self.n_heads == 0
        dim_k  = self.d_model // self.n_heads
        dim_v = self.d_model // self.n_heads

        self.padding_size = 30
        self.UNK = 5
        self.PAD = 4

        self.N = 6
        self.p = 0.1

config = Config()

class Embedding(nn.Module):
    def __init__(self,vocab_size):
        super(Embedding, self).__init__()
        # 一个普通的 embedding层，我们可以通过设置padding_idx=config.PAD 来实现论文中的 padding_mask
        self.embedding = nn.Embedding(vocab_size,config.d_model,padding_idx=config.PAD)


    def forward(self,x):
        # 根据每个句子的长度，进行padding，短补长截
        for i in range(len(x)):
            if len(x[i]) < config.padding_size:
                x[i].extend([config.UNK] * (config.padding_size - len(x[i]))) # 注意 UNK是你词表中用来表示oov的token索引，这里进行了简化，直接假设为6
            else:
                x[i] = x[i][:config.padding_size]
        x = self.embedding(torch.tensor(x)) # batch_size * seq_len * d_model
        return x



class Positional_Encoding(nn.Module):

    def __init__(self,d_model):
        super(Positional_Encoding,self).__init__()
        self.d_model = d_model


    def forward(self,seq_len,embedding_dim):
        positional_encoding = np.zeros((seq_len,embedding_dim))
        for pos in range(positional_encoding.shape[0]):
            for i in range(positional_encoding.shape[1]):
                positional_encoding[pos][i] = math.sin(pos/(10000**(2*i/self.d_model))) if i % 2 == 0 else math.cos(pos/(10000**(2*i/self.d_model)))
        return torch.from_numpy(positional_encoding)


class Mutihead_Attention(nn.Module):
    def __init__(self,d_model,dim_k,dim_v,n_heads):
        super(Mutihead_Attention, self).__init__()
        self.dim_v = dim_v
        self.dim_k = dim_k
        self.n_heads = n_heads

        self.q = nn.Linear(d_model,dim_k)
        self.k = nn.Linear(d_model,dim_k)
        self.v = nn.Linear(d_model,dim_v)

        self.o = nn.Linear(dim_v,d_model)
        self.norm_fact = 1 / math.sqrt(d_model)

    def generate_mask(self,dim):
        # 此处是 sequence mask ，防止 decoder窥视后面时间步的信息。
        # padding mask 在数据输入模型之前完成。
        matirx = np.ones((dim,dim))
        mask = torch.Tensor(np.tril(matirx))

        return mask==1

    def forward(self,x,y,requires_mask=False):
        assert self.dim_k % self.n_heads == 0 and self.dim_v % self.n_heads == 0
        # size of x : [batch_size * seq_len * batch_size]
        # 对 x 进行自注意力
        Q = self.q(x).reshape(-1,x.shape[0],x.shape[1],self.dim_k // self.n_heads) # n_heads * batch_size * seq_len * dim_k
        K = self.k(x).reshape(-1,x.shape[0],x.shape[1],self.dim_k // self.n_heads) # n_heads * batch_size * seq_len * dim_k
        V = self.v(y).reshape(-1,y.shape[0],y.shape[1],self.dim_v // self.n_heads) # n_heads * batch_size * seq_len * dim_v
        # print("Attention V shape : {}".format(V.shape))
        attention_score = torch.matmul(Q,K.permute(0,1,3,2)) * self.norm_fact
        if requires_mask:
            mask = self.generate_mask(x.shape[1])
            # masked_fill 函数中，对Mask位置为True的部分进行Mask
            attention_score.masked_fill(mask,value=float("-inf")) # 注意这里的小Trick，不需要将Q,K,V 分别MASK,只MASKSoftmax之前的结果就好了
        output = torch.matmul(attention_score,V).reshape(y.shape[0],y.shape[1],-1)
        # print("Attention output shape : {}".format(output.shape))

        output = self.o(output)
        return output


class Feed_Forward(nn.Module):
    def __init__(self,input_dim,hidden_dim=2048):
        super(Feed_Forward, self).__init__()
        self.L1 = nn.Linear(input_dim,hidden_dim)
        self.L2 = nn.Linear(hidden_dim,input_dim)

    def forward(self,x):
        output = nn.ReLU()(self.L1(x))
        output = self.L2(output)
        return output

class Add_Norm(nn.Module):
    def __init__(self):
        self.dropout = nn.Dropout(config.p)
        super(Add_Norm, self).__init__()

    def forward(self,x,sub_layer,**kwargs):
        sub_output = sub_layer(x,**kwargs)
        # print("{} output : {}".format(sub_layer,sub_output.size()))
        x = self.dropout(x + sub_output)

        layer_norm = nn.LayerNorm(x.size()[1:])
        out = layer_norm(x)
        return out


class Encoder(nn.Module):
    def __init__(self):
        super(Encoder, self).__init__()
        self.positional_encoding = Positional_Encoding(config.d_model)
        self.muti_atten = Mutihead_Attention(config.d_model,config.dim_k,config.dim_v,config.n_heads)
        self.feed_forward = Feed_Forward(config.d_model)

        self.add_norm = Add_Norm()


    def forward(self,x): # batch_size * seq_len 并且 x 的类型不是tensor，是普通list

        x += self.positional_encoding(x.shape[1],config.d_model)
        # print("After positional_encoding: {}".format(x.size()))
        output = self.add_norm(x,self.muti_atten,y=x)
        output = self.add_norm(output,self.feed_forward)

        return output

# 在 Decoder 中，Encoder的输出作为Query和KEy输出的那个东西。即 Decoder的Input作为V。此时是可行的
# 因为在输入过程中，我们有一个padding操作，将Inputs和Outputs的seq_len这个维度都拉成一样的了
# 我们知道，QK那个过程得到的结果是 batch_size * seq_len * seq_len .既然 seq_len 一样，那么我们可以这样操作
# 这样操作的意义是，Outputs 中的 token 分别对于 Inputs 中的每个token作注意力

class Decoder(nn.Module):
    def __init__(self):
        super(Decoder, self).__init__()
        self.positional_encoding = Positional_Encoding(config.d_model)
        self.muti_atten = Mutihead_Attention(config.d_model,config.dim_k,config.dim_v,config.n_heads)
        self.feed_forward = Feed_Forward(config.d_model)
        self.add_norm = Add_Norm()

    def forward(self,x,encoder_output): # batch_size * seq_len 并且 x 的类型不是tensor，是普通list
        # print(x.size())
        x += self.positional_encoding(x.shape[1],config.d_model)
        # print(x.size())
        # 第一个 sub_layer
        output = self.add_norm(x,self.muti_atten,y=x,requires_mask=True)
        # 第二个 sub_layer
        output = self.add_norm(x,self.muti_atten,y=encoder_output,requires_mask=True)
        # 第三个 sub_layer
        output = self.add_norm(output,self.feed_forward)
        return output

class Transformer_layer(nn.Module):
    def __init__(self):
        super(Transformer_layer, self).__init__()
        self.encoder = Encoder()
        self.decoder = Decoder()

    def forward(self,x):
        x_input,x_output = x
        encoder_output = self.encoder(x_input)
        decoder_output = self.decoder(x_output,encoder_output)
        return (encoder_output,decoder_output)

class Transformer(nn.Module):
    def __init__(self,N,vocab_size,output_dim):
        super(Transformer, self).__init__()
        self.embedding_input = Embedding(vocab_size=vocab_size)
        self.embedding_output = Embedding(vocab_size=vocab_size)

        self.output_dim = output_dim
        self.linear = nn.Linear(config.d_model,output_dim)
        self.softmax = nn.Softmax(dim=-1)
        self.model = nn.Sequential(*[Transformer_layer() for _ in range(N)])


    def forward(self,x):
        x_input , x_output = x
        x_input = self.embedding_input(x_input)
        x_output = self.embedding_output(x_output)

        _ , output = self.model((x_input,x_output))

        output = self.linear(output)
        output = self.softmax(output)

        return output
```


# 改进

- 【2020-6-7】[模型压缩95%，MIT韩松等人提出新型Lite Transformer](https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650789244&idx=3&sn=498864894b6e1d584a45017911ce233c&chksm=871a1102b06d98144d851133ead6bd4c69f90843d6ed5ef4ef46f56bfc3256d46f43f463b419&mpshare=1&scene=23&srcid&sharer_sharetime=1591760786074&sharer_shareid=b8d409494a5439418f4a89712efcd92a%23rd)
	- MIT 最近的研究《[Lite Transformer with Long-Short Range Attention](https://arxiv.org/abs/2004.11886v1)》中，MIT 与上海交大的研究人员提出了一种高效的移动端 NLP 架构 Lite Transformer，向在边缘设备上部署移动级 NLP 应用迈进了一大步。该论文已被人工智能顶会 ICLR 2020 收录。[代码](https://github.com/mit-han-lab/lite-transformer)
	- 核心是长短距离注意力（Long-Short Range Attention，LSRA），其中一组注意力头（通过卷积）负责局部上下文建模，而另一组则（依靠注意力）执行长距离关系建模。
	- 对于移动 NLP 设置，Lite Transformer 的 BLEU 值比基于 AutoML 的 [Evolved Transformer](http://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650756694&idx=4&sn=9de8bdbe79a5f4c45833f87418642111&chksm=871a9228b06d1b3e886f549543f8ba742ee120e4ca8f1780996fb241b6b6d05ca97882d5290b&scene=21#wechat_redirect) 高 0.5，而且它不需要使用成本高昂的架构搜索。
	- 从 Lite Transformer 与 Evolved Transformer、原版 transformer 的比较结果中可以看出，Lite Transformer 的性能更佳，搜索成本相比 Evolved Transformer 大大减少


## Transformer-XL 和 XLNet

XLNet引入了自回归语言模型以及自编码语言模型

### 作者：杨植麟

[循环智能（Recurrent）：用AI重塑沟通](https://www.cyzone.cn/article/557072.html)

【2022-1-17】杨植麟博士，**循环智能**（Recurrent AI）联合创始人，清华大学交叉信息院助理教授，智源青年科学家。

2016年5月联合创办的Recurrent AI，核心技术包括自然语言理解、语音识别、语气识别、声纹识别和推荐系统。其中，自然语言理解来自公司的核心原创算法XLNet，这套算法刷新了18项NLP（自然语言处理）任务。如今累计融资4亿元，连续三年营收增长超200%，服务银行保险等行业的头部客户，日均处理对话一亿条、覆盖数百万终端用户。
- ![](https://oss.cyzone.cn/2019/0926/631f26ab025a8c33d218a4a09424bbb4.png?x-oss-process=image/format,png)
- 循环智能创始团队，从左到右：COO揭发、CTO张宇韬、CEO陈麒聪以及AI和产品负责人杨植麟

其研究成果累计Google Scholar引用10,000余次；作为第一作者发表Transformer-XL 和 XLNet ，对NLP领域产生重大影响，分别是ACL 2019和NeurIPS 2019最高引论文之一；主导开发的盘古NLP大模型获2021年世界人工智能大会“卓越人工智能引领者之星奖”。曾入选2021年福布斯亚洲30 under 30；曾效力于Google Brain和Facebook AI。博士毕业于美国卡内基梅隆大学，本科毕业于清华大学
 
### 1. 什么是XLNet

- [XLNet预训练模型，看这篇就够了！(代码实现)](https://www.cnblogs.com/mantch/p/11611554.html)
 
XLNet 是一个类似 BERT 的模型，而不是完全不同的模型。总之，**XLNet是一种通用的自回归预训练方法**。它是CMU和Google Brain团队在2019年6月份发布的模型，最终，XLNet 在 20 个任务上超过了 BERT 的表现，并在 18 个任务上取得了当前最佳效果（state-of-the-art），包括机器问答、自然语言推断、情感分析和文档排序。

作者表示，BERT 这样基于去噪自编码器的预训练模型可以很好地建模双向语境信息，性能优于基于自回归语言模型的预训练方法。然而，由于需要 mask 一部分输入，BERT 忽略了被 mask 位置之间的依赖关系，因此出现预训练和微调效果的差异（pretrain-finetune discrepancy）。

基于这些优缺点，该研究提出了一种泛化的自回归预训练模型 XLNet。XLNet 可以：
1.  通过最大化所有可能的因式分解顺序的对数似然，学习双向语境信息；
2.  用自回归本身的特点克服 BERT 的缺点；
3.  此外，XLNet 还融合了当前最优自回归模型 Transformer-XL 的思路。
 
### 2. 自回归语言模型（Autoregressive LM）

在ELMO／BERT出来之前，大家通常讲的语言模型其实是根据上文内容预测下一个可能跟随的单词，就是常说的自左向右的语言模型任务，或者反过来也行，就是根据下文预测前面的单词，这种类型的LM被称为自回归语言模型。GPT 就是典型的自回归语言模型。ELMO尽管看上去利用了上文，也利用了下文，但是本质上仍然是自回归LM，这个跟模型具体怎么实现有关系。ELMO是做了两个方向（从左到右以及从右到左两个方向的语言模型），但是是分别有两个方向的自回归LM，然后把LSTM的两个方向的隐节点状态拼接到一起，来体现双向语言模型这个事情的。所以其实是两个自回归语言模型的拼接，本质上仍然是自回归语言模型。
 
自回归语言模型有优点有缺点：
- **缺点**是只能利用上文或者下文的信息，不能同时利用上文和下文的信息，当然，貌似ELMO这种双向都做，然后拼接看上去能够解决这个问题，因为融合模式过于简单，所以效果其实并不是太好。
- **优点**其实跟下游NLP任务有关，比如生成类NLP任务，比如文本摘要，机器翻译等，在实际生成内容的时候，就是从左向右的，自回归语言模型天然匹配这个过程。而Bert这种DAE模式，在生成类NLP任务中，就面临训练过程和应用过程不一致的问题，导致生成类的NLP任务到目前为止都做不太好。
 
### 3. 自编码语言模型（Autoencoder LM）
 
自回归语言模型只能根据上文预测下一个单词，或者反过来，只能根据下文预测前面一个单词。相比而言，Bert通过在输入X中随机Mask掉一部分单词，然后预训练过程的主要任务之一是根据上下文单词来预测这些被Mask掉的单词，如果你对Denoising Autoencoder比较熟悉的话，会看出，这确实是典型的DAE的思路。那些被Mask掉的单词就是在输入侧加入的所谓噪音。类似Bert这种预训练模式，被称为DAE LM。
 
这种DAE LM的优缺点正好和自回归LM反过来，它能比较自然地融入双向语言模型，同时看到被预测单词的上文和下文，这是好处。缺点是啥呢？主要在输入侧引入\[Mask\]标记，导致预训练阶段和Fine-tuning阶段不一致的问题，因为Fine-tuning阶段是看不到\[Mask\]标记的。DAE吗，就要引入噪音，\[Mask\] 标记就是引入噪音的手段，这个正常。
 
XLNet的出发点就是：能否融合自回归LM和DAE LM两者的优点。就是说如果站在自回归LM的角度，如何引入和双向语言模型等价的效果；如果站在DAE LM的角度看，它本身是融入双向语言模型的，如何抛掉表面的那个\[Mask\]标记，让预训练和Fine-tuning保持一致。当然，XLNet还讲到了一个Bert被Mask单词之间相互独立的问题。
 
### 4. XLNet模型
 
#### 4.1 排列语言建模（Permutation Language Modeling）
 
Bert的自编码语言模型也有对应的缺点，就是XLNet在文中指出的：
1.  第一个预训练阶段因为采取引入\[Mask\]标记来Mask掉部分单词的训练模式，而Fine-tuning阶段是看不到这种被强行加入的Mask标记的，所以两个阶段存在使用模式不一致的情形，这可能会带来一定的性能损失； 
2.  另外一个是，Bert在第一个预训练阶段，假设句子中多个单词被Mask掉，这些被Mask掉的单词之间没有任何关系，是条件独立的，而有时候这些单词之间是有关系的。
 
上面两点是XLNet在第一个预训练阶段，相对Bert来说要解决的两个问题。
 
其实思路也比较简洁，可以这么思考：XLNet仍然遵循两阶段的过程，第一个阶段是语言模型预训练阶段；第二阶段是任务数据Fine-tuning阶段。它主要希望改动第一个阶段，就是说不像Bert那种带Mask符号的Denoising-autoencoder的模式，而是采用自回归LM的模式。就是说，看上去输入句子X仍然是自左向右的输入，看到Ti单词的上文Context\_before，来预测Ti这个单词。但是又希望在Context\_before里，不仅仅看到上文单词，也能看到Ti单词后面的下文Context_after里的下文单词，这样的话，Bert里面预训练阶段引入的Mask符号就不需要了，于是在预训练阶段，看上去是个标准的从左向右过程，Fine-tuning当然也是这个过程，于是两个环节就统一起来。当然，这是目标。剩下是怎么做到这一点的问题。
- ![](https://pic4.zhimg.com/80/v2-948e085be7a9a2eb7eac2d12069b1a93_hd.jpg)
 
首先，需要强调一点，尽管上面讲的是把句子X的单词排列组合后，再随机抽取例子作为输入，但是，实际上你是不能这么做的，因为Fine-tuning阶段你不可能也去排列组合原始输入。所以，就必须让预训练阶段的输入部分，看上去仍然是x1,x2,x3,x4这个输入顺序，但是可以在Transformer部分做些工作，来达成我们希望的目标。
 
具体而言，XLNet采取了Attention掩码的机制，你可以理解为，当前的输入句子是X，要预测的单词Ti是第i个单词，前面1到i-1个单词，在输入部分观察，并没发生变化，该是谁还是谁。但是在Transformer内部，通过Attention掩码，从X的输入单词里面，也就是Ti的上文和下文单词中，随机选择i-1个，放到Ti的上文位置中，把其它单词的输入通过Attention掩码隐藏掉，于是就能够达成我们期望的目标（当然这个所谓放到Ti的上文位置，只是一种形象的说法，其实在内部，就是通过Attention Mask，把其它没有被选到的单词Mask掉，不让它们在预测单词Ti的时候发生作用，如此而已。看着就类似于把这些被选中的单词放到了上文Context_before的位置了）。
 
具体实现的时候，XLNet是用“双流自注意力模型”实现的，细节可以参考论文，但是基本思想就如上所述，双流自注意力机制只是实现这个思想的具体方式，理论上，你可以想出其它具体实现方式来实现这个基本思想，也能达成让Ti看到下文单词的目标。
 
这里简单说下“**双流自注意力机制**”，一个是内容流自注意力，其实就是标准的Transformer的计算过程；主要是引入了Query流自注意力，这个是干嘛的呢？其实就是用来代替Bert的那个\[Mask\]标记的，因为XLNet希望抛掉\[Mask\]标记符号，但是比如知道上文单词x1,x2，要预测单词x3，此时在x3对应位置的Transformer最高层去预测这个单词，但是输入侧不能看到要预测的单词x3，Bert其实是直接引入\[Mask\]标记来覆盖掉单词x3的内容的，等于说\[Mask\]是个通用的占位符号。而XLNet因为要抛掉\[Mask\]标记，但是又不能看到x3的输入，于是Query流，就直接忽略掉x3输入了，只保留这个位置信息，用参数w来代表位置的embedding编码。其实XLNet只是扔了表面的\[Mask\]占位符号，内部还是引入Query流来忽略掉被Mask的这个单词。和Bert比，只是实现方式不同而已。
- ![](https://pic1.zhimg.com/80/v2-2bb1a60af4fe2fa751647fdce48e337c_hd.jpg)

上面讲的Permutation Language Model是XLNet的主要理论创新，所以介绍的比较多，从模型角度讲，这个创新还是挺有意思的，因为它开启了自回归语言模型如何引入下文的一个思路，相信对于后续工作会有启发。当然，XLNet不仅仅做了这些，它还引入了其它的因素，也算是一个当前有效技术的集成体。感觉**XLNet就是Bert、GPT 2.0和Transformer XL的综合体变身**：
1.  首先，它通过PLM(Permutation Language Model)预训练目标，吸收了Bert的双向语言模型；
2.  然后，GPT2.0的核心其实是更多更高质量的预训练数据，这个明显也被XLNet吸收进来了；
3.  再然后，Transformer XL的主要思想也被吸收进来，它的主要目标是解决Transformer对于长文档NLP应用不够友好的问题。

#### 4.2 Transformer XL
 
目前在NLP领域中，处理语言建模问题有两种最先进的架构：RNN和Transformer。RNN按照序列顺序逐个学习输入的单词或字符之间的关系，而Transformer则接收一整段序列，然后使用self-attention机制来学习它们之间的依赖关系。这两种架构目前来看都取得了令人瞩目的成就，但它们都局限在捕捉长期依赖性上。

为了解决这一问题，CMU联合Google Brain在2019年1月推出的一篇新论文《Transformer-XL：Attentive Language Models beyond a Fixed-Length Context》同时结合了RNN序列建模和Transformer自注意力机制的优点，在输入数据的每个段上使用Transformer的注意力模块，并使用循环机制来学习连续段之间的依赖关系。
 
4.2.1 vanilla Transformer
 
为何要提这个模型？因为Transformer-XL是基于这个模型进行的改进。
 
Al-Rfou等人基于Transformer提出了一种训练语言模型的方法，来根据之前的字符预测片段中的下一个字符。例如，它使用 𝑥1,𝑥2,...,𝑥𝑛−1x1,x2,...,xn−1 预测字符 𝑥𝑛xn，而在 𝑥𝑛xn 之后的序列则被mask掉。论文中使用64层模型，并仅限于处理 512个字符这种相对较短的输入，因此它将输入分成段，并分别从每个段中进行学习，如下图所示。 在测试阶段如需处理较长的输入，该模型会在每一步中将输入向右移动一个字符，以此实现对单个字符的预测。
- ![](https://img-blog.csdnimg.cn/20190407095512873.png)
 
该模型在常用的数据集如enwik8和text8上的表现比RNN模型要好，但它仍有以下缺点：
*   **上下文长度受限**：字符之间的最大依赖距离受输入长度的限制，模型看不到出现在几个句子之前的单词。
*   **上下文碎片**：对于长度超过512个字符的文本，都是从头开始单独训练的。段与段之间没有上下文依赖性，会让训练效率低下，也会影响模型的性能。
*   **推理速度慢**：在测试阶段，每次预测下一个单词，都需要重新构建一遍上下文，并从头开始计算，这样的计算速度非常慢。
    
 4.2.2 Transformer XL
 
Transformer-XL架构在vanilla Transformer的基础上引入了两点创新：循环机制（Recurrence Mechanism）和相对位置编码（Relative Positional Encoding），以克服vanilla Transformer的缺点。与vanilla Transformer相比，Transformer-XL的另一个优势是它可以被用于单词级和字符级的语言建模。
 
1.  **引入循环机制**

与vanilla Transformer的基本思路一样，Transformer-XL仍然是使用分段的方式进行建模，但其与vanilla Transformer的本质不同是在于引入了段与段间的循环机制，使得当前段在建模的时候能够利用之前段的信息来实现长期依赖性。如下图所示：
- ![](https://img-blog.csdnimg.cn/20190407095601191.png)

在训练阶段，处理后面的段时，每个隐藏层都会接收两个输入：
- 这两个输入会被拼接，然后用于计算当前段的Key和Value矩阵。
- 该方法可以利用前面更多段的信息，测试阶段也可以获得更长的依赖。在测试阶段，与vanilla Transformer相比，其速度也会更快。在vanilla Transformer中，一次只能前进一个step，并且需要重新构建段，并全部从头开始计算；而在Transformer-XL中，每次可以前进一整个段，并利用之前段的数据来预测当前段的输出。
*   该段的前面隐藏层的输出，与vanilla Transformer相同（上图的灰色线）。
*   前面段的隐藏层的输出（上图的绿色线），可以使模型创建长期依赖关系。

3.  **相对位置编码**
    
在Transformer中，一个重要的地方在于其考虑了序列的位置信息。在分段的情况下，如果仅仅对于每个段仍直接使用Transformer中的位置编码，即每个不同段在同一个位置上的表示使用相同的位置编码，就会出现问题。比如，第i−2i-2i−2段和第i−1i-1i−1段的第一个位置将具有相同的位置编码，但它们对于第iii段的建模重要性显然并不相同（例如第i−2i-2i−2段中的第一个位置重要性可能要低一些）。因此，需要对这种位置进行区分。
    
论文对于这个问题，提出了一种新的位置编码的方式，即会根据词之间的相对距离而非像Transformer中的绝对位置进行编码。从另一个角度来解读公式的话，可以将attention的计算分为如下四个部分：
    
详细公式见：[Transformer-XL解读（论文 + PyTorch源码）](https://blog.csdn.net/magical_bubble/article/details/89060213)
    
*   基于内容的“寻址”，即没有添加原始位置编码的原始分数。
*   基于内容的位置偏置，即相对于当前内容的位置偏差。
*   全局的内容偏置，用于衡量key的重要性。
*   全局的位置偏置，根据query和key之间的距离调整重要性。
 
### 5. XLNet与BERT比较
 
尽管看上去，XLNet在预训练机制引入的Permutation Language Model这种新的预训练目标，和Bert采用Mask标记这种方式，有很大不同。其实你深入思考一下，会发现，两者本质是类似的。
 
**区别主要在于**：
* Bert是直接在输入端显示地通过引入Mask标记，在输入侧隐藏掉一部分单词，让这些单词在预测的时候不发挥作用，要求利用上下文中其它单词去预测某个被Mask掉的单词；
* 而XLNet则抛弃掉输入侧的Mask标记，通过Attention Mask机制，在Transformer内部随机Mask掉一部分单词（这个被Mask掉的单词比例跟当前单词在句子中的位置有关系，位置越靠前，被Mask掉的比例越高，位置越靠后，被Mask掉的比例越低），让这些被Mask掉的单词在预测某个单词的时候不发生作用。
    
 
所以，本质上两者并没什么太大的不同，只是Mask的位置，Bert更表面化一些，XLNet则把这个过程隐藏在了Transformer内部而已。这样，就可以抛掉表面的\[Mask\]标记，解决它所说的预训练里带有\[Mask\]标记导致的和Fine-tuning过程不一致的问题。至于说XLNet说的，Bert里面被Mask掉单词的相互独立问题，也就是说，在预测某个被Mask单词的时候，其它被Mask单词不起作用，这个问题，你深入思考一下，其实是不重要的，因为XLNet在内部Attention Mask的时候，也会Mask掉一定比例的上下文单词，只要有一部分被Mask掉的单词，其实就面临这个问题。而如果训练数据足够大，其实不靠当前这个例子，靠其它例子，也能弥补被Mask单词直接的相互关系问题，因为总有其它例子能够学会这些单词的相互依赖关系。
 
当然，XLNet这种改造，维持了表面看上去的自回归语言模型的从左向右的模式，这个Bert做不到，这个有明显的好处，就是对于生成类的任务，能够在维持表面从左向右的生成过程前提下，模型里隐含了上下文的信息。所以看上去，XLNet貌似应该对于生成类型的NLP任务，会比Bert有明显优势。另外，因为XLNet还引入了Transformer XL的机制，所以对于长文档输入类型的NLP任务，也会比Bert有明显优势。

6\. 代码实现
- [中文XLNet预训练模型](https://github.com/ymcui/Chinese-PreTrained-XLNet)


# 参考资料

## 参考文章

1.[为什么ResNet和DenseNet可以这么深？一文详解残差块为何有助于解决梯度弥散问题](https://zhuanlan.zhihu.com/p/28124810)  
2.[GRADIENTS, BATCH NORMALIZATION AND LAYER NORMALIZATION](https://theneuralperspective.com/2016/10/27/gradient-topics/)  
3.[The Annotated Transformer](http://nlp.seas.harvard.edu/2018/04/03/attention.html#position-wise-feed-forward-networks)  
4.[Building the Mighty Transformer for Sequence Tagging in PyTorch : Part I](https://medium.com/@kolloldas/building-the-mighty-transformer-for-sequence-tagging-in-pytorch-part-i-a1815655cd8)  
5.[Building the Mighty Transformer for Sequence Tagging in PyTorch : Part II](https://medium.com/@kolloldas/building-the-mighty-transformer-for-sequence-tagging-in-pytorch-part-ii-c85bf8fd145)  
6.[Attention?Attention!](https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html)  

## 参考代码
1.[jadore801120/attention-is-all-you-need-pytorch](https://github.com/jadore801120/attention-is-all-you-need-pytorch)  
2.[JayParks/transformer](https://github.com/JayParks/transformer)  


