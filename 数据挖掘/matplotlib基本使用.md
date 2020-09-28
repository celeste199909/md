## matplotlib基本使用

### 问题

#### `pip`换源

```bash
pip install django -i https://mirrors.aliyun.com/pypi/simple/

# 清华
https://pypi.tuna.tsinghua.edu.cn/simple/
# 中科大
https://pypi.mirrors.ustc.edu.cn/simple/
# 阿里云
https://mirrors.aliyun.com/pypi/simple/
# `豆瓣
http://pypi.douban.com/simple/
```

#### 中文乱码

```python
# coding:utf-8
```

#### 绘图不能显示中文

> RuntimeWarning: Glyph 26102 missing from current font.

```python
import matplotlib.pyplot as plt
plt.rcParams[‘font.sans-serif’]=[‘SimHei’] #用来正常显示中文标签
plt.rcParams[‘axes.unicode_minus’]=False #用来正常显示负号
```



### 环境搭建

- `jupyter notebook`

### `Matplotlib`绘图

- 安装

  ```bash
  # 更新pip
  python -m pip install -U pip setuptools
  # 安装matplotlib
  pip install matplotlib -i https://mirrors.aliyun.com/pypi/simple/
  ```

- 折线图

  ```python
  import matplotlib.pyplot as plt
  # %matplotlib inline
  
  # 创建画布
  plt.figure(figsize = (20, 9), dpi = 80)
  # 绘图
  plt.plot([1, 0, 9], [4, 5, 6])
  # 保存
  plt.savefig("./test1.png")
  # 展示
  plt.show()
  ```

- 完善

  ```python
  # coding:utf-8
  # 需求：画出某城市11点到12点1小时内每分钟的温度变化折线图，温度范围在15度-18度
  
  import random     # 引入随机模块
  
  # 1. 准备数据 x, y
  
  x = range(60)
  y_shanghai = [random.uniform(15, 18) for i in x]     # 均匀分布
  
  # 2. 创建画布
  
  plt.figure(figsize = (20, 8), dpi = 80)
  
  # 3. 绘制图像
  
  plt.plot(x, y_shanghai)
  
  # 修改x，y刻度
  x_label = ["11时{}分".format(i) for i in x]
  plt.xticks(x[::5], x_label[::5])
  plt.yticks(range(0, 40, 5))
  
  # 添加网格显示
  plt.grid(linestyle = "--", alpha = 0.5)
  
  # 添加描述信息
  plt.xlabel("时间变化")
  plt.ylabel("温度变化")
  plt.title("某城市11点到12点没分钟的温度变化折线图")
  
  # 4. 显示图像
  plt.show()
  ```

  更多参考[文档](https://matplotlib.org/tutorials/index.html)







