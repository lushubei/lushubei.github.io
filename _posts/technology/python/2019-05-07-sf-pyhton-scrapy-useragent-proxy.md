---
title: "scrapy中间件实现更好useragent,proxy"
subtitle: "useragent,proxy"
layout: post
author: "Shubei"
date: 2019-05-07 12:00:00
header-style: text
hidden: true
tags:
  - 技术  
  - Python  
  - 爬虫  
---
## 1. middlewares.py

```python
# -*- coding: utf-8 -*-

# Define here the models for your spider middleware
#
# See documentation in:
# http://doc.scrapy.org/en/latest/topics/spider-middleware.html

import random
import json
from scrapy import signals

from scrapy.downloadermiddlewares.retry import RetryMiddleware

from common.proxypool import ProxyPool

class ProxyMiddleware(object):
    #代理proxy中间件
    from twisted.internet.error import TimeoutError, TCPTimedOutError, ConnectionDone, ConnectError, ConnectionLost

    EXCEPTIONS_TO_RETRY = (TimeoutError,ConnectionRefusedError, ConnectionDone, ConnectError,
                           ConnectionLost, TCPTimedOutError)


    def __init__(self, proxy_pool):
        self.proxy_pool = proxy_pool

    @classmethod
    def from_crawler(cls, crawler):
        proxy_pool = ProxyPool()
        proxy_pool.def_num = 1
        proxy_pool.proxy_url = 'http://api.wandoudl.com/api/ip?app_key=bd69c332749f3801e2efe63855800024&pack=204620&num=10&xy=2&type=2&lb=\r\n&mr=2&'
        proxy_pool.pool['59.63.67.229:36410'] = ("2019-05-05 11:15:26", 0)

        return cls(proxy_pool=proxy_pool)

    def process_request(self, request, spider):

        proxy = request.meta['cur_proxy'] = request.meta['proxy'] = self.proxy_pool.get_proxy()
        spider.logger.info('proxy:{p}'.format(p=proxy))

    def process_response(self, request, response, spider):
        '''处理被禁的代理ip'''

        data = json.loads(response.text).get('data')
        error = data.get('error')
        proxy = self.get_current_proxy(request)

        self.proxy_pool.set_proxy_fail_times(proxy, -1)  #代理失败次数减
        if request.meta.get('try_times', 0) >= 1:
            print('这是重试请求的响应结果,重试次数:%s' % request.meta.get('try_times', 0))

        if error and error.get('code', '') == '1004':
            self.proxy_pool.rmv_proxy(proxy)
            spider.logger.error(
                'proxy {a} have been banned,try again'.format(a=request.meta.get('cur_proxy')))

            return self._retry(request)

        else:
           pass

        return response

    def get_current_proxy(self, request):
        '''获取当前请求的代理'''
        return request.meta.get('cur_proxy')

    def process_exception(self, request, exception, spider):

        if isinstance(exception, self.EXCEPTIONS_TO_RETRY) \
                and not request.meta.get('dont_retry', False):

            spider.logger.error('exception,{},retry'.format(type(exception)))
            proxy = self.get_current_proxy(request)
            self.proxy_pool.set_proxy_fail_times(proxy, 1)  # 代理失败次数加
            return self._retry(request)

    def _retry(self, request):
        '''请求重试'''
        #
        # proxy = self.get_current_proxy(request)
        # self.proxy_pool.set_proxy_fail_times(proxy, -1)  # 代理失败次数减
        try_times = request.meta.get('try_times', 0) + 1

        if try_times > 3:
            raise ValueError('retry over 3 times,{}'.format(request.body))
        r = request.copy()
        r.dont_filter = True
        r.meta['try_times'] = try_times

        return r

class UAMiddleware(object):
    '''随机ua'''

    def __init__(self, uas):
        self.user_agents = uas

    @classmethod
    def from_crawler(cls, crawler):
        user_agents = crawler.settings.get('UA')

        if not len(user_agents):
            raise ValueError('useragent获取失败')

        return cls(uas=user_agents)

    def process_request(self, request, spider):

        ua = request.headers['user-agent'] = random.choice(self.user_agents)
        # spider.logger.info('UA:{u}'.format(u=ua))

        return None
```


