
``` 
// perform validation only in authoring mode.
if (WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringNew
    || WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringReedit)
{
    writer.AddAttribute("evaluationfunction", "validate"
        + this.ControlToValidate);
}
}
```

我们希望当要验证的控件是 SingleImagePlaceholderControl 时，`ControlPropertiesValid()` 方法返回 true。请在 `AddAttributesToRenderer()` 方法下方添加以下代码：

``` 
protected override bool ControlPropertiesValid()
{
    Control ctrl = FindControl(ControlToValidate);
    if (ctrl != null)
    {
        return (ctrl is SingleImagePlaceholderControl);
    }
    else
    {
        return false;
    }
}
```

最后，我们实现 `EvaluateIsValid()` 方法。虽然我们已经使用客户端脚本实现了完全相同的检查，但始终建议在服务器端重复检查。这里，我们检查 `SingleImagePlaceholderControl` 的 `ImageUrl` 属性。仅当图片已上传到占位符控件时，验证才会通过。

``` 
protected override bool EvaluateIsValid()
{
    bool valid = true;
    if (CmsHttpContext.Current.Mode == PublishingMode.Update)
    {
        SingleImagePlaceholderControl ctrl;
        ctrl = FindControl(ControlToValidate) as SingleImagePlaceholderControl;
        if (ctrl.ImageUrl == "")
        {
            valid = false;
        }
    }
    return valid;
}
```

控件已完成。保存并生成项目。

### 验证 SingleAttachmentPlaceholderControl

SingleAttachmentPlaceholderControl 与其他占位符控件一样，无法使用默认的 ASP.NET 服务器端验证控件进行验证。在本节中，我们将构建一个名为 `RequiredAttachmentPHValidator` 的自定义验证控件，用于检查文件是否已上传到该控件。如果没有上传任何附件，页面将无法保存。

``` 
else
{
    return true;
}
</script>
```

#### RequiredAttachmentPHValidator

我们已经构建了两个自定义验证器，这第三个遵循熟悉的模式。我们将直接深入代码，并只强调有趣的部分。

将名为 `RequiredAttachmentPHValidator.cs` 的类文件添加到 MCMSValidators 项目中。

如下所示编写代码：

``` 
using System;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.ComponentModel;
using System.Text;
using Microsoft.ContentManagement.WebControls;
using Microsoft.ContentManagement.Publishing;
namespace MCMSValidators
{
    public class RequiredAttachmentPHValidator : BaseValidator
    {
        public RequiredAttachmentPHValidator()
        {
            // nothing to do
        }

        // register the client-side validation script
        protected override void OnPreRender(System.EventArgs e)
        {
            base.OnPreRender(e);

            // Add the script only in authoring mode.
            if (WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringNew
                || WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringReedit)
            {
                string phName = "NCPH_";
                if ((FindControl(this.ControlToValidate) == null)
                    || (!(FindControl(this.ControlToValidate)
                        is SingleAttachmentPlaceholderControl)))
                    return;

                phName +=
                    ((SingleAttachmentPlaceholderControl)
                        FindControl(this.ControlToValidate)).BoundPlaceholder.Name;

                StringBuilder sb = new StringBuilder();
                sb.Append("<script language=\"javascript\">");
                sb.Append("function validate" + this.ControlToValidate + "()");
                sb.Append("{");
                sb.Append("    var fileName = document.all." + phName + "_value; ");
                sb.Append("    if (fileName == '')");
                sb.Append("    {");
                sb.Append("        return false;");
                sb.Append("    }");
                sb.Append("    else");
                sb.Append("    {");
                sb.Append("        return true;");
                sb.Append("    }");
                sb.Append("}");
                sb.Append("</script>");

                if (this.RenderUplevel && this.EnableClientScript)
```

仔细检查代码，你会发现它与前两个自定义验证控件非常相似。

-   用于检查文件是否已上传到 SingleAttachmentPlaceholderControl 的 JavaScript 在 `OnPreRender()` 方法中被注入到模板文件中。
-   `ControlPropertiesValid()` 方法验证要验证的控件确实是 SingleAttachmentPlaceholderControl。
-   最后，`EvaluateIsValid()` 方法执行服务器端检查，以查看 `SingleAttachmentPlaceholderControl.AttachmentUrl` 属性是否包含文件的路径。如果没有，该方法返回 `false`。与前几个示例一样，此方法可能不会被触发，因为我们依赖客户端检查进行验证。如果需要进行服务器端验证，则必须按照“实现服务器端验证”一节中的描述创建自定义占位符控件。

### 总结

在本章中，我们探讨了如何验证 MCMS 占位符控件。我们首先说明了 ASP.NET 附带的验证 Web 控件不能用于 MCMS 占位符控件。为了克服这一限制，我们构建了三个验证控件：一个用于 HTMLPlaceholderControl，第二个用于 SingleImagePlaceholderControl，第三个用于 SingleAttachmentPlaceholderControl。我们演示了如何使用客户端脚本技术执行简单的空占位符检查。我们还说明了通过构建自定义占位符控件，服务器端验证是可行的。本章讨论的方法可以扩展，以满足更复杂和更严格的验证要求，从检查电话号码、邮政编码或电子邮件地址是否为有效格式，到根据外部数据库中存储的内容进行验证。

## 10 暂存静态页面

如今的网站动态且功能多样。动态站点相比静态站点具有许多优势。使用 MCMS，您可以个性化并配合 PAPI 工作，以创建真正交互式、个性化的用户体验。例如，我们已经看到了如何根据个人访问权限隐藏或显示导航结构。事实上，内容更新如此频繁，交互需求如此之高，以至于像 MCMS 这样的内容管理系统会即时组装页面。最多也只是将页面的快照缓存在内存或服务器硬盘上，以加速页面交付过程。缓存会在经过一定时间段后或内容发生变化时失效。没有任何东西会永远冻结：只会提供新鲜内容。

在一本关于创建动态站点的书中，你可能会觉得奇怪，这一整章都在讲述暂存静态页面。事实是，尽管技术需求日新月异，有时也需要稍微放慢脚步。整个网站的静态副本在以下情况下可能很有用：

-   创建页面以打包到 CD ROM 等离线媒体上。用户指南和帮助文件通常以电子文档的形式包含在盒装软件的 CD 中。甚至 Microsoft 帮助文件（或 CHM 文件）本身也是由静态 HTML 文件构建的。
-   为存档或备份目的拍摄网站快照。
-   创建静态文件，供 Index Server 或其他需要物理文件存在的搜索引擎建立索引。
-   在多台服务器之间共享内容。是的，您可以在其他服务器上安装 MCMS，甚至可以使用 Web 服务来联合内容。然而，有时最简单的解决方案就是最好的——只需制作共享内容的静态副本，并使用良好的文件复制技术（如 FTP）来传输它们（从而避免获取更多生产 MCMS 许可证的需要）。

如果需要生成 MCMS 网站的静态版本，有两种可用选项：

-   使用 MCMS 2002 附带的 Site Stager 实用程序。
-   编写一个自定义暂存工具，为每个页面生成静态快照。

> **重要说明：** Site Stager 仅基于 ASP 模板暂存来自发布的静态页面。它不适用于基于 ASP.NET 模板的发布。
