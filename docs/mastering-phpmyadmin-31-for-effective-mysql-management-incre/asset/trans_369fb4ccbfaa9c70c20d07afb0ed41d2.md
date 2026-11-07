
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

语言旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

Language 旁边的[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

Language 旁的小信息图标[content]
关于排序规则的介绍，请参阅 http://dev.mysql.com/doc/mysql/en/Charset-general.htm，关于所涉及算法的更技术性解释，请参阅 http://www.unicode.org/reports/tr10/。

## Unicode and UTF-8

> Unicode 是一种行业标准，旨在使文本和符号 [...] 能够被计算机一致地表示和处理。

获取更多信息，请访问 http://en.wikipedia.org/wiki/Unicode 和 http://www.unicode.org/。

Unicode 目前支持超过 600 种语言，这是其相对于 ISO 或 Windows 可用的其他字符集的主要优势。对于像 phpMyAdmin 这样的多语言产品来说，这一点尤其重要。

为了表示或编码这些 Unicode 字符，存在许多 Unicode 转换格式（UTF）。一种流行的转换格式是 UTF-8，它每个字符使用一到四个八位字节。详情请访问 http://en.wikipedia.org/wiki/UTF-8。

请注意，浏览器必须支持 UTF-8（大多数当前浏览器都支持）。phpMyAdmin 分发包包含 lang 子目录中每个语言文件的 UTF-8 版本。

## Language selection

语言选择器会出现在登录面板（如果有）和主页上。phpMyAdmin 的默认行为是使用浏览器首选项中定义的语言，前提是该版本存在对应的语言文件。

默认语言——如果程序无法检测到——在 config.inc.php 中定义，参数为 $cfg['DefaultLang']，值为 'en-utf-8'。此值可以更改。语言名称的可能值在 libraries/select_lang.lib.php 脚本中作为数组定义。

即使定义了默认语言，每个用户（尤其是在多用户安装中）仍可从选择器中选择其首选语言。用户的选择在可能的情况下会保存在 cookie 中。

我们还可以通过设置 $cfg['Lang'] 参数并赋予值，例如 'fr-utf-8' 来强制使用单一语言。从 2.7.0 版本开始，还有另一个参数 $cfg['FilterLanguages'] 可用。假设我们希望将可用语言列表缩短为 English 和 Français – French，因为这些是用户唯一使用的语言。这是通过构建一个正则表达式来实现的，该表达式根据这些语言的 ISO 639 代码指示我们希望显示的语言。继续我们的示例，我们将使用：

```
$cfg['FilterLanguages'] = '^(fr|en)';
```

在此表达式中，插入符号 (^) 表示以...开头，而 (|) 表示或。该表达式表示我们将列表限制为其对应 ISO 代码以 fr 或 en 开头的语言。

默认情况下，此参数为空，表示不对可用语言列表应用任何过滤。

Language 旁的小信息图标[content]
## Effective character sets and collations

在主页上，我们可以看到 MySQL 字符集信息和一个 MySQL 连接排序规则选择器。以下是 MySQL 字符集信息：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_74_0.png)

字符集信息（此处在 MySQL 字符集之后可见）直接来自于对应当前所选语言的语言文件中 $charset 变量。它用于生成 HTML 信息，告诉浏览器页面的字符集。

我们还可以使用 MySQL 连接排序规则对话框选择用于连接到 MySQL 服务器的字符集和排序规则。这会传递给 MySQL 服务器。MySQL 然后将要发送到我们浏览器的字符转换为此字符集。MySQL 也根据字符集信息解释从浏览器接收到的内容。请记住，所有表和字段都有字符集信息，描述其数据的编码方式。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_75_0.png)

通常，默认值应该可以工作。但是，如果我们使用不同的字符集输入某些字符，可以在此对话框中选择正确的字符集。

以下参数定义默认连接排序规则和字符集：

```
$cfg['DefaultConnectionCollation'] = 'utf8_unicode_ci';
```

## Left panel (navigation)

左侧面板包含以下元素：

