```
GET /poker-client/broadcast.htm HTTP/1.1
Accept: image/gif, image/jpeg, image/pjpeg, image/pjpeg, application/x-shockwave-flash, application/x-ms-application, application/x-ms-xbap, application/vnd.ms-xpsdocument, application/xhtml+xml, application/vnd.ms-excel, application/vnd.ms-powerpoint, application/msword, */*
Referer: http://www.888poker.com/poker-client/promotions.htm
Accept-Language: en-us
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; GTB7.1; Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1) ); .NET CLR 1.1.4322; .NET CLR 2.0.50727; OfficeLiveConnector.1.3; OfficeLivePatch.0.0; .NET CLR 3.0.4506.2152; .NET CLR 3.5.30729; InfoPath1)
Host: www.888poker.com
HTTP/1.1 404 Not Found
Date: Sun, 16 Oct 2011 09:11:58 GMT
Server: Microsoft-IIS/6.0
srv: 2344432
```

3. You can see the following conditions:

+   - I tried to browse the URI /poker-client/broadcast.htm (marked as 1 and 3 in the preceding screenshot)
+   - The URI was forwarded by the referrer:
+     http://www.888poker.com/poker-client/promotions.htm (marked as 2 in the preceding screenshot)
+   - The status code was 404 Not Found (marked as 4 in the preceding screenshot)

Just to clarify things, I was not playing Poker, I was working on a networking problem.

# Server errors

| Code | Status | Explanation | What to do |
|------|--------|-------------|------------|
| 500 | Internal server error | The web server encountered an unexpected condition that prevented it from carrying out the client request for access to the requested URL. | Response that is usually caused by a problem in your Perl code when a CGI program is run. |
| 501 | Not implemented | The request cannot be executed by the server. | A server problem. |
| 502 | Bad gateway | The server you're trying to reach is sending back errors. | A server problem. |
| 503 | Service unavailable | The service or file that is being requested is not currently available. | A server problem. |