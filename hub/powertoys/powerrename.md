---
title: 适用于 Windows 10 的 PowerToys PowerRename 实用工具
description: 用于大容量重命名文件的 windows shell 扩展
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 26eee9fcb954a0a97ba6f30fae8a9d09395403a3
ms.sourcegitcommit: a1b251971f7ac574275d53bbe3e9ef4a3a9dc15c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103417098"
---
# <a name="powerrename-utility"></a>PowerRename 实用程序

PowerRename 是一种批量重命名工具，可用于：

-  (修改大量文件的文件名 *，而不是将所有文件重命名为相同的名称)*。
- 在文件名的目标部分执行搜索和替换。
- 对多个文件执行正则表达式重命名。
- 在完成大容量重命名之前，检查预览窗口中预期的重命名结果。
- 完成后撤消重命名操作。

## <a name="demo"></a>演示

在此演示中，文件名为 "Pampalona" 的所有实例都将替换为 "Pamplona"。 由于所有文件的名称都是唯一的，因此这种方法需要很长时间才能手动完成。 PowerRename 启用单次大容量重命名。 请注意，"撤消重命名" ("Ctrl + Z) " 命令可撤消更改。

![PowerRename 演示](../images/powerrename-demo.gif)

## <a name="powerrename-menu"></a>PowerRename 菜单

在 Windows 文件资源管理器中选择一些文件后，右键单击并选择 " *PowerRename* (仅当在 PowerToys) 中启用时才会显示，此时将显示" PowerRename "菜单。 将显示选定)  (文件数，以及 "搜索" 和 "替换" 值、选项列表以及显示搜索结果和替换已输入值的预览窗口。

![PowerRename 菜单屏幕快照](../images/powerrename-menu.png)

### <a name="search-for"></a>搜索

输入文本或 [正则表达式](https://wikipedia.org/wiki/Regular_expression) ，查找所选内容中包含与条目匹配的条件的文件。 *预览* 窗口中会显示匹配项。

### <a name="replace-with"></a>替换为

输入文本以替换先前输入的、与您选择的文件匹配的值 *搜索* 。 您可以在 " *预览* " 窗口中查看原始文件名和重命名的文件。

### <a name="options---use-regular-expressions"></a>选项-使用正则表达式

如果选中，搜索值将被解释为 [正则表达式](https://wikipedia.org/wiki/Regular_expression) (regex) 。 替换值还可以包含 regex 变量 (参阅下面) 的示例。  如果未选中，则搜索值将被解释为纯文本，以替换为替换字段中的文本。

有关 `Use Boost library` 扩展 regex 功能的 "设置" 菜单中的选项的详细信息，请参阅 [正则表达式部分](#regular-expressions)。

### <a name="options---case-sensitive"></a>选项-区分大小写

如果选中，则在 "搜索" 字段中指定的文本将仅匹配项中的文本（如果文本的大小写）。 大小写匹配不区分 (在默认情况下不识别大小写字母) 的差异。

### <a name="options---match-all-occurrences"></a>选项-匹配所有匹配项

如果选中，则搜索字段中的所有文本匹配项都将替换为替换文本。  否则，只会将文件名中搜索文本的第一个实例替换 (从左到右) 。

例如，假设文件名 `powertoys-powerrename.txt` 为：

- 搜索： `power`
- 重命名为： `super`

重命名的文件的值将导致：

- 匹配所有匹配项 (未选中) ： `supertoys-powerrename.txt`
- 匹配所有匹配项 (选中) ： `supertoys-superrename.txt`

### <a name="options---exclude-files"></a>选项-排除文件

操作中将不包含文件。 只包含文件夹。

### <a name="options---exclude-folders"></a>选项-排除文件夹

操作中将不包含文件夹。 只包含文件。

### <a name="options---exclude-subfolder-items"></a>选项-排除子文件夹项

在操作中将不包含文件夹中的项。 默认情况下，包括所有子文件夹项。

### <a name="options---enumerate-items"></a>选项-枚举项

将数字后缀追加到在操作中修改的文件名。 例如：`foo.jpg` -> `foo (1).jpg`

### <a name="options---item-name-only"></a>选项-仅限项名称

此操作不会修改文件扩展名)  (的文件名部分。 例如：`txt.txt` ->  `NewName.txt`

### <a name="options---item-extension-only"></a>选项-仅限项扩展

此操作不会修改文件扩展名部分 (不) 文件名。 例如：`txt.txt` -> `txt.NewExtension`

## <a name="replace-using-file-creation-date-and-time"></a>使用文件创建日期和时间替换

根据下表输入变量模式，可以在 " *替换为* " 文本中使用文件的创建日期和时间属性。

变量模式 |说明
|:---|:---|
|`$YYYY`|年份由完整的四位或五位数字表示，具体取决于所使用的日历。
|`$YY`|仅由最后两位数字表示的年份。 为一位数的年份添加了前导零。
|`$Y`|年份仅用最后一个数字表示。
|`$MMMM`|月份名称
|`$MMM`|月份的缩写名称
|`$MM`|月份作为带有前导零的数字。
|`$M`|不带前导零的数字表示的月份。
|`$DDDD`|一周中某天的名称
|`$DDD`|一周中某天的缩写名称
|`$DD`|月份中的某一天的数字，为一位数天数内带有前导零的数字。
|`$D`|一个月中的第几天，表示一位数天数中不带前导零的数字。
|`$hh`|一位数小时内带前导零的小时数
|`$h`|一位数小时没有前导零的小时数
|`$mm`|用前导零表示的分钟数分钟。
|`$m`|一位数分钟没有前导零的分钟数。
|`$ss`|用前导零表示一位数秒的秒数。
|`$s`|一位数秒没有前导零的秒数。
|`$fff`|以完整三位数表示的毫秒数。
|`$ff`|仅由前两位数字表示的毫秒数。
|`$f`|仅由第一个数字表示的毫秒数。

例如，给定文件名：

- `powertoys.png`，创建于11/02/2020
- `powertoys-menu.png`，创建于11/03/2020

输入用于重命名项的条件：

- 搜索： `powertoys`
- 重命名为： `$MMM-$DD-$YY-powertoys`

重命名的文件的值将导致：

- `Nov-02-20-powertoys.png`
- `Nov-03-20-powertoys-menu.png`

## <a name="regular-expressions"></a>“正则表达式”

对于大多数用例，简单的搜索和替换就够了。 但在某些情况下，复杂的重命名任务需要进行更多的控制。 [正则表达式](https://wikipedia.org/wiki/Regular_expression) 可帮助。

正则表达式定义文本的搜索模式。 它们可用于搜索、编辑和操作文本。 正则表达式定义的模式可以匹配给定字符串的一次或多次。 PowerRename 使用 [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 语法，这是新式编程语言所共有的。

若要启用正则表达式，请选中 "使用正则表达式" 复选框。

**注意：** 使用正则表达式时，可能需要选中 "匹配所有匹配项"。

若要使用 [提升库](https://www.boost.org/doc/libs/1_74_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html) 而不是标准库，请检查 `Use Boost library` PowerToys 设置中的选项。 它启用标准库不支持的扩展功能，如 [回顾](https://www.boost.org/doc/libs/1_74_0/libs/regex/doc/html/boost_regex/syntax/perl_syntax.html#boost_regex.syntax.perl_syntax.lookbehind)。

### <a name="examples-of-regular-expressions"></a>正则表达式示例

#### <a name="simple-matching-examples"></a>简单匹配示例

| 搜索       | 描述                                           |
| ---------------- | ------------- |
| `^`              | 匹配文件名的开头                   |
| `$`              | 匹配文件名的末尾                         |
| `.*`             | 匹配名称中的所有文本                        |
| `^foo`           | 匹配以 "foo" 开头的文本                     |
| `bar$`           | 匹配以 "bar" 结尾的文本                       |
| `^foo.*bar$`     | 匹配以 "foo" 开头并以 "bar" 结尾的文本 |
| `.+?(?=bar)`     | 全部匹配 "bar"                          |
| `foo[\s\S]*bar`  | 匹配 "foo" 和 "bar" 之间的所有内容              |

#### <a name="matching-and-variable-examples"></a>匹配和变量示例

*使用变量时，必须启用 "匹配所有事件" 选项。*

| 搜索   | 替换为    | 描述                                |
| ------------ | --------------- |--------------------------------------------|
| `(.*).png`   | `foo_$1.png`   | 在现有文件名前面预置 "foo \_ " |
| `(.*).png`   | `$1_foo.png`   | 将 " \_ foo" 追加到现有文件名  |
| `(.*)`       | `$1.txt`        | 向现有文件名追加 ".txt" 扩展 |
| `(^\w+\.$)|(^\w+$)` | `$2.txt` | 仅当现有文件名没有扩展名时，向其追加 ".txt" 扩展 |
|  `(\d\d)-(\d\d)-(\d\d\d\d)` | `$3-$2-$1` | 移动文件名中的数字： "29-03-2020" 变为 "2020-03-29" |

### <a name="additional-resources-for-learning-regular-expressions"></a>用于学习正则表达式的其他资源

在线提供了很好的示例/备忘单，可帮助你

[Regex 教程—按示例快速速查表](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)

[ECMAScript 正则表达式教程](https://o7planning.org/en/12219/ecmascript-regular-expressions-tutorial)

## <a name="file-list-filters"></a>文件列表筛选器

可以在 PowerRename 中使用筛选器来缩小重命名结果的范围。 使用 " *预览* " 窗口检查预期的结果。 选择列标题可在筛选器之间切换。

- **原始**， *预览* 窗口中的第一列将循环切换：
  - 选中：选中此文件将被重命名。
  - 未选中：未选择要重命名的文件 (即使它符合搜索条件) 中输入的值。

- **重命名** 后，可以切换 *预览* 窗口中的第二列。
  - 默认预览将显示所有选定的文件，只有匹配 *搜索* 条件的文件才会显示更新后的重命名值。
  - 选择 *重命名* 的标头会将预览切换为仅显示将重命名的文件。 原始选择的其他选定文件将不可见。

![PowerToys PowerRename 筛选器演示](../images/powerrename-demo2.gif)
