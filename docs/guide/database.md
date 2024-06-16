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

#### 插入初始数据

我们使用 `func (l LibraryGorm) InsertInitializationData(list ...interface{}) (initialized bool, err error)` 这个方法来插入初始化数据

##### 触发条件

`database.insert_initialization_data` 该配置项需要设置为 `true`，才会触发此方法，否则则跳过。

触发此方法后，该配置值会变为 `false`，防止下次运行程序重复插入初始化数据

```yaml
database:
    insert_initialization_data: true
```

##### 参数

参数 `list` 是传入的待初始化的数据，可以传入多组数据

```go
var init_department_data = []Department{
	{ID: 1, Status: 1, Name: "销售部"},
}
var init_role_data = []Role{
	{ID: 1, Status: 1, Name: "管理组"},
}

func InsertInitData() {
	_, err = d.Database[d.LibraryGorm]{}.Get().InsertInitializationData(
		init_department_data,
		init_role_data,
	)
	if err != nil {
		panic(err)
	}
	// ...
}
```

##### 返回值

`initialized` 返回该函数是否执行了初始化了数据，若执行了返回 `true`

> 如果你需要在你的程序中需要多次调用插入初始化数据的方法，请再第二次调用前，请将 `database.insert_initialization_data` 该配置项需要设置为 `true`，否则第一次调用后，该配置项会自动更改成 `false`，导致第二次插入初始化数据被跳过。
>
> ```go
> d.Config[d.LibraryViper]{}.Get().Set(d.ConfigPathInsertInitializationData, true)
> ```

`err` 只有在执行插入数据库失败的时候，或更改 `database.insert_initialization_data` 该配置项为 `false` 失败的情况下返回错误。若此方法被跳过，则不返回错误。

#### GenerateFuzzyQueries

目前该方法只支持MySQL，

```go
GenerateFuzzyQueries(tx, map[string]string{"name": "John", "sex": "female"})
```

#### PaginateV2

想了解这个方法，可以先参考一下[GORM的官方写法](https://gorm.io/docs/scopes.html#Pagination)
