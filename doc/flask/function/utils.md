### 说明 :id=utils

Pear Admin Flask 项目中还提供了若干个便于后端操作的函数。以及，在于前端通信时也有一个规定的返回信息格式，此章将介绍这些函数。

这些函数的原型均位于 ```applications/common/utils``` 中，各位开发者可以打开源代码进行参考。

> 此处针对 master 分支，其他分支等待完善。

### xss过滤

#### 函数原型

函数调用位于项目代码 ```applications/common/utils/validate.py``` 中，函数原型如下：

```
def str_escape(s: str) -> str:
    """
    xss过滤，内部采用flask自带的过滤函数。
    与原过滤函数不同的是此过滤函数将在 s 为 None 时返回 None。

    :param s: 要过滤的字符串
    :type s: str
    :return: s 为 None 时返回 None，否则过滤字符串后返回。
    :rtype: str
    """
    ...
```

#### 使用方法

```python
from applications.common.utils.validate import str_escape
real_name = xss_escape(request.args.get('realName', type=str))
```


### 邮件发送

+ 原邮件发送函数

#### 函数原型

函数调用位于项目代码 ```applications/common/utils/mail.py``` 中，函数原型如下：

```
def send_mail(subject, recipients, content):
    """原发送邮件函数，不会记录邮件发送记录

    失败报错，请注意使用 try 拦截。

    :param subject: 主题
    :param recipients: 接收者 多个用英文分号隔开
    :param content: 邮件 html
    """
    ...
```

#### 实例代码

```python
#在.flaskenv中配置邮箱
from applications.common.utils import mail

mail.send_mail（"subject", "test@test.com", "<h1>Hello</h1>")
```

+ 基于二次开发的邮件发送函数

#### 函数原型

函数调用位于项目代码 ```applications/common/utils/mail.py``` 中，函数原型如下：

```
def add(receiver, subject, content, user_id):
    """
    发送一封邮件，若发送成功立刻提交数据库。

    :param receiver: 接收者 多个用英文逗号隔开
    :param subject: 邮件主题
    :param content: 邮件 html
    :param user_id: 发送用户ID（谁发送的？） 可以用 from flask_login import current_user ; current_user.id 来表示当前登录用户
    :return: 成功与否
    """
    ...
```

#### 示例代码

```python
#在.flaskenv中配置邮箱
from applications.common.utils import mail

mail.add("test@test.com", "subject", "<h1>Hello</h1>", current_user)
```



### 返回格式

> 后端响应时我们推荐使用规定的API响应格式。

#### 函数原型

函数调用位于项目代码 ```applications/common/utils/http.py``` 中，函数原型如下：

```
def success_api(msg: str = "成功"):
    """ 成功响应 默认值“成功” """
    return jsonify(success=True, msg=msg)


def fail_api(msg: str = "失败"):
    """ 失败响应 默认值“失败” """
    return jsonify(success=False, msg=msg)


def table_api(msg: str = "", count=0, data=None, limit=10):
    """ 动态表格渲染响应 """
        res = {
            'msg': msg,
            'code': 0,
            'data': data,
            'count': count,
            'limit': limit

        }
        return jsonify(res)
```

#### 示例代码

```python
from applications.common.utils.http import success_api, fail_api, table_api

@admin_log.get('/operateLog')
@authorize("admin:log:main")
def operate_log():
    # orm查询
    # 使用分页获取data需要.items
    log = AdminLog.query.filter(
        AdminLog.url != '/passport/login').order_by(
        desc(AdminLog.create_time)).layui_paginate()
    count = log.total
    return table_api(data=model_to_dicts(schema=LogOutSchema, data=log.items), count=count)
```

```python
from applications.common.utils.http import success_api, fail_api, table_api

@admin_power.post('/save')
@authorize("admin:power:add", log=True)
def save():
    ...  # 若干操作
    if success:
        return success_api(msg="成功")
    return fail_api(msg="成功")    
```
