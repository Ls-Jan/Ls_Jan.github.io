

1. 很多绘制都是不可控的，相当多情况下得借助样式表才能修改样式；
2. 除了文本外，无法获取样式表对控件的作用的实际参数，也无法通过任何方式将这些参数传递进``QStyle.drawControl``中(例如背景色、边框色、边框粗细、复选框大小、滚动条各种细化参数等等等等)；
3. 想通过在控件类的绘制函数``paintEvent``中选择调用``QStyle.drawControl``并不会给你带来多少快来，反倒是折磨，还不如自己手绘；
4. 在经过前三点的受苦体验后，你决定在``QWidget.paintEvent``中自己手动绘制，只不过效果只会限制在相应的控件类中，如果想重用性更高的话倒不如重写``QProxyStyle.drawControl``，然后控件直接调用``setStyle(style)``，以达到更高级的代码复用；








