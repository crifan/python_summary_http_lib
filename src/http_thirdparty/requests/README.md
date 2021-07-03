# requests

* `requests`
  * 概述：为人类用起来很方便而开发的Python的网络库。是目前最流行的Python的网络库
  * GitHub
    * psf/requests: A simple, yet elegant HTTP library
      * https://github.com/psf/requests
  * 官网文档
    * https://docs.python-requests.org

## 如何使用requests

官网文档，已经写的足够好了，就不重复了。详见：

* requests官网资料
  * 英文
    * Quickstart — Requests 2.11.1 documentation
      * http://docs.python-requests.org/en/master/user/quickstart/#make-a-request
  * 中文
    * Requests: 让 HTTP 服务人类 — Requests 2.18.1 文档
      * http://cn.python-requests.org/zh_CN/latest/
    * 快速上手 — Requests 2.18.1 文档
      * http://cn.python-requests.org/zh_CN/latest/user/quickstart.html
        * host在readthedocs
          * Quickstart — Requests 2.23.0 documentation
            * https://requests.readthedocs.io/en/master/user/quickstart/

此处仅仅（摘录）列举最简单的内容，供了解：

### 安装
```bash
pip install requests
```

#### 基本用法举例

导入，get请求：

```python
import requests

r = requests.get('https://api.github.com/events')
```

POST带参数，内部会自动把`dict`转换成`json`：

```python
r = requests.post('https://httpbin.org/post', data = {'key':'value'})
```

GET带参数，内部自动转换`dict`为`key=value`的`query parameter`

```python
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get('https://httpbin.org/get', params=payload)
```

返回的response：

```python
r = requests.get('https://api.github.com/events')
r.text
# '[{"repository":{"open_issues":0,"url":"https://github.com/...
```

response有各种属性可用：

查看字符编码：

```python
r.encoding
# 'utf-8'
```

手动设置编码：

```python
r.encoding = 'ISO-8859-1'
```

返回内容：

```python
r.content
# b'[{"repository":{"open_issues":0,"url":"https://github.com/...
```

等等。
