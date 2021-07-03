# urllib

此处先介绍Python的内置网络库的发展历史：

* 背景：Python有2个大的版本：`Python2`和`Python3`
* `Python2`
  * 有独立的2个库：[urllib](https://docs.python.org/2.7/library/urllib.html) 和[urllib2](https://docs.python.org/2.7/library/urllib2.html)
    * 缺点：功能划分的不是很清楚
* `Python3`
    * 所以合并了`urllib`和`urllib2`为单个库：[urllib](https://docs.python.org/3/library/urllib.html)
      * 其中：`Python2`的`urllib2`合并成`Python3`的[urllib.request](https://docs.python.org/3/library/urllib.request.html#module-urllib.request)

## crifan的Python的网络函数

自己很早之前在折腾爬虫期间，就利用(Python2的)`urllib`总结出了自己的获取`http`的`response`或`html`的函数：

* [Python2 crifanLib.py](https://github.com/crifan/crifanLibPython/blob/master/python2/crifanLib.py)
  * `getUrlResponse`
  * `getUrlRespHtml`

且还给出了详细的[说明文档](https://www.crifan.com/files/doc/docbook/crifanlib_python/release/html/crifanlib_python.html#network_http_funcs)
