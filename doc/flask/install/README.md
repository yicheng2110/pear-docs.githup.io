### 环境要求 :id=install
- Python >= 3.6
- Mysql >= 5.7.0

###  安装配置

#### 克隆远程仓库

您可以使用 git 来克隆远程仓库：

```shell
# 进入项目主目录
cd Pear Admin Flask

# 使用 git 克隆远程仓库
git clone https://gitee.com/pear-admin/pear-admin-flask.git

# 切换分支
git checkout master  # master, main or mini
```

或者直接前往 Pear Admin Flask 项目的[Gitee 主页](https://gitee.com/pear-admin/pear-admin-flask)下载项目仓库。

#### 搭建开发环境

我们推荐使用 Python 的虚拟环境来开发该项目，这样便于项目的迁移与二次开发。当然，您也可以选择使用原 Python 环境。

如果你想创建 Python 虚拟环境，你可以使用下面的命令行：

```shell
# 在当前目录的venv文件夹创建虚拟环境
python -m venv venv

# 激活虚拟环境
venv\Scripts\activate
```

**如果在创建虚拟环境时报错 “ModuleNotFoundError” ，这说明您的 Python 版本小于 3.3 。**

#### 安装项目依赖

```shell
# 使用 pip 安装必要模块（对于 master 分支）
pip install -r requirement\requirement.txt

# 使用 pip 安装必要模块（对于 mini 分支）
pip install -r requirement.txt
```

或者您可以尝试：

```shell
# 使用 pip 安装必要模块（对于 master 分支）
python -m pip install -r requirement\requirement.txt

# 使用 pip 安装必要模块（对于 mini 分支）
python -m pip install -r requirement.txt
```

#### 配置数据库

+ master 分支：

将项目文件中的 ```pear.sql``` 文件导入 Mysql 中。

+ mini 分支：

默认的使用 `sqlite3` 作为测试环境的数据库进行演示,不需要按照mysql即可查看演示。如果需要二次开发，建议改成 `mysql` 。

如果需要在开发环境使用 mysql 作为数据库，请查看 `applications/configs/config.py` 文件里面的相关配置文件, 注释掉 sqlite 的配置即可

如果需要修改数据的配置信息，请在 `.flaskenv` 里面调整即可 

```bash
flask db init
flask db migrate -m '数据初始化'
flask db upgrade

flask init-db
```

+ main 分支

```bash
-- 删除旧数据库
drop database if exists pear_admin_flask;
-- 创建新数据库
create database pear_admin_flask character set `utf8mb4`;
```

### 设置

+ master 分支

```.flaskenv
.flaskenv文件
# flask配置
FLASK_APP = app.py
FLASK_ENV = development
FLASK_DEBUG = 1
FLASK_RUN_HOST = 127.0.0.1	# 如果远程开发或者内网访问,改成 0.0.0.0
FLASK_RUN_PORT = 5000

# pear admin flask配置
SYSTEM_NAME = Pear Admin	#系统的名称，前端展示

# MySql配置信息
MYSQL_HOST=127.0.0.1
# MYSQL_HOST=dbserver		#docker-compose的link地址
MYSQL_PORT=3306
MYSQL_DATABASE=PearAdminFlask
MYSQL_USERNAME=root
MYSQL_PASSWORD=root

# Redis 配置
REDIS_HOST=127.0.0.1
REDIS_PORT=6379

# 密钥配置(记得改)
SECRET_KEY='pear-admin-flask'	#很重要，部署一定要改

# 邮箱配置
MAIL_SERVER = 'smtp.qq.com'
MAIL_USERNAME = '123@qq.com'
MAIL_PASSWORD = 'XXXXX' # 生成的授权码
```

- 如果想允许全部网络计算机访问，将FLASK_RUN_HOST设置为 0.0.0.0 。但是我们并不推荐在发布期间这么做，这无疑会增加被攻击的风险。

  