- Logo（见下一节）
- 服务器列表（如果 $cfg['LeftDisplayServers'] 设置为 TRUE）
- Home 链接或图标（带您返回 phpMyAdmin 主页）
- 注销链接或图标
- 指向查询窗口的链接或图标
- 显示 phpMyAdmin 和 MySQL 文档的图标
- 数据库和表，以及每个数据库的表数量统计

如果 $cfg['MainPageIconic'] 设置为 TRUE（默认值），我们会看到图标。但是，如果设置为 FALSE，我们会看到 Home、注销和查询窗口链接。

左侧面板可以通过点击并移动垂直分隔线在首选方向上拖动来调整大小，以显示更多数据，以防数据库或表名对于默认的左侧面板大小过长。

我们可以自定义此面板的外观。通常，所有参数都位于 `themes/themename/layout.inc.php` 中，除非另有说明。`$cfg['LeftWidth']` 包含左侧框架的默认宽度（以像素为单位）。背景颜色在 `$cfg['LeftBgColor']` 中定义，默认值为 `'#D0DCE0'`。`$cfg['LeftPointerColor']` 参数，默认值为 `'#CCFFCC'`，定义指针颜色。（指针在使用稍后讨论的全模式时出现。）为了激活任何正在使用的主题的左侧指针，`config.inc.php` 中存在一个主设置 `$cfg['LeftPointerEnable']`。其默认值是 TRUE。

## Logo configuration

Logo 显示行为由多个参数控制。首先，`$cfg['LeftDisplayLogo']` 必须设置为 TRUE，以启用 Logo 的任何显示。它默认为 true。点击此 Logo 会将界面带到 `$cfg['LeftLogoLink']` 参数所列的页面，通常是主 phpMyAdmin 页面（默认值 `main.php`），但可以是任何 URL。最后，`$cfg['LeftLogoLinkWindow']` 参数指示在点击 Logo 后新页面出现在哪个窗口。默认情况下，它在主页面上（值 `main`）。但是，使用值 `new` 可以在全新的窗口中打开。

Logo 图像本身来自 `logo_left.png` 文件，该文件位于每个特定主题目录结构中。

## Database and table list

以下示例显示尚未选择任何数据库：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_76_0.png)

也可能看到以下屏幕：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_77_0.png)

这意味着我们当前的 MySQL 权限不允许我们查看任何现有数据库。

> MySQL 服务器始终至少有一个数据库（名为 **mysql**），但在这种情况下，我们没有权限查看它。此外，从 MySQL 5.0.2 开始，一个名为 **information_schema** 的特殊数据库始终出现在数据库列表中。它包含一组视图，描述登录用户可见的元数据。

我们可能有权创建其中一个，如第 4 章所述。

## Light mode

左侧面板可以以两种方式显示——轻量模式和完整模式。轻量模式默认使用，由 $cfg['LeftFrameLight'] 中的 TRUE 值定义。此模式显示可用数据库的下拉列表，并且仅显示当前选择的数据库的表。它比完整模式更高效；原因在稍后章节中出现的完整模式部分中解释。这里我们选择了 **mysql** 数据库：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_77_1.png)

点击名称会在右侧面板中打开 **Database** 视图，点击表名会在右侧面板中打开 **Table** 视图以浏览该表。（请参阅 *Right Panel* 部分了解详情。）

## Tree display of database names

用户可能只被允许在一个数据库上工作——例如 **marc**。
一些系统管理员通过允许用户 **marc** 创建多个数据库提供更灵活的方案，前提是所有数据库名称都以 **marc** 开头，例如 **marc_airline** 和 **marc_car**。在这种情况下，左侧面板可以设置为显示这些数据库名称的树，如下所示：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_78_0.png)

此功能目前仅在轻量模式下提供，并由以下参数控制：

```
$cfg['LeftFrameDBTree'] = TRUE;
$cfg['LeftFrameDBSeparator'] = '_';
```

$cfg['**LeftFrameDBTree**'] 中 TRUE 的默认值确保此功能被激活。分隔符的流行值是 '_'。

## Full mode

之前的示例在轻量模式下显示，但将 $cfg['**LeftFrameLight**'] 参数设置为 FALSE 会使用可折叠菜单（如果浏览器支持）生成我们数据库和表的完整布局：
