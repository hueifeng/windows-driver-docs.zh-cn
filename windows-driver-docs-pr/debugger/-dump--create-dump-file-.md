---
title: .dump（创建转储文件）
description: .Dump 命令创建的用户模式或内核模式崩溃转储文件。
ms.assetid: df6bcf7f-eb2e-4605-87a0-c0a7e9e4776b
keywords:
- 创建转储文件 (.dump) 命令
- 转储文件，创建转储文件 (.dump) 命令
- .dump （创建转储文件） Windows 调试
ms.date: 08/01/2018
topic_type:
- apiref
api_name:
- .dump (Create Dump File)
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: fa90f8f9e53325aada256104c3690cf30521250f
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63334539"
---
# <a name="dump-create-dump-file"></a>.dump（创建转储文件）


**.Dump**命令创建一个用户模式或内核模式崩溃转储文件。

```dbgcmd
.dump Options FileName 
.dump /?
```

## <a name="span-idddkmetacreatedumpfiledbgspanspan-idddkmetacreatedumpfiledbgspanparameters"></a><span id="ddk_meta_create_dump_file_dbg"></span><span id="DDK_META_CREATE_DUMP_FILE_DBG"></span>参数


<span id="_______Options______"></span><span id="_______options______"></span><span id="_______OPTIONS______"></span> *选项*   
表示一个或多个以下选项

<span id="_o"></span><span id="_O"></span>**/o**  
将现有的转储文件覆盖具有相同的名称。 如果不使用此选项，并且没有具有相同的文件名称的文件，则不写入转储文件。

<span id="_f_FullOptions_"></span><span id="_f_fulloptions_"></span><span id="_F_FULLOPTIONS_"></span>**/f\[**<em>FullOptions</em>**\]**  
(内核模式:)创建[完全内存转储](complete-memory-dump.md)。

