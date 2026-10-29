

```
; ;=> "Connection" "close", "Server" "Jetty(9.2.10.v20150310)"},
; ;=> :body "{\"makoto\":{\"name\":\"Makoto Hashimoto\",\"country\":\"Japan\"},
; ;=> \"nico\":{\"name\":\"Nicolas Modrzyk\",\"country\":\"France\"}}\
```

然后，我们将测试 POST：

```
(client/post "http://localhost:3000/customer"
        {:body (generate-string {:id "rh" :name "Rich Hickey" :country "United States"})})
; ;=> {:status 201, :headers {"Date" "Sun, 26 Jun 2016 14:33:28 GMT",
; ;=> "Content-Type" "application/json", "Vary" "Accept",
"Connection" "close",
; ;=> "Server" "Jetty(9.2.10.v20150310)"}, :body "", :request-time 8,
; ;=> :trace-redirects ["http://localhost:3000/customer"],
:orig-content-encoding nil}
```

让我们检查之前的条目是否存在。我们只需要响应的 body：

```
(->
  (client/get "http://localhost:3000/customer/rh" )  :body
  (parse-string true))
; ;=> {:name "Rich Hickey", :country "United States"}
```

我们将检查 PUT 方法是否有效：

```
(client/put "http://localhost:3000/customer/rh"
        {:body (generate-string {:name "Rich Hickey" :country "USA"})})
; ;=> {:status 204, :headers {"Date" "Sun, 26 Jun 2016 22:04:35 GMT",
; ;=> "Content-Type" "application/json", "Vary" "Accept",
"Connection" "close",
; ;=> "Server" "Jetty(9.2.10.v20150310)"}, :body nil, :request-time 9,
; ;=> :trace-redirects ["http://localhost:3000/customer/rh"],
:orig-content-encoding nil}
```

因此，让我们验证更新是否成功：

```
(->
  (client/get "http://localhost:3000/customer/rh")
    :body
      (parse-string true))
;;=> {:name "Rich Hickey", :country "USA"}
```

最后，我们将测试 DELETE 方法：

```
(client/delete "http://localhost:3000/customer/rh")
;;=> {:status 204, :headers {"Date" "Sun, 26 Jun 2016 14:38:58 GMT",
;;=> "Content-Type" "application/json", "Vary" "Accept",
"Connection" "close",
;;=> "Server" "Jetty(9.2.10.v20150310)"}, :body nil, :request-time 8,
;;=> :trace-redirects ["http://localhost:3000/customer/rh"],
:orig-content-encoding nil}
(.stop server)
;;=> nil
```

## 它是如何工作的...

在这里，我们将回顾主要的 HTTP 响应代码，然后学习如何使用 Liberator 调试和追踪应用程序。

## HTTP 的方法和状态码

下表显示了主要的方法，以及它们在 Liberator 中的描述和操作：

| 方法 | 描述 |  |
|---|---|---|
| GET | 对资源的只读访问 | exists? |
| PUT | 创建新资源 | put! |
| DELETE | 用于删除资源 | delete! |
| POST | 更新现有资源或创建新资源 | post! |
| PATCH | 与 PUT 类似但只发送差异 | patch! |

主要的 HTTP 状态码如下：

| 处理键 | 状态码 |
|---|---|
| handle-ok | 200 |
| handle-created | 201 |
| handle-no-content | 204 |
| handle-moved-permanently | 301 |
| handle-malformed | 400 |
| handle-unauthorized | 401 |
| handle-forbidden | 403 |
| handle-not-found | 404 |
| handle-method-not-allowed | 405 |
| handle-not-acceptable | 406 |
| handle-exception | 500 |
| handle-not-implemented | 501 |
| handle-unknown-method | 501 |
| handle-service-not-available | 503 |

## 追踪请求

Liberator 具有追踪和调试功能。在下面的代码中，我们将使用 liberator.dev/wrap-trace 重新定义一个处理器，以显示追踪文本和追踪图：

```
(def handler
  (-> customer
    wrap-params
    (wrap-trace :header :ui)))
```

```
; ;=> #'liberator-example.example/handler
```

让我们再次启动服务器：

```
(def server (run-jetty handler {:port 3000 :join? false}))
; ;=> #'liberator-example.example/server
```

现在让我们发起一次调用并查看 X-Liberator-Trace 头中的决策追踪，如下所示：

```
(client/get "http://localhost:3000/customer")
{:status 200,
:headers
{"Date" "Mon, 27 Jun 2016 01:52:09 GMT",
"Content-Type" "application/json;charset=UTF-8",
"Vary" "Accept",
"X-Liberator-Trace-Id" "uren6",
"Link" "</ //x-liberator/requests/uren6>; rel=x-liberator-trace",
"X-Liberator-Trace"
[":decision (:initialize-context {})"
":decision (:service-available? true)"
":decision (:known-method? :get)"
":decision (:uri-too-long? false)"
":decision (:method-allowed? :get)"
":decision (:malformed? false)"
":decision (:authorized? true)"
":decision (:allowed? true)"
":decision (:valid-content-header? true)"
":decision (:known-content-type? true)"
":decision (:valid-entity-length? true)"
":decision (:is-options? false)"
":decision (:accept-exists? [false {:representation {:media-type \"application/json\"}}])"
":decision (:accept-language-exists? nil)"
":decision (:accept-charset-exists? nil)"
":decision (:accept-encoding-exists? \"gzip, deflate\")"
":decision (:encoding-available? {:representation {:encoding \"identity\"}})"
":decision (:processable? true)"
":decision (:exists? {:liberator-example.example/entry {:makoto {:name \"Makoto Hashimoto\", :country \"Japan\"}, :nico {:name \"Nicolas Modrzyk\", :country \"France\"}, :rh {:name \"Rich Hickey\", :country \"USA\"}}})"
":decision (:if-match-exists? nil)"
```

