---
title: "每天一个python标准库00"
date: 2019-04-08T22:41:26+08:00
draft: false
---

## itertools --- 为高效循环而创建迭代器的函数

```python
In [2]: dir(itertools)
Out[2]: 
['__doc__',
 '__loader__',
 '__name__',
 '__package__',
 '__spec__',
 '_grouper',
 '_tee',
 '_tee_dataobject',
 'accumulate',
 'chain',
 'combinations',
 'combinations_with_replacement',
 'compress',
 'count',
 'cycle',
 'dropwhile',
 'filterfalse',
 'groupby',
 'islice',
 'permutations',
 'product',
 'repeat',
 'starmap',
 'takewhile',
 'tee',
 'zip_longest']

```

### 无穷迭代器

```python
from itertools import count
[i for i in count(10)] -- > [10,11,12,13,14....]
```

