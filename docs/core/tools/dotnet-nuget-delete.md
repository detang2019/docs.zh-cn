---
title: dotnet nuget delete 命令
description: dotnet-nuget-delete 命令从服务器删除或取消列出包。
author: karann-msft
ms.date: 06/26/2019
ms.openlocfilehash: 0950f03c0986bde17ae3e2e7170d402ea8222853
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76733117"
---
# <a name="dotnet-nuget-delete"></a>dotnet nuget delete

 本文适用于：✔️ .NET Core 1.x SDK 及更高版本

<!-- todo: uncomment when all CLI commands are reviewed
[!INCLUDE [topic-appliesto-net-core-all](../../../includes/topic-appliesto-net-core-all.md)]
-->

## <a name="name"></a>“属性”

`dotnet nuget delete` - 从服务器删除或取消列出包。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet nuget delete [<PACKAGE_NAME> <PACKAGE_VERSION>] [--force-english-output] [--interactive] [-k|--api-key] [--no-service-endpoint]
    [--non-interactive] [-s|--source]
dotnet nuget delete [-h|--help]
```

## <a name="description"></a>描述

`dotnet nuget delete` 命令从服务器删除或取消列出包。 对于 [NuGet.org](https://www.nuget.org/)，该操作将取消列出包。

## <a name="arguments"></a>自变量

* **`PACKAGE_NAME`**

  要删除的包的名称/ID。

* **`PACKAGE_VERSION`**

  要删除的包的版本。

## <a name="options"></a>选项

* **`--force-english-output`**

  使用固定的、基于英语的区域性强制运行应用程序。

* **`-h|--help`**

  打印出有关命令的简短帮助。

* **`--interactive`**

  对于身份验证等操作，允许命令阻止并要求手动操作。 自 .NET Core 2.2 SDK 起可用的选项。

* **`-k|--api-key <API_KEY>`**

  服务器的 API 密钥。

* **`--no-service-endpoint`**

  不将“api/v2/package”追加至源 URL。 自 .NET Core 2.1 SDK 起可用的选项。

* **`--non-interactive`**

  不提示用户输入或确认。

* **`-s|--source <SOURCE>`**

  指定服务器 URL。 Nuget.org 的支持 URL 包括 `https://www.nuget.org`、`https://www.nuget.org/api/v3` 和 `https://www.nuget.org/api/v2/package`。 对于专用源，请替换主机名（例如，`%hostname%/api/v3`）。

## <a name="examples"></a>示例

* 删除包 `Microsoft.AspNetCore.Mvc` 的 1.0 版：

  ```dotnetcli
  dotnet nuget delete Microsoft.AspNetCore.Mvc 1.0
  ```

* 删除包 `Microsoft.AspNetCore.Mvc` 的 1.0 版（不提示用户需要凭据或其他输入）：

  ```dotnetcli
  dotnet nuget delete Microsoft.AspNetCore.Mvc 1.0 --non-interactive
  ```
