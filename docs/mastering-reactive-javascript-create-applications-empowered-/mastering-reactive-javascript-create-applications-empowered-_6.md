## 索引

A
应用程序
浏览器，测试 167
服务器，测试 164
测试，RxJS 使用 169
B
背压
关于 95, 96, 190
处理，常见策略 97
处理，丢失策略 97
处理，使用无损操作符 107
Bacon.interval() 36
bacon.js 可观察对象
与 RxJS 可观察对象对比 49
bacon.js
关于 47
冷可观察对象 49
错误，处理 33
热可观察对象 49
安装 11
可观察对象 20
可观察对象，惰性求值 44
可观察对象，修改 28
可观察对象，重用 42
属性 27
用于转换事件 36
browserify 256
缓冲 108
bufferWithCount() 操作符 108
bufferWithTime() 操作符 112
bufferWithTimeOrCount() 操作符 114
C
回调地狱 134
CasperJS 282
catch() 操作符 156, 161
客户端应用程序
构建 259, 261, 262, 263
结构 257, 259
测试 281, 284, 285, 286
冷可观察对象 49, 106
CompositeDisposable 77
concat() 操作符
使用 134
concatAll() 操作符
关于 141
使用 141
D
数据
聚合 189
过滤 187, 188, 189
DataSource
关于 228, 229
使用 Redis 进行消息 252
debounce() 操作符 102
依赖
安装 256, 257
Disposable 49, 69
do() 操作符 199, 200
关于 198
别名 200
DOM
更改，批量 277, 279, 280
不要重复自己 (DRY) 133, 151
E
empty() 方法 62
错误
应用程序，测试 164
catch() 操作符 156
处理 152

处理 154
mergeDelayError() 操作符 162
onErrorResumeNext() 操作符 159
retry() 操作符 161
测试 156
ES6 特性 10
事件
  转换，bacon.js 使用 36
EventStream 12

## F

过滤数据
  操作符 121
过滤函数 9, 19
filter() 操作符 88
finally() 操作符 195, 196
first() 操作符 122
flatMap() 操作符 86
  用于创建过滤元素的函数 182, 183, 186, 187
  与 map() 操作符对比 178, 179, 180, 181, 182
flatMapFirst() 操作符 194, 195
flatMapLatest() 操作符 191, 192, 193, 194
forkJoin() 操作符
  关于 142
  使用 142
函数式响应式编程
  用法 9
函数，错误处理
  onCompleted 154
  onError 154
  onNext 154

## G

Google
  URL 170
groupBy() 操作符 197

## H

热可观察对象 49

## I

interval() 方法 72
ioredis 250

## J

JavaScript 代码
  性能，比较 transducer 使用 216, 217, 218, 220
JavaScript
  transducer，实现 203

## L

无损策略
  缓冲 107
  bufferWithCount() 操作符 108
  bufferWithTime() 操作符 112
  bufferWithTimeOrCount() 操作符 114
  受控可观察对象 118
  用于处理背压 107
  可暂停可观察对象 117
有损策略
  debounce() 操作符 102
  用于处理背压 97
  pausable() 可观察对象 105
  sample() 操作符 100
  throttle() 操作符 97

## M

map() 操作符
  关于 84
  与 flatMap() 操作符对比 178, 179, 180, 181, 182
merge() 操作符
  使用 137
mergeAll() 操作符
  使用 141
mergeDelayError() 操作符 162
压缩器 257
mocha 166

## N

Node.js
  示例 12
  安装 10
  参考链接 10

## O

可观察对象
  组合 132, 142, 190, 191
  concat() 操作符，使用 134
  concatAll() 操作符，使用 141
  串联 134
  创建 21
  创建，从 interval 24
  创建，从其他源 25
  创建，使用 fromArray() 方法 24
  创建，使用 sequentially() 方法 24
EventStream 20
EventStream，从 DOM 事件创建 21
EventStream，从 ES6 promises 创建 22
执行 133
延迟求值 44
merge() 操作符，使用 137
mergeAll() 操作符，使用 141
node 事件发射器，实现 23
操作符，添加 82
Property 20
重用 42
zip() 操作符，使用 145
zipIterable() 操作符，使用 148
观察者
  关于 72, 177
  onCompleted() 事件 177
  onError() 事件 177
  onNext() 事件 177