## 2.settings.py中设置启用下载中间件
```python
DOWNLOADER_MIDDLEWARES = {
    'middlewares.ProxyMiddleware': 100,
    'middlewares.UAMiddleware': 200,
}
```



## 3. 附件
#### UA
```python

UA = [
     'Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6_8; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50',
     'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50',
     'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0;',
     'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; Trident/4.0)',
     'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)',
     'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)',
     'Mozilla/5.0 (Macintosh; Intel Mac OS X 10.6; rv:2.0.1) Gecko/20100101 Firefox/4.0.1',
     'Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/20100101 Firefox/4.0.1',
     'Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; en) Presto/2.8.131 Version/11.11',
     'Opera/9.80 (Windows NT 6.1; U; en) Presto/2.8.131 Version/11.11',
     'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_0) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11',
     'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Maxthon 2.0)',
     'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; TencentTraveler 4.0)',
     'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)',
     '?Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; The World)',
     '?Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SE 2.X MetaSr 1.0; SE 2.X MetaSr 1.0; .NET CLR 2.0.50727; SE 2.X MetaSr 1.0)',
     'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; 360SE)',
     'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Avant Browser)',
     'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1)',
     'Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_3_3 like Mac OS X; en-us) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8J2 Safari/6533.18.5',
     'Mozilla/5.0 (iPod; U; CPU iPhone OS 4_3_3 like Mac OS X; en-us) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8J2 Safari/6533.18.5',
     'Mozilla/5.0 (iPad; U; CPU OS 4_3_3 like Mac OS X; en-us) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8J2 Safari/6533.18.5',
     'Mozilla/5.0 (Linux; U; Android 2.3.7; en-us; Nexus One Build/FRF91) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1',
     'MQQBrowser/26 Mozilla/5.0 (Linux; U; Android 2.3.7; zh-cn; MB200 Build/GRJ22; CyanogenMod-7) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1',
     'Opera/9.80 (Android 2.3.4; Linux; Opera Mobi/build-1107180945; U; en-GB) Presto/2.8.149 Version/11.10',
     'Mozilla/5.0 (Linux; U; Android 3.0; en-us; Xoom Build/HRI39) AppleWebKit/534.13 (KHTML, like Gecko) Version/4.0 Safari/534.13',
     'Mozilla/5.0 (BlackBerry; U; BlackBerry 9800; en) AppleWebKit/534.1+ (KHTML, like Gecko) Version/6.0.0.337 Mobile Safari/534.1+',
     'Mozilla/5.0 (hp-tablet; Linux; hpwOS/3.0.0; U; en-US) AppleWebKit/534.6 (KHTML, like Gecko) wOSBrowser/233.70 Safari/534.6 TouchPad/1.0',
     'Mozilla/5.0 (SymbianOS/9.4; Series60/5.0 NokiaN97-1/20.0.019; Profile/MIDP-2.1 Configuration/CLDC-1.1) AppleWebKit/525 (KHTML, like Gecko) BrowserNG/7.1.18124',
     'Mozilla/5.0 (compatible; MSIE 9.0; Windows Phone OS 7.5; Trident/5.0; IEMobile/9.0; HTC; Titan)',
     'NOKIA5700/ UCWEB7.0.2.37/28/999',
     'Openwave/ UCWEB7.0.2.37/28/999',
     'Mozilla/4.0 (compatible; MSIE 6.0; ) Opera/UCWEB7.0.2.37/28/999'
     ]
```

