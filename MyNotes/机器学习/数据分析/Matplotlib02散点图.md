### `Matplotlib`绘制散点图

1. 导入`matplotlib.pyplot`和`font_maneger`

   ```python
   import matplotlib.pyplot as plt
   from matplotlib import font_manager
   ```

2. 设置字体

   ```python
   my_font = font_manager.FontProperties(fname="/usr/share/fonts/opentype/noto/NotoSansCJK-Bold.ttc")
   ```

   `fname`是字体的安装路径，可以通过`fc-list`查看字体，`fc-list :lang=zh`查看中文字体

3. 设置`x`和`y`的值

   ```python
   y_3 = [11,12,14,15,15,15,12,22,23,15,23,24,34,14,15,30,15,15,16,18,15,12,22,18,33,15,35,15,34,38,39]
   y_10 = [11,12,23,34,23,25,32,12,23,25,13,22,24,34,12,20,35,15,16,15,13,22,32,14,34,13,33,13,24,33,33]
   
   x3 = range(0,31)
   x10 = range(31,62)
   ```

4. 设置图片大小

   ```python
   plt.figure(figsize=(20,8), dpi=80)
   ```

   `figsize`是图像的尺寸大小，`dpi`是清晰度

5. 绘点

   ```python
   plt.scatter(x3,y_3, label="3月份")
   plt.scatter(x10,y_10, label="10月份")
   plt.legend(loc="upper left", prop=my_font
   ```

   `label`是设置图标的名字， `loc` 是图标出现的位置，`prop`是设置中文字体，防止中文乱码

6. 调整`x`轴刻度

   ```python
   _x = list(x3)+list(x10)   # 拼接ｘ轴坐标
   _xtick_labels = ["3月{}日".format(i) for i in x3]  # ｘ轴的刻度名字
   _xtick_labels += ["10月{}日".format(i) for i in x10]  #　同ｘ轴的名字
   plt.xticks(_x[::3], _xtick_labels[::3], fontproperties=my_font, rotation='45')  # 绘制ｘ轴坐标
   ```

   `fontproperties`设置中文字体, `rotation`刻度的旋转角度

7. 添加坐标轴描述信息

   ```python
   plt.xlabel("时间", fontproperties=my_font)
   plt.ylabel("温度", fontproperties=my_font)
   ```

8. 显示图像

   ```python
   plt.show()
   ```

   

### 完整代码

```python
# -*- coding:utf-8 -*-
import matplotlib.pyplot as plt
from matplotlib import font_manager


my_font = font_manager.FontProperties(fname="/usr/share/fonts/opentype/noto/NotoSansCJK-Bold.ttc")

# 设置ｘ和ｙ的值
y_3 = [11,12,14,15,15,15,12,22,23,15,23,24,34,14,15,30,15,15,16,18,15,12,22,18,33,15,35,15,34,38,39]
y_10 = [11,12,23,34,23,25,32,12,23,25,13,22,24,34,12,20,35,15,16,15,13,22,32,14,34,13,33,13,24,33,33]

x3 = range(0,31)
x10 = range(31,62)
# 设置图片大小
plt.figure(figsize=(20,8), dpi=80)

plt.scatter(x3,y_3, label="3月份")
plt.scatter(x10,y_10, label="10月份")
plt.legend(loc="upper left", prop=my_font)

# 调整ｘ轴刻度
_x = list(x3)+list(x10)
_xtick_labels = ["3月{}日".format(i) for i in x3]
_xtick_labels += ["10月{}日".format(i) for i in x10]
plt.xticks(_x[::3], _xtick_labels[::3], fontproperties=my_font, rotation='45')

# 添加坐标轴描述信息
plt.xlabel("时间", fontproperties=my_font)
plt.ylabel("温度", fontproperties=my_font)

plt.show()
```

