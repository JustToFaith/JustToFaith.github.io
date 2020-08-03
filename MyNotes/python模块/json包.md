# JSON

**dumps** 将字典传入，接受返回值是一个json字符串

```python
import json


data = {"name": "python", "age": 18}
json_str = json.dumps(data)
# 返回的json_str是一个json格式的字符串
```



**loads** 将json格式的字符串转换为python中的字典

```python
import json


a = '{"name": "python", "age": 18}'
dic = json.loads(a)
# 返回的dic是一个python字典
```





