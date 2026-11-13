## 第 5 章. 与网络交互

到目前为止，我们一直使用 HttpURLConnection HTTP 客户端来传输来自网络或发送到网络的数据，例如从 HTTP 服务器下载图像以及与远程 HTTP 服务器同步信息。我们一直在盲目地使用这个 Android HTTP 客户端，而没有深入了解其内部机制以及这个方便的框架为我们透明处理 HTTP 协议所提供的功能。

在本章中，我们将了解 HttpURLConnection 的高级功能，以及使用 HTTP 协议与远程服务器进行异步和安全通信的新技术。

同时，我们将学习如何使用自定义 HTTP 客户端通过安全通道进行通信、调整 HTTP 客户端以应对网络延迟，以及学习如何与 Web API 交互。

在本章中，我们将涵盖以下内容：

- 介绍 Android HTTP 客户端
- 异步执行 HTTP 请求
- 与 JSON Web API 交互
- 与 XML Web API 交互
- 优化 HTTP 超时
- 通过 SSL 会话安全通信
- HTTP 开源库

### 介绍 Android HTTP 客户端

近来，从远程服务器发送和接收数据的能力已成为所有应用程序都应具备的基本功能，以创建动态且令人印象深刻的体验。如今，几乎每个应用程序都使用网络来获取数据信息、执行远程业务逻辑操作以及下载或上传资源。

应用程序与远程服务器之间发生的网络交互通常定义为一组使用网络协议穿越网络的请求/响应消息。

一般来说，HTTP 协议常用于在对等体之间传输消息，Android SDK 随附两个可直接用于发送和接收数据的高级 HTTP 客户端：AndroidHttpClient 和 HttpURLConnection。

HTTP 通信协议是一种无状态的标准文本应用层协议，由互联网工程任务组（IETF）和万维网联盟（W3C）维护，并在互联网上广泛用于客户端（通常称为用户代理）与服务器之间交换数据。

该协议随着时间的推移进行了一些改进，但大多数服务器和客户端都将其实现基于 HTTP 1.1，这是原始 HTTP 1.0 的修订版，向原始协议引入了连接复用功能和分块传输编码。

在典型的 HTTP 流程中，发起操作的客户端通过连接向服务器发送请求并等待服务器的响应。在另一端，服务器从通信通道读取请求，处理请求，并将响应发送回客户端。在下一个图中，我们可以可视化对等体之间交换的请求和响应示例：



| 请求 | 响应 |
|---|---|
| GET /greetings HTTP/1.1 | HTTP/1.1 200 OK |
| User-Agent: httpserver/7.16.3 | Server: Apache/1.3.3.7 (Unix) |
| Host: www.packtpublishing.com | Host: www.packtpublishing.com |
| Accept-Language: en, mi | Content-Length: 18 |
| | Content-Type: text/plain |
| | Connection: close |
| 头部 | 头部 |
| | 正文 |
| | 这是一个示例 |

新的 HTTP 修订版 2.0 于 2015 年 5 月发布，尚未得到广泛采用，且 Android SDK 中没有官方支持。

在对 HTTP 协议进行初步介绍之后，我们将尝试比较 Android SDK 上可用的 HTTP 客户端。

## AndroidHttpClient

AndroidHttpClient 客户端库基于 Apache HTTP 客户端，自 API Level 9 (Gingerbread) 起已被弃用，但它提供了一个庞大且灵活的 API 来访问 HTTP 服务器，支持 Cookie 管理、超时自定义、基本身份验证方案和安全通信渠道。

该客户端在 API Level 8 (Froyo) 和 API Level 7 (Eclair) 上比 HttpURLConnection 更稳定。

在 API Level 23 (Marshmallow) 上，由于缺乏透明响应压缩和响应缓存，对该客户端的支持已被移除，转而支持 HttpURLConnection。

## HttpURLConnection

该客户端框架支持安全通信会话 (HTTPS)、透明响应压缩、响应缓存、网络超时自定义、网络连接轮询、IPv6 支持、代理服务器和流式传输。

根据 Google 的说法，在 API Level 8 (Froyo) 之前，该客户端存在一些重要问题，可能会影响 HTTP 连接重用。

### 注意

自 Android 4.4 (KitKat) 起，此实现引擎基于开源 OkHttp Square 库。

鉴于 HttpURLConnection 是 Google 为高于 API Level 9 的 Android 版本推荐的 HTTP 客户端，我们将基于此 HTTP 客户端编写代码示例。不过，通过根据 API Level 使用不同的 Android HTTP 客户端，可以克服此碎片化问题：

```java
if (Build.VERSION.SDK_INT >= 9) {
    // After Gingerbread, use the google recommended
    // client, HttpUrlConnection
    ...
} else {
    // Prior to Gingerbread, use the Apache based
    // client, AndroidHttpClient
    ...
}
```

在下一节中，我们将开始基于 HttpURLConnection 编写我们的 HTTP 异步工具包，并探索客户端提供的用于与远程对等体通信的高级功能。

## 异步执行 HTTP 请求

迄今为止，我们在代码示例中一直使用 HttpURLConnection 客户端和 AsyncTask 来异步检索远程数据。

虽然此方案在大多数情况下可行，但我们的应用程序中可能最终会出现大量重复代码。

在本节中，我们将创建一个简洁的高级异步 HTTP 客户端，在主线程之外执行远程请求，并使用回调处理程序将请求结果转发到应用程序的主线程。此方法与应用程序 UI 模型非常契合，因为回调处理程序在主线程上执行，能够使用从服务器检索到的数据更新 UI。

首先，让我们定义我们的异步客户端应遵循的基本接口，以便在后台执行远程请求：

```java
public interface AsyncHTTPClient {
    void execute(HTTPRequest request, ResponseHandler handler);
}
```

HTTPRequest 类是一个 Java 模型，用于定义构建 HTTP 请求所需的所有参数。我们将省略一些实现细节，但借助 Builder 类，我们将能够定义 HTTP 请求动词、请求 URL、HTTP 头部、HTTP 查询参数和 HTTP 正文：

