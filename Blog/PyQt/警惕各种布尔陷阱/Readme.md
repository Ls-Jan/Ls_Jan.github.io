

如题。

这里给出几个实例供大伙乐呵乐呵，真的被整麻。


<br>

# ``QLayout``布局：
诸如``QBoxLayout``、``QGridLayout``等，它们判断布尔的依据是布局中是否有内容物，判断逻辑和基础容器``list``、``set``是一致的。
而有时候会习惯性地将``QWidget.layout()``直接丢if语句中判断，这就已经是一发背刺了，因为``QWidget.layout()``返回非None的``QLayout``有可能依旧是进入False判断，因为空的``QLayout``的布尔值为False。

```py
vbox=QVBoxLayout()
print(bool(vbox))#bool(vbox)实际上是(not vbox.isEmpty())
```

<br>

# ``QSize``：
它的布尔值实际上是调用``QSize.isValid``，这玩意儿不可靠，如果宽高为0依旧是返回True，被狠狠地背刺了一把，破口大骂。
``QSize.isEmpty``才能保证数据是否有效。

```py
s1=QSize()#实际上是(-1,-1)
s2=QSize(0,0)
s3=QSize(1,1)
print(bool(s1))#False
print(bool(s2))#True。没错，宽高只要不为负数，这厮就返回True
print(bool(s3))#True
print(not s1.isEmpty())#False
print(not s2.isEmpty())#False。用QSize.isEmpyt就能保证宽高大于0时才返回True
print(not s3.isEmpty())#True
```




