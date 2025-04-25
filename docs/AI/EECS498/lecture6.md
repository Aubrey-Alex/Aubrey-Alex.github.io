# 反向传播

## 回顾：神经网络

神经网络是将输入变换为输出的复杂非线性函数：

$$f(x) = W_2 \max(0, W_1x + b_1) + b_2$$

[神经网络结构图：展示输入层、隐藏层和输出层的连接方式]

!!! question "问题"
    如何计算神经网络的梯度？

我们需要能够计算各个参数的梯度：

$$\frac{\partial L}{\partial W_1}, \frac{\partial L}{\partial W_2}, \frac{\partial L}{\partial b_1}, \frac{\partial L}{\partial b_2}$$

只有这样，我们才能使用SGD来优化神经网络。

---

## 传统方法的问题

### 直接在纸上推导梯度

❌ **问题1**：非常繁琐，需要大量的矩阵微积分，需要很多纸

❌ **问题2**：不够模块化，如果想改变损失函数（如从SVM换到Softmax），需要重新从头推导

❌ **问题3**：对于非常复杂的模型不可行

---

## 计算图方法

更好的方法是使用**计算图**：

[计算图示例图：展示一个简单的神经网络的计算图表示]

计算图可以表示非常复杂的模型：

* 深度网络（如AlexNet）
* 神经图灵机
* 任何可微分的计算流程

[复杂模型计算图：展示AlexNet或神经图灵机的复杂计算图]

---

## 反向传播：简单示例

让我们通过一个简单的例子来理解反向传播：

$$f(x, y, z) = x + y \cdot z$$

假设：$x = -2, y = 5, z = -4$

### 正向传播：计算输出

1. $q = y \cdot z = 5 \cdot (-4) = -20$
2. $f = x + q = -2 + (-20) = -22$

[反向传播简单示例计算图：展示x、y、z的计算关系]

### 反向传播：计算梯度

反向传播的目标是计算：$\frac{\partial f}{\partial x}$, $\frac{\partial f}{\partial y}$, $\frac{\partial f}{\partial z}$

首先，我们知道$\frac{\partial f}{\partial f} = 1$（基础情况）

然后，我们可以使用链式法则：

$$\frac{\partial f}{\partial x} = \frac{\partial f}{\partial f} \cdot \frac{\partial f}{\partial x} = 1 \cdot 1 = 1$$

$$\frac{\partial f}{\partial q} = \frac{\partial f}{\partial f} \cdot \frac{\partial f}{\partial q} = 1 \cdot 1 = 1$$

$$\frac{\partial f}{\partial y} = \frac{\partial f}{\partial q} \cdot \frac{\partial q}{\partial y} = 1 \cdot z = 1 \cdot (-4) = -4$$

$$\frac{\partial f}{\partial z} = \frac{\partial f}{\partial q} \cdot \frac{\partial q}{\partial z} = 1 \cdot y = 1 \cdot 5 = 5$$

!!! tip "链式法则"
    $$\text{下游梯度} = \text{局部梯度} \times \text{上游梯度}$$

[梯度流图：展示梯度如何从输出反向流到各个变量]

---

## 梯度流模式

在反向传播中，不同的操作有不同的梯度流模式：

### 加法门：梯度分配器

```
   +
  / \
 3   4
    / \
   2   2
```

加法的局部梯度是1，所以上游梯度直接分配给所有输入。

### 复制门：梯度加法器

```
     7
    /|\
   / | \
  7  7  7
     |
   4 + 2
```

复制操作的梯度是所有下游梯度的总和。

### 乘法门：梯度交换乘法器

```
    ×
   / \
  2   3
     / \
    5   5
```

乘法的局部梯度是另一个输入值：
- $\frac{\partial}{\partial x}(x \cdot y) = y$
- $\frac{\partial}{\partial y}(x \cdot y) = x$

### 最大值门：梯度路由器

```
   max
   / \
  4   5
     / \
    0   9
```

最大值操作只将梯度传递给最大的输入，其他输入的梯度为0。

---

## 反向传播的代码实现

### "扁平"梯度计算代码

正向传播计算输出：

```python
def sigmoid(x):
    return 1.0 / (1.0 + np.exp(-x))

x = 3
a = x + 1
b = a * 2
c = -b
d = c + 1
e = d * 2
f = sigmoid(e)
```

反向传播计算梯度（从后向前）：

```python
# 基础情况：df/df = 1
df = 1.0

# df/de = df/df * df/de 
de = df * f * (1 - f)

# df/dd = df/de * de/dd
dd = de * 2

# df/dc = df/dd * dd/dc
dc = dd * 1

# df/db = df/dc * dc/db
db = dc * (-1)

# df/da = df/db * db/da
da = db * 2

# df/dx = df/da * da/dx
dx = da * 1
```

!!! note "注意"
    反向传播代码看起来像是正向传播的反向版本！

