

# Advanced Microsoft Content
Management Server
Development

使用 Publishing API、占位符、搜索、
Web 服务、RSS 和 SharePoint 集成

Lim Mei Ying
Stefan Goßner
Angus Logan
Andrew Connell

![](img/442e26bf28f2df81fbf8dff3478f88e3_1_0.png)

## Credits

**Authors**

Lim Mei Ying  
Stefan Goßner  
Angus Logan  
Andrew Connell

**Technical Reviewers**

Mick Badran  
Spencer Harbar  
David Mielcarek  
Chester Ragel  
Christopher Walker  
Joel Ward

**Editorial Manager**

Dipali Chittar

**Development Editor**

Douglas Paterson

**Technical Editor**

Richard Deeson

**Indexer**

Niranjan Jahagirdar

**Proofreader**

Chris Smith

**Production Coordinator**

Manjiri Nadkarni

**Cover Designer**

Helen Wood

## Chester Ragel

是一名计算机专业人士，获得莫拉图瓦大学计算机科学与工程学位。他持有 MCSD.NET 资格，喜欢使用 Microsoft 技术。

Chester 为 MCMS 社区和多家 IT 杂志做出贡献。他居住在斯里兰卡，可在 http://chestermr.blogspot.com 上找到他的博客。

## Christopher Walker

是 Microsoft Services 的高级顾问，为集成门户解决方案提供专业建议（SharePoint、MCMS、BizTalk 和 InfoPath）。Christopher 在 Unisys 担任门户开发和技术工作，并曾在门户软件 ISV 任职。

Christopher 通过其博客 http://blogs.msdn.com/cjwalker/ 为社区做出贡献，为使用 Microsoft 技术的客户提供实际解决方案。

Christopher 居住在澳大利亚布里斯班。

## Joel Ward

在 Booz Allen Hamilton 担任技术经理和开发人员。Joel 喜欢从事 ASP.NET、MCMS 和 SharePoint 项目，并帮助在 Booz Allen 创建了一个 .NET 用户组。他在设计、编程和架构方面有背景，自 2001 年 MCMS 首次发布以来就一直使用 MCMS。

作为 Microsoft 最有价值专家，Joel 因其在全球技术社区中的积极参与而受到同行和 Microsoft 的认可。Joel 居住在美国弗吉尼亚州。

## Table of Contents

Adding Details to the Title Tag ........................................ 111

Using META Tags ........................................................ 112

# Summary ................................................................. 112

# Chapter 5: Searching MCMS with SharePoint ............................ 113

# MCMS Search Options ................................................... 113

# Microsoft SharePoint Portal Server Search ............................. 114

# Preparing the MCMS Site for Indexing .................................. 115

Disabling Channel Names to Host Header Names Mapping .................. 115

Assigning a Search Account ............................................ 116

Enable Guest Access for Tropical Green ................................. 117

Output Caching and Last-Modified Dates of Postings ..................... 118

The Connector SearchMetaTagGenerator Control ........................... 119

# Configuring SharePoint Portal Server Search ............................ 121

Creating a New Content Source ......................................... 122

Creating a Content Source with the MCMS Connector Utility ............. 122

Creating a Content Source Manually .................................... 125

Search Rights for the MCMS Application Pool Account ................... 128

# Adding a Search Page to the MCMS Site ................................ 129

Searching with the MCMS SharePoint Connector ........................... 130

Building a Custom Search Implementation ............................... 135

About the SharePoint Portal Server Query Service ...................... 135

Building a Search Input Control ....................................... 135

The Advanced Search and Results Page ................................... 137

# Summary ................................................................. 150

# Chapter 6: Publishing Content Between MCMS and SharePoint ............. 151

# Using MCMS and SharePoint Together .................................... 152

# Joining MCMS and SharePoint in a Single Solution ...................... 153

# Using SharePoint to Display MCMS Content ............................... 155

The MCMS Connector for SharePoint Technologies ........................ 156

Installing the MCMS Connector Web Parts ............................... 157

MCMS Page Listing Web Part ............................................ 158

The MCMS Pages in Production Web Part ................................. 166

The MCMS Pages Waiting for Approval Web Part .......................... 167

Limitations of the MCMS Connector for SharePoint ...................... 168

# Using MCMS to Display SharePoint Content .............................. 169

The MCMS Connector for SharePoint Technologies ........................ 169

Using the SharePoint Document Placeholder ............................. 170

## Table of Contents

- Generating the Table of Attachments ................................ 239
- Deleting Attachments .................................................. 243
- Reusing the Insert Attachment Dialog ................................. 244
  - Calling the Dialog .................................................. 244
  - Returning Values from the Dialog ................................... 246
- Saving the List of Attachments ....................................... 251
- Retrieving Saved Content ............................................. 252
  - Preparing Arrays for Storing Information about the Attachments ...... 252
  - Retrieving Previously Saved XML .................................... 253
  - Extracting Information about the Attachments from the XML .......... 253
  - Populating the Table with the Saved Attachments .................... 253
- Displaying the Attachments ........................................... 255
- Using the MultipleAttachmentPlaceholderControl ...................... 256

- **An Image Rotator Placeholder Control** ............................ 256
  - The ImageRotatorPlaceholderControl Class .......................... 256
  - Specifying the Resource Gallery ................................... 257
  - Loading the TextBox ................................................ 258
  - Saving the Selected Resource Gallery Path .......................... 258
  - Loading Previously Saved Values .................................... 259
  - Rotating Images Randomly ........................................... 259
  - Using the Image Rotator Placeholder Control ........................ 263

- **A Placeholder Control to Store All HTML Tags** .................... 263
  - The AllTagsHtmlPlaceholderControl Class ........................... 266
  - Saving All Kinds of Tags ........................................... 266
    - Escaping Tags .................................................... 267
    - UnEscaping Tags .................................................. 267
  - Loading Content for Authoring and Presentation ..................... 267
  - Using the AllTagsHtmlPlaceholderControl ........................... 268

- **A DataGrid Placeholder Control** .................................. 269
  - The DataGridPlaceholderControl Class .............................. 270
  - Loading the DataGrid for Authoring ................................ 271
  - Retrieving Placeholder Content .................................... 273
  - Saving the Modified XML ............................................ 276
  - Adding a Row ....................................................... 277
  - Deleting a Row ..................................................... 278
  - Editing a Row ...................................................... 278
  - Updating a Row ...................................................... 278

