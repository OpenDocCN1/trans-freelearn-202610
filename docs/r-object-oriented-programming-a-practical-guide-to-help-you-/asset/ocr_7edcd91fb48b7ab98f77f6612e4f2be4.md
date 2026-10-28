# Basic socket operations

Aside from the higher level option given in the previous section, there are also more primitive commands for creating and using a socket. The commands examined are the `make.socket`, `read.socket`, `write.socket`, and the `close.socket` commands. Sockets are scarce resources, and checks need to be put in place to ensure that they are released when something goes wrong. For that reason, a socket is usually created within a function with extra checks. The example here is basic, and it is provided simply to demonstrate the commands. You should see the help pages for the socket commands for a more comprehensive example.

To replicate the preceding example, the socket is opened, and the HTTP request is submitted:

```
```r
> socketRead <- make.socket("waterdata.usgs.gov",80)
> write.socket(socketRead,"GET /ny/nwis/dv?cb_00060=on&format=rdb&site_no=04267500&referred_module=sw&period=&begin_date=2013-05-08&end_date=2014-05-08 HTTP/1.1\n");
> write.socket(socketRead,"Host: waterdata.usgs.gov\n\n\n");
> incoming <- read.socket(socketRead);
> close.socket(socketRead)
[1] FALSE
```
```

The incoming variable will now contain the result of the operation.