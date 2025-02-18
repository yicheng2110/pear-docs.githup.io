### 说明 :id=authorize

Pear Admin Flask 项目中集成很多实用的功能，为了便于二次开发，同样也提供了许多便于开发的自定义函数。

Pear Admin Flask 项目支持多用户，不同用户有不同的权限，此处将介绍 Pear Admin Flask 中的权限管理函数的用法。

> 此处针对 master 分支，其他分支等待完善。

### 函数原型

函数调用位于项目代码 ```applications/common/utils/rights.py``` 中，函数原型如下：

```python
def authorize(power: str, log: bool = False):
    """
    用户权限判断，用于判断目前会话用户是否拥有访问权限

    :param power: 权限标识
    :type power: str
    :param log: 是否记录日志, defaults to False
    :type log: bool, optional
    """
    ...
```

### 基本用法

+ 后端用法

```python
from applications.common.utils.rights import authorize

@app.route("/test")
@authorize("admin:power:remove", log=True)
def test_index():
    return 'You are allowed.'
```

> 使用装饰器 @authorize时需要注意，该装饰器需要写在	@app.route之后

+ 前端用法

在前端中，例如增加，删除按钮，对于没有编辑权限的用户不显示的话，可以使用

 `{% **if** authorize("admin:user:edit") %}`

 `{% endif %}`

例如

```python
	{% if authorize("admin:user:edit") %}
        <button class="pear-btn pear-btn-primary pear-btn-sm" lay-event="edit">
    	<i class="pear-icon pear-icon-edit"></i>
        </button>
    {% endif %}
    {% if authorize("admin:user:remove") %}
        <button class="pear-btn pear-btn-danger pear-btn-sm" lay-event="remove">
        <i class="pear-icon pear-icon-ashbin"></i>
        </button>
    {% endif %}
```

