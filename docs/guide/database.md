# 数据库

## 配置

连接数据库所需要的配置参数，获取参数的字段定义如下

```go
const (
	ConfigPathDatabaseHost                = "database.host"
	ConfigPathDatabaseName                = "database.name"
	ConfigPathDatabaseUser                = "database.user"
	ConfigPathDatabasePassword            = "database.password"
	ConfigPathTimeoutReconnectionInterval = "database.timeout_reconnection_interval"
	ConfigPathInsertInitializationData    = "database.insert_initialization_data"
)
```

你首先需要在 `config.yaml`中配置相关的参数

```yaml
database:
    host: YOUR_DATABASE_HOST		#必填，你的数据库主机名带端口，如：localhost:3306
    name: YOUR_DATABASE_NAME		#必填，你的数据库名
    user: YOUR_DATABASE_USER		#必填，你的数据库用户
    password: YOUR_PASSWORD		#必填，你的数据库密码
    insert_initialization_data: false 	#选填，是否插入初始化数据，默认为false
    timeout_reconnection_interval: 10 	#选填，数据库未连接自动重连间隔，单位：秒，默认为10s
```

## 调用

使用下面的方法来调用数据库

```go
Config[InterfaceConfig]{}.Get()
```

其中 `InterfaceConfig`代表你需要用哪种第三方库来实现你的业务逻辑

## 库

### LibraryGorm

我们把GORM简单的封装成了 `LibraryGorm`这个库，这个库实现了一些简单的方法，以便于快速开发

当然，你也可以使用下面的方式来获取 `gorm.DB`全部的方法

```go
d.Database[d.LibraryGorm]{}.Get().DB
```

#### GenerateFuzzyQueries

目前该方法只支持MySQL，

```go
GenerateFuzzyQueries(tx, map[string]string{"name": "John", "sex": "female"})
```

#### PaginateV2

想了解这个方法，可以先参考一下[GORM的官方写法](https://gorm.io/docs/scopes.html#Pagination)
