# requests使用心得

自己在使用requests期间，有些心得和总结，整理如下：

## crifan的requests总结

把基于requests的常用功能，封装成函数，供需要的参考：

https://github.com/crifan/crifanLibPython/blob/master/python3/crifanLib/thirdParty/crifanRequests.py

## 其他心得

### get加上参数

举例：

```python
payload = {'key1': 'value1', 'key2': 'value2’}
r = requests.get('https://httpbin.org/get', params=payload)
print(r.url)
# https://httpbin.org/get?key2=value2&key1=value1
```

### 加代理

* 说明：加上`proxies`参数

* 举例：

```py
proxyDict = {
  "http":  "http://127.0.0.1:58591",
  "https": "http://127.0.0.1:58591",
}

...

resp = requests.post(someUrl, proxies=proxyDict, headers=reqHeaderDict, data=reqBodyJsonStr)
```

额外说明：

如果是带用户名和密码的（往往是第三方代理服务器），则代理地址是类似于这种：

```py
ProxyHost = "http-proxy-t3.dobel.cn"
ProxyPort = "9180"

#账号密码
ProxyUser = "YourUserName"
ProxyPass = "YourPassword"

ProxyUri = "http://%(user)s:%(pass)s@%(host)s:%(port)s" % {
  "host" : ProxyHost,
  "port" : ProxyPort,
  "user" : ProxyUser,
  "pass" : ProxyPass,
}
```

-》 对应完整代理地址，类似这种：

`http://YourUserName:YourPassword@http-proxy-t3.dobel.cn:9180`

更多例子和解释，详见：

[requests · 网络中转站：代理技术](https://book.crifan.org/books/web_transfer_proxy_tech/website/add_proxy/program_language/python/requests.html)

### header和post的body

举例：

```python
gHeaders = {
  "Authorization": "JWT xxx", # JWT eyJ0xxxxxxxiJ9.xxxxxxxxx.hklGrByjU-v_xxxxx_-dc
}

postBodyDict = {
  "username": Username,
  "password": Password,
}
getTokenResp = requests.post(GetJwtTokenUrl, data=postBodyDict)

saveScriptResp = requests.post(CreateScriptUrl, headers=gHeaders, data=curScriptDict)
```

### 传入post的data时，不要dict的json，而是要str的json字符串

注：对标常见情况是，自己把dict的用dict转换成json字符串，再给data

```python
import requests
import json

scritpJsonStr = json.dumps(curScriptDict)
saveScriptResp = requests.post(CreateScriptUrl, headers=gHeaders, data=scritpJsonStr)
```

或：

或者把dict传递给json，requests自动帮你转换成json

```python
import requests

saveScriptResp = requests.post(CreateScriptUrl, headers=gHeaders, json=curScriptDict)
```

-> 说明不是两者同时传递的参数，而是二选一

### 传递json类型参数时，记得加上content-type的header

```python
gHeaders = {
  'Content-Type': 'application/json; charset=utf-8',
  "Accept": 'application/json',
}
```

否则可能会导致 [TypeError string indices must be integers](http://www.crifan.com/python_call_api_typeerror_string_indices_must_be_integers)

### get返回二进制，保存到文件

下载文件 下载二进制文件

```python
import requests
resp = requests.get(pictureUrl)
with open(saveFullPath, 'wb') as saveFp:
  saveFp.write(resp.content)
```

举例：

```python
gHeaders = {
  "User-Agent": UserAgent_Mac_Chrome,
}
curPictureUrl = "https://bp.pep.com.cn/ebook/yybanjxc/files/mobile/1.jpg?200209175611"
saveFullPath = "output/义教教科书英语八年级下册/mobile_1.jpg"
resp = requests.get(curPictureUrl, headers=gHeaders)
if resp.ok:
  with open(saveFullPath, 'wb') as saveFp:
    saveFp.write(resp.content)
```

### 上传multipart/form-data表单参数和文件

#### 上传multipart/form-data表单form参数

`curl`命令：

```bash
curl -X PUT http://127.0.0.1:8080/api/xxx ...
-H 'content-type: multipart/form-data; boundary=----xxx' \
-F taskStatus=1
```

对应`Python`的`requests`的：[More complicated POST requests](https://requests.readthedocs.io/en/master/user/quickstart/#more-complicated-post-requests)

代码：

```python
    updateTaskUrl = "http://127.0.0.1:8080/api/xxx"
    updateInfoDict = {
        "taskStatus": 1,
    }
    resp = requests.put(updateTaskUrl, data=updateInfoDict)
```

#### 上传multipart/form-data文件

`curl`命令：

```bash
curl -X POST http://127.0.0.1:8080/api/xxx ...
-H 'content-type: multipart/form-data; boundary=----xxx' \
-F file=@/Users/xxx.txt
```

对应`Python`的`requests`的：[POST a Multipart-Encoded File](https://requests.readthedocs.io/en/master/user/quickstart/#post-a-multipart-encoded-file)

代码：

```python
    filePath = "/Users/xxx.txt"
    fileFp = open(filePath, 'rb')
    fileInfoDict = {
        "file": fileFp,
    }
    resp = requests.post(uploadResultUrl, files=fileInfoDict)
```
