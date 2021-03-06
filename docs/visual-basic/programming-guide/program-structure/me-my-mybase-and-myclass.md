---
title: Me、My、MyBase 和 MyClass
ms.date: 07/20/2015
f1_keywords:
- MyClass
- vb.Me
- MyBase
- vb.MyBase
- Me
- vb.MyClass
- vb.This
- vb.My
helpviewer_keywords:
- My object
- self-reference [Visual Basic], Me keyword
- MyClass keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], relationship to similar programming elements
- Me keyword [Visual Basic], referring to the current instance of an object
- Me keyword [Visual Basic]
- self-reference
- current instance [Visual Basic], Me keyword
- MyBase keyword [Visual Basic], relationship to similar programming elements
ms.assetid: f8e241ae-b1ed-4886-9aa0-08c632154029
ms.openlocfilehash: a21dfeb12e8d99f5f8b8afede084846711c299ab
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347341"
---
# <a name="me-my-mybase-and-myclass-in-visual-basic"></a>Visual Basic 中的 Me、My、MyBase 和 MyClass
Visual Basic 中 `Me`、`My`、`MyBase`和 `MyClass` 的名称相似，但用途不同。 本主题将介绍其中的每个实体，以便将它们区分开来。  
  
## <a name="me"></a>Me  
 `Me` 关键字提供了一种方法来引用代码当前正在执行的类或结构的特定实例。 `Me` 的行为类似于引用当前实例的对象变量或结构变量。 在将有关当前正在执行的类或结构实例的信息传递给另一类、结构或模块中的过程时，使用 `Me` 特别有用。  
  
 例如，假设你在模块中具有以下过程。  
  
```vb  
Sub ChangeFormColor(FormName As Form)  
   Randomize()  
   FormName.BackColor = Color.FromArgb(Rnd() * 256, Rnd() * 256, Rnd() * 256)  
End Sub  
```  
  
 您可以调用此过程，并使用以下语句将 <xref:System.Windows.Forms.Form> 类的当前实例作为参数传递。  
  
```vb  
ChangeFormColor(Me)  
```  
  
## <a name="my"></a>My  
 `My` 功能提供了对多个 .NET Framework 类的简单直观访问，使 Visual Basic 用户可以与计算机、应用程序、设置、资源等进行交互。  
  
## <a name="mybase"></a>MyBase  
 `MyBase` 关键字的行为类似于引用类的当前实例的基类的对象变量。 `MyBase` 通常用于访问在派生类中被重写或隐藏的基类成员。 `MyBase.New` 用于从派生类构造函数中显式调用基类构造函数。  
  
## <a name="myclass"></a>MyClass  
 `MyClass` 关键字的行为类似于引用最初实现的类的当前实例的对象变量。 `MyClass` 类似于 `Me`，但其上的所有方法调用都被视为 `NotOverridable`了方法。  
  
## <a name="see-also"></a>另请参阅

- [继承的基础知识](../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