#### proxypool.py
代理池，维护一个可用代理池
```python
#!/usr/bin/env python
# encoding: utf-8
'''
@author: xiaobei
@time: 2019/4/27 14:16
@desc: 代理池
'''
import time
import random
import datetime
import threading

from .proxy import Proxy

class ProxyPool(object):

    def __init__(self):
        self.pool = dict()  #{'ip':(expire_time,faild_times)}
        self.pool_lock = threading.Lock()  #代理池锁
        self.def_num = 1  #代理池缺省至少保留代理数
        self.proxy_url = ''
        self.lock = threading.Lock()

        self.generate_status = 0  #代理生成器状态 0空闲,1生产中
        self.generater_lock = threading.Lock()  #代理生成器锁
        self.proxy_faile_times_limit = 5 #5次为代理失败上限删除

    def get_proxy(self) ->str:
        '''获取可用代理'''

        if len(self.pool) >= self.def_num:
            proxy = random.sample(self.pool.keys(), 1)[0]

            if self.proxy_is_expired(proxy, self.pool[proxy][0]) or \
               self.get_proxy_fail_times(proxy) > self.proxy_faile_times_limit:
                self.rmv_proxy(proxy)
                return self.get_proxy()

        elif not self.generate_status:
            self.generate_status = True
            print('代理消耗完,重新获取')
            # self.lock.acquire()
            self.generater_lock.acquire()
            proxy_l = self.generate_proxy(self.proxy_url)
            for p in proxy_l:
                self.add_proxy(p['ip'], p['expire_time'])

            self.generate_status = False
            # self.lock.release()
            self.generater_lock.release()
            proxy = self.get_proxy()

        else:
            while 1:
                time.sleep(3) #休息3秒,再获取
                print('休息3秒再获取')
                return self.get_proxy()

        return proxy

    def add_proxy(self, proxy, expire_time=None) -> bool:
        '''添加代理进代理池'''

        if expire_time and self.proxy_is_expired(proxy, expire_time):
            ret = False
        else:
            self.pool_lock.acquire()
            self.pool[proxy] = (expire_time, 0)
            self.pool_lock.release()
            ret = True

        return ret

    def rmv_proxy(self, proxy) ->bool:
        '''删除代理'''

        self.pool_lock.acquire()
        if proxy in self.pool:
            del self.pool[proxy]
        self.pool_lock.release()

        return True

    def set_proxy_fail_times(self, proxy, step=0):
        '''设置代理失败次数'''

        if proxy in self.pool:
            fail_times = self.pool[proxy][1] + step
            self.pool[proxy] = (self.pool[proxy][0], fail_times if fail_times > 0 else 0)

    def get_proxy_fail_times(self, proxy):
        '''获取代理失败次数'''

        if proxy in self.pool:
            return self.pool[proxy][1]
        else:
            print('要查询的代理已被删除,{p}'.format(p=proxy))
            return 10000  #一个很大的数

    def generate_proxy(self, url) -> str:
        '''生成代理'''
        ips = Proxy.get_proxy_jiguang(url)
        # ips = Proxy.get_ip_from_vps('58.221.0.139', '20063')  # todo debug
        # ips = [{'ip': '121.226.74.21:3828',
        #         'expire_time': '2050-01-01 00:00:00'
        #         }]

        return ips

    def proxy_is_expired(self, proxy, expire_time)->bool:
        '''代理是否超时'''

        # 1分钟以内, 则丢弃
        if expire_time < (datetime.datetime.now() + datetime.timedelta(microseconds=1 * 60000000)).strftime(
                '%Y-%m-%d %H:%M:%S'):
            print('有效期1分钟以内,代理即将失效')
            return True

        else:
            return False

```

#### proxy.py
动态搭理获取
````
#!/usr/bin/env python
# encoding: utf-8
'''
@author: xiaobei
@time: 2019/3/15 14:39
@desc: 动态代理
'''

import datetime
import time
import requests
import pexpect

