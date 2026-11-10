
## 16.3 文件夹映射示例

本节包含示例，解释如何在 Documentum 中实际实现文件夹映射。

### 16.3.1 属性匹配：使用通配符 (*)

让我们从一个简单的示例开始，其中文件夹映射配置为：仅当用户在创建内容时为其 subject 属性提供非空白值时，内容文件才会被放置在 Web Cabinet 中的指定位置。

1. 以管理员身份登录 Web Publisher，并在 Site Manager | Configurations | Foldermaps 下检出 FolderMap.xml 文件。按如下所示修改 FolderMap.xml 文件，然后将其签回 Docbase。

```
<folder_map>
    <rule>
        <attr_list>
            <attr>
                <name>subject</name>
                <value>*</value>
            </attr>
        </attr_list>
        <path_list>
            <path>/Test_webCabinet/Subject_Content</path>
        </path_list>
    </rule>
    <rule>
        <attr_list>
            <attr>
                <name></name>
                <value></value>
            </attr>
        </attr_list>
        <path_list>
            <path></path>
        </path_list>
    </rule>
</folder_map>
```

尝试使用模板创建一些内容并将 Subject: 字段留空。会抛出以下错误；可在 Web Publisher 状态栏中点击 All Messages 按钮查看：

新内容无法创建。根据文件夹映射，此文件无法链接到任何 Web 柜。您的文件将不会被创建。

## 文件夹映射

2. 通过为 Subject: 字段提供非空白值来创建新内容，如图 16.4 所示。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_251_0.png)

图 16.4：为 Subject 属性提供非空白值

3. 请注意，新内容已成功创建（参见图 16.5），位于 Web Cabinet 中指定的 Docbase 位置 /Test_WebCabinet/Subject_Content 下。

> 确保 Web Cabinet Test_WebCabinet 已存在于 Docbase 中。如果不存在，请在开始创建内容并尝试文件夹映射示例之前先创建它。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_252_0.png)

图 16.5：在 Web Cabinet 文件夹中成功创建内容

### 16.3.2 属性匹配：在 <attr_list> 中使用多个属性

现在让我们举一个示例，其中文件夹映射配置为：仅当内容满足以下两个属性条件时，内容文件才会被放置在 Web Cabinet 中的指定位置：

它应具有非空白的 Name: 字段（object_name 属性），并且其 Subject: 字段（subject 属性）的值应为 office。在同时满足这两个条件之前，内容文件不会被创建。

1. 以管理员身份登录 Web Publisher，并在 Site Manager | Configurations | Foldermaps 下检出 FolderMap.xml 文件。按如下所示修改 FolderMap.xml 文件，然后将其签回 Docbase。

```
<folder_map>
    <rule>
        <attr_list>
            <attr>
                <name>object_name</name>
                <value></value>
            </attr>
            <attr>
                <name>subject</name>
                <value>office</value>
            </attr>
        </attr_list>
        <path_list>
            <path>/Test_WebCabinet/official docs</path>
        </path_list>
    </rule>
    <rule>
        <attr_list>
            <attr>
                <name></name>
                <value></value>
            </attr>
        </attr_list>
        <path_list>
            <path></path>
        </path_list>
    </rule>
</folder_map>
```

尝试使用模板创建内容并执行以下任一操作：

- 将 Name: 字段留空
- 将 Subject: 字段留空
- 将 Subject: 字段的值写为 OFFICE
- 将 Subject: 字段的值提供为 office 以外的任何内容，例如 JUNK

在所有四种情况下，都会抛出以下错误（可在 Web Publisher 状态栏中查看 All Messages 按钮），并且新内容不会在 Web Publisher 中创建：

新内容无法创建。根据文件夹映射，此文件无法链接到任何 Web 柜。您的文件将不会被创建。

2. 现在让我们提供一个有效场景。创建具有有效非空白名称的新内容，并将其 Subject: 字段写为 office，如图 16.6 所示：

![](img/b18244e6621ecc39cef03ff6c0e55e7c_253_0.png)

图 16.6：在内容创建期间提供了有效的 Name 和 Subject 字段

