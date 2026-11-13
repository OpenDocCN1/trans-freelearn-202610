

```java
XMLResponseHandler(Class<Response> responseClass,
    Class<Error> errorClass) {
  this.responseClass = responseClass;
  this.errorClass = errorClass;
}
```

```java
// Callback invoked with the converted Response object instance
abstract public void onSuccess(Response response);

// Callback invoked with the converted Error object instance
abstract public void onFailure(Error response);
```

```java
// Convert the body to a POJO object using the our converter
//Override
public void onSuccess(HTTPResponse response) {
  RawBody body = (RawBody)response.mBody;

  if ( body != null ) {
    Response obj = null;
    try {
      obj = new XMLConverter<Response>(responseClass)
        .decode(body);
      onSuccess(obj);
    } catch (Exception e) {
      onError(e);
    }
  } else { onSuccess((Response)null); }
}
//.. Failure elided for brevity
```

现在我们准备从伪服务器收集派发的 GetUserInfoResponse，因此让我们在 Activity 中实现一个匿名内部 XMLResponseHandler 子类，它将使用用户详细信息更新 UI：

```java
XMLResponseHandler<GetUserInfoResponse, Error> xmlResponseHandler
  = new XMLResponseHandler<GetUserInfoResponse, Error>(
  GetUserInfoResponse.class, Error.class) {

  // Updates the UI with the user details
  @Override
  public void onSuccess(GetUserInfoResponse getUserInfoResponse) {
    TextView nameTv = (TextView)findViewById(R.id.nameValue);
    TextView emailTv = (TextView)findViewById(R.id.emailValue);
    nameTv.setText(getUserInfoResponse.getUser().name);
    emailTv.setText(getUserInfoResponse.getUser().email);
  }
  @Override
  public void onFailure(Error response) {
  // Do Something with the Error
  }
  @Override
  public void onError(Throwable error) {
  // Do Something with the Throwable
  }
};
```

为了完成我们的 Activity，我们需要构建请求，并请求我们的异步客户端在后台借助 HTTPAsyncTask 类执行我们的请求：

```java
HTTPRequest.Builder builder = new HTTPRequest.Builder();
// Set the HTTP POST Verb
builder.setVerb(HTTPRequest.Verb.POST);
// Set location of the remote resource
builder.setUrl("http://demo1472539.mockable.io/GetUserInfo");
// Tell the Server that you are able to consume
// application/xml contents on the response
builder.addHeader(new Header("Accept", "application/xml"));
// Build the Request Body object
GetUserInfo query = new GetUserInfo();
query.setUserId("123");
try {
  // Encode the POJO into a XML Document
  Body body = new XMLConverter<GetUserInfo>(GetUserInfo.class)
    .encode(query, "application/xml");
  builder.setBody(body);
} catch (Exception e) {
  // Catch and display and error to the user
  ...
}
PacketAsyncHTTPClient client = new PacketAsyncHTTPClient();
client.execute(builder.build(), xmlResponseHandler);
```

一旦来自伪服务器的响应返回，UI 将根据伪端点中指定的 XML 更新，以显示用户姓名、电子邮件和其他属性。

未提及错误流；不过，我挑战你创建一个返回错误对象的伪端点，以测试 onError 回调。带有错误流的完整源代码可从 Packt Publishing 网站下载。请查看它，以了解错误处理是如何实现的。错误 XML 文档可能类似于此：

```xml
<?xml version="1.0"?>
<p:Error
xmlns:p="https://www.packtpub.com/asynchronous_android">
<p:ResultCode>1000</p:ResultCode>
<p:ResultMessage>The LDAP Server is down</p:ResultMessage>
</p:Error>
```

目前为止，我们已介绍了行业中 HTTP 客户端和服务器之间交换数据时最常用的格式（文本、XML 和 JSON）。每种格式都有其自身优势；然而，由于其紧凑性和简单性（易于解析、语法等），JSON 格式已被 API 设计师、电子产品制造商以及软件即服务（SaaS）供应商广泛采用。

在此过程中，我们将构建一个核心框架，该框架可轻松扩展以支持不同的数据交换格式，例如 YAML 或二进制协议。因此，你可以编写自己的 BodyEncoder、BodyDecoder 和 ResponseHandler，以适配标准数据格式甚至你自己的数据格式。

在本节中，我们将向读者介绍 HttpUrlConnection 上的一组超时设置，用于适应请求执行过程中的网络延迟。

## 自定义 HTTP 超时

当 HttpUrlConnection 在低带宽网络（2G、3G 等）上连接、读取或写入内容时，无法避免遭遇不可预测的通信延迟。此外，除了移动网络延迟之外，当 HTTP 服务器经历高流量时，可能会引入显著的响应延迟（服务器延迟）。

尽管 HttpUrlConnection 使用的默认超时值足够长以应对这些延迟，但存在一些特殊用例，你可能希望根据需求自定义默认值。例如，当 HTTP 请求在前往应用服务器的途中经过某些代理时。

HttpUrlConnection 为我们提供了两个成员方法，可用于更改默认超时：

```java
void setConnectTimeout(int timeoutMillis)
void setReadTimeout(int timeoutMillis)
```

setConnectTimeout(int) 能够重新定义客户端允许等待直到与远程主机建立 TCP 连接的最长时间（以毫秒为单位，例如服务器宕机的情况）。每当连接失败时（例如服务器宕机或因资源不足无法及时响应），都会抛出 ConnectTimeoutException，因此你应当小心，并在代码中干净利落地处理此异常。

当主机名解析为多个地址时，客户端将依次尝试连接，且超时将多次适用。如果超时设置为 0，连接将阻塞直到 TCP 超时过期，在 Android 上通常为数分钟。

setReadTimeout(int) 定义了客户端在读取操作上被允许阻塞的最长时间，直到任何可用数据可以被读取。默认值 0 将无限期阻塞读取操作，直到数据可用、远程对端断开连接或套接字上发生错误。当超时大于 0 且超时过期时没有可用数据，将抛出 SocketTimeoutException。

现在我们了解了每个超时的含义，让我们使用一些新的 setter 方法更新 HTTPRequest.Builder：

```java
public class HTTPRequest {
    ...
    public static class Builder {

        private int mReadTimeout = 0; // Default Value
        private int mConnectTimeout = 0; // Default Value

        public void setConnectTimeout(int connectTimeoutMs) {
            this.mConnectTimeout = connectTimeoutMs;
        }
        public void setReadTimeout(int readTimeoutMs) {
            this.mReadTimeout = readTimeoutMs;
        }
    }
}
```

最后，我们必须更新 HTTPAsyncTask，以在用于连接到远程服务器的 HttpURLConnection 对象上设置 HTTPRequest 中指定的超时：

```java
public class HTTPAsyncTask extends
AsyncTask<HTTPRequest, Void, Result<HTTPResponse>> {
@Override
protected Result<HTTPResponse> doInBackground(
HTTPRequest... params) {
conn = (HttpURLConnection) url.openConnection();
conn.setReadTimeout(request.mReadTimeout);
conn.setConnectTimeout(request.mConnectTimeout);
...
}
}
```

强烈建议你在多种网络延迟级别下测试自定义的超时，并考虑到服务器在负载下可能会将响应延迟几秒钟的时间。

典型的网络延迟可能从 LTE 移动网络上的 80-120 ms 到 GPRS 移动网络上的 150-550 ms

当你在互联网上运行应用程序并需要处理敏感数据，例如个人信息、支付信息或商业...

