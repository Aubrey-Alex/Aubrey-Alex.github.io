!!! abstract "机器学习"
    该笔记为学习机器学习课程时所涉及到的知识点的总结，观看视频为李宏毅2021/2022春机器学习课程，而在该课程中主要涉猎的是机器学习中的深度学习领域。

## PyTorch

### 基本概念

=== "什么是PyTorch"
    PyTorch是一个基于Python的机器学习框架，具有两个主要特点：
    1. 支持在GPU上进行N维张量计算（类似NumPy）
    2. 支持深度神经网络的自动微分功能

=== "训练流程概述"
    训练神经网络主要包含以下步骤：
    1. 定义神经网络结构
    2. 定义损失函数
    3. 选择优化算法
    4. 进行训练-验证-测试循环

### Dataset & DataLoader

=== "Dataset"
    Dataset是PyTorch中用于存储数据样本和对应标签的类。自定义Dataset需要实现以下方法：
    ```python
    from torch.utils.data import Dataset
    
    class MyDataset(Dataset):
        def __init__(self, file):
            self.data = ...  # 读取和预处理数据
            
        def __getitem__(self, index):
            return self.data[index]  # 返回一个数据样本
            
        def __len__(self):
            return len(self.data)    # 返回数据集大小
    ```

=== "DataLoader"
    DataLoader用于批量加载数据，支持：
    - 数据批处理（batching）
    - 数据打乱（shuffling）
    - 多进程加载
    ```python
    from torch.utils.data import DataLoader
    
    dataset = MyDataset(file)
    dataloader = DataLoader(
        dataset,
        batch_size=5,    # 每批数据量
        shuffle=True,     # 是否打乱数据
        num_workers=2     # 多进程加载
    )
    ```

=== "使用方法"
    ```python
    # 遍历数据加载器
    for batch_data, batch_labels in dataloader:
        # batch_data: 一批输入数据
        # batch_labels: 对应的标签
        ...
    ```

### Tensor

Tensor是PyTorch中用于存储和操作数据的类,可以理解为numpy中的ndarray。

=== "create tensor"
    可以直接输入数据创建tensor，也可以通过使用torch.zeros、torch.ones、torch.rand、torch.randn等函数创建。
    ```python
    # 直接从数据创建
    x = torch.tensor([[1, -1], [-1, 1]])
    # 从numpy数组创建
    x = torch.from_numpy(np.array([[1, -1], [-1, 1]]))
    # 创建全零张量
    x = torch.zeros([2, 2])
    # 创建全一张量
    x = torch.ones([1, 2, 5])
    ```

=== "common operations"
    常见的张量操作包括：
    ```python
    # 求和
    y = x.sum()
    # 求平均
    y = x.mean()
    # 加法
    z = x + y
    # 减法
    z = x - y
    # 幂运算
    y = x.pow(2)
    ```

=== "tensor shape operations"
    张量形状操作：
    1. transpose：转置指定维度
    ```python
    x = torch.zeros([2, 3])
    x = x.transpose(0, 1)  # 从[2,3]变为[3,2]
    ```
    2. squeeze：移除长度为1的维度
    ```python
    x = torch.zeros([1, 2, 3])
    x = x.squeeze(0)  # 从[1,2,3]变为[2,3]
    ```
    3. unsqueeze：增加新的维度
    ```python
    x = torch.zeros([2, 3])
    x = x.unsqueeze(1)  # 从[2,3]变为[2,1,3]
    ```
    4. cat：连接多个张量
    ```python
    x = torch.zeros([2, 1, 3])
    y = torch.zeros([2, 3, 3])
    z = torch.zeros([2, 2, 3])
    w = torch.cat([x, y, z], dim=1)  # dim=1维度上拼接
    ```

=== "device"
    张量可以在CPU或GPU上运行：
    ```python
    # 移动到CPU
    x = x.to('cpu')
    # 移动到GPU
    x = x.to('cuda')
    # 检查是否有GPU
    torch.cuda.is_available()
    ```

### 神经网络构建

=== "基本层"
    ```python
    import torch.nn as nn
    
    # 线性层（全连接层）
    layer = nn.Linear(32, 64)  # 输入32维，输出64维
    
    # 激活函数
    sigmoid = nn.Sigmoid()
    relu = nn.ReLU()
    ```

=== "构建模型"
    ```python
    class MyModel(nn.Module):
        def __init__(self):
            super(MyModel, self).__init__()
            self.net = nn.Sequential(
                nn.Linear(10, 32),
                nn.Sigmoid(),
                nn.Linear(32, 1)
            )
        
        def forward(self, x):
            return self.net(x)
    ```

### 损失函数与优化器

=== "损失函数"
    常用的损失函数：
    ```python
    # 均方误差损失（回归问题）
    criterion = nn.MSELoss()
    
    # 交叉熵损失（分类问题）
    criterion = nn.CrossEntropyLoss()
    ```

=== "优化器"
    ```python
    # 随机梯度下降优化器
    optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
    
    # 优化步骤
    optimizer.zero_grad()  # 梯度清零
    loss.backward()       # 反向传播
    optimizer.step()      # 参数更新
    ```

### 模型训练流程

=== "训练准备"
    ```python
    # 数据加载
    dataset = MyDataset(file)
    tr_set = DataLoader(dataset, batch_size=16, shuffle=True)
    
    # 模型初始化
    model = MyModel().to(device)
    criterion = nn.MSELoss()
    optimizer = torch.optim.SGD(model.parameters(), lr=0.1)
    ```

=== "训练循环"
    ```python
    for epoch in range(n_epochs):
        model.train()
        for x, y in tr_set:
            optimizer.zero_grad()
            x, y = x.to(device), y.to(device)
            pred = model(x)
            loss = criterion(pred, y)
            loss.backward()
            optimizer.step()
    ```

=== "验证/测试"
    ```python
    model.eval()
    with torch.no_grad():
        for x, y in val_set:
            x, y = x.to(device), y.to(device)
            pred = model(x)
            # 计算验证指标
    ```

### 模型保存与加载

```python
# 保存模型
torch.save(model.state_dict(), 'model.pth')

# 加载模型
model.load_state_dict(torch.load('model.pth'))
```

### PDF 资料

=== "课程讲义"
    <embed src="pdfs/Pytorch_Tutorial_1.pdf" type="application/pdf" width="100%" height="400px" />

=== "实际案例"
    <embed src="pdfs/Pytorch_Tutorial_2.pdf" type="application/pdf" width="100%" height="400px" />


