

## 解决方案

现在，让我们来看看如何着手实现这些新功能。

### 回顾解决方案结构

在深入细节之前，让我们快速回顾一下解决方案的结构。由于到本书结尾时，这个站点将变得相当庞大，并涵盖所有功能，因此有必要进行一定的模块化。同时，为了正确遵循我们之前讨论的应用分层概念，提前以正确的方式设置事物非常重要。我们的解决方案结构如下所示：

![](img/15a62e134135dde036b0ac3cbb01dab6_135_0.png)

各个项目都有特定的用途。让我们来看看这些项目：

- **业务逻辑：**所有与业务相关的逻辑、规则等都将放在此项目中。
- **通用：**这是任何层都可以使用的通用功能。
- **组件：**这些也是通用组件，但可以根据需要被不同的实现所替换。本质上，这些组件实现了 MEF。
- **数据访问：**该层是对实际数据访问的封装，包含各种使用实体框架生成的实体来操作数据库的仓储。
- **接口：**这些是组件项目中各个类所实现的接口。
- **Web：**这是表示层，包含所有 ASPX 页面及相关逻辑。

### 实现数据库

我们将从实现数据库开始，然后自底向上逐步推进。下面的截图显示了本章中我们将使用的主要表。

![](img/15a62e134135dde036b0ac3cbb01dab6_136_0.png)

### Profiles 表

在真正组装一个界面之前，我们需要讨论一下个人资料（profile）的数据将如何存储。参见上一张截图中的 profiles 表。让我们讨论一些不太常见的字段。这里我们指的是使用 **varbinary(MAX)** 字段的 **头像**存储。有了 MAX 大小限制（其实也算不上什么限制！），一个人理论上可以把一张 DVD 的内容存放到这个字段中。这当然是很荒唐的，所以我们需要确保在前端设置一些限制，以便将文件存储大小保持在最小。稍后您会看到，这个字段本质上是一个字符数组！

### 经验水平

一旦我们的基础个人资料容器就绪，我们就可以创建一些周围的表了。其中最简单的是经验水平系统。随着站点的其他功能逐步构建，该系统也会被用到。最初，用户可以进入并设置其个人资料，并声明一种经验水平。然后，当用户在论坛中添加帖子、创建新的博客文章，或以任何其他方式与我们的社区互动时，我们可以动态调整用户的经验水平。这可以基于用户获得的勋章，我们将在第 11 章中讨论这一点。

### Attributes 表

个人资料系统中另一个最简单的部分就是动态属性系统。回忆一下，这将允许站点管理员轻松扩展为每个用户收集的数据。有两张表——ProfileAttribute 和 ProfileAttributeType。第一张表用于用户输入的属性，第二张表用于站点中使用的各种属性。

### Privacy 表

现在我们可以讨论隐私系统了。这本质上是一个一对多关系系统，在关系的子端有一些配置/查找表。我们有 PrivacyFlag 用于存储用户创建的值。然后我们有 PrivacyFlagType 来定义该标志所保护的内容。最后，我们有 VisibilityLevel，它定义了谁可以看到受保护的数据。

### Alerts 表

接下来，我们来看看支持个人资料页面上新闻源或“提醒”（Alerts）概念的表。这包括 Alert 表和 AlertType 表。

这里一个值得讨论的有趣字段是 IsHidden 字段。稍后我们将使用它，让用户可以从其新闻源中隐藏某条提醒。我们也可以使用它，让用户根据提醒类型隐藏整组提醒。

在业务层包含大量业务逻辑的应用程序中，可以创建独立的业务实体，并在实体之间进行转换（例如，从 DataAccess 实体转换为业务实体，反之亦然）。类似地，如果表示层要求实体具有特定于表示层的额外属性，则可以创建表示实体。

让我们通过在 DataAccess 项目中创建一个新的 Profile 分部类来扩展我们的 Profile 对象。在这个类中，让我们添加一个属性来存储个人资料属性的列表，以及一个属性来保存个人资料的经验水平。我们不需要相应的标量属性，因为这些属性是可用的，因此我们无需担心在这个分部类中添加它们。Profile 分部类看起来如下所示：

```csharp
//Fisharoo/DataAccess/Profile.cs
namespace Fisharoo.DataAccess
{
    public partial class Profile
    {
        public List<ProfileAttribute> Attributes { get; set; }
        public LevelOfExperienceType LevelOfExperienceType { get; set; }

        public Profile()
        {
            Attributes = new List<ProfileAttribute>();
        }
    }
}
```

类似地，我们将为 AlertType、PrivacyFlagType、ProfileAttribute 和 VisibilityLevel 创建分部实体类。有关这些类的详细信息，请参阅第 4 章提供的代码。

### 构建仓储

随着新表的添加，将会有新的仓储来访问存储在表中的数据。我们将创建几个仓储来支持我们的个人资料需求，即 ProfileRepository、ProfileAttributeRepository、PrivacyRepository、LevelOfExperienceTypeRepository 和 AlertRepository。

我们的每个仓储都会有一个方法用于按 ID 选择、按父 ID 选择、保存和删除。一旦你见过一个仓储，基本上就相当于见过所有仓储了。让我们来看一下 ProfileRepository.cs：

```csharp
//Fisharoo/DataAccess/Repositories/ProfileRepository.cs
namespace Fisharoo.DataAccess.Repositories
{
    [Export(typeof(IProfileRepository))]
    public class ProfileRepository : IProfileRepository
    {
        private Connection conn;
        public ProfileRepository()
        {
            conn = new Connection();
        }

        public Profile GetProfileByAccountID(int AccountID)
        {
            Profile profile;

            using (FisharooDataContext dc = conn.GetContext())
            {
                profile = (from p in dc.Profiles.Include(
                    "ProfileAttributes.ProfileAttributeType")
                    .Include("LevelOfExperienceType")
                    where p.AccountID == AccountID
                    select p).FirstOrDefault();
            }
            if (profile != null)
            {
                foreach (ProfileAttribute item in profile.ProfileAttributes)
                {
                    profile.Attributes.Add(item);
                    item.profileAttributeType = item.ProfileAttributeType;
                }
                profile.LevelOfExperienceType = profile.LevelOfExperienceType;
            }
            return profile;
        }

        public Int32 SaveProfile(Profile profile)
        {
            Int32 profileID = 0;
            Boolean HasAttributeId = false;
            profile.LastUpdateDate = DateTime.Now;
            using (FisharooDataContext dc = conn.GetContext())
            {
                if (profile.ProfileID > 0)
                {
                    dc.Profiles.Attach(new Profile { ProfileID = profile.ProfileID });
                    dc.Profiles.ApplyCurrentValues(profile);
                    if (profile.Attributes != null)
                    {
                        foreach (ProfileAttribute item in profile.Attributes)
                        {
                            dc.ProfileAttributes.Attach(
                                new ProfileAttribute
                                {
                                    ProfileAttributeID = item.ProfileAttributeID,
                                });
                            dc.ProfileAttributes.ApplyCurrentValues(item);
                        }
                    }
                }
                else
                {
                    profile.CreateDate = DateTime.Now;
                    //when working with new profile we don't expect attributes to be null, but check anyway
                    if (profile.Attributes != null)
                    {
                        foreach (ProfileAttribute item in profile.Attributes)
                        {
                            dc.ProfileAttributes.AddObject(new ProfileAttribute
                            {
                                ProfileAttributeID = item.ProfileAttributeID,
                                ProfileAttributeTypeID = item.ProfileAttributeTypeID,
                                ProfileID = item.ProfileID,
```

