### Matplotlib



一、基本语法

1. 导入`Matplotlib`

   ```python
   import matplotlib.pyplot as plt
   ```

2. 设置x轴和y轴

   ```python
   x = []
   y = []
   ```

3. 设置显示图像大小

   ```python
   plt.figure(figsize=(x, y ), dpi=80)
   ```

   x 和y 是图形的长和宽，`dpi`是图像的清晰度

4. 绘制图形

   ```python
   plt.plot(x, y,
           lable="图例", 
           color="orange",  # 颜色
           linestyle="--", # 线条风格
           linewidth=5, # 线条粗细
           alpha=0.5　# 透明度)
   ```

5. 保存图形，在绘制之后

   ```python
   plt.savefig("./图片.png")
   ```

6. 展示图形

   ```python
   plt.show()
   ```

   

二、更多语法

1. 对x轴刻度的调整

   ```python
   _x = x
   _xtick_labels = ["hello,{}".format(i) for i in _x]
   plt.xticks(x, _xtick_labels,rotation=90)
   ```
   
这样`x`轴上会显示`hello1`到`hello119`的字符,`rotation`旋转90度。`rotation`是将`x`轴字符旋转90°
   
2. 设置中文字体`rc`

   ```python
   import matplotlib
   from matplotlib import font_manager
   
   
   # windows 和 linux 设置字体方式
   font = {
       'family': 'MicroSoft YaHe',
       'weight': 'bold',
       'size': 'larger'
   }
   matplotlib.rc("font", **font)
   matplotlib.rc("font", family ='MicroSoft YaHe',weight='bold')
   
   # 设置字体的另一种方式Linux/windows/mac
   my_font = font_manager.FontProperties(fname="/usr/share/fonts/opentype/noto/NotoSansCJK-Bold.ttc")
   
   plt.xticks(_x, _xtick_labels, rotation=45, fontproperties=my_font)
   
   ```

3. 添加网格，设置网格的宽可以通过设置`x`或`y`的坐标尺来设置　　

   ```python
   plt.grit(alpha=0.1)  # alpha设置网格的粗细
   ```

4. 设置图例

   ```python
   plt.plot(x, y, label="标签名")
   plt.legend(prop=my_font, loc="upper letf")  # loc设置图例的位置，可以是数字，也可以是英文
   ```



**下面代码可以跑，可以自己试一试效果怎么样**

```python
# -*- coding:utf-8 -*-
import matplotlib.pyplot as plt
import random
from matplotlib import rc
import matplotlib 
from matplotlib import font_manager


# windows和linux设置字体方式
# font = {
#     'family': 'MicroSoft YaHe',
#     'weight': 'bold',
#     'size': 'larger'
# }
# matplotlib.rc("font", **font)
# matplotlib.rc("font", family ='MicroSoft YaHe',weight='bold')


# 设置字体的另一种方式
my_font = font_manager.FontProperties(fname="/usr/share/fonts/opentype/noto/NotoSansCJK-Bold.ttc")

y1 = [1,0,1,1,2,4,3,2,3,4,4,5,6,5,4,3,3,1,1,1]
y2 = [1,0,3,1,2,2,3,3,2,1,2,1,1,1,1,1,1,1,1,1]

x = range(11, 31)


plt.figure(figsize=(20, 8), dpi=80) 

plt.plot(x, y1, label="自己", color="orange", linestyle="-")
plt.plot(x, y2, label="同桌", color="red", linestyle=":")

_xtick_labels = ["{}岁".format(i) for i in x] 
plt.xticks(x, _xtick_labels, rotation=45, fontproperties=my_font)

# 添加描述信息
plt.xlabel("年龄", fontproperties=my_font)
plt.ylabel("数目", fontproperties=my_font)
plt.title("交朋友的数目", fontproperties=my_font)

plt.grid(alpha=0.4, linestyle=":")

plt.legend(prop=my_font, loc=0)

plt.show()
```

