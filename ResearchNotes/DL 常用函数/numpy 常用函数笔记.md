# 1 numpy 常用函数

`import numpy as np`



## np.repeat

> `data 为 numpy 数组  ` `size 为扩充的大小` `axis 为扩充的维度`

将  `data` 数组的 第2维度 数据扩充为 size 大小, 如果 `data.shape = (100,1)`, 扩充后结果为 `(100,size)`,  以第2维数据作为填充值。

```python
data.repeat(size, axis=1)  # 扩展数据
```



## np.asarray

> 将列表等数据转换为 numpy 数组

```python
data = np.asarray(data)
```



## np.swapaxes

> 更换 data 数组的 0 - 1 维度

```python
np.swapaxes(data, 0, 1)
```



##  np.expand_dims

> 扩展 data 的维度, axis 代表要扩维度的轴

```python
 np.expand_dims(data, axis=3)     # 扩展维度
```



## np.squeeze(axis=None)

> 去除 numpy 数组 轴 = 1 的对应维度 

```python
np.squeeze(axis=1)
```



## np.concatenate((a1,...,a3), axis=None)

> 拼接 numpy 数组，可以拼接多个数组（需要保证维度一致）

```python
numpy.concatenate((a1,a2,a3), axis=0)
```



## np.repeat()

> 用于重复数组中的元素
>
> params: a - numpy 数组, repeats - 重复次数(int), axis - 维度

```python
numpy.repeat(a, repeats, axis=None)
```





## 使用 numpy 实现像 list append 操作一样的拼接效果

> list 拼接然后转为 numpy 是最简单的方法， 但是numpy的直接拼接在某些场景也是必须的。

`numpy实现`

```python
import numpy as np


if __name__ == '__main__':
    array = np.empty(4)		# 赋值空数组
    array = np.expand_dims(array, axis=0)		# 扩展维度
    for i in range(4):
        temp = np.asarray([i, i+1, i+2, i+3])
        temp = np.expand_dims(temp, axis=0)  # 扩展维度
        array = np.concatenate((array, temp), axis=0)  # 拼接
    array = array[1:]
    print(array)
    
>>>
[[0. 1. 2. 3.]
 [1. 2. 3. 4.]
 [2. 3. 4. 5.]
 [3. 4. 5. 6.]]
```

`list实现`

```python
import numpy as np


if __name__ == '__main__':
    array = []
    for i in range(4):
        temp = [i, i+1, i+2, i+3]
        array.append(temp)
    array = np.asarray(array)
    print(array)
>>>
[[0 1 2 3]
 [1 2 3 4]
 [2 3 4 5]
 [3 4 5 6]]
```



 

# 2 torch 常用函数

## torch.cuda.is_available

> 测试 GPU 是否可用

```python
torch.cuda.is_available()
```



## tensor.to

> 将数据转换到设备上进行运算
>
> `device= 'cpu' | 'cuda'`

```python
trainX.to(device)
```



## torch.tensor

> 将 numpy 数据转换为 tensor 格式

```python
torch.tensor(test_segments)
```



## tensor.detach().numpy()

> 将 tensor 数据转换为 numpy 格式

```python
test_predict.detach().numpy()
```



## torch.unsqueeze(temp, dim=0)

> 增加 tensor 数组的维度

```python
temp = torch.unsqueeze(temp, dim=0)
```



## torch.squeeze()

> 缩减 tensor 数组的维度, 自动缩减 维度 = 1

```python
temp = torch.squeeze()
```



## torch.cat()

> 拼接多个 tensor 数组

```python
results = torch.cat((results, temp), dim=0)
```





## 使用 torch 实现拼接

> 这个主要用在损失函数上， 自定义损失函数必须保证 tensor 格式，否则无法在张量图上做反向传播，则损失函数无效

```python
import torch

if __name__ == '__main__':
    results = torch.empty(4)
    results = torch.unsqueeze(results, dim=0)
    for i in range(4):
        temp = torch.from_numpy(np.asarray([i, i+1, i+2, i+3]))
        temp = torch.unsqueeze(temp, dim=0)
        results = torch.cat((results, temp), dim=0)
    # 删除之前的未初始化的内容
    results = results[1:, :]
    print(results)
>>>
tensor([[0., 1., 2., 3.],
        [1., 2., 3., 4.],
        [2., 3., 4., 5.],
        [3., 4., 5., 6.]])
```



# 3 数据处理 API

##  train_test_split

> `from sklearn.model_selection import train_test_split`
>
> 将数据集根据划分比例划分, 默认乱序。
>
> `test_size = 小数 | 整数    小数代表划分比例, 整数代表测试集个数`
>
> `shuffle = True | False   开启或关闭乱序`

```python
X_train, X_test, y_train, y_test = train_test_split(self.train_data, self.train_label,test_size=1,random_state=None, shuffle=False)  	# 开启随机
```



## metrics.r2_score()

> `from sklearn import metrics`
>
> 对预测结果的置信度进行评价

```python
metrics.r2_score(labels, prediction)
```
