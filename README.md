# 一个爬取v2ex.com网站的爬虫

学习scrapy写的一个小爬虫

## 不建议自行运行爬虫，数据已经有了

## 爬取相关数据说明

数据都放在了sqlite数据库，方便分，整个数据库大小2.1GB。

爬虫源码放在了GitHub，在GitHub我release了完整的sqlite数据库文件

爬虫从`topic_id = 1`开始爬，路径为`https://www.v2ex.com/t/{topic_id}`。 服务器可能返回404/403/302/200，如果是404说明帖子被删除了，如果是403说明是爬虫被限制了，302一般是跳转到登陆页面，有的也是跳转到主页，200返回正常页面。

爬虫没有登陆，所以爬取的数据不完全，比如水下火热的帖子就没有爬到，还有就是如果是302的帖子会记录帖子id，404/403不会记录。

爬取过程中会帖子内容，评论，以及评论的用户信息。

注1：爬了一半才发现V站帖子附言没有爬，附言从`topic_id = 448936`才会爬取

注2：select count(*) from member 得到的用户数比较小，大概20W，是因为爬取过程中是根据评论，以及发帖信息爬取用户的，如果一个用户注册之后既没有评论也没有发帖，那么这个账号就爬不到。还有就是因为部分帖子访问不了，也可能导致部分账号没有爬。还有部分用户号被删除，这一部分也没有爬。（代码改了，可以爬，但是都已经爬完了……）

注3：时间均为UTC+0的秒数

## 运行

### 安装依赖

```bash
pip install -r .\requirements.txt
```

### 配置

#### 代理

更改 `v2ex_scrapy/settings.py` 中 `PROXIES`的值 如

```python
[
     "http://127.0.0.1:7890"
]
```

请求会随机选择一个代理，如果需要更高级的代理方式可以使用第三方库，或者自行实现Middleware

### 运行爬虫

```bash
scrapy crawl v2ex
```

### 注意事项

爬取过程中出现403基本上是因为IP被限制了，等待一段时间即可