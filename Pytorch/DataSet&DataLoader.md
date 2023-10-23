## 创建个人DataSet

![](https://cdn.nlark.com/yuque/0/2023/png/22902390/1698032126365-a5b2ef60-59c9-4907-929b-aea7b3f20e97.png)

自定义的DataSet类需要继承自DataSet类, 实现__getitem__函数, __init__函数, __len__函数

示例如下:

```Python
import os
import pandas as pd
from torchvision.io import read_image

class CustomImageDataset(Dataset): 
    def __init__(self, annotations_file, img_dir, transform=None, target_transform=None):
        self.img_labels = pd.read_csv(annotations_file)
        self.img_dir = img_dir
        self.transform = transform
        self.target_transform = target_transform

    def __len__(self):
        return len(self.img_labels)

    def __getitem__(self, idx):
        img_path = os.path.join(self.img_dir, self.img_labels.iloc[idx, 0])
        image = read_image(img_path)
        label = self.img_labels.iloc[idx, 1]
        if self.transform:
            image = self.transform(image)
        if self.target_transform:
            label = self.target_transform(label)
        return image, label
```

