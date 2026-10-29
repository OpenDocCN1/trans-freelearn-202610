```
; ;=> {:name "makoto-bucket-1"}
(s3/create-bucket "makoto-bucket-2")
; ;=> {:name "makoto-bucket-2"}
(s3/create-bucket "makoto-bucket-3")
; ;=> {:name "makoto-bucket-3"}
```

s3/list-buckets returns bucket information:

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

We can see that there are three buckets in your AWS console, as shown in the following screenshot: