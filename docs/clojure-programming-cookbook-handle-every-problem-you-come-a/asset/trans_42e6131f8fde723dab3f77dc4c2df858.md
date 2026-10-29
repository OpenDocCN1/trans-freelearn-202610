

```
; ;=> {:name "makoto-bucket-1"}
(s3/create-bucket "makoto-bucket-2")
; ;=> {:name "makoto-bucket-2"}
(s3/create-bucket "makoto-bucket-3")
; ;=> {:name "makoto-bucket-3"}
```

s3/list-buckets 返回存储桶信息：

```
(s3/list-buckets)
; ;=> [{:creation-date #object[org.joda.time.DateTime 0x6a09e119 "2016-08-01T07:01:05.000+09:00"],
; ;=> :owner
; ;=> {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
; ;=> :display-name "tokoma1"},
; ;=> :name "makoto-bucket-1"}
; ;=> {:creation-date #object[org.joda.time.DateTime 0x7392252c "2016-08-01T17:35:30.000+09:00"],
; ;=> :owner
; ;=> {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
; ;=> :display-name "tokoma1"},
; ;=> :name "makoto-bucket-2"}
; ;=> {:creation-date #object[org.joda.time.DateTime 0x4d59b4cb "2016-08-01T17:38:59.000+09:00"],
; ;=> :owner
; ;=> {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
; ;=> :display-name "tokoma1"},
; ;=> :name "makoto-bucket-3"}]
```

我们可以看到 AWS 控制台中有三个存储桶，如下图所示：

![](img/d7b631352a782ee50160cf8eb995152f_682_0.png)

让我们按如下方式删除其中两个存储桶：

```
(s3/delete-bucket "makoto-bucket-2")
;;=> nil
(s3/delete-bucket "makoto-bucket-3")
;;=> nil
(s3/list-buckets)
;;=> [{:creation-date #object[org.joda.time.DateTime 0x56387509 "2016-08-01T07:01:05.000+09:00"],
;;=> :owner {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27", :display-name "tokoma1"}, :name "makoto-bucket-1"}]
```

我们现在可以看到只有一个存储桶，如下图所示：

![](img/d7b631352a782ee50160cf8eb995152f_682_1.png)

现在我们将演示如何将本地数据发送到 s3。s3/put-object 将文件内容上传到指定的存储桶和键。以下代码上传 /etc/hosts 到 makoto-bucket-1：

```
(s3/put-object
:bucket-name "makoto-bucket-1"
:key "test/hosts"
:file (java.io.File. "/etc/hosts"))
;;=> {:requester-charged? false, :content-md5 "HkBljfktNT106yScnMRsjA==",
;;=> :etag "1e40658df92d353974eb249c9cc46c8c", :metadata {:content-disposition nil,
;;=> :expiration-time-rule-id nil, :user-metadata nil,
:instance-length 0, :version-id nil,
;;=> :server-side-encryption nil, :etag "1e40658df92d353974eb249c9cc46c8c", :last-modified nil,
;;=> :cache-control nil, :http-expires-date nil, :content-length 0, :content-type nil,
;;=> :restore-expiration-time nil, :content-encoding nil,
:expiration-time nil, :content-md5 nil,
;;=> :ongoing-restore nil}}
```

s3/list-objects 按如下方式列出存储桶中的对象：

```
(s3/list-objects :bucket-name "makoto-bucket-1")
;;=> {:truncated? false, :bucket-name "makoto-bucket-1", :max-keys 1000, :common-prefixes [],
;;=> :object-summaries [{:storage-class "STANDARD", :bucket-name "makoto-bucket-1",
;;=> :etag "1e40658df92d353974eb249c9cc46c8c",
;;=> :last-modified #object[org.joda.time.DateTime 0x1b76029c "2016-08-01T07:01:16.000+09:00"],
;;=> :owner {:id "3d6e87f691897059c23bcfb88b17da55f0c9aa02cc2a44e461f1594337059d27",
;;=> :display-name "tokomal1"}, :key "test/hosts", :size 380}]}
```

要获取存储桶中对象的内容，请使用 s3/get-object：

```
(s3/get-object :bucket-name "makoto-bucket-1" :key "test/hosts")
;;=> {:bucket-name "makoto-bucket-1", :key "test/hosts",
;;=> :input-stream #object[com.amazonaws.services.s3.model.S3ObjectInputStream 0x24f810e9
```
```
; ;=> .....
; ;=> :last-modified #object[org.joda.time.DateTime 0x79ad1ca9 "2016-08-01T07:01:16.000+09:00"],
; ;=> :cache-control nil, :http-expires-date nil, :content-length 380, :content-type "application/octet-stream",
; ;=> :restore-expiration-time nil, :content-encoding nil,
:expiration-time nil, :content-md5 nil,
; ;=> :ongoing-restore nil}
```

结果是一个 map；内容是流数据，值为 :object-content。要将结果作为字符串获取，我们将按如下方式使用 slurp_：

