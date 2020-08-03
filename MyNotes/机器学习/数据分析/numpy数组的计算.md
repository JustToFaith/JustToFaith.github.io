1. ``numpy`读取本地数据

   ```python
   np.loadtxt(frame, dtype=np.float, delimiter=None, skiprows=0, usecols=None, unpack=False)
   ```

   

| 参数        | 解释                                                         |
| ----------- | ------------------------------------------------------------ |
| `frame`     | 文件、字符串或产生器，可以是`.gz`或`bz2`压缩文件             |
| `dtype`     | 数据类型，可选，CSV的字符串以什么数据类型读入数组中，默认`np.float` |
| `delimiter` | 分隔字符串，默认是任何空格，改为**逗号**                     |
| `usecols`   | 跳过前x行，索引，元组类型                                    |
| `unpack`    | 如果True，读入属性将分别写入不同数组变量，False读入数据只写入一个数组变量，默认False |

`unpack`是转置效果，行和列进行交换。

 	2. 其他的转置
     - `transpose()`
     - `T`
     - `swapaxes(1, 0)`默认情况下轴的序列的(0,1)，这里通过交换轴来达成转置的目的



