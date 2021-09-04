# rest framework之访问频率控制基本实现

## 基本使用

- 类,继承:BaseThrottle
  - 实现:allow_request/wait
- 类,继承:SimpleRateThrottle
  - 实现:get_cache_key/scope = "Bengen"(配置文件中的key)

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

## 局部

```python
# 对用户访问频率控制
class UserThrottle(SimpleRateThrottle):
    scope = 'BengenUser'

    def get_cache_key(self, request, view):
        # 返回用户名
        return request.user.username

class LoginView(APIView):
     # 引用节流
    throttle_classes = [UserThrottle,]
```

## 全局(settings.py)

```python
REST_FRAMEWORK={
    # 节流
    "DEFAULT_THROTTLE_CLASSES":['api.utils.throttle.VisitThrottle'],
    # 内置,控制访问次数
    "DEFAULT_THROTTLE_RATES":{
        "Bengen":'3/m',
        "BengenUser":'10/m'
    }
}
```

