## 一、PIL/Pillow核心概念

### 1. **库名演变**

- **PIL** (Python Imaging Library)：原始库，已停止维护
    
- **Pillow**：PIL的分支，继续维护，完全兼容PIL
    
- **安装命令**：`pip install pillow`
    
- **导入方式**：仍使用`from PIL import ...`保持兼容
    

### 2. **主要用途**

- 图像文件的读取、处理和保存
    
- 基本图像操作（裁剪、旋转、缩放等）
    
- 格式转换（JPEG、PNG、BMP等）
    
- 图像绘制和文字添加
    

## 二、PIL的核心模块

### 1. **Image模块**（最常用）

python

from PIL import Image

# 基本操作
img = Image.open('image.jpg')      # 打开图像
img.save('output.png')             # 保存图像
img.show()                         # 显示图像

# 图像属性
print(img.size)      # 尺寸 (宽, 高)
print(img.mode)      # 颜色模式 (RGB, L, RGBA等)
print(img.format)    # 格式 (JPEG, PNG等)

# 图像转换
img_rgb = img.convert('RGB')       # 转为RGB模式
img_gray = img.convert('L')        # 转为灰度图

### 2. **常用图像操作**

python

# 几何变换
img_resized = img.resize((224, 224))    # 调整大小
img_rotated = img.rotate(45)            # 旋转
img_cropped = img.crop((10, 10, 100, 100))  # 裁剪

# 颜色处理
img_transposed = img.transpose(Image.FLIP_LEFT_RIGHT)  # 水平翻转

### 3. **ImageFilter模块**（图像滤波）

python

from PIL import ImageFilter

img_blur = img.filter(ImageFilter.GaussianBlur(radius=2))
img_sharp = img.filter(ImageFilter.SHARPEN)
img_edge = img.filter(ImageFilter.FIND_EDGES)

### 4. **ImageDraw模块**（绘图）

python

from PIL import ImageDraw, ImageFont

draw = ImageDraw.Draw(img)
draw.rectangle([50, 50, 150, 150], outline='red', width=2)  # 画矩形
draw.text((100, 100), "Text", fill='blue')                  # 添加文字

## 三、在深度学习中的应用

### 1. **数据预处理**

python

from PIL import Image
from torchvision import transforms

# 定义预处理流程
transform = transforms.Compose([
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406],
                        std=[0.229, 0.224, 0.225])
])

# 加载和处理图像
img = Image.open('image.jpg').convert('RGB')
img_tensor = transform(img)  # 转为PyTorch Tensor

### 2. **自定义数据集**

python

from torch.utils.data import Dataset
from PIL import Image

class CustomImageDataset(Dataset):
    def __init__(self, file_paths, labels, transform=None):
        self.file_paths = file_paths
        self.labels = labels
        self.transform = transform
    
    def __len__(self):
        return len(self.file_paths)
    
    def __getitem__(self, idx):
        # 用PIL加载图像
        image = Image.open(self.file_paths[idx]).convert('RGB')
        label = self.labels[idx]
        
        if self.transform:
            image = self.transform(image)
        
        return image, label

### 3. **数据增强**

python

from torchvision import transforms
import random
from PIL import Image, ImageFilter

class CustomAugmentation:
    def __call__(self, img):
        # 随机颜色抖动
        if random.random() > 0.5:
            img = transforms.ColorJitter(
                brightness=0.1,
                contrast=0.1,
                saturation=0.1
            )(img)
        
        # 随机高斯模糊
        if random.random() > 0.7:
            img = img.filter(ImageFilter.GaussianBlur(radius=1))
        
        return img

## 四、与其他图像库的对比

| 库                | 优点                | 缺点         | 适用场景        |
| ---------------- | ----------------- | ---------- | ----------- |
| **PIL/Pillow**   | 简单易用，Pythonic，轻量级 | 功能相对基础     | 基本图像处理，数据加载 |
| **OpenCV**       | 功能强大，速度快，算法丰富     | 接口复杂，BGR格式 | 计算机视觉，视频处理  |
| **scikit-image** | 科研算法丰富，文档好        | 性能一般       | 图像算法研究      |
| **matplotlib**   | 可视化强大，集成在科学计算中    | 不适合大规模处理   | 图像显示，简单处理   |