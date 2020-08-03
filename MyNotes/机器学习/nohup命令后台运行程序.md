nohup 命令
运行命令

```
nohup jupyter notebook --allow-root &
```

搜索相关进程，后面grep -v 是忽略grep自身进程。

```
ps -ux | grep jupyter-notebook | grep -v grep
```

结束进程

```
kill -9 pid
```

