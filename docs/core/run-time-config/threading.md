---
title: 线程配置设置
description: 了解为 .NET Core 应用配置线程的运行时设置。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: 68b8e93ca6ec3f708a7a627307655ada1955500a
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2020
ms.locfileid: "76789856"
---
# <a name="run-time-configuration-options-for-threading"></a>用于线程的运行时配置选项

## <a name="cpu-groups"></a>CPU 组

- 配置是否在各 CPU 组之间自动分布线程。
- 默认：禁用 (`0`)。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | 不可用 | 不可用 |
| **环境变量** | `COMPlus_Thread_UseAllCpuGroups` | `0` - 禁用<br/>`1` - 启用 |

## <a name="minimum-threads"></a>最小线程数

- 指定工作线程池的最小线程数。
- 对应于 <xref:System.Threading.ThreadPool.SetMinThreads%2A?displayProperty=nameWithType> 方法。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `System.Threading.ThreadPool.MinThreads` | 一个表示最小线程数的整数 |
| MSBuild 属性  | `ThreadPoolMinThreads` | 一个表示最小线程数的整数 |
| **环境变量** | 不可用 | 不可用 |

### <a name="examples"></a>示例

runtimeconfig.json 文件： 

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Threading.ThreadPool.MinThreads": 4
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
  </PropertyGroup>

</Project>
```

## <a name="maximum-threads"></a>最大线程数

- 指定工作线程池的最大线程数。
- 对应于 <xref:System.Threading.ThreadPool.SetMaxThreads%2A?displayProperty=nameWithType> 方法。

| | 设置名 | 值 |
| - | - | - |
| **runtimeconfig.json** | `System.Threading.ThreadPool.MaxThreads` | 一个表示最大线程数的整数 |
| MSBuild 属性  | `ThreadPoolMaxThreads` | 一个表示最大线程数的整数 |
| **环境变量** | 不可用 | 不可用 |

### <a name="examples"></a>示例

runtimeconfig.json 文件： 

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Threading.ThreadPool.MaxThreads": 20
      }
   }
}
```

项目文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ThreadPoolMaxThreads>20</ThreadPoolMaxThreads>
  </PropertyGroup>

</Project>
```
