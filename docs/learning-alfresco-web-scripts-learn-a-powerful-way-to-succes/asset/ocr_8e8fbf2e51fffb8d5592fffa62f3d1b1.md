# Understanding JavaScript-based Web Scripts in Detail

The root objects from `DeclarativeWebScript` are shown in the following table:

| Root object | Name of the class/object | Description |
|---|---|---|
| model | This is a map object | This will be used by the controller to pass the data to the response template |
| status | `org.springframework.extensions.webscripts.Status` | This represents the response status |
| cache | `org.springframework.extensions.webscripts.Cache` | This allows the control to cache the response of the web script |

The root objects from `TenantRepositoryContainer` are shown in the following table:

| Root object | Name of the class/object | Description |
|---|---|---|
| cmis | `org.alfresco.repo.cmis.client.CMISLocalConnectionManagerImpl` | This represents CMIS client |
| paging | `org.alfresco.repo.web.util.paging.Paging` | This is a paging API |

The root objects from `RepositoryContainer` are as follows:

| Root object | Name of the class/object | Description |
|---|---|---|
| roothome | `org.alfresco.repo.jscript.ScriptNode` | This represents the repository root node. Accessible to authenticated users only. |
| companyhome | `org.alfresco.repo.jscript.ScriptNode` | This represents the company home object. Accessible to authenticated users only. |
| person | `org.alfresco.repo.jscript.ScriptNode` | This represents the currently authenticated user. |
| userhome | `org.alfresco.repo.jscript.ScriptNode` | This represents the current user's home space. |