# Go 语言资源整合


### 编程规范

[uber-guide](https://github.com/uber-go/guide): uber go guide

[clean-go-article](https://github.com/Pungyeon/clean-go-article): go clean code

### 常用包

[viper](https://github.com/spf13/viper): 配置读取

[zap](https://github.com/uber-go/zap): 日志输出

[lumberjack](https://github.com/natefinch/lumberjack): 日志滚动记录器

[validator](https://github.com/go-playground/validator): 数据校验

[cast](https://github.com/spf13/cast): 类型转换

[lo](https://github.com/samber/lo): 工具库，切片，映射，元组，集合...

[carbon](https://github.com/uniplaces/carbon): 时间库的扩展

[cron](https://github.com/robfig/cron): 定时任务

[ants](https://github.com/panjf2000/ants): goroutine 池

[wire](https://github.com/google/wire): 依赖注入

[cobra](https://github.com/spf13/cobra): 命令行程序

[resty](https://github.com/go-resty/resty): HTTP/REST 客户端

[testify](https://github.com/stretchr/testify): 测试库，断言、mock

[go-funk](https://github.com/thoas/go-funk): Go 实用工具库（map、find、contains、filter、chunk、reverse，...）

[jsoniter](https://github.com/json-iterator/go): 标准库 encoding/json 的升级，可直接替换，高性能

[gjson](https://github.com/tidwall/gjson): 快速从 json 中获取值

[carbon](https://github.com/golang-module/carbon): 一个轻量级、语义化、对开发者友好的 golang 时间处理库，支持链式调用

[afero](https://github.com/spf13/afero): Go 的文件系统抽象

[fsm](https://github.com/looplab/fsm): 有限状态机库

---

[fasthttp](https://github.com/valyala/fasthttp): 标准库 net/http 的升级，更快

[agollo](https://github.com/philchia/agollo): 连接 apollo 配置中心

[msgp](https://github.com/tinylib/msgp): MessagePack 序列化，比 json 更快，数据量更小

[redsync](https://github.com/go-redsync/redsync): Redis 分布式锁

[sqlx](https://github.com/jmoiron/sqlx): 标准库 database/sql 的扩展

[kafka-go](https://github.com/segmentio/kafka-go): kafka 库

### 框架

[rpcx](https://github.com/smallnest/rpcx): rpc 框架

[gin](https://github.com/gin-gonic/gin): web 框架

[fiber](https://github.com/gofiber/fiber): web 框架

[iris](https://github.com/kataras/iris): HTTP/2 web 框架

[cooly](https://github.com/gocolly/colly): 爬虫框架

[watermill](https://github.com/ThreeDotsLabs/watermill): 事件驱动框架

### 其它

[fync](https://github.com/fyne-io/fyne): GUI

[bubbletea](https://github.com/charmbracelet/bubbletea): 终端应用 TUI

[plot](https://github.com/gonum/plot): 绘图

[smocker](https://github.com/Thiht/smocker): HTTP mock server

[primitive](https://github.com/fogleman/primitive): 使用几何形状将图片变为抽象画

### 工具

[golines](https://github.com/segmentio/golines): 代码格式化，处理行过长的行

- 安装：`go install github.com/segmentio/golines@latest`
- 使用：`golines -w .`

