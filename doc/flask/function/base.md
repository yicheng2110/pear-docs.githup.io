### 说明 :id=Schema

项目中时常会涉及到数据库的读写，在读入数据时可以采用SQLalchemy，将模型查询的数据对象转化为字典。

> 此处针对 master 分支，其他分支等待完善。

### Schema 序列化

> Schema 是序列化类，我们把他放在了models文件里，因为觉得没有必要新建一个文件夹叫 Schema ，也方便看着模型写序列化类。

```python
# 例如
class DeptSchema(ma.Schema):  # 序列化类
    deptId = fields.Integer(attribute="id")
    parentId = fields.Integer(attribute="parent_id")
    deptName = fields.Str(attribute="dept_name")
    leader = fields.Str()
    phone = fields.Str()
    email = fields.Str()
    address = fields.Str()
    status = fields.Str()
    sort = fields.Str()
```

> 这一部分有问题的话请看 marshmallow 文档

### 模型到字典

#### 函数原型

函数调用位于项目代码 ```applications/common/curd.py``` 中，函数原型如下：

```
def model_to_dicts(schema: ma.Schema, data):
    """
    将模型查询的数据对象转化为字典
    
    :param schema: schema类
    :param model: sqlalchemy查询结果
    :return: 返回单个查询结果
    """
    ...
```

#### 基本用法

+ model写的是查询后的对象

```python
from applications.common import curd
from applications.models import Dept
from applications.schemas import DeptOutSchema

def test():  # 某函数内
    dept = Dept.query.order_by(Dept.sort).all()
    res = curd.model_to_dicts(Schema=DeptOutSchema, model=dept)
```

## 查询多字段构造器

> 此内容不属于 matser 分支

```python
from applications.common.helper import ModelFilter
mf = ModelFilter()

field_name: 模型字段名称，models里的字段名称,sqlalchemy的field字段
    class User(db.Model, UserMixin):
    	username = db.Column(db.String(20), comment='用户名')
    就是username
value: 值，前端传过来的
    request.args.get('UserName', type=str)

# 准确查询字段
mf.exact(field_name, value)

# 不等于查询字段
mf.neq(field_name, value):

# 大于查询字段
mf.greater(field_name, value):

# 小于查询字段
mf.less(field_name, value):
# 模糊查询字段(%+xxx+%)
mf.vague(field_name, value: str):

# 左模糊 (% + xxx)
mf.left_vague(field_name, value: str):
      
# 右模糊查询字段(xxx+ %)
mf.right_vague(field_name, value: str):
        
# 包含查询字段
mf.contains(field_name, value: str):

#范围查询字段
mf.between(field_name, value1, value2)


# 查询
你的Model.query.filter(mf.get_filter(model=User)).all()


# 例如

# 获取请求参数
real_name = xss_escape(request.args.get('realName', type=str))
username = xss_escape(request.args.get('username', type=str))
dept_id = request.args.get('deptId', type=int)
    # 查询参数构造
mf = ModelFilter()
    if real_name:
        mf.contains(field_name="realname", value=real_name)
    if username:
        mf.contains(field_name="username", value=username)
    if dept_id:
        mf.exact(field_name="dept_id", value=dept_id)
    # orm查询
    # 使用分页获取data需要.items
    user = User.query.filter(mf.get_filter(model=User)).layui_paginate()

```