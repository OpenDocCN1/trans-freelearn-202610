
```
<security relayClientAuthenticationType="RelayAccessToken" />
</binding>
</webHttpRelayBinding>
</bindings>
```

10. 定义一个行为，指定本地服务的连接详细信息以及用于 RESTful 服务的 webHttp 行为。这些凭据在服务尝试在云地址上开始侦听时发送到 Service Bus。

```
<endpointBehaviors>
    <behavior name="CloudBehavior">
        <transportClientEndpointBehavior>
            <clientCredentials>
                <sharedSecret issuerName="owner" issuerSecret="your secret" />
            </clientCredentials>
        </transportClientEndpointBehavior>
        <webHttp />
    </behavior>
</endpointBehaviors>
```

11. 最后，我们通过定义服务和端点将所有内容连接起来。引用上面创建的绑定和行为。

```
<services>
<service name="DiscountService">
<endpoint
address="https://<your namespace>.servicebus.windows.net/DiscountService"
behaviorConfiguration="CloudBehavior" binding="webHttpRelayBinding"
bindingConfiguration="WebRestBinding"
name="CloudRestEndpoint"
contract="IDiscountService" />
</service>
</services>
```

12. 将服务部署到 IIS Web 服务器并启动服务。确保可以通过浏览到您的云服务端点（在我这里是 https://richardseroter.servicebus.windows.net/DiscountService/12345/Discount）成功连接，您应该会收到一条消息，提示授权标头丢失。我们的服务需要客户端身份验证，而浏览器不会传递有效的 Service Bus 凭据，因此此消息是预期的。

## 构建一个消费云服务的 Visualforce 页面

在本步骤中，我们构建一个 Visualforce 页面和 Apex 控制器。控制器负责获取访问控制令牌，然后将该令牌传递到 Service Bus 调用中。Visualforce 页面包含显示 Account 详细信息和 Service Bus 结果所需的标记：

- 1. 我们的本地和云连接环境已准备好供使用。前往 Salesforce.com 并进入设置屏幕。

![](img/23d1402649b6528b085cd6b900a2c9c3_513_0.png)

- 2. 为了使 Apex 代码与 Azure 之间的调用成功，我们必须显式允许对 Service Bus 端点的出站调用。具体来说，我们必须将 Azure 访问控制和 Service Bus URL 注册到我们的 Salesforce.com 账户。在管理设置下，导航到安全控制，并单击远程站点设置。

![](img/23d1402649b6528b085cd6b900a2c9c3_513_1.png)

3. 单击 **新建远程站点** 按钮并提供名称 **AzureAC**，将 **Url** 设置为 `https://<your namespace>-sb.accesscontrol.windows.net`，并确保 **Active** 标志已选中。

4. 再添加一个名为 **AzureSB** 的远程站点，并将 **Url** 设置为 `https://<your namespace>.servicebus.windows.net`，并将站点设置为 **Active**。

![](img/23d1402649b6528b085cd6b900a2c9c3_514_0.png)

5. 我们此处的目标是将从 BizTalk Server 查询到的折扣详细信息添加到账户对象中。因此，让我们扩展该对象以包含保存所需值的字段。在设置屏幕中，导航到 **App Setup**，然后 **Customize**，找到 **Accounts**。单击 **Fields** 链接。添加一个名为 **DiscountPercentage** 的新字段，使用 **Percent** 数据类型，**Length** 为十六且最多保留两位小数。接下来，添加一个名为 **DiscountLastUpdated** 的字段，其数据类型为 **Text**，长度为 50 个字符。最后，创建一个名为 **DiscountBestPrice** 的字段，使用 **Checkbox** 数据类型。

6. 通过查看 Salesforce.com 站点中的任何账户并找到新字段，确认我们的字段已成功添加。

![](img/23d1402649b6528b085cd6b900a2c9c3_515_0.png)

7. 我们现在准备构建一个自定义 VisualForce 页面，显示账户详细信息并允许我们通过云查询 BizTalk Server 服务。Salesforce.com 使用模型-视图-控制器（MVC）模式，我们的本地服务由控制器调用。需要构建的第一件事是自定义页面使用的控制器。导航到 App Setup，然后 Develop，选择 Apex Classes。单击 New 按钮构建新的 Apex 类。您看到的下一个屏幕是基于 Web 的 Apex IDE。

![](img/23d1402649b6528b085cd6b900a2c9c3_515_1.png)

