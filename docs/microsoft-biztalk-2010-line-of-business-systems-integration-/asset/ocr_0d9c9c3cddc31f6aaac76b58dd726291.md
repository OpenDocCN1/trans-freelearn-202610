![](bbox=[0.35, 0.15, 0.65, 0.44])

6. Within this **Message Assignment** shape, we are going to leverage a Helper method that exists in our Helper C# Project called Chapter8ManipulatingLists.Utility.

```
//We want a typed response document so that we can map to
//the flat file message for our ERP
msgGetListItemsResponseHelper = Chapter8ManipulatingLists.Utility.toXML(msgGetListItemsResponse);
```

7. The toXML method will simply accept the Get List Items Response message from SharePoint, update the Target Namespace so that it matches our msgGetListItemsResponseHelper Target Namespace, and return the message back to BizTalk so that it can be cast against the msgGetListItemsResponseHelper's schema. The Utility class is fairly straightforward with one method, toXML:

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Xml;

//needed to receive message from BizTalk
using Microsoft.XLANGs.BaseTypes;
namespace Chapter8ManipulatingLists
{
```