```
(slurp (:object-content (s3/get-object :bucket-name "makoto-bucket-1" :key "test/hosts")))
; ;=> "127.0.0.1\tlocalhost\n127.0.1.1\tphenix\n\n# The following lines are desirable for IPv6 capable hosts\n::1 ip6-localhost ip6-loopback\nfe00::0 ip6-localnet\nff00::0 ip6-mcastprefix\nff02::1 ip6-allnodes\nff02::2 ip6-allrouters\n\n52.8.30.189 my-cluster01-proxyl \n52.8.169.10 my-cluster01-master1 \n52.8.198.115 my-cluster01-slave01 \n52.9.12.12 my-cluster01-slave02\n\n52.8.197.100 my-node01\n"
```

## 使用 Amazon SQS

Amazon SQS 是一种高性能、高可用性和可扩展的队列服务。我们将演示使用 Clojure 处理 SQS 队列中的消息是多么容易：

```
(ns aws-examples.sqs-example
  (:require [amazonica.core :as core]
            [amazonica.aws.sqs :as sqs]))
```

要创建队列，可以按如下方式使用 sqs/create-queue：

```
(sqs/create-queue :queue-name "makoto-queue"
  :attributes
  {:VisibilityTimeout 3000
   :MaximumMessageSize 65536
   :MessageRetentionPeriod 1209600
   :ReceiveMessageWaitTimeSeconds 15})
; ;=> {:queue-url "https://sqs.ap-northeast-1.amazonaws.com/864062283993/makoto-queue"}
```

要获取队列信息，请按如下方式使用 sqs/get-queue-attributes：

```
(sqs/get-queue-attributes "makoto-queue")
;;=> {:QueueArn "arn:aws:sqs:ap-northeast-
1:864062283993:makoto-queue", ...
```

您可以使用 sqs/assign-dead-letter-queue 按如下方式配置死信队列（由针对不存在的队列的消息组成的队列）：

```
(sqs/create-queue "DLQ")
;;=> {:queue-url "https://sqs.ap-northeast-
1.amazonaws.com/864062283993/DLQ"}
(sqs/assign-dead-letter-queue (sqs/find-queue "makoto-queue")
(sqs/find-queue "DLQ") 10)
;;=> nil
```

让我们列出已定义的队列：

```
(sqs/list-queues)
;;=> {:queue-urls
;;=> ["https://sqs.ap-northeast-
1.amazonaws.com/864062283993/DLQ"
;;=> "https://sqs.ap-northeast-
1.amazonaws.com/864062283993/makoto-queue"]}
```

以下截图显示了 SQS 控制台：

![](img/d7b631352a782ee50160cf8eb995152f_682_1.png)

## 让我们检查队列的 URL：

```
(sqs/find-queue "makoto-queue")
;;=> "https://sqs.ap-northeast-1.amazonaws.com/864062283993/makoto-queue"
(sqs/find-queue "DLQ")
;;=> "https://sqs.ap-northeast-1.amazonaws.com/864062283993/DLQ"
```

要发送消息，我们使用 sqs/send-message：

```
(sqs/send-message (sqs/find-queue "makoto-queue") "hello sqs from Clojure")
;;=> {:md5of-message-body "00129c8cc3c7081893765352a2f71f97", :message-id "690ddd68-a2f6-45de-b6f1-164eb3c9370d"}
```

要接收消息，我们使用 sqs/receive-message：

```
(sqs/receive-message "makoto-queue")
;;=> {:messages [
;;=>     {:md5of-body "00129c8cc3c7081893765352a2f71f97",
;;=>      :receipt-handle "AQEB.....", :message-id "bd56fea8-4c9f-4946-9521-1d97057f1a06",
;;=>      :body "hello sqs from Clojure"}]}

```

要删除队列中的所有消息，我们使用 sqs/purge-queue：

```
(sqs/purge-queue :queue-url (sqs/find-queue "makoto-queue"))
;;=> nil

```

要删除队列，我们使用 sqs/delete-queue：

```
(sqs/delete-queue "makoto-queue")
;;=> nil
(sqs/delete-queue "DLQ")
;;=> nil

```

## 使用 AWS Lambda 的无服务器 Clojure

Lambda 是 AWS 的产品，允许您在无需设置和维护服务器环境的情况下运行 Clojure 代码。幕后仍然有服务器，但就您而言，它是一个无服务器环境。上传一个 JAR 即可开始使用。Lambda 上的代码会响应事件调用，例如文件上传到 S3，或根据指定的计划调用。在生产环境中，Lambda 通常用于包含标准服务器环境以处理离散计算任务的更广泛的 AWS 部署中。

特别是，它可以根据并发请求非常轻松地扩展。对于从事个人项目的 Clojurian 来说，Lambda 是功能与限制的绝佳结合。鉴于 AWS 设定的限制，您能将 Lambda 发挥到什么程度？

## Clojure 命名空间 helloworld

从使用 lein new 生成的干净空项目开始。从那里，在您选择的 IDE 中配置并打包一个新的 Clojure 源文件。在以下示例中，包是 com.sakkam，