(用户模式下:)创建*完整的用户模式转储*。 有关详细信息，请参阅[种用户模式转储文件的](user-mode-dump-files.md#varieties)。 尽管其名称，最大的小型转储文件实际包含比完整的用户模式转储的详细信息。 例如， **.dump /mf**或 **.dump /ma**创建更大、 更完整的文件，而非 **.dump /f**。 在用户模式下 **.dump** **/m\[**<em>MiniOptions</em> **\]** 始终是优于 **.dump /f**。

可以添加以下*FullOptions*更改转储文件; 的内容的选项是区分大小写。

|||
|--- |--- |
|FullOption|效果|
|y|将 AVX 注册信息添加到转储文件。|
 

<span id="_m_MiniOptions_"></span><span id="_m_minioptions_"></span><span id="_M_MINIOPTIONS_"></span>**/m\[**<em>MiniOptions</em>**\]**  
创建*很小的内存转储*（在内核模式下） 或*小型转储*（在用户模式下） 的详细信息，请参阅[用户模式转储文件](user-mode-dump-files.md)。 如果既没有 **/f**也不 **/m**指定，则 **/m**是默认值。

在用户模式下 **/m**可以与其他遵循*MiniOptions*指定要包含在转储的额外数据。 如果没有*MiniOptions*是包含，转储将包括模块、 线程和堆栈信息，但没有其他数据。 您可以添加任何以下*MiniOptions*更改内容转储文件中; 它们是区分大小写。

|MiniOption|效果|
|--- |--- |
|a|与所有可选的新增内容创建一个小型转储。 /Ma 选项相当于 /mfFhut--它将完整的内存数据、 处理的数据、 卸载的模块信息、 基本内存信息和线程时间信息添加到小型转储。 无法访问内存中读取任何失败会导致终止的小型转储生成。|
|A|/MA 选项相当于 /ma 只不过它会忽略任何无法读取无法访问内存，并继续生成小型转储。|
|f|将完整的内存数据添加到小型转储。 将包含在目标应用程序所拥有的所有可访问已提交的页面。|
|F|将所有基本内存信息添加到小型转储。 这将流添加到包含所有基本内存信息，而不仅仅是有效的内存信息的小型转储。 这使调试器能够在调试小型转储时重新构造完整的虚拟内存布局的过程。|
|h|添加有关小型转储到目标应用程序与关联的句柄的数据。|
|u|将卸载的模块信息添加到小型转储。 这是仅在 Windows Server 2003 和更高版本的 Windows 中可用。|
|t|将其他线程信息添加到小型转储。 这包括线程时间，可以使用显示 ！ 失控扩展或调试小型转储时.ttime （显示线程时间） 命令。|
|i|将辅助内存添加到小型转储。 辅助内存是堆栈或后备存储，再加上的小区域周围此地址的指针所引用的任何内存。|
|p|将进程环境块 (PEB) 和线程环境块 (TEB) 数据添加到小型转储。 这很有用，如果需要访问 Windows 应用程序的进程和线程的系统信息。|
|W|将所有已提交的读写专用页添加到小型转储。|
|d|将可执行映像中的所有读写数据段都添加到小型转储。|
|c|将添加在图像中的代码段。|
|r|从小型转储中删除不是可用于重新创建的堆栈跟踪这些部分的堆栈和应用商店的内存。 本地变量和其他数据类型值也将被删除。 此选项不会进行小型转储较小 （因为这些内存部分只需将清零），但如果你想要保护隐私的其他应用程序非常有用。|
|R|从小型转储中删除所有模块路径。 将包含仅模块名称。 如果你想要保护隐私的用户的目录结构，这是一个有用的选项。|
|y|将 AVX 注册信息添加到转储文件。|

 

### <a name="span-idadditionalinformationspanspan-idadditionalinformationspanspan-idadditionalinformationspanadditional-information"></a><span id="Additional_Information"></span><span id="additional_information"></span><span id="ADDITIONAL_INFORMATION"></span>其他信息

有关内核模式转储文件的说明和其用法的说明，请参阅[内核模式转储文件](kernel-mode-dump-files.md)。 用户模式转储文件的说明和其用法的说明，请参阅[用户模式转储文件](user-mode-dump-files.md)。

## <a name="remarks"></a>备注
-------

在许多情况下，可以使用此命令：

-   在实时用户模式调试，此命令将目标的应用程序生成转储文件，但目标应用程序不会终止。

-   在实时内核模式调试，此命令指示目标计算机来生成转储文件，但在目标计算机不会崩溃。

-   崩溃转储在调试期间，此命令从旧创建新的故障转储文件。 如果有大型崩溃转储文件，并想要创建一个较小，这非常有用。

您可以控制将生成的转储文件类型：

- 在内核模式下，以生成[完全内存转储](complete-memory-dump.md)，使用 **/f**选项。 若要生成[很小的内存转储](small-memory-dump.md)，使用 **/m**选项 （或任何选项）。 无法生成.dump 命令[内核内存转储](kernel-memory-dump.md)。

- 在用户模式下 **.dump** **/m\[**<em>MiniOptions</em> **\]** 是最佳选择。 使用此"m"代表"小型转储"，虽然创建转储文件*MiniOption*从很小到非常大的大小而异。 通过指定了正确*MiniOptions*可以控制哪些信息究竟是包含。 例如， **.dump /ma**生成使用大量的信息的转储。 较旧的命令， **.dump /f**，产生较大的"标准转储"文件，并不能自定义。

不能指定哪个进程转储。 将转储所有正在运行的进程。

**/Xc**， **/xr**， **/xp**，以及 **/xt**选项用于存储转储文件中的异常和上下文信息。 这允许[ **.ecxr （显示异常上下文记录）** ](-ecxr--display-exception-context-record-.md)命令对此转储文件运行。

下面的示例将创建用户模式下小型转储，其中包含完整的内存和句柄信息：

```dbgcmd
0:000> .dump /mfh myfile.dmp 
```

可以通过使用读取句柄的信息[ **！处理**](-handle.md)扩展命令。

 

 