这是作业2中需要使用的方法。

---

## 更复杂的例子

对于SVM分类器：

```python
# 正向传播
scores = W.dot(X)
margins = np.maximum(0, scores - scores[y] + 1)
margins[y] = 0
loss = np.sum(margins)

# 反向传播
dscores = np.zeros_like(scores)
dscores[margins > 0] = 1
dscores[y] -= np.sum(margins > 0)
dW = dscores.dot(X.T)
```

对于两层神经网络：

```python
# 正向传播
hidden = np.maximum(0, X.dot(W1) + b1)
scores = hidden.dot(W2) + b2
loss = ...

# 反向传播
dscores = ...
dW2 = hidden.T.dot(dscores)
db2 = np.sum(dscores, axis=0)
dhidden = dscores.dot(W2.T)
dhidden[hidden <= 0] = 0
dW1 = X.T.dot(dhidden)
db1 = np.sum(dhidden, axis=0)
```

---

## 模块化API实现

更实用的方法是使用模块化API：

```python
class ComputationalGraph:
    def forward(inputs):
        # 1. 依次调用每个操作的前向传播
        # 2. 保存中间结果
        # 3. 返回输出
        
    def backward():
        # 1. 从输出开始初始化梯度为1
        # 2. 按照相反顺序调用每个操作的反向传播
        # 3. 使用链式法则组合梯度
        # 4. 返回对输入和参数的梯度
```

### PyTorch中的实现示例

PyTorch提供了自动梯度计算：

```python
class MultiplyBackward(Function):
    @staticmethod
    def forward(ctx, x, y):
        ctx.save_for_backward(x, y)  # 保存值用于反向传播
        return x * y
        
    @staticmethod
    def backward(ctx, grad_output):
        x, y = ctx.saved_tensors
        return y * grad_output, x * grad_output  # 返回对x和y的梯度
```

[PyTorch激活函数代码截图：展示Sigmoid层的forward和backward实现]

---

## 矢量化的反向传播

到目前为止，我们讨论的是标量函数的反向传播。但在实际中，我们常常处理矢量或矩阵函数。

### 矢量导数回顾

1. 标量对标量的导数：$\frac{dy}{dx} \in \mathbb{R}$
    * 例：$y = x^2$, $\frac{dy}{dx} = 2x$

2. 标量对向量的导数（梯度）：$\frac{\partial y}{\partial \mathbf{x}} \in \mathbb{R}^n$
    * 例：$y = ||\mathbf{x}||^2$, $\frac{\partial y}{\partial \mathbf{x}} = 2\mathbf{x}$

3. 向量对向量的导数（雅可比矩阵）：$\frac{\partial \mathbf{y}}{\partial \mathbf{x}} \in \mathbb{R}^{m \times n}$
    * 例：$\mathbf{y} = A\mathbf{x}$, $\frac{\partial \mathbf{y}}{\partial \mathbf{x}} = A$

### 矢量化反向传播过程

1. 正向传播计算输出
2. 反向传播从损失$L$（标量）开始
3. 对于每个变量，计算损失对该变量的梯度

[矢量反向传播图：展示上游梯度、局部雅可比矩阵和下游梯度之间的关系]

#### 矢量化反向传播公式

$$\frac{\partial L}{\partial \mathbf{x}} = \frac{\partial \mathbf{y}}{\partial \mathbf{x}}^T \cdot \frac{\partial L}{\partial \mathbf{y}}$$

其中，$\frac{\partial \mathbf{y}}{\partial \mathbf{x}}$ 是雅可比矩阵。

---

## 矢量化示例：ReLU函数

考虑元素级别的ReLU函数：$f(\mathbf{x}) = \max(0, \mathbf{x})$

假设输入$\mathbf{x}$和上游梯度$\frac{\partial L}{\partial \mathbf{y}}$：

```
输入x:      上游梯度dL/dy:
[ 1 ]       [ 4 ]
[-2 ]       [-1 ]
[ 3 ]       [ 5 ]
[-1 ]       [ 9 ]
```

```
输出y:
[ 1 ]
[ 0 ]
[ 3 ]
[ 0 ]
```

雅可比矩阵$\frac{\partial \mathbf{y}}{\partial \mathbf{x}}$是一个对角矩阵：

```
[ 1 0 0 0 ]
[ 0 0 0 0 ]
[ 0 0 1 0 ]
[ 0 0 0 0 ]
```

下游梯度计算：$\frac{\partial L}{\partial \mathbf{x}} = \frac{\partial \mathbf{y}}{\partial \mathbf{x}}^T \cdot \frac{\partial L}{\partial \mathbf{y}}$

