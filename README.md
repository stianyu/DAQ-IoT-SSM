## 基于SSM框架搭建的物联网数据采集系统服务器端
* Spring+SpringMVC部分使用XML+注解形式开发，MyBatis部分使用纯注解开发。
* 前端页面仅供测试，本系统主要是为底层传感网络提供数据提交和管理的平台。
* 默认请求路径 http://localhost:80/iot/  其中iot是项目虚拟路径，可修改。
* 默认测试url：http://localhost:80/iot/api/home  请求成功则随机响应一行英文名言。

### 注意：
* 以下所有API除用户登录之外，都会受到登录拦截器的拦截，所以调用API需要先登录一个用户。如果觉得登录麻烦可以把LoginInterceptor中preHandle方法的return true之前的代码都注释掉。
* 本系统除下载部分外，所有响应数据均为同样的JSON格式。
  * 格式：{"status" : true/false, "message" : "description...", "data": data }
    * status: 表示请求是否成功。
    * message: 对请求的描述，如果响应失败，描述失败原因。
    * data: 请求成功时响应的数据，为Object类型，可以是任何类型的数据。data的具体json的格式可参考domain包中的实体类结构。

### 提供API
#### 1. 用户相关 
|功能|请求uri|请求方式|请求参数|
|----|------|--------|-------|
|用户登录|iot/api/user/login|POST|User对象|
|查看登录用户|iot/api/user/info|GET|无|
|退出登录|iot/api/user/exit|GET|无|
|用户注册|iot/api/user/regist|POST|User对象|
|修改密码|iot/api/user/password|POST|User对象、新密码|
|修改基本信息|iot/api/user/modify|POST|User对象|

#### 2. 网关相关(restful风格)
|功能|请求uri|请求方式|请求参数|
|----|------|--------|-------|
|添加网关|iot/api/gateway|POST|Gateway对象|
|更新网关|iot/api/gateway|PUT|Gateway对象|
|删除网关|iot/api/gateway/{id}|DELETE|id值|
|查询网关|iot/api/gateway/{id}|GET|id值|
|查询所有网关|iot/api/gateway|GET|无|
|关联查询网关下的传感器|iot/api/gateway?withSensors=true|GET|布尔值|

#### 3. 传感器分类相关(restful风格)
|功能|请求uri|请求方式|请求参数|
|----|------|--------|-------|
|添加传感器分类|iot/api/classify|POST|SensorClassify对象|
|查询传感器分类|iot/api/classify/{id}|GET|id值|
|查询所有传感器分类|iot/api/classify|GET|无|
|关联查询传感器分类下的传感器|iot/api/classify?withSensors=true|GET|布尔值|
|查询某一网关下的所有传感器分类|iot/api/classify/gateway/{id}|GET|网关id值|

#### 4. 传感器相关(restful风格)
|功能|请求uri|请求方式|请求参数|
|----|------|--------|-------|
|添加传感器|iot/api/sensor|POST|Sensor对象|
|更新传感器|iot/api/sensor|PUT|Sensor对象|
|删除传感器|iot/api/sensor/{id}|DELETE|id值|
|查询传感器|iot/api/sensor/{id}|GET|id值|
|查询所有传感器|iot/api/sensor|GET|无|
|查询某一分类所有传感器|iot/api/sensor/classify/{id}|GET|分类id|
|查询某一网关所有传感器|iot/api/sensor/gateway/{id}|GET|网关id|

#### 5. 传感器数据相关
|功能|请求uri|请求方式|请求参数|
|----|------|--------|-------|
|提交一个数据|iot/api/data/receive|POST|Data对象的json格式字符串|
|提交多个数据|iot/api/data/receiveAll|POST|Data对象数组的json格式字符串|
|查询一个传感器的所有数据|iot/api/data/sensor/{id}|GET|传感器id|

#### 6. 异常相关
|功能|请求uri|请求方式|请求参数|
|----|------|--------|-------|
|查询网关异常|iot/api/gatewayException|GET|无|
|查询网关异常（分页）|iot/api/gatewayException/page/{page}|GET|页码|
|查询一段时间内的网关异常|iot/api/gatewayException/{timetamp}|GET|字符串，格式:"时间戳1@时间戳2"|
|查询传感器异常|iot/api/sensorException|GET|无|
|查询传感器异常（分页）|iot/api/sensorException/page/{page}|GET|页码|
|查询一段时间内的传感器异常|iot/api/sensorException/{timetamp}|GET|字符串，格式:"时间戳1@时间戳2"|

#### 7. 下载相关
|功能|请求uri|请求方式|请求参数|
|----|------|--------|-------|
|下载测试文件|iot/api/file/test|GET|无|
|下载所有网关的xls表格|iot/api/file/gateway|GET|无|
|下载所有传感器的xls表格|iot/api/file/sensor|GET|无|
|下载一个网关及其所有传感器的xls表格|iot/api/file/gateway/{id}|GET|网关id|
|下载一个传感器及其所有数据的xls表格|iot/api/file/sensor/{id}|GET|传感器id|

#### 其他
本项目中已经将Spring框架整合了Redis，并且配置好了RedisTemplate，但是实际运用中遇到了一些bug，所以暂时没有调用RedisTemplate，有待以后更新和优化。
