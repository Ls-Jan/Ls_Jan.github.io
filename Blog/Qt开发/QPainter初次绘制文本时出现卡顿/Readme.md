

原因不明，至少确定是``QPainter``的锅，多半是懒汉初始化搞的鬼。
它导致了``QPainter.drawText``和``QTextDocument.drawContents``在初次调用时出现短暂的卡顿。
为了避免这厮在部分场合造成问题，可以选择在程序or控件的初始化阶段显式调用``QPainter.drawText``提前处理掉这卡顿。


<br>

# 测试代码-1：
下例代码运行后会出现一个窗口，点击窗口可观察控制台输出情况。
首次点击窗口时程序会很明显地在第二步**2.DrawText**处发生了短暂停顿，但再次点击时卡顿问题消失，
简单的说就是使用``QPainter.drawText``首次绘制文本时会有卡顿问题。

```py
from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
from PyQt5.QtGui import *

class Test(QLabel):
	def mousePressEvent(self,event):
		tx="ABCDE"
		print("1.DrawStart")
		pix=QPixmap(self.size())
		pix.fill(Qt.transparent)
		ptr=QPainter(pix)
		ptr.setFont(QFont("",32))
		print("2.DrawText")
		ptr.drawText(0,100,tx)
		ptr.end()
		print("3.DrawEnd")
		self.setPixmap(pix)
		print("4.SetPix\n")
	def paintEvent(self,event):
		print('1.PaintStart')
		super().paintEvent(event)
		print('2.PaintEnd\n')


if True:
	app=QApplication([])
	lb=Test()
	lb.resize(640,480)
	lb.show()

	app.exec()
```

<br>

# 测试代码-2：
同样的，下例代码运行后会出现一个窗口，点击窗口后很明显地在**1.PaintStart**处发生了短暂停顿。
说明这卡顿问题同样发生在``QLabel.paintEvent``中。

简单的说就是这问题并不是``QPainter-QPixmap``组合触发的，在``QPainter-QWidget``也出现同样问题，
这就很好的说明了卡顿问题完完全全是由``QPainter``造成的。

```py
from PyQt5.QtWidgets import *
from PyQt5.QtCore import *
from PyQt5.QtGui import *

class Test(QLabel):
	def mousePressEvent(self,event):
		tx="ABCDE"
		print('1.SetStart')
		self.setText(tx)
		print('2.SetUpdate')
		self.update()
		print('3.SetEnd\n')
	def paintEvent(self,event):
		print('1.PaintStart')
		super().paintEvent(event)
		print('2.PaintEnd\n')

if True:
	app=QApplication([])
	lb=Test()
	lb.resize(640,480)
	lb.show()

	app.exec()
```
