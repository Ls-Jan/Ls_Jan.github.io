

这里不会提及如何请求数据，只是稍微提一嘴，通常使用的http请求都不会带有缓存的，像是使用``requests.Session.get(str)``、``QNetworkAccessManager.get(QNetworkRequest)``，它们都是调用一次就立马请求一次数据。

怎么说呢，就是复数个请求都是指向同一资源的话它们并不会合并为一个请求，因此最好写一个缓存代理类来对每个请求的url进行记录/缓存。


<br>

***

这里附上一个简单的http本地服务器代码，以便证明上面“http请求没带缓存”的说法。样例代码说实话懒得写，并且感觉没那必要。
使用的类是``BaseHTTPRequestHandler``，虽然可以使用套接字``socket``但很显然用这玩意儿的话还得手动构造一段长的要死的HTTP响应。

```py
from http.server import HTTPServer, BaseHTTPRequestHandler
class ProxyHandler(BaseHTTPRequestHandler):
	def do_GET(self):
		# 处理GET请求
		print(">>>",self.path,end=' ')
		# 发送响应
		self.send_response(200)
		self.send_header('Content-type', 'text/html')
		self.end_headers()
		self.wfile.write(b"AAAAAA")
		self.wfile.write(b"AAAAAA")
	def do_POST(self):
		# 处理POST请求
		# 实现自定义逻辑
		pass

if True:
	host = ('127.0.0.1', 12345)
	server = HTTPServer(host, ProxyHandler)
	server.serve_forever()
```


# 参考：
- 基于BaseHTTPRequestHandler的HTTP服务器基础实现：[https://www.cnblogs.com/beyond-tester/p/17786114.html](https://www.cnblogs.com/beyond-tester/p/17786114.html)
- Python 使用 BaseHTTPRequestHandler 写入响应体：[https://geek-docs.com/python/python-ask-answer/175_python_writing_response_body_with_basehttprequesthandler.html](https://geek-docs.com/python/python-ask-answer/175_python_writing_response_body_with_basehttprequesthandler.html)
- socket发送Http协议的请求与响应格式解析：[https://blog.csdn.net/qq_37910618/article/details/103870446](https://blog.csdn.net/qq_37910618/article/details/103870446)
- 正确理解requests库的requests.Session()：[https://www.cnblogs.com/Avicii2018/p/17637325.html](https://www.cnblogs.com/Avicii2018/p/17637325.html)




