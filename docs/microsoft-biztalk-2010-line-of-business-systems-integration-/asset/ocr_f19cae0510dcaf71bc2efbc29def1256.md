2. BizTalk should pick this file up, transform it into the format that InfoPath is expecting, and send it to SharePoint. During this process, BizTalk should also populate SharePoint columns using data from the message's payload.

![](bbox=[0.22, 0.205, 0.78, 0.32])

3. Notice that in the Type column, it contains an InfoPath icon. What this means is that SharePoint has determined that the data within this document, is a match for an InfoPath template that can be found within this Forms Library. What makes this demonstration interesting is that nowhere in BizTalk have we specified an InfoPath template name. In fact BizTalk has no idea that this data matches a template as we did not have to provide any Processing instructions like we had to do in BizTalk 2006. To validate this, turn the Track Message Bodies feature of this Send Port on and look at the content of the message. Nowhere will you find any indication that this data belongs to an InfoPath Template.

![](bbox=[0.24, 0.51, 0.76, 0.62])