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

比如，你想在你的程序中获取到数据库密码，你首先需要在 `config.yaml`中定义你的字段

```go
database:
  password: "YOUR_PASSWORD"
```
