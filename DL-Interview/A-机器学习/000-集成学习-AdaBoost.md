- 思想：多个学习器组合成性能更好的学习器
- **集成学习为什么有效？**集成模型的平均性能至少与成员表现一致；**如果成员的误差是独立的**，集成模型将显著的更好。

## 策略

### 1. Boosting（提升方法）

- **Boosting**：某个**基学习器**出发，反复学习得到一系列基学习器，组合成一个强学习器。
- **串行**：新的学习器依赖旧的学习器。
- **代表算法/模型**：[提升方法 AdaBoost](#提升方法-adaboost)、提升树、梯度提升树 GBDT

#### Boosting 的两个基本问题

1. 每一轮如何改变数据的权值/概率分布？

1. 如何组合弱分类器成一个强分类器？

   

### 2. Bagging

- **并行**：基学习器不存在依赖关系，并行训练。

- **代表算法/模型**：[随机森林](#随机森林)、神经网络的 **Dropout** 策略；

  

### 3. Stacking

- 略

  

## AdaBoost 

- Boosting 策略

**解决 [Boosting 两个基本问题](#boosting-策略要解决的两个基本问题)**

1. 开始：每个样本的权值相等；
1. 迭代：提高上一轮分类错误样本的权值，降低分类正确样本的权值；
1. 集成：加权表决（[加法模型](#加法模型)）。分类误差小的学习器权值高，在表决中起更大的作用；

### 算法描述

- 输入：训练集 `T={...}, xi ∈ R^n, yi ∈ {-1,+1}`，基学习器 `G1(x)`
- 输出：最终学习器 `G(x)`

1. 初始化训练数据 ---- 均匀分布

   [![](..\_assets\公式_20180715133122.png)](http://www.codecogs.com/eqnedit.php?latex=D_1=(w_{1,1},\cdots,w_{1,i},\cdots,w_{1,N}),\quad&space;w_{1,i}=\frac{1}{N},\quad&space;i=1,2,\cdots,N)

1. 迭代 `m=1,2,..,M`

   1. 用权值分布为`D_m`的训练集，训练基学习器：

      [![](..\_assets\公式_20180715132515.png)](http://www.codecogs.com/eqnedit.php?latex=G_m(x):\chi&space;\rightarrow&space;\{-1,&plus;1\})

   1. 计算 `G_m(x)` 在训练集上的分类误差：

      [![](..\_assets\公式_20180715142042.png)](http://www.codecogs.com/eqnedit.php?latex=\begin{aligned}&space;e_m&=P(G_m(x_i)\neq&space;y_i)

      > `I(x)` 指示函数 ---> 分类误差率 == 所有**分类错误样本的权值之和**

   1. 计算 `G_m(x)` 的系数:

      [![](..\_assets\公式_20180715133256.png)](http://www.codecogs.com/eqnedit.php?latex=\alpha_m=\frac{1}{2}\ln\frac{1-e_m}{e_m})

   1. 更新训练集的权值分布:
      [![](..\_assets\公式_20180715140328.png)](http://www.codecogs.com/eqnedit.php?latex=\begin{aligned}&space;D_{{\color{Red}m&plus;1}}&=(w_{m&plus;1,1},\cdots,w_{m&plus;1,i},\cdots,w_{m&plus;1,N})\\&space;w_{{\color{Red}m&plus;1},i}&=\frac{w_{{\color{Red}m},i}\cdot\exp(-\alpha_{\color{Red}m}\cdot{\color{Blue}y_iG_m(x_i)&space;})

       `Z_m` 为**规范化因子**（类似 `Softmax`），使 `D_m+1` 成为**概率分布**；

因为 `y, G(x) ∈ {-1, 1}`，所以：
        
[![](..\_assets\公式_20180715134916.png)](http://www.codecogs.com/eqnedit.php?latex={\color{Blue}y_iG_m(x_i)&space;}=\left\{\begin{matrix}&space;1,&&space;G_m(x_i)=y_i&space;\\&space;-1,&&space;G_m(x_i)\neq&space;y_i&space;\end{matrix}\right.)
        
因此 `w_{m+1,i}` 也可以写作
        
[![](..\_assets\公式_20180715135945.png)](http://www.codecogs.com/eqnedit.php?latex=\dpi{120}&space;w_{m&plus;1,i}=\left\{\begin{matrix}&space;\frac{w_{m,i}}{Z_m}e^{\color{Red}&space;{-\alpha_m}},&space;&&space;G_m(x_i)=y_i&space;\\&space;\frac{w_{m,i}}{Z_m}e^{\color{Red}&space;{\alpha_m}},&&space;G_m(x_i)\neq&space;y_i&space;\end{matrix}\right.)
    

基学习器**线性组合**：

[![](..\_assets\公式_20180715141210.png)](http://www.codecogs.com/eqnedit.php?latex=G(x)=\mathrm{sign}(\sum_{m=1}^M\alpha_mG_m(x))

### AdaBoost 算法要点说明

- 开始：所有数据均匀分布（权值相同）
- 计算分类误差率 == 分类错误样本的权值之和
- `G_m(x)` 系数 `α_m` ： 随 `e_m` 的减小而增大；---> 分类错误样本权值提高，而分类正确的权值减小；
  [![](..\_assets\公式_20180715133256.png)](http://www.codecogs.com/eqnedit.php?latex=\alpha_m=\frac{1}{2}\ln\frac{1-e_m}{e_m}) ，错误率 `e_m <= 1/2` 时，`α_m >= 0`；
  
  

### **加法模型**

[![](..\_assets\公式_20180715212915.png)](http://www.codecogs.com/eqnedit.php?latex=f(x)=\sum_{m=1}^M\beta_m\,b(x;\gamma_m))

其中`b(x;γ)`为基函数，`γ`为基函数的参数；`β`为系数；

- 给定训练数据和损失函数`L(y,f(x))`，加法模型的学习 --- 最小化损失函数：

  [![](..\_assets\公式_20180715230941.png)](http://www.codecogs.com/eqnedit.php?latex={\color{Red}&space;\underset{\beta_m,\gamma_m}{\min}}\sum_{i=1}^N&space;L\left&space;(&space;y_i,{\color{Blue}&space;\sum_{m=1}^M\beta_m\,b(x;\gamma_m)}&space;\right&space;))

### 前向分步算法

从前向后，每一步学习一个基函数及其系数，逐步优化目标函数；

- 输入：训练集 `T={(x1,y1),..,(xN,yN)}`，损失函数 `L(y,f(x))`，基函数集 `{b(x;γ)}`
- 输出：加法模型 `f(x)`

1. 初始化 `f_0(x)=0`

1. 对 `m=1,2,..,M`

   1. 极小化损失函数，得到 `(β_m,γ_m)`

      [![](..\_assets\公式_20180716142855.png)](http://www.codecogs.com/eqnedit.php?latex=(\beta_m,\gamma_m)=\arg\underset{\beta,\gamma}{\min}\sum_{i=1}^NL\left&space;(&space;y_i,{\color{Red}&space;f_{m-1}(x_i)&plus;\beta&space;b(x_i;\gamma)}&space;\right&space;))

   1. 更新模型 `f_m(x)`

      [![](..\_assets\公式_20180716143232.png)](http://www.codecogs.com/eqnedit.php?latex=f_m(x)={\color{Red}&space;f_{m-1}(x)&plus;\beta&space;b(x;\gamma)})

1. 得到加法模型

   [![](..\_assets\公式_20180716143509.png)](http://www.codecogs.com/eqnedit.php?latex=f(x)=f_M(x)={\color{Red}&space;\sum_{m=1}^M}{\color{Blue}&space;\beta_m}b(x;\gamma_m))

- 前向分步算法：将**同时**求解`m=1,2,..,M`的所有参数`(β_m,γ_m)`**简化**为**逐次**求解各`(β_m,γ_m)`

- 注：AdaBoost 是前向分步算法的特例 ---- 基函数为基分类器，损失函数为指数函数`L(y,f(x)) = exp(-y*f(x))`