3. 新内容已成功创建（参见图 16.7），位于 Web Cabinet 中指定的 Docbase 位置：/Test_webCabinet/Official docs。

如前所述，您必须首先确保 Web Cabinet Test_webcabinet 已存在于 Docbase 中。如果不存在，请在开始创建内容之前先创建它。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_254_0.png)

图 16.7：在 Official docs 文件夹下创建的内容

### 16.3.3 使用 <path_list> 将内容文件放置在多个位置

现在让我们举一个示例，其中文件夹映射配置为将同一内容文件放置在 Web Cabinet 中的两个不同指定位置。内容必须满足的一个重要条件是它应具有非空白的文件名（object_name 属性）。

如果满足上述条件，则同一内容文件将被放置在 Web Cabinet 中的两个文件夹下：/Test_WebCabinet/FilePath One 和 /Test_WebCabinet/FilePath Two。

1. 以管理员身份登录 Web Publisher，并在 Site Manager | Configurations | Foldermaps 下检出 FolderMap.xml 文件。按如下所示修改 FolderMap.xml 文件，然后将其签回 Docbase。

```
<folder_map>
    <rule>
        <attr_list>
            <attr>
                <name>object_name</name>
                <value>*</value>
            </attr>
        </attr_list>
        <path_list>
            <path>/Test_webCabinet/FilePath One</path>
            <path>/Test_webCabinet/FilePath Two</path>
        </path_list>
    </rule>
    <rule>
        <attr_list>
            <attr>
                <name></name>
                <value></value>
            </attr>
        </attr_list>
        <path_list>
            <path></path>
        </path_list>
    </rule>
</folder_map>
```

## 文件夹映射

尝试使用模板创建内容并提供非空白的文件名（Name: 字段），如图所示 16.8。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_255_0.png)

图 16.8：在创建新内容时提供文件名

2. 您会注意到新内容已成功在 Web Cabinet 中的两个指定 Docbase 位置下创建（参见图 16.9 和 16.10）：/Test_WebCabinet/FilePath One 和 /Test_WebCabinet/FilePath Two。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_256_0.png)

图 16.9：在第一个 Docbase 位置创建的内容

![](img/b18244e6621ecc39cef03ff6c0e55e7c_256_1.png)

图 16.10：在第二个 Docbase 位置下创建的相同内容

### 16.3.4 属性匹配：简单重复属性

到目前为止，我们处理的是单值属性。现在让我们举一个示例，其中文件夹映射配置为：如果内容满足特定重复属性条件，则内容文件将被放置在 Web Cabinet 中的指定位置，如下所示：

> 它应具有非空白的 keywords 属性，且其第一个关键字为 abc。

如果满足上述条件，则内容文件将被放置在 Web Cabinet 中的以下位置：/Test_WebCabinet/Keywords_ABC。

1. 以管理员身份登录 Web Publisher，并在 Site Manager | Configurations | Foldermaps 下检出 FolderMap.xml 文件。
按如下所示修改 FolderMap.xml 文件，然后将其签回 Docbase。

## 文件夹映射

```
<folder_map>
    <rule>
        <attr_list>
            <attr>
                <name>keywords</name>
                <value>abc</value>
            </attr>
        </attr_list>
        <path_list>
            <path>/Test_WebCabinet/Keywords_ABC</path>
        </path_list>
    </rule>
    <rule>
        <attr_list>
            <attr>
                <name></name>
                <value></value>
            </attr>
        </attr_list>
        <path_list>
            <path></path>
        </path_list>
    </rule>
</folder_map>
```

2. 尝试使用模板创建内容并执行以下任一操作：

- 为 Keywords 值提供 ABC：keywords[0]='ABC'
- 为 Keywords 值提供 xyz：keywords[0]='xyz'
- 将 xyz 和 abc 作为 Keywords 值提供。请注意，这里 abc 是第二个关键字而不是第一个关键字。
  keywords[0]='xyz'
  keywords[1]='abc'

您会注意到以下错误被抛出（通过点击 Web Publisher 状态栏中的 All Messages 按钮查看），并且新内容不会在 Web Publisher 中创建：

新内容无法创建。

根据文件夹映射，此文件无法链接到任何 Web 柜。您的文件将不会被创建。
