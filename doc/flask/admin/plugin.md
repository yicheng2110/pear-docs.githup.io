### 说明

插件功能旨在最大限度不修改原框架的前提下添加新功能，我们提供了三个示例插件。

> 插件功能需要最新版的 master 分支。

### 插件配置

将插件文件夹放置在 ```plugins``` 文件夹中，并且在 .flaskenv 中配置。再配置项中填入插件的文件夹名，已 json 格式写入其中。

```.flaskenv
# 插件配置
PLUGIN_ENABLE_FOLDERS = ["helloworld"]
```

### 插件目录

```
Plugin
│  __init__.json
└─ __init__.py
```

这是一个非常简单的插件。

### 插件信息

插件信息保存在 ```__init__.py``` 中，以测试插件“helloword”为例。插件的数据应该不少于下面三项：

```json
{
  "plugin_name": "Hello World",
  "plugin_version": "1.0.0.1",
  "plugin_description": "一个测试的插件。"
}
```

### 插件格式

插件的入口点为 ```__init__.py``` 文件，在插件被启用后，程序启动时此 Python 文件中的 ```event_init``` 函数。代码如下：

```python
from flask import Flask

def event_init(app: Flask):
    """初始化完成时会调用这里"""
    print("加载完毕后，我会输出一句话。")
```

调用时需要一个参数来接收``app``。

插件还有另外两个事件：```event_enable```、```event_disable```。分别在插件被启用时与被禁用时执行的函数。

```python
import os

# 获取插件所在的目录（结尾没有分割符号）
dir_path = os.path.dirname(__file__).replace("\\", "/")
folder_name = dir_path[dir_path.rfind("/") + 1:]  # 插件文件夹名称

def event_enable():
    """当此插件被启用时会调用此处"""
    print(f"启用插件，dir_path: {dir_path} ; folder_name: {folder_name}")

def event_disable():
    """当此插件被禁用时会调用此处"""
    print(f"禁用插件，dir_path: {dir_path} ; folder_name: {folder_name}")

```