```
" :decision (:if-unmodified-since-exists? nil)"
" :decision (:if-none-match-exists? nil)"
" :decision (:if-modified-since-exists? nil)"
" :decision (:method-delete? false)"
" :decision (:method-patch? false)"
" :decision (:post-to-existing? false)"
" :decision (:put-to-existing? false)"
" :decision (:multiple-representations? false)"
" :handler (:handle-ok)"],
"Connection" "close",
"Server" "Jetty(9.2.10.v20150310)}",
:body
" {"makoto":{"name":"Makoto
Hashimoto","country":"Japan"},"nico":{"name":"Nicolas
Modrzyk","country":"France"},"rh":{"name":"Rich
Hickey","country":"USA"}}",
:request-time 8,
:trace-redirects ["http://localhost:3000/customer"],
:orig-content-encoding nil,
:links {:x-liberator-trace {:href "//x-liberator/requests/uren6"}}}
```

我们可以在以下 URL 中看到执行流程的追踪：
http://localhost:3000/x-liberator/requests/.

你可以在浏览器中看到你请求的请求历史，如下图所示：

## Liberator 请求追踪

get /customer at Mon Jun 27 10:52:09 JST 2016 (242s ago)
get /customer at Mon Jun 27 10:44:54 JST 2016 (677s ago)
get /customer/rh at Mon Jun 27 10:44:43 JST 2016 (688s ago)
put /customer/rh at Mon Jun 27 08:36:12 JST 2016 (8399s ago)
put /customer/rh at Mon Jun 27 08:36:09 JST 2016 (8402s ago)
put /customer/rh at Mon Jun 27 08:35:55 JST 2016 (8416s ago)
put /customer/rh at Mon Jun 27 08:35:41 JST 2016 (8431s ago)
post /customer at Sun Jun 26 23:32:59 JST 2016 (40992s ago)
get /customer at Sun Jun 26 23:32:35 JST 2016 (41016s ago)
get /customer at Sun Jun 26 23:32:02 JST 2016 (41049s ago)
get /counter at Sun Jun 26 23:28:15 JST 2016 (41276s ago)
get /customer at Sun Jun 26 23:28:13 JST 2016 (41278s ago)
get /counter at Sun Jun 26 23:28:11 JST 2016 (41280s ago)
get /customer at Sun Jun 26 23:28:09 JST 2016 (41282s ago)
get /counter at Sun Jun 26 23:28:07 JST 2016 (41284s ago)

让我们打开其中一个请求条目。里面有头信息、追踪和图：

所有追踪的列表

## Liberator 请求追踪 #uren6 at Mon Jun 27 10:52:09 JST 2016 (328s ago)

请求为 ":GET /customer"

## 参数

## 头信息

connection
    close
user-agent
    Apache-HttpClient/4.5.1 (Java/1.8.0_65)
host
    localhost:3000
accept-encoding
    gzip, deflate

## 追踪

+   1. decision: initialize-context {}
+   2. decision: service-available? true
+   3. decision: known-method? :get
+   4. decision: uri-too-long? false
+   5. decision: method-allowed? :get
+   6. decision: malformed? false

生成的图太大无法整体查看。以下是图的一部分，帮助你理解其结构。你可以参考此处创建的图生成自己的图：

我们将使用 `apache/derby` 和 `org.clojure/java.jdbc` 来访问 derby，这是一个小型 rdb 库。因此，我们将在 `project.clj` 中添加这些库：

```
```clojure
:dependencies [[org.clojure/clojure "1.8.0"]
               [liberator "0.14.1"]
               [compojure "1.3.4"]
               [ring "1.5.0-RC1"]
               [ring/ring-core "1.5.0-RC1"]
               [buddy "0.13.0"]
               [ring/ring-jetty-adapter "1.4.0"]
               [clj-http "2.2.0"]
               [cheshire "5.6.1"]
               [org.clojure/java.jdbc "0.6.1"]
               [org.apache.derby/derby "10.12.1.1"]]]
```
```

添加条目后，重启你的 REPL。

## 我们的服务规范

我们将创建一个小型应用程序来处理产品信息，如下所示：

| 产品 | 名称 | 描述 | 时钟 | RAM | 价格 |
|---|---|---|---|---|---|
| macbook air 11.6 | Apple | Apple MacBook Air MVM2JLL/A 11.6-Inch laptop | 1.6 | 4 | 797.00 |
| thinkpad x260 | Levovo | Lenovo ThinkPad X260 20F6005HUS 12.5" Ultrabook | 2.3 | 8 | 1,009.00 |
| ASUS VivoBook | ASUS | ASUS VivoBook E200HA-US01-GD Portable 11.6 | 1.8 | 2 | 199.99 |
| Dell Inspiron | DELL | Dell Inspiron 11.6-Inch 2 in 1 Convertible Touchscreen Laptop | 2.16 | 4 | 279.99 |

为了将之前的条目存储在数据库中，我们将在 Apache Derby 中创建以下表：

| id | name | maker | description | clock | ram | price |
|---|---|---|---|---|
| int | varchar(20) | varchar(10) | varchar(100) | float | Integer | BigDecimal |

