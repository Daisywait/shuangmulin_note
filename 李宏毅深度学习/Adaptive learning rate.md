## 引入：
![[Pasted image 20251105103739.png]]
##### 自学习率：
![[Pasted image 20251105104554.png]]
## 计算方式：
### Adagrad
![[Pasted image 20251105104921.png]]
#### 为什么有用？
不考虑方向和动量不一样
![[Pasted image 20251105105114.png]]
### RMSProp
![[Pasted image 20251105105558.png]]
![[Pasted image 20251105105858.png]]
#### Adam
![[Pasted image 20251105110131.png]] 
会导致乱喷？
### learning rate scheduling
##### learning rate decay
![[Pasted image 20251105110711.png]]
##### warm up
![[Pasted image 20251105111320.png]]
# 总结
## 1. Critical Point的误解与真相

### 1.1 常见误区

- **现象**：训练中loss不再下降时，常误认为卡在critical point
- **验证**：多数人未检查gradient实际大小
- **真相**：实验显示loss停滞时gradient未必很小

### 1.2 实际问题分析

- **典型情况**：参数在error surface的峡谷间震荡
- **特征**：
    - loss不再减小
    - gradient norm仍很大
    - 非真正的critical point/local minimum

## 2. 梯度下降的局限性

### 2.1 简单案例演示

- **error surface**：椭圆形convex函数
- **问题**：
    - 横轴：坡度平缓（gradient小）
    - 纵轴：坡度陡峭（gradient大）

### 2.2 固定学习率的缺陷

| 学习率设置    | 现象   | 原因       |
| -------- | ---- | -------- |
| 过大(10⁻²) | 峡谷震荡 | 步幅过大无法收敛 |
| 过小(10⁻⁷) | 收敛过慢 | 平缓区域无法前进 |

### 2.3 核心结论

> "Gradient descent连简单error surface都处理不好，更难的问题更不可能做好"

## 3. 自适应学习率方法

### 3.1 基本原则

- **平缓方向**：增大学习率（gradient小）
- **陡峭方向**：减小学习率（gradient大）

### 3.2 Adagrad算法

#### 更新公式：

其中：

- ：第t次迭代的gradient

#### 特点：

- 首次更新：步幅固定为±η
- 后续更新：根据历史gradient调整

### 3.3 RMSprop改进

#### 核心创新：

#### 优势：

- 通过α控制历史gradient权重
- 动态适应坡度变化（如新月形error surface）
- 快速响应gradient变化（"刹车机制"）

### 3.4 Adam优化器

- **组成**：RMSprop + Momentum
- **特点**：
    - 结合历史gradient方向
    - 自适应学习率调整
    - PyTorch等框架已实现
    - 默认参数通常效果良好

## 4. 学习率调度策略

### 4.1 Learning Rate Decay

- **原理**：随训练进行逐步减小η
- **效果**：接近终点时自动"刹车"
- **应用**：解决Adagrad后期震荡问题

### 4.2 Warmup技术

#### 现象：

- ResNet、Transformer等模型需要
- 学习率先增后减

#### 可能解释：

1. 初期σ统计不准确
2. 小学习率利于探索error surface
3. 后期增大加速收敛

#### 典型应用：

- Transformer：遵循特定函数变化
- ResNet：0.01→0.1→衰减

## 5. 优化技术演进总结

### 5.1 技术栈

`基础梯度下降 ├─ Momentum（历史方向） ├─ Adagrad（历史幅度） ├─ RMSprop（动态权重） └─ Adam（Momentum+RMSprop）`

### 5.2 关键结论

1. Critical point非主要障碍
2. 自适应学习率是核心解决方案
3. Learning rate scheduling常需配合使用
4. 实际应用建议直接使用框架默认优化器

> 注：部分技术（如Warmup）机理尚未完全明确，属经验性黑科技