```java
public class HTTPRequest {
    final Verb mVerb;
    final String mUrl;
    final List<Header> mHeaders;
    final Map<String, String> mParameters;
    final Body mBody;

    private HTTPRequest(Builder builder) {...}
}
```

ResponseHandler 是定义回调的类，这些回调将在服务器发送成功或失败响应，或操作执行期间发生异常时被调用。因此，我们将定义一个抽象的 ResponseHandler 类供子类实现：

```java
public abstract class ResponseHandler {
    // Method invoked when a success response is returned
    // 200 Response Code
    abstract public void onSuccess(HTTPResponse response) ;

    // Method invoked when a failure response is returned
    // 4XX, 5XX
    abstract public void onFailure(HTTPResponse response) ;

    // Method Invoked when an error happens
    abstract public void onError(Throwable error);
}
```

当响应或错误准备好分发到处理程序时，所有回调方法会自动转发到主线程。所有网络和输入/输出操作以及内存分配都必须在后台线程上完成，以避免任何不需要的 UI 停顿。

当服务器返回 HTTP 响应时，根据响应消息返回的代码，将调用以下方法之一：onSuccess 或 onFailure。因此，当任何回调被调用时，会交付一个 HTTPResponse 对象以供进一步处理。

目前，HTTPResponse 类携带有关请求代码、响应头部和响应正文的信息：

```java
public class HTTPResponse {
    final int mResponseCode;
    final List<Header> mHeaders;
    final Body mBody;
}
```

在已定义基础类和接口的情况下，让我们借助 HttpURLConnection 类来实现我们的异步高级客户端。

由于我们已经知道如何基于 AsyncTask 类构建后台处理管道，为求简单，我们将基于此构造来实现。将来，您可以将 AsyncTask 替换为 AsyncTaskLoader 以支持配置更改：

```java
public class HTTPAsyncTask extends
AsyncTask<HttpRequest, Void, Result<HttpResponse>> {

// Response Handler to be invoked In onPostExecute
// on the UI Thread
final ResponseHandler mHandler;

// Handler is passed on the constructor
public HTTPAsyncTask(ResponseHandler handler) {
this.mHandler = handler;
}

...
}
```

如前面的代码所示，我们的 AsyncTask 的输入参数类型是 HttpRequest，因此 Result<HttpResponse> 类型的对象会发送到 UI 线程。结果是一个泛型类，如前几章中所定义，能够携带错误或 HttpResponse 对象。

在已定义 HTTPAsyncTask 的泛型参数后，现在是时候覆写 doInBackground 以在后台发送 HTTP 请求并处理响应了：

```java
@Override
protected Result<HttpResponse> doInBackground(HttpRequest... params) {

HttpRequest request = params[0];
Body body = null;
HttpURLConnection conn = null;
Result<HttpResponse> response = new Result<HttpResponse>();
try {

// Retrieve the request URL from the request object
URL url = new URL(request.mUrl);

// Opens up the connection to the remote peer
conn = (HttpURLConnection) url.openConnection();

// Set the HTTP Request verb
conn.setRequestMethod(request.mVerb);
// set The HTTP Request Headers
setRequestHeaders(conn,request);
// Allows Receiving data on the response
// conn.setDoInput(true);
// Retrieve the response code
int responseCode = conn.getResponseCode();
// Build the HTTP Response Object
HTTPResponse.Builder builder = new HTTPResponse.Builder()
.setResponseCode(responseCode);
// Fill the HTTP Response Headers
fillHeaders(conn.getHeaderFields(), builder);
// Read the Body from the Connection Input Stream
// using a factory
body = BodyFactory.read(conn.getContentType(),
conn.getInputStream());
// Set the HTTPResponse body
builder.setBody(body);
// Build the HTTP Response Object
response.result = builder.build();
} catch (Exception e) {
response.error = e;
} finally {
if ( conn != null ) {
conn.disconnect();
}
}
return response;
}
```

```java
// Write any header to the Request Ex: Accept: text/xml
void setRequestHeaders(HttpURLConnection con, HTTPRequest request )
{
for (Header header : request.mHeaders) {
con.addRequestProperty(header.getName(), header.getValue());
}
}
```

openConnection 将与 URL 对象中指定的资源建立 TCP 连接。一旦连接建立，并且我们尝试检索



状态行响应码，我们的 HTTP 请求头和正文被发送到网络。

读取状态行后，我们处理接收到的 HTTP 响应头，并将其存储在响应对象中以供进一步处理。您可能已经知道，HTTP 响应可能包含与请求 URL 指定的资源相关的 HTTP 消息正文数据。

为了进一步处理，数据将从连接的 InputStream 中读取，以构建 Body 对象。为了检测 HTTP 响应中接收到的数据内容类型，客户端应查看标头 Content-Type 内容。为了简化这一识别过程，HttpURLConnection 类提供了成员方法 getContentType()，它直接从标头中检索内容。

正文的消费和构建在以下代码所示的 BodyFactory 类中完成：

```
public class BodyFactory {
    public static Body read(String mimeType,
        InputStream is) throws IOException {
        Body result = null;
        if ( mimeType.startsWith("text") ) {
            result = new TextPlainBody(mimeType);
            result.consume(is);
        }
        return result;
    }
}
```

由于已知内容类型，我们准备好消费正文，并将接收到的字节存储在 Body 对象中，以便由我们的 ResponseHandler 进一步处理。

Body 是一个抽象类，本身无法读取任何类型的内容，尽管我们可以直接扩展 Body 类，从接收到的数据构建文本正文。

我们的 Body 子类 TextPlainBody 将实现抽象 consume 函数，以便从 InputStream 构建正文：

```
abstract void consume(InputStream is)
throws IOException;
```

为简洁起见，TextPlainBody 的 consume 代码被省略，但完整源代码可从 Packt Publishing 网站下载。请查看 TextPlainBody 源代码，以了解我们如何使用 InputStream 构建字符串。