onErrorResumeNext() 操作符 159
操作符
  添加，到可观察对象 82
  工作 34
操作符，过滤数据
  first() 操作符 122
  skip() 操作符 127
  skipWhile() 操作符 129
  take() 操作符 124
  takeLast() 操作符 125
  takeWhile() 操作符 126

## P

pausable() 可观察对象 105
pausableBuffered() 117

## R

Promises/A+ 59
Property 12
range() 方法 55
Reactive Extensions (RxJS)
  关于 7
  背压 190
  数据，聚合 189
  数据，过滤 187, 188, 189
  do() 操作符 198, 199, 200
  finally() 操作符 195, 196
  flatMapFirst() 操作符 194, 195
  flatMapLatest() 操作符 191, 192, 193, 194
  基础 177, 178
  groupBy() 操作符 197
  安装 11, 50
  交互，改进 275, 277
  map() 操作符，与 flatMap() 操作符对比 178, 179, 180, 181, 182
  可观察对象，组合 190, 191
  操作符 191
  参考链接 50
  transducer，使用 207, 208, 209, 210, 211, 212
  用法 189
  使用，用于管理用户输入 267
响应式范式 8
响应式编程
  用于编码 12
Redis
  连接到 250
  使用，作为消息的 DataSource 252
reduce() 操作符 90, 189
RefCountDisposable 77
retry() 操作符 161
RxJS Disposable 76
RxJS 可观察对象
  关于 48
  创建 50
  创建，空可观察对象 59
  创建，从任意参数 61
  创建，从任意源 68
  创建，从回调 57
  创建，从 DOM 事件 65
  创建，从错误 63
  创建，从 EventEmitter 65
  创建，从工厂函数 61
  创建，从可迭代对象 51
  创建，从 promise 58
  创建，从序列工厂 53
  创建，从单个值 60
  创建，使用时间段 55
  创建，使用 range() 55
  与 bacon.js 可观察对象对比 49
RxJS Schedulers 79
RxJS Subjects 73

## S

sample() 操作符 100
Scheduler 49
Selenium 282
服务器应用程序
  连接 264, 265, 266
服务器通信
  连接 269, 270
服务，用于阻止用户消息
  关于 238, 239
  实现 240, 241
  测试实现 239, 240
服务，用于监听消息
  关于 234
  实现 237, 238
  测试实现 235, 237
服务，用于发送命令
  关于 241, 242
  实现 246
  测试实现 242
服务，用于发送消息
  关于 229
  实现 233, 234
  测试实现 230, 232, 233
单页应用程序 (SPA) 256
skip() 操作符 127
skipWhile() 操作符 129
Subject 49
subscribe() 方法
  使用 70
subscriber 49
订阅
  关于 29
  assign 方法，使用 32
  log 方法，使用 31
  onValue() 方法，使用 29
订阅 29

## T

take() 操作符 124
takeLast() 操作符 125
takeWhile() 操作符 126
throttle() 操作符 97
工具
  安装 10
transducer
  关于 202, 203
  实现，用于 JavaScript 203
  参考链接 204
  参考 203
  测试 212, 213, 214, 215, 216
  用于比较 JavaScript 代码性能 216, 217, 218, 220
  使用 204, 207
  使用，与 RxJS 207, 208, 209, 210, 211, 212
transducers-js API 204

## U

用户输入
  管理，RxJS 使用 267
用户交互
  连接 269, 270

## W

web 聊天应用程序
  创建 225, 226
web 聊天服务器 224
WebSocket 连接
  处理 247, 248
WebSocket 服务器
  关于 224
  架构 226, 227
  连接，处理 247, 248
DataSource 228, 229
实现 228
Redis，连接到 250
Redis，使用作为消息的 DataSource 252
扩展 250

## Z

服务，用于阻止用户消息 238, 239
服务，用于监听消息 234
服务，用于发送命令 241, 242
服务，用于发送消息 229
Zepto.js 21
zip() 操作符
  关于 145
  使用 145
zipIterable() 操作符
  使用 148
