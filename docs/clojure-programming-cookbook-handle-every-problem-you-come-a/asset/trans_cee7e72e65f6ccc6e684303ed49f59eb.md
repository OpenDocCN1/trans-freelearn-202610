

[cheshire "5.6.1"]

Ring 服务器提供了一个简单的 Web 服务器，使测试变得更容易。我们将在 project.clj 中按如下方式添加一个 ring 插件：

```
:ring {:handler liberator-example.example/handler}
:plugins [[lein-ring "0.9.7"]]
```

## How to do it...

是时候尝试 Liberator 了。这里，我们将学习如何使用 Liberator 实现 GET、POST、PUT 和 DELETE。但让我们先使用 Liberator 创建一个简单的 hello Clojure 服务器！

## Your first Liberator

首先，我们将声明所需的库。我们还将使用 ring 服务器和 Compojure。Ring 服务器是用 Clojure 编写的著名 Web 服务器。Compojure 是一个用于将 Web 请求分派到目标资源的路由器。

clj.http 和 cheshire.core 使客户端能够向 Liberator 提供的 REST API 服务发出请求：

```
(ns liberator-example.example
  (:require
    [liberator.core :refer [resource defresource]]
    [ring.adapter.jetty :refer [run-jetty]]
    [ring.middleware.params :refer [wrap-params]]
    [compojure.core :refer
      [defroutes ANY GET PUT POST DELETE OPTIONS]]
    [clj-http.client :as client]
    [cheshire.core :refer [parse-string generate-string]]))
;;=> nil
```

以下代码为客户端生成静态 HTML 内容：

```
(defroutes app
  (ANY "/hello" []
    (resource :available-media-types ["text/html"]
              :handle-ok "<html><h1>Hello Clojure !</h1></html>"))
  (route/not-found "Not Found !"))
;;=> #'liberator-example.example/app
```

在之前的代码中，我们使用 compojure.core/defroutes 定义了 /hello 上下文，而 liberator.core/resource 定义了一个资源。

然后，让我们定义一个处理器并运行它。ring.middleware.params/wrap-params 解析请求的查询字符串和表单主体：

```
(def handler
  (-> app
      wrap-params))
;;=> #'liberator-example.example/handler
```

然后，启动服务器并从浏览器发出请求：

```
(def server (run-jetty handler {:port 3000 :join? false}))
;;=> #'liberator-example.example/server
```

使用 http://localhost:3000/hello 打开浏览器。你将在浏览器中看到以下字符串：

![](img/d7b631352a782ee50160cf8eb995152f_532_0.png)

要从命令行发出请求，请使用 curl 命令：

```
$ curl -X GET http://localhost:3000/hello
```

下一张图片展示了 curl 命令的工作方式：

![](img/d7b631352a782ee50160cf8eb995152f_533_0.png)

如果测试成功，让我们停止服务器以进行食谱中的后续主题：

```
(.stop server)
;;=> nil
```

## Using defresource

这里，我们将使用 defresource 宏来定义资源：

```
(defresource hello :available-media-types ["text/html"]
  :handle-ok "<html><h1>Hello Clojure from defresource
!</h1></html>")
;;=> #'liberator-example.example/hello
```

使用 defresource，你可以按如下方式使用任何路由重用资源：

```
(defroutes app
  (ANY "/hello" [] hello)
  (route/not-found "Not Found !"))
;;=> #'liberator-example.example/app
```

让我们按如下方式定义一个处理器并启动服务器：

```
(def handler
  (-> app
      wrap-params))
;;=> #'liberator-example.example/handler
(def server (run-jetty handler {:port 3000 :join? false}))
;;=> #'liberator-example.example/server
```

你将看到与第一个 Liberator 示例类似的结果。在完成对 defresource 的测试后，让我们停止服务器：

```
(.stop server)
;;=> nil
```

## Parameterized resources

我们将学习如何定义带参数的 REST API。因此，让我们按如下方式定义一个具有参数 x 的 parameterized-hello 资源：

```
(defresource parameterized-hello [x]
  :available-media-types ["text/html"]
  :handle-ok (format "<html><h1>Hello Clojure from %s !</h1>
</html>" x)
)
;;=> #'liberator-example.example/parameterized-hello
```

前面的示例使我们能够将任意字符串设置为 HTML 内容。

defroutes 也有参数 x 并传递给 parameterized-hello：

```
(defroutes parameterized-hello-app
  (ANY "/hello/:x" [x] (parameterized-hello x))
  (route/not-found "Not Found !"))
;;=> #'liberator-example.example/parameterized-hello-app
```

定义你的处理器并启动服务器：