目前，我们仅支持 text/* 类型；不过，在接下来的章节中，我们将扩展 BodyFactory 类以支持其他有趣的 mime 类型，例如 JSON 文档。

响应正文完全读取完毕后，与远程服务器的连接将关闭，连接所占用的资源将被释放。

### 注

连接不会立即销毁，而是会被推送到连接池中以供将来使用。在池中空闲有限时间（idleTimeout）后，连接将被销毁。

在 doInBackground 执行期间，可能会发生网络或输入/输出异常，因此为了避免应用程序崩溃，我们必须捕获并通过 result.error 将其转发给 postExecute 函数，并随后转发给启动执行时指定的 ResponseHandler。

从 AsyncTask 的方法中传播出去的未检查异常会导致我们的应用程序崩溃，因此我们必须仔细测试并在必要时处理这些异常。

为了使我们的 AsyncTask 子类有用，我们必须编写 onPostExecute 函数，以将响应或错误转发给 ResponseHandler 对象：

```
protected void onPostExecute(Result<HTTPResponse> result) {
    if ( result.error != null ) {
        mHandler.onError(result.error);
    } else if ( result.obj.mResponseCode ==
        HttpURLConnection.HTTP_OK ) {
        mHandler.onSuccess(result.obj);
    } else {
        mHandler.onFailure(result.obj);
    }
}
```

### 注

如前所述在第 3 章《探索 AsyncTask》中，onPostExecute 回调将在主线程上执行，因此应避免在以下回调中执行任何耗时操作：onError、onSuccess 和 onFailure。

剩下的就是从我们的 AsyncHTTPClient 子类 PacktAsyncHTTPClient 中调用我们的 AsyncTask：

```
public class PacktAsyncHTTPClient implements AsyncHTTPClient {

    @Override
    public void execute( HTTPRequest request,
                         ResponseHandler handler) {
        // Execute the HTTP Request on the default AsyncTask Executor
        new HTTPAsyncTask(handler).execute(request);
    }
    ...
}
```

太棒了！现在我们有了一个支持文本 mime 类型的核心异步 HTTP 客户端实现。在下一节中，我们将使用高级客户端从远程服务器获取文本消息。

## 获取文本响应

利用支持文本响应的异步 HTTP 客户端，我们可以获取动态文本，因此让我们创建一个显示远程 URL 资源上可用文本的 activity。

首先，我们需要使用 HTTPRequest.Builder 类构建 HTTPRequest：

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    HTTPRequest.Builder builder = new HTTPRequest.Builder();
    // Set the HTTP Verb to GET
    builder.setVerb(HTTPRequest.Verb.GET);
    // Sets location of the remote resource
    builder.setUrl("http://<hostname>:<port>/greetings");
    // Build the request object
    HTTPRequest request = builder.build();
    ...
}
```

我们希望提请您注意，您应替换 <hostname> 和 <port>，以使其适用于您的 HTTP 服务器。

如前所述，要通过我们的异步客户端 AsyncHTTPClient 执行请求，必须向 execute 方法提供一个 ResponseHandler 对象。此外，我们还想定义一个对象，用于用获取到的文本更新 UI。

首先，我们将扩展我们的 ResponseHandler 抽象类并创建一个类来处理正文，并将响应转发给一个接收文本消息作为输入的回调：

```
public abstract class TextResponseHandler
extends ResponseHandler {
    // Response Callback receiving the string body
    abstract void onSuccess(String response);

    @Override
    public void onSuccess(HTTPResponse response) {
        TextPlainBody body = (TextPlainBody)response.mBody;
        // Invoke the callback that receives a string
        onSuccess(body.getContent());
    }
}
```

```
}
```

接下来，我们需要完成 Activity 代码以发送 HTTP 请求，同时还将显示一个进度对话框，让用户知道后台正在进行某些操作：

```
HTTPRequest request = builder.build();
// Create a client Instance object
PacktAsyncHTTPClient client = new PacktAsyncHTTPClient();
// Enable a progress bar
ProgressBar pb = (ProgressBar) findViewById(R.id.loading);
pb.setVisibility(View.VISIBLE);
// Retrieve the response on the background
client.execute(request, textResponseHandler);
...
```

现在，我们只需要定义 textResponseHandler，这是一个实现 TextResponseHandler 的匿名内部类，它使用接收到的字符串更新 View，并关闭启用的不确定进度对话框：

```
TextResponseHandler greetingsHandler = new TextResponseHandler() {
    // Invoked when request was processed with success by the server
    @Override
    void onSuccess(String response) {

        // Update the View with the String received on the
        // HTTP body
        EditText et = (EditText) findViewById(R.id.inputText);
        et.setText(response);
        dismissProgress();
    }

    // Invoked when the server returned an error
    @Override
    public void onFailure(HTTPResponse response) {
        Log.e("GreetingsActivity", "Server returned an error: " +
        response.mResponseCode + " " +
        response.mResponseMessage);
        dismissProgress();
    }

    // Invoked when an error happened
    @Override
    public void onError(Throwable error) {
        Log.e("GreetingsActivity", "Exception happened: " +
        error.getMessage(), error);
        dismissProgress();
    }
};
```

请注意，所有回调函数都将在主线程上执行，因此在这些函数中，您应遵循不阻塞线程的黄金法则，以避免 UI 延迟。

当服务器返回错误或执行期间发生异常时，将分别调用以下回调方法：onFailure 和 onError。

支持从远程服务器读取文本资源是一个很好的起点，尽管在大多数情况下，我们打算与远程服务器通信以交换结构化文档格式，例如 JSON、XML，甚至二进制数据。





## 与 JSON Web API 交互

我们之前的 TextResponseHandler 能够处理带有文本响应的通用 HTTPResponse，并将来自 JSON Web API 的 String 转发到 TextResponseHandler 的 onReceive(String) 回调。

现在我们想进一步，将 doInBackground 返回的 HTTPResponse 转换为一个普通旧式 Java 对象 (POJO)，用于表征我们业务逻辑中的模型。为实现此目标，我们必须将 HTTP body 中返回的 JSON 结构内容转换为先前定义的 Java 对象。

为了将处理转发到上面定义的回调，我们将创建 JSONResponseHandler，它是 ResponseHandler 的抽象子类，实现 onSuccess(HTTPResponse)、onFailure(HTTPResponse)，并分别将对象转换为 <ResponseType> 和 <ErrorType>：

```java
public abstract class JsonResponseHandler<ResponseType, ErrorType>
extends ResponseHandler {

abstract public void onSuccess(ResponseType response);
abstract public void onFailure(ErrorType response);
...
}
```

为了支持处理不同的 body 内容，我们需要更新 BodyFactory。因此，在继续 JSONResponseHandler 实现之前，我们将更新 BodyFactory 函数以支持 application/json MIME 类型：

```java
public class BodyFactory {

public static Body read(String mimeType,
InputStream is)
throws IOException {
...
} else if ( mimeType.startsWith("application/json")){
result = new RawBody(mimeType);
result.consume(is);
}
return result;
}
}
```

请注意，我们从网络读取内容，并将数据存储在名为 RawBody 的新 Body 类中，该类只是将 HTTP Response body 中的内容存储在内部字节内存缓冲区中。

### 注意

存储完整 body 对于我们的简单客户端用例可能可行。但是，如果我们愿意处理兆字节级数据的 body，则必须使用另一种策略来读取和消耗 body，分块消耗 body，或将 body 保存到本地文件系统。

既然带有 JSON Web API 数据的 body 已经存储在设备内存中，我们就可以借助 JSONResponseHandler 来处理它了。

### 将 Java 对象转换为 JSON

要将 JSON 文档格式转换为普通对象，我们将使用一个在 Android 社区中非常知名的开源库——Google GSON 库：https://github.com/google/gson。

GSON 库是由 Google 开发和维护的库，能够将 Java 对象转换为 JSON 对象，反之亦然。

因此，在继续之前，请确保将该库添加到您的 Gradle 或 Eclipse 项目中：

```
compile 'com.google.code.gson:gson:2.5'
```

要从协议编码的内容 body 转换为 POJO，我们将定义 BodyDecoder 接口：

```java
public interface BodyDecoder <T> {
    T decode(Body body) throws Exception ;
}
```

要从 POJO 转换为 Body 对象，我们将定义 BodyEncoder 接口：

```java
public interface BodyEncoder<T> {
    T bodyEncode(T obj, String mimeType) throws Exception;
}
```

要将 JSON 文档解码为 POJO，我们需要子类化此泛型接口，并编写借助 GSON 库处理 JSON 解码的代码：

```java
public class JSONConverter<POJO>
implements BodyEncoder<POJO>, BodyDecoder<POJO> {
// Store the Generic Type information
private final Type pojoType;

JSONConverter(Type pojoType) {
this.pojoType = pojoType;
}

@Override
public POJO decode(Body body) throws Exception {
Gson gson = new Gson();
RawBody rawBody = (RawBody) body;
InputStream is = null;
POJO obj = null;
try {
is = new ByteArrayInputStream(rawBody.getContent());
BufferedReader bfReader =
new BufferedReader(new InputStreamReader(is));
obj = gson.fromJson(bfReader, pojoType);
} finally {
if (is != null) is.close();
}
return obj;
}
}
```

最后，我们准备好实现我们的通用 JSONResponseHandler，它将 onSuccess 和 onFailure 方法中返回的转换后的 JSON 结构文档转发到泛型类型 Response 和 Error。

这是 JsonResponseHandler 代码的样子：

```java
public abstract class JsonResponseHandler<ResponseType, ErrorType>
extends ResponseHandler {
// Store the Response Type class information
private final Type responseType;
// Store the Error Type class information
private final Type errorType;

JsonResponseHandler(Type responseType, Type errorType) {
    this.responseType = responseType;
    this.errorType = errorType;
}

// Callback invoked on the main Thread that converts
// a body to a POJO object
@Override
public void onSuccess(HTTPResponse response) {
    RawBody body = (RawBody)response.mBody;

    if ( body != null ) {
        Response obj = null;
        try {
            obj = new JSONConverter<Response>(responseType).
                decode(body);
            onSuccess(obj);
        } catch (Exception e) {
            onError(e);
        }
    } else {
        onSuccess((Response)null);
    }
}

@Override
public void onFailure(HTTPResponse response) {...}
}
```

这个泛型类可以消耗从 onSuccess 或 onFailure 转发的 RawBody 对象，并暴露两个泛型类型参数。第一个 Response 是指定成功响应（onSuccess）的 POJO 类类型的泛型类型，Error 泛型类型指定错误函数（onFailure）的 POJO 类类型。

当任何接收 HTTPResponse 的回调函数被调用时，借助 GSON 库，我们将存储在 RawBody 中的 JSON 文档转换为响应/错误对象。

onFailure 函数被省略，因为其代码与 onSuccess 方法非常相似；不过，您可以查看 Packt Publishing 网站上的完整源代码以了解差异。

最后，我们准备通过从 HTTP JSON body 读取模型来练习我们的异步客户端：

ContentType: application/json

```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "...",
    "body": "..."
  },...
]
```

要测试我们的客户端，我们将利用 JSONPlaceHolder (http://jsonplaceholder.typicode.com/)，这是一个用于测试和原型开发的假在线 REST API。

在尝试访问它之前，让我们先定义我们将在示例中使用的用户 POJO 类：

```java
public class User {
  public int id;
  public String name;
  public String username;
  public String email;
  public String phone;
  public String website;
}
```

我们还将为失败函数定义一个 POJO 类：

```java
public class Error {
  public int resultCode;
  public String resultMessage;
}
```

为了显示假 API 返回的模型，我们将创建一个新的 UserListActivity，它将实现 JSONResponseHandler 并显示来自 http://jsonplaceholder.typicode.com/users 的 JSON 文档中所有用户的名称和电子邮件。

让我们先定义 JsonResponseHandler<List<User>, Error> 匿名类，它接收 List<User> 作为参数，用于成功回调，以及接收 Error 对象作为参数，用于失败回调。

onSuccess(List<User>) 将更新一个 ListView，显示名称后跟电子邮件：

```java
JsonResponseHandler<List<User>, Error> jsonResponseHandler =
    new JsonResponseHandler<List<User>, Error>(
    new TypeToken<ArrayList<User>>() {}.getType(),
    new TypeToken<Error>() {}.getType()) {
    // On Success Callback
    @Override
    public void onSuccess(List<User> users) {
    // Update the List View with the a List Adapter that
    displays
    // the user name and email per user
    ListView listView = (ListView)
    findViewById(R.id.usersList);
    ListAdapter adapter = new UserListAdapter(
    UserListActivity.this, users);
    listView.setAdapter(adapter);
    }
    // Prints the Error object
    @Override
    public void onFailure(Error response) {
    Log.e("UserListActivity",
    "Error happened retrieving greetings " +
    response.toString());
    }
    @Override
    public void onError(Throwable error) {
    // Do Something with the exception
    }
    };
```

剩下的就是编写 Activity 的 onCreate 函数，构建 HTTP 请求并将 GET /users 请求分发到 JSONPlaceholder API：

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.user_list_layout);

    HTTPRequest.Builder builder = new HTTPRequest.Builder();
    // Set the HTTP Verb to GET
    builder.setVerb(HttpRequest.Verb.GET);
}
```



```
// Sets location of the remote resource
builder.setUrl("http://jsonplaceholder.typicode.com/users");

// Notify the server that the client is able to receive json
// documents
builder.addHeader(new Header("Accept", "application/json"));
HTTPRequest request = builder.build();

PacketAsyncHTTPClient client = new PacketAsyncHTTPClient();
client.execute(request, jsonResponseHandler);
}
```

当服务器向我们的请求发送响应时，JSONResponseHandler 对象将解码包含 10 个用户列表的 JSON 文档，并将该文档转换为 List<User> 对象。

由于其简洁性、可读性、紧凑性以及缺乏数据结构刚性，JSON 数据交换格式已在大多数最近部署到互联网上的 API 和 Web 服务中占据主导地位。

不过，基于 XML 的远程 Web 服务（主要基于 SOA 数据交换协议）仍然存在，以覆盖高级用例，在其中我们希望拥有更严格的数据模型架构验证和建模、内置命名空间支持以及高级信息提取工具。

基于 XML 的 API 在行业中已经失去了一些影响力，尽管基于 XML 的 API 仍然很常见，因此在下一节中，我们将扩展工具包以支持对 HTTP 请求中的 XML 内容进行编码和解码。

## 与 XML Web API 交互

在过去 10 年中，基于 XML 消息的 API 已成功用于在应用程序和远程服务器之间交换数据（SOAP）以及运行远程过程（XML-RPC）。此外，如今，一些 REST API 同时构建了对 JSON 和 XML 的支持，因此由开发者决定是否希望使用 XML 或 JSON 文档与 API 进行交互。

出于这一考虑，我们将扩展工具包以交换 XML 消息，从而与远程服务器进行通信。

在前面的章节中，我们实现了一个 ResponseHandler 来解码发送在 HTTP 主体中的 JSON 文档，但现在我们希望进一步添加对在 HTTP 请求中发送 XML 文档的支持。

如果我们回过头来看 HTTPAsyncTask 的细节，就会发现支持请求主体传输所需的代码有所缺失；因此，现在是重写代码以在 HTTP 请求中携带实体主体的绝佳时机：

```
@Override
protected Result<HTTPResponse> doInBackground(HTTPRequest... params) {
    ...
    if ( request.mBody != null ) {
        // Allows Sending data on the request
        conn.setDoOutput(true);
        // Specifies The ContentType based on the Body Mime Type
        conn.setRequestProperty(
            "Content-type", request.mBody.getMimeType());
        // Retrieves the connection stream to write content
        OutputStream os = conn.getOutputStream();
        request.mBody.write(os);
    }
    // Retrieve the response code
    int responseCode = conn.getResponseCode();
    ...
}
```

在前面的代码中，当 HTTPRequest 对象上可用主体且在我们尝试读取响应状态行的 responseCode 之前，我们会从 HttpURLConnection 中检索输出流，并将 Body 对象中存储的内容写入其中。

除了写入的数据外，还会根据主体的 MIME 类型在 HTTP 请求头部设置 Content-Type 头部。

现在，我们已准备好分别在 HTTP 请求和响应中发送和接收实体主体，因此可以开始处理我们的 XML 实现示例了。

在接下来的段落中，并遵循 JSON 部分，我们将编写代码将 API 请求序列化为 XML 文档，以及将响应中接收到的 XML 文档反序列化为 API 响应对象的代码。

### 将 Java 对象转换为 XML

为了在 Java 对象和 XML 之间进行反序列化和序列化，我们将使用开源库 SimpleXML（http://simple.sourceforge.net/）；因此，要使用它，请将此库添加到应用程序的外部依赖项列表中。

如果您使用 Android Studio，以下是要添加到 build.gradle 依赖项列表中的内容：

```
dependencies {
    ...
    compile('org.simpleframework:simple-xml:2.7.+'){
        exclude module: 'stax'
        exclude module: 'stax-api'
        exclude module: 'xpp3'
    }
}
```

使用先前定义的 User 模型，我们将构建一个名为 GetUserInfo 的 APIRequest，用于获取请求中提交的用户标识符的用户详细信息（GetUserInfoResponse）。

为了进一步说明这一点，让我们定义 API 请求 POJO 对象：

```
@Root(name = "GetUserInfo")
@Namespace(prefix="p",
    reference="https://www.packtpub.com/asynchronous_android")
public class GetUserInfo {
    @Namespace(reference=
        "https://www.packtpub.com/asynchronous_android")
    @Element(name="UserId")
    public String userId;
    // ... Setters and Getters
}
```

并且让我们定义 API 响应 POJO 对象：

```
@Root
@Namespace(prefix = "p",
        reference = "https://www.packtpub.com/asynchronous_android")
public class GetUserInfoResponse {

    @Element(name = "User")
    @Namespace(prefix = "p",
            reference = "https://www.packtpub.com/asynchronous_android" )
    private User user = new User();
    // ... Setters and Getters
}
```

其他类（User、Address 和 Company）的注解与上面的非常相似，所有元素都引用 https://www.packtpub.com/asynchronous_android 命名空间，因此为了简洁起见，我们将省略 POJO 更改。

完成 Java 对象与 XML 请求和响应消息之间的映射后，让我们开始处理将 GetUserInfo 对象转换为 XML 文档的代码。

要从 Java 对象编码 XML 文档，我们将子类化此泛型接口，在 SimpleXML 的帮助下将 POJO 泛型类型编码为 XML 文档：

```
public class XMLConverter<POJO>
    implements Encoder<POJO>{

    private final Class<POJO> clazz;
    XMLConverter(Class<POJO> clazz){ this.clazz = clazz; }

    @Override
    public Body encode(POJO obj, String mimeType)
        throws Exception {
// Creates SimpleXML Serializer Instance
Serializer serializer = new Persister();
ByteArrayOutputStream output = new ByteArrayOutputStream();

// Converts from obj -> xml document
serializer.write(obj, output);

// Build a RawBody body
RawBody body = new RawBody(mimeType);
output.close();

// Stores the result on the body
body.setContent(output.toByteArray());
return body;
}
}
```

### 将 XML 转换为 Java 对象

要将 XML 文档解码为 Java POJO 对象，我们将 BodyDecoder<POJO> 添加到 XMLConverter 类定义接口中，并在 SimpleXML 库的帮助下编写将 XML 文档转换为 POJO 泛型类的代码：

```
public class XMLConverter<POJO>
implements BodyEncoder<POJO>, BodyDecoder<POJO> {
...
@Override
public POJO decode(Body body) throws Exception {
// Instantiate a SimpleXML Serializer
Serializer serializer = new Persister();
InputStream is = null;
RawBody rawBody = (RawBody)body;
POJO obj = null;
try {
is = new ByteArrayInputStream(rawBody.getContent());
BufferedReader bfReader = new BufferedReader(new InputStreamReader(is));
obj = (POJO) serializer.read(clazz, bfReader);
} finally {
if ( is != null ) is.close();
}
return obj;
}
...
```

XML 序列化和反序列化已准备就绪，让我们开始编写 Activity，它将使用上面定义的 GetUserInfo 和 GetUserInfoResponse 从远程服务器检索用户信息并在 UI 上显示用户信息。

为了测试我们的客户端，我们将借助模拟 Web 应用程序 https://www.mockable.io/ 创建一个假 XML Web 服务。该 Web 应用程序允许我们创建可配置的 REST API，这些 API 会将静态 JSON 或 XML 文档作为响应返回给客户端，因此此工具将极大地帮助我们在受控环境中测试客户端。

当任何 HTTP 请求发送到以下 URI 时，我们的假 HTTP API 将返回以下 XML 文档，http://demo1472539.mockable.io/GetUserInfo：

```
<?xml version="1.0" ?>
<p:GetUserInfoResponse
xmlns:p="https://www.packtpub.com/asynchronous_android">
<p:User>
<p:Id>12</p:Id>
<p:Name>John</p:Name>
<p:Username>John</p:Username>
...
<p:Company>
<p:Name>Packt</p:Name>
...
</p:Company>
</p:User>
</p:GetUserInfoResponse>
```

为了将由 onSuccess(HTTPRequest) 和 onFailure(HTTPResponse) 回调接收到的 HTTPResponse 对象转换为领域模型，我们将不得不子类化 ResponseHandler 抽象类并创建反序列化泛型 POJO 类的代码：

```
public abstract class XMLResponseHandler<Response, Error> extends ResponseHandler {
// Class used to by Simple to convert to the ResponseType
private final Class<Response> responseClass;

private final Class<Error> errorClass;
```





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



在文档中，使用安全层保护您的通信渠道非常重要，该层可隐藏交换的数据，防止外部攻击者窃取或避免任何伪造攻击。在下一节中，我们将扩展高级客户端，以支持与远程服务器的安全 SSL 连接渠道。

## 通过 SSL 会话进行安全通信

迄今为止，我们一直使用明文连接与远程 HTTP 服务器通信。尽管当交换的数据不敏感时，这种连接可能适合您的应用程序要求，但存在必须使用安全通道来发送或接收的场景，以防止任何第三方读取或更改网络上交换的数据。

为了与远程服务器建立 SSL 会话，我们的客户端将借助一些加密工具，创建一个加密通信通道，其中所有数据都使用在安全连接握手期间交换的密钥的对称密码进行加密。除此之外，使用先前交换的密钥接收和加密的内容会与其他对等方公钥进行验证，以证明数据来自并由正确的来源签名。

在连接建立过程中，作为 SSL 握手的一部分，服务器必须证明它持有受信任证书的私钥。受信任证书是指来自我们受信任**证书颁发机构（CA）**列表的证书，或由您受信任 CA 中的某个证书签名的证书。

### 注意

Android 平台内置了一系列知名的受信任 CA，帮助我们确保互联网上大多数服务器的身份。

因此，当我们联系使用知名 CA 签名证书的服务器时，之前用于创建 HTTPS 连接的代码无需任何更改：

```java
URL url = new URL("https://packtpub.com");
con = (HttpsURLConnection) url.openConnection();
```

此代码示例将使用 Android 平台指定的默认加密密码套件创建 SSL 连接，并根据平台 CA 验证服务器。

虽然这在大多数情况下都可以工作，但可能需要使用特定 CA、受信任证书列表，或仅使用最安全的密码套件（TLSv1）。在开发人员需要构建自己的 SSLContext 并指定自己的 TrustManager 和 KeyManager 才能成功连接到远程服务器的情况下。

在接下来的段落中，我们将扩展客户端，以使用存储在 Java 密钥库中的私钥和 CA，以便验证并与由我们管理 **证书颁发机构（CA）** 的服务器进行通信。

首先，我们将创建一个 SSLOptions 类，在其中定义我们希望在 SSL 会话中使用的密码套件：

```java
public class SSLOptions {

enum CipherSuite {
    DEFAULT("Default"), // Supported on Android API Level > 9
    SSL("SSL"), // Supported on Android API Level > 9
    SSLv3("SSLv3"), // Supported on Android API Level > 9
    TLS("TLS"), // Supported on Android API Level > 1
    TLSv1("TLSv1"), // Supported on Android API Level > 1+
    TLSv1_1("TLSv1.1"), // Supported on Android API Level > 16+
    TLSv1_2("TLSv1.2"); // Supported on Android API Level > 16+
    ...
}

// Cipher Suite used on the connection
final CipherSuite cipherSuite;
final SSLContext sslContext;

public SSLOptions(Context ctx, Builder builder)
throws Exception {
    ...
    // Build up our own SSLContext
    sslContext = SSLContext.
    getInstance(cipherSuite.toString());
    // Will use the Android default KeyManager and TrustManager
    sslContext.init(null,null,new SecureRandom()));
}

public static class Builder {
private CipherSuite cipherSuite = CipherSuite.DEFAULT;
...
SSLOptions build(Context ctx) throws Exception {
    return new SSLOptions(ctx, this);
       }
   }
}
```

借助前面的代码，我们可以控制 SSLContext 使用的密码套件；但是，为了使用我们自己的私钥、证书和受信任 CA，我们需要使用自己的 TrustManager 和 KeyManager 初始化 SSLContext：

```
void init(KeyManager[] km, TrustManager[] tm, SecureRandom sr)
```

为简单起见，我们将从应用程序资源目录中存储的密钥库文件加载我们的 KeyManager 和 TrustManager。Java 密钥库可在 Packt Publishing 网站上获取。因此，在继续之前，请从 Packt 网站下载 asynchronous_client.ks 文件，其中包含可直接使用的私钥、证书以及作为受信任机构签名证书的自定义 CA 证书。

### 注意

请注意，您可以构建自己的 TrustManager 和 KeyManager 自定义子类，以从不同来源加载私钥和证书，但为简单起见，我们将从文件加载它们。

让我们借助 keytool 应用程序来查看我们的 asynchronous_client.ks Java 密钥库文件。

在命令行上，请运行以下命令以列出密钥库的内容：

```
keytool -list -v -keystore asynchronous_client.ks -storetype BKS
provider org.bouncycastle.jce.provider.BouncyCastleProvider
providerpath bcprov-jdk15on-146.jar
Alias name: asynchronous_client
Entry type: PrivateKeyEntry
Certificate[1]:
Owner: C=UK,ST=Birmingham,L=Birmingham,O=Packt Publishing,OU=Packt Publishing,CN=asynchronous_client
Issuer: C=UK,...,CN=packt
Certificate[2]:
Owner: C=UK,...,CN=packt
Alias name: ca
Entry type: trustedCertEntry
Owner: C=UK,..., CN=packt
Issuer: C=UK,..., CN=packt
```

我们的密钥库文件（将充当受信任存储文件和密钥库文件）包含名为 asynchronous_client 的公钥和私钥，以及名为 ca 的受信任 CA。keytool 需要 Bouncy Castle Provider JAR 来读取文件内容，因此请先从 Packt 网站下载该文件。

请注意，由于我们拥有 C=UK、ST=Birmingham、L=Birmingham、O=Packt Publishing 和 CN=packt 作为受信任 CA，我们将允许我们的 HTTP 连接到出示由此机构签名证书或所有中间证书均可信的证书（受信任证书链）的 SSL 端点。

### 注意

每台 Android 设备都预安装了一系列受信任证书，可用于对安全的远程对等方进行身份验证。

既然了解了我们的密钥库和信任库的详细信息，让我们更新 SSL 选项，以从 asynchronous_client.ks 加载 KeyManager 和 TrustManager：

```java
public class SSLOptions {
final CipherSuite cipherSuite;
final SSLContext sslContext;
private final String keyStore;
private final String keyStorePassword;
private final String trustStore;
private final String trustStorePassword;
...

public SSLOptions(Context ctx, Builder builder)
throws Exception {
...
sslContext = initSSLContext(ctx);
}
// Initialize the SSLContext with loaded
private SSLContext initSSLContext(Context ctx)
throws Exception {
    KeyManagerFactory kmf = getKeyManagerFactory(ctx);
    TrustManagerFactory tmf = getTrustManagerFactory(ctx);
    // Use the cipher suite defined SSL, SSLv3, TLS, TLSv1,
    SSLContext result = SSLContext.getInstance(
        cipherSuite.toString());

    result.init( kmf != null ? kmf.getKeyManagers() : null,
        tmf != null ? tmf.getTrustManagers() : null,
        new SecureRandom());
    return result;
}
}
```

请注意，我们使用 KeyManagerFactory 和 TrustManagerFactory 分别返回的 KeyManager 列表和 TrustManager 列表初始化 SSLContext。因此，下一步是编写代码成员方法以获取我们的工厂。那么，让我们从 getKeyManagerFactory 开始：

```java
KeyManagerFactory getKeyManagerFactory(Context ctx)
throws Exception {
    KeyManagerFactory kmf = null;
    // Initialize Key store
    if (keyStore != null) {
        // Load the file keystore from the assets directory
        InputStream keyStoreIs = ctx.getResources().getAssets().open(keyStore);

        String algorithm = KeyManagerFactory.getDefaultAlgorithm();
        kmf = KeyManagerFactory.getInstance(algorithm);

        // Create BouncyCastle Key Store
        KeyStore ks = KeyStore.getInstance("BKS");

        // Load the KeyManagers available on the file using
        // a password
        ks.load(keyStoreIs, keyStorePassword.toCharArray());
        kmf.init(ks, keyStorePassword.toCharArray());
    }
    return kmf;
}
```

之前的代码将从我们先前准备的 BKS 密钥库中加载公钥和私钥。因此，剩下的就是用于加载我们受信任 CA 的 getTrustManagerFactory 函数：

```java
TrustManagerFactory getTrustManagerFactory(Context ctx)
throws Exception {
```



```
throws Exception {
TrustManagerFactory tmf = null;
if ( trustStore != null ) {
InputStream keyStoreIs = ctx.getResources().
getAssets().open(trustStore);
String algorithm = TrustManagerFactory.getDefaultAlgorithm();
tmf = TrustManagerFactory.getInstance(algorithm);
KeyStore ts = KeyStore.getInstance("BKS");
ts.load(keyStoreIs, trustStorePassword.toCharArray());
tmf.init(ts);
}
return tmf;
}
```

既然我们有了用于初始化 SSLContext 的 SSLOptions 类，现在来处理 HTTPRequest，并更新我们的 Builder 以存储 SSLOptions：

```
public class HTTPRequest {

final SSLOptions mSSLOptions;

private HTTPRequest(Builder builder) {
this.mSSLOptions = builder.mSSLOptions;
}

public static class Builder {
...
private SSLOptions mSSLOptions = null;

public Builder setSSLOptions(SSLOptions options) {
this.mSSLOptions = options;
return this;
}
}
...
}
```

最后，我们准备更新 HTTPAsyncTask，以使用我们的 SSLOptions 对象来定制 SSL 客户端端点。因此，我们将能够验证我们服务器的身份，该服务器持有由我们自己的 CA 签名的证书（c=UK，ST=Birmingham，L=Birmingham，O=Packt Publishing，CN=packt），反之亦然：

```
@Override
protected Result<HTTPResponse>
doInBackground(HTTPRequest... params) {
    // Retrieve the request URL from the request object
    URL url = new URL(request.mUrl);
    // Opens up the connection to the remote peer
    conn = initConnection(request, url);
    ...
}
```

```
HttpURLConnection
initConnection( HTTPRequest request, URL url) throws IOException {
    HttpURLConnection genCon = (HttpURLConnection) url.
        openConnection();

    if ( url.getProtocol().equals("https") ) {
        HttpsURLConnection con = (HttpsURLConnection) genCon;
        // Apply our SSL Options to the connection
        if ( request.mSSLOptions != null ) {
            applySSLContext(request, con);
        }
    }
    return result;
}
```

```
void
applySSLContext(HTTPRequest request, HttpsURLConnection con) {
    // Initialize the SSL Session with your own
    // keystore and truststore
    if ( request.mSSLOptions != null ) {
        SSLContext ctx = request.mSSLOptions.sslContext;
        con.setSSLSocketFactory(ctx.getSocketFactory());
        con.setHostnameVerifier(new AcceptAllHostNameVerifier());
    }
}
```

请注意，我们的实现不执行主机名验证，因为服务器 CN 可能与用于联系 HTTP 服务器的主机名不匹配。

但是，如果你希望在这方面更严格，请将 setHostnameVerifier 行更改为使用默认行为，实现你自己的 HostnameVerifier，或使用 Android SDK 中可用的主机名验证器，例如 Apache X509HostnameVerifier，它用于检查提供的主机名是否与提供的证书 CN 或 Subject-Alts 中的任何一个匹配。

最后，让我们说明如何使用我们的客户端连接到持有以下证书链的服务器：

```
```
// Server Certificate
Certificate[1]:
Owner: CN=asynchronous_server, OU=Packt Publishing, O=Packt Publishing, L=Birmingham, ST=Birmingham, C=UK
Issuer: CN=packt, ..., C=UK

// CA Certificate
Certificate[2]:
Owner: CN=packt, OU=Packt Publishing, O=Packt Publishing,
L=Birmingham, ST=Birmingham, C=UK
Issuer: CN=packt, ..., C=UK
```
```

以下代码展示了如何与协议为 https 的 URL 建立 SSL 会话：

```
```
// Set the HTTP Verb to GET
builder.setVerb(HTTPRequest.Verb.GET);
// Sets location of the remote resource
builder.setUrl("https://<server_hostname>:<port>/hello_ssl");
builder.addHeader(new Header("Accept", "text/plain"));
SSLOptions.Builder sslBuilder = new SSLOptions.Builder();

// TLS Cipher Suite using the asynchronous_client.ks
// as the truststore file and keystore file
sslBuilder.setKeyStore("asynchronous_client.ks", "123qwe")
    .setTrustStore("asynchronous_client.ks", "123qwe")
    .setCipherSuite(SSLOptions.CipherSuite.TLS);

// The Application context is required to load
// the keystore from the assets
builder.setSSLOptions(sslBuilder.build(getApplication()));

HTTPRequest request = builder.build();

PacketAsyncHTTPClient client = new PacketAsyncHTTPClient();
client.execute(request, textResponseHandler);
```
```

如果对等体之间的 SSL 握手成功完成，服务器将验证我们的身份，而我们的客户端将验证服务器的身份。结果，两个实体之间建立了一个加密通道，使数据对第三方入侵者保密。

## 摘要

本章详细探讨了 HttpURLConnection Android HTTP 客户端，并构建了一个基本且可扩展的异步客户端，以便与 HTTP Web API 交互。

在第一节中，我们阐述了 HttpURLConnection 客户端与预 Marshmallow SDK 上可用的已弃用 Apache HTTP 客户端之间的主要区别。

接下来，我们为异步客户端编写了核心类和回调接口，并扩展了高级客户端以与 JSON 和 Web API 交互。此外，我们构建了用于将 Java 模型转换为 JSON 或 XML 文档的代码。

后来，我们学习了如何配置 HTTP 超时，并设置能够使用我们自己的签名证书、密钥和 CA 的安全通信。在我们的示例中，我们创建并准备了一个 SSL 上下文，用于基于准备好的 Java 安全密钥库建立安全通道。

在下一章中，我们将学习如何使用 Google GCM 从服务器高效推送和拉取实时消息，以及如何使用 Google Play Services 框架安排工作。
