# 解决场景：flask需要并发

原文地址：https://blog.csdn.net/danny_amos/article/details/50859383

完整代码:<https://github.com/wskssau/my_notespace>的 python/todo_app

解决方案: flask+gevent

安装gevent

```
pip install gevent1
```

修改代码

```
# 文件头部
from gevent import monkey
from gevent.pywsgi import WSGIServer

# 在玩websockets，可以无视之哈，有空贴下flask websockets实现哈
from geventwebsocket.handler import WebSocketHandler

import time

＃　gevent的猴子魔法
monkey.patch_all()

app = Flask(__name__)

app.config.update(
    DEBUG=True
)

@app.route('/asyn/1/', methods=['GET'])
def test_asyn_one():
    if request.method == 'GET':
        time.sleep(10)
        return 'hello asyn'


@app.route('/test/', methods=['GET'])
def test():
    return 'hello test'


if __name__ == "__main__":
    # app.run()
    http_server = WSGIServer(('', 5000), app, handler_class=WebSocketHandler)
    http_server.serve_forever()
```

运行之后可以先访问/asyn/1/再访问/test/，可以明显发现，/asyn/1/在做耗时任务时不会影响其他请求
