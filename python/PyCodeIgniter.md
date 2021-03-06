##1.Install

###1.1 dependency
+ `logging`
+ `pymysql`
+ `DBUtils`

###1.2 how to install
```
pip install pymysql
pip install DBUtils
pip install --upgrade PyCodeigniter


```


##2. How to use? (if you want to have high performance, recommand using `gevent`)


####2.1 simple example (just for test)
```python

#!/usr/bin/env python
# -*- coding:utf8 -*-
__author__ = 'xiaozhang'
from codeigniter.system.core.CI_Application import CI_Application

def main():
    app=CI_Application(application_path=r'./')

    app.start_server()

if __name__ == '__main__':
    main()

    
```


command line

```
python app.py
```

visit website

```
http://127.0.0.1:8005/Index/index

```

####2.2 how to integrate with `web.py`

```python
#!/usr/bin/env python
# -*- coding:utf8 -*-
__author__ = 'xiaozhang'

import web

from codeigniter.system.core.CI_Application import CI_Application


ci=CI_Application(application_path=r'./')


urls = (
    '/.*', ci.router.webpy_route
)
app = web.application(urls, globals())

session = web.session.Session(app, web.session.DiskStore('sessions'))


if __name__ == "__main__":


    app.run()
```


####2.3 how to integrate with  `gevent`　(recommed）

```python
#!/usr/bin/python
"""A web.py application powered by gevent"""

from gevent import monkey; monkey.patch_all()
from gevent.pywsgi import WSGIServer
import time
import web
import json

from codeigniter.system.core.CI_Application import CI_Application

ci=CI_Application(application_path=r'./',config_file=r'./config.py')
port=ci.config['server']['port']
host=ci.config['server']['host']

def application(env, start_response):
    html=''

    code,obj=ci.router.wsgi_route(env)
    if not isinstance(obj,str):
        html=json.dumps(obj)
        start_response(str(code), [('Content-Type', 'application/json')])
    else:
        start_response(str(code), [('Content-Type', 'text/html')])
        html=obj
    return [str(html)]



if __name__ == "__main__":
    print 'Serving on %s...' % port
    WSGIServer((host, port), application).serve_forever()



```



##3. Q&A


+ how to config your application?

```
#you can edit application/config/config.py 

config.py

```


+ how to visit your website?

```

#http://127.0.0.1:8005/conntroller_class/function
#


http://127.0.0.1:8005/Index/index



```


+ how to get controller class instance?

```
app.loader.ctrl('classname')

```


+ how to get model class instance?

```
app.loader.model('classname')

```

+ how to operate database?


```
#you can use active record.

app.db.query('select * from test')

app.db.insert('test',{'name':'test'})

```

+ how to write log in your application ?

```
app.logger.info('message')

app.logger.warn('message')

app.logger.error('message')

```

+ how to send email?

```

#send html
app.mail.send('to','subject','message',true)

#send text
app.mail.send('to','subject','message',false)


```