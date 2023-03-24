# Mysql主从同步模式Docker开发环境(compose版本)
---
该项目本地以Compose方式部署开发环境所使用的模板

## 准备工作
### 1. 编辑`.env-dist`(非必须)
如果本地使用了多套该compose运行环境，则必须调整各自目录下该文件的`COMPOSE_PROJECT_NAME`参数值，以确保相互间唯一

### 2. 创建`docker-compose.yml`
#### 2.1 从模板文件复制生成`docker-compose.yml`
```bash
cp docker-compose.yml.template docker-compose.yml
```
#### 2.2 内容调整
替换相关自定义密码

---

## 常用命令
### Build
`make build`

### Start
`make start`

### Stop
`make stop`

### Status
`make status`

### Down
`make down`

---

## 其他
### MySQL主从配置
初次构建并运行该项目后，需配置MySQL主从：
1. 进入MySQL Master容器并进入MySQL服务获取当前master状态：
```
docker exec -it [MYSQL_MASTER_CONTAINER] bash
mysql -u[user] -p[pass]
show master status;
```
记录`File`和`Position`对应的内容

2. 进入MySQL Slave容器并进入MySQL服务配置同步服务：
```
docker exec -it [MYSQL_SLAVE_CONTAINER] bash
mysql -u[user] -p[pass]
```
执行：
```
CHANGE MASTER TO master_host = 'mysql_master',
master_user = 'slave',
master_password = '[Master服务slave账户访问密码]',
master_port = 3306,
master_log_file = '[上一步得到的File内容]',
master_log_pos = [上一步得到的Position内容],
master_connect_retry = 30;
```
启动从服务器复制功能：
```
start slave;
```
最后查看主从同步状态：
```
show slave status \G;
```
下面两个参数都是Yes，则说明主从配置成功：
```
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
```
注：如果出现错误，先重置。执行完下面两条命令：
```
stop slave;
reset master;
```

> 该compose模板支持不同需求下开发环境的扩展，但仅限于在本地进行软件开发测试场景使用，不推荐在实际的生产环境下部署使用
