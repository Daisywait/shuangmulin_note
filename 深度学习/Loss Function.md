##### CrossEntropyLoss 交叉熵损失
为什么不选MSEloss：
![[Pasted image 20251107205425.png]]
##### L1 loss
![[Pasted image 20251111142737.png]]
##### NMAE
![[Pasted image 20251111142108.png]]
![[Pasted image 20251111142146.png]]
##### MSE 
![[Pasted image 20251111073221.png]]
![[Pasted image 20251111073203.png]]

```
import torch.nn as nn
loss_fn = nn.MSELoss()  # 除以n，不除以2
```
