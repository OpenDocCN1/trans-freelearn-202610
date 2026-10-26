|  |  |  |  |  |
|---|---|---|---|---|
| **Interoperability** | Can easily interact with other parts of WSS/SPS | Interacting with WSS/SPS objects is more complex but still possible. | Can easily interact with other parts of WSS/SPS | Can easily interact with other parts of WSS/SPS | Interacting with WSS/SPS objects is more complex but still possible. |
| **Deployment** | No deployment required for out-of-the-box Web Parts and very simple deployment for downloadable Web Parts | No deployment required | Installation of the Smart Part Web Part is required. | Complex | Simple |
| **Security** | No security impact | No SPS/WSS security impact as security will be enforced by the ASP.NET Web service | No security impact | Security is enforced by SPS/WSS. | Enforced by the ASP.NET Web application |
| **Notes** | Requires the least amount of development knowledge | Great for .NET developers that need to display data external to WSS/SPS in a Web Part Page | Allows you to run .NET User Controls as Web Parts. Allows development of a rich UI within the Web Part Framework. | Provides the most flexibility during development but requires manual definition of the UI. Is executed within the Web Part Framework. | Allows a separate ASP.NET Web application to be used via SharePoint |

Throughout this chapter, we will focus on the ASP.NET Server Control method.

# The SharePoint MCMS Navigation Control

During the course of this chapter, we will develop a Web Part as a server control by building a Web Part that acts as an MCMS navigation component. The server control methodology has been selected as it is the most flexible method. End users with WSS/SPS Web Designer rights will be able to add the Web Part to a Web Part Page and configure the Web Part by specifying the MCMS Channel. The items in the configured MCMS Channel will be rendered in the Web Part using an XSLT (Extensible Stylesheet Language Transformations) stylesheet to produce the following display: