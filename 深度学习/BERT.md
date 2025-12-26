[[MAE]]

- 架构：基于 **Transformer Encoder**
    
- 自监督方法：**Masked Language Modeling (MLM)** → 随机遮盖句子中的词，然后让模型预测被遮盖的词
    
- 所以 BERT 的“强大”不仅因为 Transformer 架构，还因为这种**自监督的表征学习方法**