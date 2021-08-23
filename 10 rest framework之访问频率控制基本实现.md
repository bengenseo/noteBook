# rest framework之访问频率控制基本实现

## 自定义

```python
import time
# 数组放在缓存里或数据库里
VISIT_RECORD= {}
class VisitThrottle(object):
    '''
    访问次数限制
    '''
    def __init__(self):
        self.history=None
    def allow_request(self,request,view):
        ctime = time.time()
        # 获取用户IP
        reomte_addr = request.META.get('REMOTE_ADDR')
        if reomte_addr not in VISIT_RECORD:
            VISIT_RECORD[reomte_addr]=[ctime,]
            return True
        # 获取访问次数记录
        history = VISIT_RECORD.get(reomte_addr)
        self.history = history

        while history and history[-1] < ctime - 60:
            history.pop()
        if len(history) < 3:
            history.insert(0, ctime)
            return True

        # return True 可以继续访问
        # return False 访问频率过高,被限制
    def wait(self):
        '''
        访问倒计时
        时间不能放在全局ctime,
        否则倒计时不会变化
        '''
        ctime = time.time()
        return 60 - (ctime - self.history[-1])
```

## 引用

```python
class LoginView(APIView):
	#  
    throttle_classes = [VisitThrottle,]
```

