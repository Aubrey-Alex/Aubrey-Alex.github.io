# Denoising Diffusion Probabilistic Models (DDPM) 阅读笔记

!!! note "论文解读精选"
    [DDPM通俗化讲解](https://zhuanlan.zhihu.com/p/610012156)

    [DDPM公式推导](https://zhuanlan.zhihu.com/p/719095916)

    [DDPM笔记](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/)

    [DDPM视频讲解](https://www.youtube.com/watch?v=fbLgFrlTnGU)

## 论文信息
- 标题：Denoising Diffusion Probabilistic Models
- 作者：Jonathan Ho, Ajay Jain, Pieter Abbeel
- 会议：NeurIPS 2020
- 链接：[arXiv](https://arxiv.org/abs/2006.11239)

## 摘要
本文提出了一种基于扩散概率模型的高质量图像生成方法。扩散模型是一类受非平衡热力学启发的潜变量模型。通过训练一个加权的变分下界，我们获得了最佳结果，这个下界基于扩散概率模型与去噪分数匹配和Langevin动力学之间的新联系。我们的模型自然地允许一个渐进的有损解压缩方案，可以解释为自回归解码的推广。在无条件CIFAR10数据集上，我们获得了9.46的Inception分数和3.17的FID分数，达到了最先进水平。

## 预备知识学习

### 概率论与数理统计

#### 1. 贝叶斯定理

贝叶斯定理的标准形式：

$$
P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}
$$

扩展形式（全概率公式展开）：

$$
P(A_i|B) = \frac{P(B|A_i)P(A_i)}{\sum_{j=1}^n P(B|A_j)P(A_j)}
$$

#### 2. 马尔科夫链

马尔科夫链是一个随机过程，具有无记忆性：

$$
P(X_{t+1} = x|X_t = x_t, X_{t-1} = x_{t-1}, ..., X_0 = x_0) = P(X_{t+1} = x|X_t = x_t)
$$

转移概率矩阵：

$$
P = \begin{bmatrix}
p_{11} & p_{12} & \cdots & p_{1n} \\
p_{21} & p_{22} & \cdots & p_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
p_{n1} & p_{n2} & \cdots & p_{nn}
\end{bmatrix}
$$

[马尔科夫链](https://zhuanlan.zhihu.com/p/448575579)

#### 3. KL散度（相对熵）

KL散度用于衡量两个概率分布的差异：

$$
D_{KL}(P||Q) = \sum_{x \in \mathcal{X}} P(x) \log \frac{P(x)}{Q(x)}
$$

连续形式：

$$
D_{KL}(P||Q) = \int_{-\infty}^{\infty} p(x) \log \frac{p(x)}{q(x)} dx
$$

[信息熵、交叉熵、相对熵（KL散度）](https://zhuanlan.zhihu.com/p/573385147)

#### 4. 极大似然估计（MLE）

给定观测数据$X$，寻找参数$\theta$使得似然函数最大：

$$
\hat{\theta}_{MLE} = \arg\max_{\theta} P(X|\theta)
$$

对数似然函数：

$$
\mathcal{L}(\theta) = \log P(X|\theta) = \sum_{i=1}^n \log P(x_i|\theta)
$$

[极大似然估计](https://www.zhihu.com/question/24124998/answer/1547063354)

### 信息论基础

#### 1. 熵（Entropy）

离散随机变量的熵：

$$
H(X) = -\sum_{x \in \mathcal{X}} p(x) \log p(x)
$$

连续随机变量的微分熵：

$$
h(X) = -\int_{\mathcal{X}} p(x) \log p(x) dx
$$

#### 2. 互信息（Mutual Information）

互信息衡量两个随机变量的依赖程度：

$$
I(X;Y) = \sum_{x \in \mathcal{X}} \sum_{y \in \mathcal{Y}} p(x,y) \log \frac{p(x,y)}{p(x)p(y)}
$$

连续形式：

$$
I(X;Y) = \int_{\mathcal{X}} \int_{\mathcal{Y}} p(x,y) \log \frac{p(x,y)}{p(x)p(y)} dxdy
$$

[互信息](https://www.zhihu.com/question/304499706/answer/31256368530)

#### 3. 交叉熵（Cross Entropy）

交叉熵用于衡量两个概率分布之间的差异：

$$
H(P,Q) = -\sum_{x \in \mathcal{X}} P(x) \log Q(x)
$$

连续形式：

$$
H(P,Q) = -\int_{\mathcal{X}} p(x) \log q(x) dx
$$

### 随机过程

#### 1. 马尔可夫过程

马尔可夫过程是一个随机过程，其未来状态只依赖于当前状态：

$$
P(X_{t+1} = x|X_t = x_t, X_{t-1} = x_{t-1}, ..., X_0 = x_0) = P(X_{t+1} = x|X_t = x_t)
$$

[马尔可夫过程](https://zhuanlan.zhihu.com/p/86995916)

#### 2. 扩散过程

扩散过程可以用随机微分方程描述：

$$
dX_t = \mu(X_t, t)dt + \sigma(X_t, t)dW_t
$$

其中：

- $\mu(X_t, t)$是漂移项
- $\sigma(X_t, t)$是扩散项
- $W_t$是维纳过程（布朗运动）

## 核心概念

### 扩散模型的基本思想

扩散模型是一种受非平衡热力学启发的潜变量模型。其基本思想是通过两个过程：

1. **前向过程（扩散过程）**：逐步向数据添加噪声
2. **反向过程（去噪过程）**：学习如何从噪声中恢复数据

!!! note "马尔可夫链"
    马尔可夫链是一个序列，其中每个节点的状态只与上一个状态有关：
    $$p(x_t|x_{t-1},x_{t-2},...,x_0) = p(x_t|x_{t-1})$$

### 前向过程详解

!!! info "前向过程（扩散过程）"
    前向过程是一个固定的马尔可夫链，逐步向数据添加高斯噪声：

    $$q(x_{1:T}|x_0) := \prod_{t=1}^T q(x_t|x_{t-1})$$

    其中：
    $q(x_t|x_{t-1}) := \mathcal{N}(x_t; \sqrt{1-\beta_t}x_{t-1}, \beta_tI)$

=== "参数解释"
    - $x_0$: 原始数据（如图像）
    - $x_t$: 第t步的噪声数据
    - $\beta_t$: 噪声调度参数，控制每一步添加的噪声量
    - $\mathcal{N}$: 高斯分布
    - $I$: 单位矩阵

=== "重要性质"
    1. 任意时刻t的分布可以直接计算：$q(x_t|x_0) = \mathcal{N}(x_t; \sqrt{\bar{\alpha}_t}x_0, (1-\bar{\alpha}_t)I)$
    2. 其中
        $\alpha_t = 1-\beta_t$, $\bar{\alpha}_t = \prod_{s=1}^t \alpha_s$
    3. 当t足够大时，$x_t$趋近于标准高斯分布

### 反向过程详解

!!! info "反向过程（去噪过程）"
    反向过程是一个可学习的马尔可夫链，用于从噪声中恢复数据：

    $$p_\theta(x_{0:T}) := p(x_T)\prod_{t=1}^T p_\theta(x_{t-1}|x_t)$$

    其中：
    $p_\theta(x_{t-1}|x_t) := \mathcal{N}(x_{t-1}; \mu_\theta(x_t,t), \Sigma_\theta(x_t,t))$

=== "参数解释"
    - $\theta$: 神经网络参数
    - $\mu_\theta$: 神经网络预测的均值
    - $\Sigma_\theta$: 神经网络预测的方差
    - $p(x_T)$: 标准高斯分布

=== "关键发现"
    1. 预测噪声比预测均值效果更好
    2. 方差可以固定为$\beta_t$或$\tilde{\beta}_t$
    3. 使用简化的训练目标可以获得更好的样本质量

### 训练目标详解

!!! info "变分下界（ELBO）"
    扩散模型的训练目标是最小化变分下界：

    $$L = \mathbb{E}_q\left[-\log\frac{p_\theta(x_{0:T})}{q(x_{1:T}|x_0)}\right]$$

=== "ELBO的重写形式"
    ELBO可以重写为：

    $$L = L_T + \sum_{t>1} L_{t-1} + L_0$$
    
    其中：

    - $L_T = D_{KL}(q(x_T|x_0) \| p(x_T))$
    - $L_{t-1} = D_{KL}(q(x_{t-1}|x_t,x_0) \| p_\theta(x_{t-1}|x_t))$
    - $L_0 = -\log p_\theta(x_0|x_1)$

=== "简化训练目标"
    作者发现使用简化的训练目标可以获得更好的样本质量：

    $$L_{simple}(\theta) := \mathbb{E}_{t,x_0,\epsilon}\left[\|\epsilon - \epsilon_\theta(\sqrt{\bar{\alpha}_t}x_0 + \sqrt{1-\bar{\alpha}_t}\epsilon, t)\|^2\right]$$

=== "训练算法"
    ```python
    # Algorithm 1: Training
    while not converged:
        x_0 = sample_data()              # 采样原始图像
        t = sample_uniform(1, T)         # 采样时间步
        ε = sample_normal()              # 采样噪声
        loss = mse(ε, predict_noise(x_0, t, ε))  # 计算损失
        update_parameters()              # 更新模型参数
    ```

    ```python
    # Algorithm 2: Sampling
    x_T = sample_normal()               # 从标准正态分布采样
    for t in reversed(range(1, T+1)):
        z = 0 if t == 1 else sample_normal()  # 采样噪声
        # 预测并更新x_{t-1}
        x_prev = predict_mean(x_t, t) + σ_t * z
    return x_0
    ```

## 技术细节

### 网络架构

!!! info "U-Net架构"
    - 使用U-Net作为主干网络
    - 包含组归一化
    - 在16×16特征图分辨率上使用自注意力机制
    - 使用Transformer的正弦位置编码来表示时间步

### 超参数设置

=== "时间步设置"
    - T = 1000
    - β₁ = 10⁻⁴
    - β_T = 0.02
    - β_t线性增加

=== "数据预处理"
    - 图像数据缩放到[-1, 1]
    - 使用离散解码器处理最终输出

## 实验结果

=== "CIFAR10结果"
    | 模型 | IS | FID | NLL |
    |------|----|-----|-----|
    | DDPM | 9.46 | 3.17 | ≤3.75 |
    | StyleGAN2 | 9.74 | 3.26 | - |

=== "LSUN结果"
    - Church: FID=7.89
    - Bedroom: FID=4.90

## 总结与展望

=== "主要优势"
    - 样本质量高
    - 训练稳定
    - 实现简单
    - 可解释性强

=== "未来方向"
    - 扩展到其他数据模态
    - 作为其他生成模型的组件
    - 探索更高效的采样方法
