k-means算法以k为参数，把n个对象分成k个簇，是簇内具有较高的相似度，而簇间的相似度较低

- 随机选取k个点作为初始的聚类中心
- 对于剩下的点，根据其与聚类中心的距离，将其归入最近的簇
- 对每一个簇，计算所有点的均值作为新的聚类中心
- 重复2、3直到聚类中心不再发生改变

```python
if __name__ == '__main__':
    data,cityName = loadData('city.txt')    # 利用loadDate方法读取数据
    km = KMeans(n_clusters = 3)
    expenses = np.sum(km.cluster_centers_,axis = 1) 	# 聚类中心点的数值加和，也就是平均消费水平
    # print(expenses)
    CityCluster = [[],[],[]]
    for i in range(len(cotyName)):
        CityCluster[label[i].append(cityName[i])]
    for i in range(len(CityCluster)):
        print('Expenses:%.2f' % expenses[i])
        print(CityCluster)
```

##### 1.利用loadData方法读取数据

##### 2.创建示例

##### 3.调用Kmeans() fit_predict()方法进行计算

- n_clusters:用于指定聚类中心的个数
- init:初始聚类中心的初始化方法
- max_iter:最大的迭代次数
- 一般调用时只能给出n_clusters即可，init默认是k-means++, max_iter 默认是300
- data:加载的数据
- label:聚类后各数据所属的标签
- fit_predict():计算簇中心以及簇分配序号

##### 重点方法解释：data,cityName = loadData('city.txt')

```python
def loadData(filePath):
    fr = open(filePath,'r+')
    lines = fr.readlines()
    retData = []
    retCityName = []
    for line in lines:
        items = line.strip().split(",")
        retCityName.append(items[0])
        retData.append([float(items[i]) for i in range(1,len(items))])
        return retData,retCityName

```

- r+: 读取打开一个文本文件
- .readlines() 一次读取整个文件（类似于.read()）
- retCityName: 用来存储城市名称
- retData: 用来存储城市的各项消费信息
- 返回值：返回城市名称，以及该城市的各项消费信息

