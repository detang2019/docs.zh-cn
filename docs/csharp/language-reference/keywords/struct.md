---
title: struct - C# 参考
ms.date: 07/20/2015
f1_keywords:
- struct_CSharpKeyword
helpviewer_keywords:
- struct keyword [C#]
- structs [C#], struct keyword
ms.assetid: ff3dd9b7-dc93-4720-8855-ef5558f65c7c
ms.openlocfilehash: 77d5c83dd4c81b96bc62ace6e609db8bc411dc41
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2020
ms.locfileid: "77093157"
---
# <a name="struct-c-reference"></a>struct（C# 参考）

`struct` 类型是一种值类型，通常用来封装小型相关变量组，例如，矩形的坐标或库存商品的特征。 下面的示例显示了一个简单的结构声明：

```csharp
public struct Book
{
    public decimal price;
    public string title;
    public string author;
}
```

## <a name="remarks"></a>备注

结构还可以包含[构造函数](../../programming-guide/classes-and-structs/constructors.md)、[常量](../../programming-guide/classes-and-structs/constants.md)、[字段](../../programming-guide/classes-and-structs/fields.md)、[方法](../../programming-guide/classes-and-structs/methods.md)、[属性](../../programming-guide/classes-and-structs/properties.md)、[索引器](../../programming-guide/indexers/index.md)、[运算符](../operators/index.md)、[事件](../../programming-guide/events/index.md)和[嵌套类型](../../programming-guide/classes-and-structs/nested-types.md)，但如果同时需要上述几种成员，则应当考虑改为使用类作为类型。

有关示例，请参阅[使用结构](../../programming-guide/classes-and-structs/using-structs.md)。

结构可以实现接口，但它们无法继承另一个结构。 因此，结构成员无法声明为 `protected`。

有关详细信息，请参阅[结构](../../programming-guide/classes-and-structs/structs.md)。

## <a name="examples"></a>示例

有关示例和详细信息，请参阅[使用结构](../../programming-guide/classes-and-structs/using-structs.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关示例，请参阅[使用结构](../../programming-guide/classes-and-structs/using-structs.md)。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 关键字](index.md)
- [值类型](../builtin-types/value-types.md)
- [class](class.md)
- [interface](interface.md)
- [类和结构](../../programming-guide/classes-and-structs/index.md)