```
(def handler
  (-> parameterized-hello-app
      wrap-params))
;;=> #'liberator-example.example/handler
(def server (run-jetty handler {:port 3000 :join? false}))
;;=> #'liberator-example.example/server
```

现在，使用以下 URL 打开浏览器：

```
http://localhost:3000/hello/your-name:
```

![](img/d7b631352a782ee50160cf8eb995152f_535_0.png)

```
(.stop server)
;;=> nil
```

## Defining GET/POST/PUT/DELETE methods for REST APIs

这里，我们将为资源定义 HTTP 方法。我们将为客户定义一个资源，并在以下表格中为其定义五种方法：

| Method | Action | Parameter | URL |
| GET | Returns a list of customers | --- | http://localhost:3000/customer |
| POST | Creates a new customer | --- | http://localhost:3000/customer |
| GET | Gets a customer | Customer ID | http://localhost:3000/customer/id |
| PUT | Updates a customer | Customer ID | http://localhost:3000/customer/id |
| DELETE | Deletes a customer | Customer ID | http://localhost:3000/customer/id |

因此，让我们定义一个客户的 ref，其属性为 name 和 country：

```
(def customer-ref
  (ref
    {:makoto {:name "Makoto Hashimoto" :country "Japan"}
     :nico {:name "Nicolas Modrzyk" :country "France"}}))
;;=> #'liberator-example.example/customer-ref
```

资源 customer-list 没有参数。我们将定义 GET 和 POST 方法：

```
(defresource customer-list
  :allowed-methods [:get :post]
  :available-media-types ["application/json"]
  :exists? (fn [_]
            (let [result @customer-ref]
              (if-not (nil? result)
                {::entry result})))
  :post! (fn [ctx]
           (let [body (parse-string (slurp (get-in ctx [:request :body])) true)]
             (dosync
               (alter customer-ref assoc (keyword (:id body))
                 (dissoc body :id)))))
  :handle-ok ::entry
  :handle-not-found {:error "id is not found"})
;;=> #'liberator-example.example/customer-list
```

get 方法返回 customer-ref 的值。get 的回复由 :exists? 条目的匿名函数处理，如果 customer-ref 的值不为 nil，则返回一个带有 ::entry 键的映射。在这种情况下，:handle-ok? 的条目被应用并返回结果，状态代码为 200。否则，:handle-not-found 的函数被应用并返回 404 状态代码。

post 请求由与 :post? 关联的函数处理，该函数将 post 的主体添加到 customer-ref。post 请求的请求主体的格式如下：

```
{"id":"some-id","name":"some-name","country":"country-name"}
```

下一个名为 customer-entity 的资源用于参数化请求。此资源具有 GET、PUT 和 DELETE 方法：

```
(defresource customer-entity [id]
  :allowed-methods [:get :put :delete]
  :available-media-types ["application/json"]
  :exists? (fn [_]
            (let [result (get @customer-ref (keyword id))]
              (if-not (nil? result)
                {::entry result})))
```

```
:put!  (fn [ctx]
    (let [body (parse-string (slurp (get-in ctx [:request :body])) ) true)]
      (dosync
        (alter customer-ref assoc (keyword id) (dissoc body :id)))))
:delete!  (fn [_]
    (dosync
      (alter customer-ref dissoc (keyword id))))
:existed? (fn [_] (nil? (get @customer-ref id ::sentinel)))
:new? (fn [_] (nil? (get @customer-ref id ::sentinel)))
:handle-ok ::entry
:handle-not-found {:error "id is not found"}
;;=> #'liberator-example.example/customer-entity
```

然后，让我们使用 defroutes 按如下方式为 /customer 和 /customer/id 定义路径：

```
(defroutes customer
  (ANY "/customer" [] customer-list)
  (ANY "/customer/:id" [id] (customer-entity id)))
;;=> #'liberator-example.example/app
```

我们将定义一个处理器并启动 Jetty 服务器：

```
(def handler
  (-> customer
      wrap-params))
;;=> #'liberator-example.example/handler
(def server (run-jetty handler {:port 3000 :join? false}))
;;=> #'liberator-example.example/server
```

这里，我们将使用基于 Clojure 的 HTTP 客户端来测试客户 REST 服务。clj-http.client 对 GET、POST、PUT 和 DELETE 发出调用。

使用 http://localhost:3000/customer 的 get 方法检索 customer-ref 中的所有客户：

```
(client/get "http://localhost:3000/customer")
;;=> {:status 200, :headers {"Date" "Sun, 26 Jun 2016 20:14:34 GMT",
;;=> "Content-Type" "application/json;charset=UTF-8", "Vary" "Accept",
