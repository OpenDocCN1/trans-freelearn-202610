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

We can see the trace of executing flows in the following URL:
http://localhost:3000/x-liberator/requests/.

You can see the history of requests you have requested in your browser as in the following screenshot: