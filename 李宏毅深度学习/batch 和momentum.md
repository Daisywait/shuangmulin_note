# 截图笔记
## 大小Batch比较
![[Pasted image 20251105095820.png]]
![[Pasted image 20251024160707.png]]
![[Pasted image 20251024161001.png]]
#### 平行运算：
![[Pasted image 20251024161149.png]]
![[Pasted image 20251024161446.png]]
#### 帮助trainning
###### 用小的batchsize的optimization会好一点
![[Pasted image 20251024161730.png]]
###### 可以逃离鞍点saddlepoint
![[Pasted image 20251024161857.png]]
>在train的时候大batch和小batch的acc差不多，但是testing的时候小的batch也比大的好
#### 帮助testing
![[Pasted image 20251105094346.png]] 

## 动量
由于有惯性！![[Pasted image 20251105100119.png]]![[Pasted image 20251105100511.png]]
考虑了之前梯度的“总和”
![[Pasted image 20251105100743.png]]

# 总结
## 一、Batch训练技术

### 1.1 基本概念

- **Batch（批次）**‍：将训练数据分成小批次进行训练
- **Mini-batch**：与Batch同义，指小批次训练
- **Epoch（回合）**‍：所有数据看过一遍称为一个epoch
- **Shuffle（洗牌）**‍：每个epoch重新随机分批

### 1.2 Batch训练流程

1. 将数据分成多个batch
2. 每次取一个batch计算loss和gradient
3. 更新参数
4. 重复直到所有batch处理完成

### 1.3 Batch大小的影响

#### 1.3.1 极端情况对比

|特性|Batch Size=1|Batch Size=全部数据|
|---|---|---|
|更新频率|每样本更新一次|所有样本看完才更新|
|梯度稳定性|Noisy（噪声大）|稳定|
|每epoch更新次数|N次|1次|

#### 1.3.2 GPU并行运算的影响

- **实验数据**（MNIST数据集）：
    - Batch Size 1-1000：计算时间几乎相同
    - Batch Size 60000：约10秒（V100 GPU）
- **关键发现**：
    - 小batch：每epoch需要更多更新次数，总时间更长
    - 大batch：每epoch更新次数少，总时间更短

### 1.4 Batch大小对训练效果的影响

#### 1.4.1 实验结果

- **训练准确率**：小batch > 大batch
- **验证准确率**：小batch > 大batch
- **测试准确率**：小batch > 大batch（即使训练准确率相同）

#### 1.4.2 理论解释

1. **优化角度**：
    
    - 小batch的noisy gradient有助于跳出局部最小值
    - 每个batch的loss略有差异，避免卡在saddle point
2. **泛化角度**：
    
    - 小batch倾向于找到"宽盆地"（好的局部最小值）
    - 大batch倾向于找到"窄峡谷"（坏的局部最小值）
    - 训练/测试分布差异时，宽盆地泛化更好

### 1.5 Batch大小选择建议

- 需要作为超参数调整
- 大batch：训练效率高
- 小batch：优化效果好，泛化能力强
- 可考虑混合方法（如大batch训练+特殊优化技术）

## 二、Momentum技术

### 2.1 基本概念

- **物理类比**：参数视为球，error surface视为斜坡
- **核心思想**：不仅考虑当前梯度，还考虑历史移动方向

### 2.2 数学表达

`m₀ = 0 mₜ = λ·mₜ₋₁ - η·∇L(θₜ₋₁) θₜ = θₜ₋₁ + mₜ`

其中：

- mₜ：第t步的动量
- λ：动量系数（超参数）
- η：学习率
- ∇L：梯度

### 2.3 工作原理

1. 初始时只按梯度方向移动
2. 后续步骤结合：
    - 当前梯度方向
    - 前一步移动方向
3. 即使梯度为0（局部最小值），仍可继续移动

### 2.4 优势

- 有助于跳出局部最小值
- 可越过saddle point
- 加速收敛
- 减少振荡