

是否出现过这种情况：
重写了``QWidget.sizeHint``后，无论怎么设置大小策略``QWidget.setSizePolicy``，调整窗口时大小依旧是无法压下去？(也就是被一股莫名的力量限制住最小宽高)。
因为还少了个函数``QWidget.minimumSizeHint``没写呢哈哈哈哈哈，一套组合技哐哐给我来两拳，搞我心态，


# 参考：
- Qt学习之控件尺寸调整策略（QSizePolicy）：[https://blog.csdn.net/kongcheng253/article/details/128769765](https://blog.csdn.net/kongcheng253/article/details/128769765)
- QT sizeHint 及 Policy的用法：[https://blog.csdn.net/qq_40732350/article/details/86703749](https://blog.csdn.net/qq_40732350/article/details/86703749)