```
下游梯度dL/dx:
[ 4 ]  (1 * 4 + 0 * (-1) + 0 * 5 + 0 * 9)
[ 0 ]  (0 * 4 + 0 * (-1) + 0 * 5 + 0 * 9)
[ 5 ]  (0 * 4 + 0 * (-1) + 1 * 5 + 0 * 9)
[ 0 ]  (0 * 4 + 0 * (-1) + 0 * 5 + 0 * 9)
```

!!! tip "实现技巧"
    实际实现中不会显式构造雅可比矩阵，而是利用其特殊结构进行高效计算：
    
    ```python
    dx = np.copy(dy)
    dx[x <= 0] = 0
    ```

---

## 矩阵乘法的反向传播

考虑矩阵乘法：$Y = XW$

- $X \in \mathbb{R}^{N \times D}$
- $W \in \mathbb{R}^{D \times M}$
- $Y \in \mathbb{R}^{N \times M}$

假设我们有上游梯度$\frac{\partial L}{\partial Y} \in \mathbb{R}^{N \times M}$，如何计算$\frac{\partial L}{\partial X}$和$\frac{\partial L}{\partial W}$？

对于$\frac{\partial L}{\partial X}$：

$$\frac{\partial L}{\partial X_{i,j}} = \sum_{k} \frac{\partial L}{\partial Y_{i,k}} \cdot \frac{\partial Y_{i,k}}{\partial X_{i,j}} = \sum_{k} \frac{\partial L}{\partial Y_{i,k}} \cdot W_{j,k}$$

矩阵形式：$\frac{\partial L}{\partial X} = \frac{\partial L}{\partial Y} W^T$

对于$\frac{\partial L}{\partial W}$：

$$\frac{\partial L}{\partial W_{i,j}} = \sum_{k} \frac{\partial L}{\partial Y_{k,j}} \cdot \frac{\partial Y_{k,j}}{\partial W_{i,j}} = \sum_{k} \frac{\partial L}{\partial Y_{k,j}} \cdot X_{k,i}$$

矩阵形式：$\frac{\partial L}{\partial W} = X^T \frac{\partial L}{\partial Y}$

!!! tip "记忆技巧"
    这些公式很容易记忆，因为它们是唯一的形状匹配方式！

---

## 自动微分

反向传播是自动微分的一种形式，特别是"反向模式自动微分"。

### 反向模式自动微分

从计算图的末端（损失函数）开始，向后计算梯度：

$$\frac{\partial L}{\partial x_0} = \frac{\partial x_1}{\partial x_0} \frac{\partial x_2}{\partial x_1} \frac{\partial x_3}{\partial x_2} \frac{\partial L}{\partial x_3}$$

矩阵乘法是可结合的，从右到左计算可以避免矩阵-矩阵乘积，只需要矩阵-向量乘积。

反向模式对于计算标量输出相对多个输入的梯度非常高效。

### 前向模式自动微分

从计算图的开始处计算梯度：

$$\frac{\partial x_3}{\partial a} = \frac{\partial x_3}{\partial x_2} \frac{\partial x_2}{\partial x_1} \frac{\partial x_1}{\partial x_0} \frac{\partial x_0}{\partial a}$$

前向模式对于计算多个输出相对标量输入的梯度很高效。

[自动微分对比图：展示前向和反向模式的区别]

PyTorch主要使用反向模式，但也提供前向模式的beta实现。

---

## 高阶导数

有时我们需要计算二阶导数，如海森矩阵（Hessian）：

$$H = \frac{\partial^2 L}{\partial \mathbf{x}^2}$$

海森矩阵乘以向量计算：

$$H \mathbf{v} = \frac{\partial}{\partial \mathbf{x}} \left( \frac{\partial L}{\partial \mathbf{x}} \cdot \mathbf{v} \right)$$

这可以通过两次反向传播实现：
1. 第一次反向传播计算$\frac{\partial L}{\partial \mathbf{x}}$
2. 第二次反向传播计算$\frac{\partial}{\partial \mathbf{x}} \left( \frac{\partial L}{\partial \mathbf{x}} \cdot \mathbf{v} \right)$

[高阶导数计算图：展示如何通过两次反向传播计算海森矩阵-向量积]

这在PyTorch/TensorFlow中已实现，可用于：
- 二阶优化方法
- 正则化梯度范数（如WGAN-GP）

```python
# PyTorch中的示例
grad_outputs = torch.autograd.grad(outputs=loss, inputs=x, create_graph=True)
grad_norm = grad_outputs.norm()
grad_of_grad = torch.autograd.grad(outputs=grad_norm, inputs=x)
```

---

## 总结

计算图和反向传播是现代深度学习的基础：

1. 使用计算图表示复杂表达式
2. 正向传播计算输出
3. 反向传播计算梯度
4. 每个节点接收上游梯度，乘以局部梯度，计算下游梯度

实现方式：
- "扁平"代码：反向传播看起来像逆序的正向传播（A2使用）
- 模块化API：每个操作有配对的前向/反向函数（A3将使用）

下一讲：卷积神经网络
