# Building SharePoint Web Parts

For a variety of reasons, including closer integration, more specific requirements, and performance or complexity issues, you may need to create a Web Part from the ground up. There are several avenues a developer can take during the early stages in the design of a Web Part:

+   - Configure the out-of-the-box WSS/SPS lists and Web Parts to provide the required method of data storage, data input, and presentation
+   - Develop the Web Part from scratch as an ASP.NET Server Control
+   - Develop the business logic as a Web Service and use the XML Web Part or the DataView Web Part as the GUI
+   - Develop a separate web form and use an IFrame to integrate the portal and your satellite application. Browser compatibility should be considered when using this approach
+   - Design and develop your Web Part as a .NET User Control and leverage generic Web Parts such as 'SmartPart' (http://www.smartpart.info) to host your .NET user control

Each of these approaches has advantages and disadvantages that should be weighed up at design time such as ease of development, extensibility, scalability, interoperability, deployment, and security. The following table sums these up:

| Method | Existing Web Parts | Using Web service as business layer with XML Web Part / DataView Web Part | User Control integrated with Smart Part | ASP.NET Server Control | IFrame to ASP.NET Web Form |
|---|---|---|---|---|---|
| Ease of Development | No development required | Simple | Requires knowledge of SmartPart but is simply ASP.NET User Control development | Complex | Simple |
| Extensibility | Depends on the functionality provided by the specific Web Part | Developer has limited control over the UI but gains FrontPage 2003 as a development tool and a separate ASP.NET business layer. | Very Extensible | Very Extensible | Very Extensible |
| Scalability | Scaled with the WSS/SPS server. | End point Web service must be scaled as necessary. | Scaled with the WSS/SPS server | Scaled with the WSS/SPS server | The framed ASP.NET application must be scaled as necessary. |