8. 在 Visualforce 环境中，我们可以选择控制器类型。标准控制器与开箱即用的 Salesforce.com 对象（如 Accounts）相关联。它们公开像 Save 这样的操作，可保存对底层对象的更改。自定义控制器是不使用标准控制器并指定全新行为的控制器。最后，控制器扩展扩展标准或自定义控制器。我们想要使用控制器扩展，以便在添加调用云的新功能的同时获得与 Account 控制器关联的现有操作和数据。通过定义指向活动账户的本地引用变量并在默认构造函数中实例化它来开始控制器类。

```
public class accountDiscountExtension{

private final Account myAcct;

public accountDiscountExtension(ApexPages.StandardController controller) {
    this.myAcct = (Account)controller.getRecord();
}

}
```

9. 定义将调用 Azure AppFabric 服务的操作。添加一个执行 HTTP 通信的 Http 对象变量。然后，为访问控制（AC）服务和 Service Bus 服务添加 HTTP 请求对象。

```
public void GetDiscountDetails()
{
    Http httpProxy = new Http();
    HttpRequest acReq = new HttpRequest();
    HttpRequest sbReq = new HttpRequest();
}
```

10. 创建保存 AC URL 以及编码密码的变量。

// ** Getting Security Token from STS
String acUrl =
'https://<your namespace>-sb.accesscontrol.windows.net/WRAPv0.9/';
String encodedPW = EncodingUtil.urlEncode('<issuer password>',
'UTF-8');

11. 设置 HTTP 请求以向 AC 服务执行 POST。请注意，我们手动构建负载，其中包含发行者名称、密码和请求的范围。请记住第 6 章中我们可以将发行者与范围关联到更细粒度的级别；实际上，您将为 Salesforce.com 定义一个发行者，并仅将其分配给其有权访问的范围。在本例中，我们将利用账户所有者凭据。

```
acReq.setEndpoint(acUrl);
acReq.setMethod('POST');
acReq.setBody('wrap_name=owner&wrap_password=' + encodedPW +
'&wrap_scope=http://<your namespace>.servicebus.windows.net/');
acReq.setHeader('Content-Type','application/x-www-form-urlencoded');
```

12. 我们现在准备调用 AC 服务。服务调用的结果被放入字符串变量以供后期处理。

```
HttpResponse acRes = httpProxy.send(acReq);
String acResult = acRes.getBody();
```

13. 我们从 AC 服务收到的令牌与 Service Bus 所需的格式并不完全匹配。来自 AC 服务的原始响应如下所示：

```
wrap_access_token=net.windows.servicebus.action%3dListen%252cSend%252cManage%26Issuer%3dhttps%253a%252f%252frichardseroter-sb.accesscontrol.windows.net%252f%26Audience%3dhttp%253a%252f%252frichardseroter.windows.net%26ExpiresOn%3d1290807349%26HMACSHA256%3dEcvt eRJ9T1hDo9gwWT5MWwfhOL OOR eoeCKP9esor p7JA%253d&wrap_access_token_expires_in=1200
```

14. SB 不需要全部内容，因此我们需要解析出几部分。首先，删除与号（&）之后的部分（wrap_access_token_expires_in），然后提取开头部分（wrap_access_token=），最后进行 URL 解码。我们的令牌需要 WRAP access_token= 前缀，以便 Service Bus 知道提供了何种类型的身份验证。

// clean up result
    String suffixRemoved = acResult.split('&')[0];
    String prefixRemoved = suffixRemoved.split('=')[1];
    String decodedToken = EncodingUtil.urlDecode(prefixRemoved, 'UTF-8');
    String finalToken = 'WRAP access_token="' + decodedToken + '"';

15. 手持可供 Service Bus 使用的令牌后，我们可以调用 Service Bus 端点并调用我们的本地 BizTalk 服务（通过 REST 代理）。由于我们拥有 REST 服务，HTTP 请求中没有需要传递的“正文”，因为所有详细信息都在 URL 中。我们使用客户的账户号码构建 REST URL，并向 HTTP 请求添加授权标头。

```
String sbUrl = 'https://<your namespace>.servicebus.windows.net/DiscountService/' + myAcct.AccountNumber + '/Discount';
sbReq.setEndpoint(sbUrl);
sbReq.setMethod('GET');
sbReq.setHeader('Content-Type', 'text/xml');
sbReq.setHeader('Authorization', finalToken);
```
