---
title: 数组维度
ms.date: 07/20/2015
helpviewer_keywords:
- dimensions, arrays
- arrays [Visual Basic], dimensions
- arrays [Visual Basic], rectangular
- arrays [Visual Basic], rank
- rectangular arrays
- ranking, arrays
ms.assetid: 385e911b-18c1-4e98-9924-c6d279101dd9
ms.openlocfilehash: 12e983ae62fa9f9ea762d434ffe5b73873a4a2e8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351897"
---
# <a name="array-dimensions-in-visual-basic"></a>Array Dimensions in Visual Basic

*维度*是可以改变数组元素规范的方向。 保存月份每一天的销售总额的数组有一个维度（月中的第几天）。 每个月的每一天都包含按部门列出的销售总额的数组有两个维度（部门号和月份日期）。 数组的维数称为 "*秩*"。

> [!NOTE]
> 您可以使用 <xref:System.Array.Rank%2A> 属性来确定数组的维数。

## <a name="working-with-dimensions"></a>使用维度

您可以通过为数组的每个维度提供*索引*或*下标*来指定数组的元素。 元素是从索引0到该维度的最高索引的每个维度连续的。

下图显示具有不同秩的数组的概念结构。 图中的每个元素都显示了访问它的索引值。 例如，可以通过指定索引 `(1, 0)`来访问二维数组的第二行的第一个元素。

![显示一维数组的关系图。](./media/array-dimensions/one-dimensional-array.gif)

![显示二维数组的关系图。](./media/array-dimensions/two-dimensional-array.gif)

![显示三维数组的关系图。](./media/array-dimensions/three-dimensional-array.gif)

### <a name="one-dimension"></a>一个维度

许多数组只有一个维度，例如每个年龄段的人员数。 指定元素的唯一要求是该元素保留计数的期限。 因此，此类数组只使用一个索引。 下面的示例声明一个变量，用于保存 age 0 到120的*一维*期限计数。

```vb
Dim ageCounts(120) As UInteger
```

### <a name="two-dimensions"></a>两个维度

某些阵列有两个维度，例如每个校园建筑的每个楼层的办公室数。 元素的规范要求生成号和楼层，每个元素都包含建筑物和地面的组合的计数。 因此，此类数组使用两个索引。 下面的示例声明一个变量，用于保存办公室计数的二维*数组*（即建筑物0到40，地面0到5）。

```vb
Dim officeCounts(40, 5) As Byte
```

二维数组也称为*矩形数组*。

### <a name="three-dimensions"></a>三个维度

几个数组具有三个维度，如三维空间中的值。 此类数组使用三个索引，在此示例中，表示物理空间的 x、y 和 z 坐标。 下面的示例声明一个变量，以便在三维卷中的各个点上保存一*维*的空气温度。

```vb
Dim airTemperatures(99, 99, 24) As Single
```

### <a name="more-than-three-dimensions"></a>超过三个维度

尽管数组最多可以有32个维度，但这种情况很少超过三个。

> [!NOTE]
> 将维度添加到数组时，数组所需的总存储量会大幅增加，因此请谨慎使用多维数组。

## <a name="using-different-dimensions"></a>使用不同尺寸

假设您想要跟踪当月每一天的销售总额。 您可以声明一个包含31个元素的一维数组，每个月对应一个元素，如下例所示。

```vb
Dim salesAmounts(30) As Double
```

现在，假设您想要跟踪每个月的每一天的相同信息，而不是一年中的每个月。 您可以声明一个二维数组，其中包含12个行（对于月份）和31个列（表示天数），如下例所示。

```vb
Dim salesAmounts(11, 30) As Double
```

现在假设你决定让你的数组将保存多个为一年的信息。 如果要跟踪5年的销售额，则可以使用5个层、12行和31列声明一个三维数组，如下例所示。

```vb
Dim salesAmounts(4, 11, 30) As Double
```

请注意，由于每个索引的最大值均为0，因此 `salesAmounts` 的每个维度声明为小于该维度所需的长度。 另请注意，数组的大小随每个新维度的增加而增加。 上述示例中的三个大小分别为31、372和1860个元素。

> [!NOTE]
> 无需使用 `Dim` 语句或 `New` 子句即可创建数组。 例如，您可以调用 <xref:System.Array.CreateInstance%2A> 方法，或其他组件可以通过此方式传递您的代码。 此类数组可以具有0以外的下限。 您始终可以使用 <xref:System.Array.GetLowerBound%2A> 方法或 `LBound` 函数测试维度的下限。

## <a name="see-also"></a>另请参阅

- [数组](../../../../visual-basic/programming-guide/language-features/arrays/index.md)
- [数组疑难解答](../../../../visual-basic/programming-guide/language-features/arrays/troubleshooting-arrays.md)