class Proxy():
    url_jiguang = 'http://api.wandoudl.com/api/ip?app_key=bd69c332749f3801e2efe63855800024&pack=204620&num=10&xy=2&type=2&lb=\r\n&mr=2&'
    # url_ip3366 = 'http://gec.ip3366.net/api/?key=20190218182629436&getnum=50&anonymoustype=2&area=1&order=2&formats=2&proxytype=01'
    url_ip3366 = 'http://gec.ip3366.net/api/?key=20190218182629436&getnum=100&anonymoustype=2&area=1&order=1&sarea=1&formats=2&proxytype=01'
    N = 1

    @classmethod
    def get_proxy_jiguang(cls, url=None,expire_time=None, retry_times=0,len_ips = 1):

        try:
            if url is None:
                url = cls.url_jiguang
            ipools = requests.get(url).json()
        except Exception as e:
            print('请求异常,重试:{}'.format(retry_times))
            if retry_times <= 2:  # 重试2次
                retry_times += 1
                return cls.get_proxy_jiguang(expire_time, retry_times=retry_times)
            else:
                raise e

        PROXIES = []
        for i in ipools['data']:
            expire_time = i.get('expire_time')
            print(i)
            #低于20分钟的丢弃
            # if expire_time < (datetime.datetime.now() + datetime.timedelta(microseconds= 20*60000000)).strftime('%Y-%m-%d %H:%M:%S'):
            #     continue

            # info = {'ip' :'http://' + str(i['ip']) + ':' + str(i['port']),
            #         'expire_time' : i.get('expire_time')
            #         }
            info = {'ip': str(i['ip']) + ':' + str(i['port']),
                    'expire_time': i.get('expire_time')
                    }
            PROXIES.append(info)

        while len(PROXIES) < cls.N:
            import time
            time.sleep(1)
            if url is None:
                url = cls.url_jiguang
            ipools = requests.get(url).json()

            for i in ipools['data']:
                expire_time = i.get('expire_time')

                # 低于20分钟的丢弃
                # if expire_time < (datetime.datetime.now() + datetime.timedelta(microseconds=20 * 60000000)).strftime(
                #         '%Y-%m-%d %H:%M:%S'):
                #     continue

                # info = {'ip': 'http://' + str(i['ip']) + ':' + str(i['port']),
                #         'expire_time': i.get('expire_time')
                #         }
                info = {'ip': str(i['ip']) + ':' + str(i['port']),
                        'expire_time': i.get('expire_time')
                        }
                PROXIES.append(info)
                if len(PROXIES) >= cls.N:
                    break

        return PROXIES

    @classmethod
    def get_proxy_ip3366(cls,expire_time=None,retry_times=0):

        try:
            ipools = requests.get(cls.url_ip3366).json()
        except Exception as e:
            print('proxy,请求异常,重试:{}'.format(retry_times))
            time.sleep(2)
            if retry_times <= 3: #重试3次
                retry_times += 1
                return cls.get_proxy_ip3366(expire_time,retry_times=retry_times)
            else:
                raise e

        PROXIES = []
        for i in ipools:
            # info = {'ip': 'http://' + str(i['Ip']) + ':' + str(i['Port']),
            #         'expire_time': expire_time
            #         }
            info = {'ip': str(i.get('Ip', i.get('ip'))) + ':' + str(i.get('Port', i.get('port'))),
                    'expire_time': expire_time
                    }
            PROXIES.append(info)
        return PROXIES

    @classmethod
    def get_ip_from_vps(self, host, port):
        ip_address = None
        try:
            cmd = 'ssh root@{host} -p {port} \'bash get_new_ip\''.format(host=host, port=port)
            child = pexpect.spawn(cmd)
            ret = child.expect(['assword', pexpect.TIMEOUT])
            if ret == 0:
                child.sendline('touyan123')
                ret_ip = child.expect([pexpect.EOF, pexpect.TIMEOUT])
                if ret_ip == 0:
                    print('get ip')
                    ip_address = str(child.before, encoding='utf-8').replace('\r', '').split('\n')[2] + ':3828'
                else:
                    print('no ip')
            else:
                print('no ip')
        except:
            print('exception, no ip')

        child.close()

        info = {'ip': ip_address,
                'expire_time': '2050-01-01 00:00:00'
                }

        return [info]
````