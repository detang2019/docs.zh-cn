---
title: 自定义结构封送 - .NET
description: 了解如何自定义 .NET 将结构封送到本机表示形式的方式。
author: jkoritzinsky
ms.author: jekoritz
ms.date: 01/18/2019
dev_langs:
- csharp
- cpp
ms.openlocfilehash: c4d2d84a59aebedda2d1e6380caeef170051c0a3
ms.sourcegitcommit: b56d59ad42140d277f2acbd003b74d655fdbc9f1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2019
ms.locfileid: "56411369"
---
# <a name="customizing-structure-marshalling"></a><span data-ttu-id="86f52-103">自定义结构封送</span><span class="sxs-lookup"><span data-stu-id="86f52-103">Customizing structure marshalling</span></span>

<span data-ttu-id="86f52-104">有时，结构的默认封送规则无法完全满足要求。</span><span class="sxs-lookup"><span data-stu-id="86f52-104">Sometimes the default marshalling rules for structures aren't exactly what you need.</span></span> <span data-ttu-id="86f52-105">.NET 运行时提供了几个扩展点以自定义结构的布局和字段的封送方式。</span><span class="sxs-lookup"><span data-stu-id="86f52-105">The .NET runtimes provide a few extension points for you to customize your structure's layout and how fields are marshaled.</span></span>

## <a name="customizing-structure-layout"></a><span data-ttu-id="86f52-106">自定义结构布局</span><span class="sxs-lookup"><span data-stu-id="86f52-106">Customizing structure layout</span></span>

<span data-ttu-id="86f52-107">.NET 提供了 <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> 属性和 <xref:System.Runtime.InteropServices.LayoutKind?displayProperty=nameWithType> 枚举，允许用户自定义字段在内存中的放置方式。</span><span class="sxs-lookup"><span data-stu-id="86f52-107">.NET provides the <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> attribute and the <xref:System.Runtime.InteropServices.LayoutKind?displayProperty=nameWithType> enumeration to allow you to customize how fields are placed in memory.</span></span> <span data-ttu-id="86f52-108">以下指南将帮助你避免常见问题。</span><span class="sxs-lookup"><span data-stu-id="86f52-108">The following guidance will help you avoid common issues.</span></span>

<span data-ttu-id="86f52-109">✔️ 请考虑尽量使用 `LayoutKind.Sequential`。</span><span class="sxs-lookup"><span data-stu-id="86f52-109">**✔️ CONSIDER** using `LayoutKind.Sequential` whenever possible.</span></span>

<span data-ttu-id="86f52-110">✔️ 当本机结构还具有显式布局（如联合）时，请务必仅将 `LayoutKind.Explicit` 用于封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-110">**✔️ DO** only use `LayoutKind.Explicit` in marshalling when your native struct is also has an explicit layout, such as a union.</span></span>

<span data-ttu-id="86f52-111">❌ 在非 Windows 平台上封送结构时，请避免使用 `LayoutKind.Explicit`。</span><span class="sxs-lookup"><span data-stu-id="86f52-111">**❌ AVOID** using `LayoutKind.Explicit` when marshalling structures on non-Windows platforms.</span></span> <span data-ttu-id="86f52-112">.NET Core 运行时不支持在 Intel 或 AMD 64 位的非 Windows 系统上按值将显式结构传递到本机函数。</span><span class="sxs-lookup"><span data-stu-id="86f52-112">The .NET Core runtime doesn't support passing explicit structures by value to native functions on Intel or AMD 64-bit non-Windows systems.</span></span> <span data-ttu-id="86f52-113">但是，运行时支持在所有平台上按引用传递显式结构。</span><span class="sxs-lookup"><span data-stu-id="86f52-113">However, the runtime supports passing explicit structures by reference on all platforms.</span></span>

## <a name="customizing-boolean-field-marshalling"></a><span data-ttu-id="86f52-114">自定义布尔字段封送</span><span class="sxs-lookup"><span data-stu-id="86f52-114">Customizing boolean field marshalling</span></span>

<span data-ttu-id="86f52-115">本机代码具有许多不同的布尔表示形式。</span><span class="sxs-lookup"><span data-stu-id="86f52-115">Native code has many different boolean representations.</span></span> <span data-ttu-id="86f52-116">仅在 Windows 上，有三种方式可用于表示布尔值。</span><span class="sxs-lookup"><span data-stu-id="86f52-116">On Windows alone, there are three ways to represent boolean values.</span></span> <span data-ttu-id="86f52-117">运行时不知道结构的本机定义，因此，它最多只能对如何封送布尔值做出猜测。</span><span class="sxs-lookup"><span data-stu-id="86f52-117">The runtime doesn't know the native definition of your structure, so the best it can do is make a guess on how to marshal your boolean values.</span></span> <span data-ttu-id="86f52-118">.NET 运行时提供指示如何封送布尔字段的方式。</span><span class="sxs-lookup"><span data-stu-id="86f52-118">The .NET runtime provides a way to indicate how to marshal your boolean field.</span></span> <span data-ttu-id="86f52-119">下面的示例介绍如何将 .NET `bool` 封送到不同的本机布尔类型。</span><span class="sxs-lookup"><span data-stu-id="86f52-119">The following examples show how to marshal .NET `bool` to different native boolean types.</span></span>

<span data-ttu-id="86f52-120">布尔值默认作为本机 4 字节 Win32 [`BOOL`](/windows/desktop/winprog/windows-data-types#BOOL) 值进行封送，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="86f52-120">Boolean values default to marshalling as a native 4-byte Win32 [`BOOL`](/windows/desktop/winprog/windows-data-types#BOOL) value as shown in the following example:</span></span>

```csharp
public struct WinBool
{
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

<span data-ttu-id="86f52-121">如果想要明确指出，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.Bool?displayProperty=nameWithType> 值获取如上所述的行为：</span><span class="sxs-lookup"><span data-stu-id="86f52-121">If you want to be explicit, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.Bool?displayProperty=nameWithType> value to get the same behavior as above:</span></span>

```csharp
public struct WinBool
{
    [MarshalAs(UnmanagedType.Bool)]
    public bool b;
}
```

```cpp
struct WinBool
{
    public BOOL b;
};
```

<span data-ttu-id="86f52-122">使用下面的 `UmanagedType.U1` 或 `UnmanagedType.I1` 值，可以告知运行时将 `b` 字段作为 1 字节本机 `bool` 类型进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-122">Using the `UmanagedType.U1` or `UnmanagedType.I1` values below, you can tell the runtime to marshal the `b` field as a 1-byte native `bool` type.</span></span>

```csharp
public struct CBool
{
    [MarshalAs(UnmanagedType.U1)]
    public bool b;
}
```

```cpp
struct CBool
{
    public bool b;
};
```

<span data-ttu-id="86f52-123">在 Windows 上，可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.VariantBool?displayProperty=nameWithType> 值告知运行时将布尔值封送到 2 字节的 `VARIANT_BOOL` 值：</span><span class="sxs-lookup"><span data-stu-id="86f52-123">On Windows, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.VariantBool?displayProperty=nameWithType> value to tell the runtime to marshal your boolean value to a 2-byte `VARIANT_BOOL` value:</span></span>

```csharp
public struct VariantBool
{
    [MarshalAs(UnmanagedType.VariantBool)]
    public bool b;
}
```

```cpp
struct VariantBool
{
    public VARIANT_BOOL b;
};
```

> [!NOTE]
> <span data-ttu-id="86f52-124">`VARIANT_BOOL` 与 `VARIANT_TRUE = -1` 和 `VARIANT_FALSE = 0` 中的大多数 bool 类型不同。</span><span class="sxs-lookup"><span data-stu-id="86f52-124">`VARIANT_BOOL` is different than most bool types in that `VARIANT_TRUE = -1` and `VARIANT_FALSE = 0`.</span></span> <span data-ttu-id="86f52-125">此外，不等于 `VARIANT_TRUE` 的所有值都将被视为 false。</span><span class="sxs-lookup"><span data-stu-id="86f52-125">Additionally, all values that aren't equal to `VARIANT_TRUE` are considered false.</span></span>

## <a name="customizing-array-field-marshalling"></a><span data-ttu-id="86f52-126">自定义数组字段封送</span><span class="sxs-lookup"><span data-stu-id="86f52-126">Customizing array field marshalling</span></span>

<span data-ttu-id="86f52-127">.NET 还包括自定义数组封送的多种方式。</span><span class="sxs-lookup"><span data-stu-id="86f52-127">.NET also includes a few ways to customize array marshalling.</span></span>

<span data-ttu-id="86f52-128">默认情况下，.NET 将数组作为指向元素的连续列表的指针进行封送：</span><span class="sxs-lookup"><span data-stu-id="86f52-128">By default, .NET marshals arrays as a pointer to a contiguous list of the elements:</span></span>

```csharp
public struct DefaultArray
{
    public int[] values;
}
```

```cpp
struct DefaultArray
{
    int* values;
};
```

<span data-ttu-id="86f52-129">如果要与 COM API 交互，则可能必须将数组作为 `SAFEARRAY*` 对象进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-129">If you're interfacing with COM APIs, you may have to marshal arrays as `SAFEARRAY*` objects.</span></span> <span data-ttu-id="86f52-130">可以使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> 值告知运行时将数组作为 `SAFEARRAY*` 进行封送：</span><span class="sxs-lookup"><span data-stu-id="86f52-130">You can use the <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> and the <xref:System.Runtime.InteropServices.UnmanagedType.SafeArray?displayProperty=nameWithType> value to tell the runtime to marshal an array as a `SAFEARRAY*`:</span></span>

```csharp
public struct SafeArrayExample
{
    [MarshalAs(UnmanagedType.SafeArray)]
    public int[] values;
}
```

```cpp
struct SafeArrayExample
{
    SAFEARRAY* values;
};
```

<span data-ttu-id="86f52-131">如果需要自定义 `SAFEARRAY` 中的元素类型，则可以使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> 字段自定义 `SAFEARRAY` 的确切元素类型。</span><span class="sxs-lookup"><span data-stu-id="86f52-131">If you need to customize what type of element is in the `SAFEARRAY`, then you can use the <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType?displayProperty=nameWithType> and <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType?displayProperty=nameWithType> fields to customize the exact element type of the `SAFEARRAY`.</span></span>

<span data-ttu-id="86f52-132">如果需要就地封送数组，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType> 值告知封送处理程序就地封送数组。</span><span class="sxs-lookup"><span data-stu-id="86f52-132">If you need to marshal the array in-place, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.ByValArray?displayProperty=nameWithType> value to tell the marshaler to marshal the array in-place.</span></span> <span data-ttu-id="86f52-133">使用此封送时，还必须为数组中的元素数对应的 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> 字段提供一个值，以便运行时可以正确地为结构分配空间。</span><span class="sxs-lookup"><span data-stu-id="86f52-133">When you're using this marshalling, you also must supply a value to the <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> field  for the number of elements in the array so the runtime can correctly allocate space for the structure.</span></span>

```csharp
public struct InPlaceArray
{
    [MarshalAs(UnmanagedType.ByValArray, SizeConst = 4)]
    public int[] values;
}
```

```cpp
struct InPlaceArray
{
    int values[4];
};
```

> [!NOTE]
> <span data-ttu-id="86f52-134">.NET 不支持将变长度数组字段作为 C99 可变数组成员进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-134">.NET doesn't support marshalling a variable length array field as a C99 Flexible Array Member.</span></span>

## <a name="customizing-string-field-marshalling"></a><span data-ttu-id="86f52-135">自定义字符串字段封送</span><span class="sxs-lookup"><span data-stu-id="86f52-135">Customizing string field marshalling</span></span>

<span data-ttu-id="86f52-136">.NET 还提供用于封送字符串字段的各种自定义。</span><span class="sxs-lookup"><span data-stu-id="86f52-136">.NET also provides a wide variety of customizations for marshalling string fields.</span></span>

<span data-ttu-id="86f52-137">默认情况下，.NET 将字符串作为指向以 null 结尾的字符串的指针进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-137">By default, .NET marshals a string as a pointer to a null-terminated string.</span></span> <span data-ttu-id="86f52-138">编码取决于 <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType> 中的 <xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> 字段的值。</span><span class="sxs-lookup"><span data-stu-id="86f52-138">The encoding depends on the value of the <xref:System.Runtime.InteropServices.StructLayoutAttribute.CharSet?displayProperty=nameWithType> field in the <xref:System.Runtime.InteropServices.StructLayoutAttribute?displayProperty=nameWithType>.</span></span> <span data-ttu-id="86f52-139">如果未指定任何属性，则编码将默认为 ANSI 编码。</span><span class="sxs-lookup"><span data-stu-id="86f52-139">If no attribute is specified, the encoding defaults to an ANSI encoding.</span></span>

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char* str;
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

<span data-ttu-id="86f52-140">如果需要对不同字段使用不同编码或只是希望在结构定义中更加明确，则可以在 <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> 属性上使用 <xref:System.Runtime.InteropServices.UnmanagedType.LPStr?displayProperty=nameWithType> 或 <xref:System.Runtime.InteropServices.UnmanagedType.LPWStr?displayProperty=nameWithType> 值。</span><span class="sxs-lookup"><span data-stu-id="86f52-140">If you need to use different encodings for different fields or just prefer to be more explicit in your struct definition, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.LPStr?displayProperty=nameWithType> or <xref:System.Runtime.InteropServices.UnmanagedType.LPWStr?displayProperty=nameWithType> values on a <xref:System.Runtime.InteropServices.MarshalAsAttribute?displayProperty=nameWithType> attribute.</span></span>

```csharp
public struct AnsiString
{
    [MarshalAs(UnmanagedType.LPStr)]
    public string str;
}
```

```cpp
struct AnsiString
{
    char* str;
};
```

```csharp
public struct UnicodeString
{
    [MarshalAs(UnmanagedType.LPWStr)]
    public string str;
}
```

```cpp
struct UnicodeString
{
    char16_t* str; // Could also be wchar_t* on Windows.
};
```

<span data-ttu-id="86f52-141">如果想要使用 UTF-8 编码封送字符串，则可以在 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 中使用 <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> 值。</span><span class="sxs-lookup"><span data-stu-id="86f52-141">If you want to marshal your strings using the UTF-8 encoding, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> value in your <xref:System.Runtime.InteropServices.MarshalAsAttribute>.</span></span>


```csharp
public struct UTF8String
{
    [MarshalAs(UnmanagedType.LPUTF8Str)]
    public string str;
}
```

```cpp
struct UTF8String
{
    char* str;
};
```

> [!NOTE]
> <span data-ttu-id="86f52-142">使用 <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> 需要 .NET Framework 4.7（或更高版本）或 .NET Core 1.1（或更高版本）。</span><span class="sxs-lookup"><span data-stu-id="86f52-142">Using <xref:System.Runtime.InteropServices.UnmanagedType.LPUTF8Str?displayProperty=nameWithType> requires either .NET Framework 4.7 (or later versions) or .NET Core 1.1 (or later versions).</span></span> <span data-ttu-id="86f52-143">不能在 .NET Standard 2.0 中使用。</span><span class="sxs-lookup"><span data-stu-id="86f52-143">It isn't available in .NET Standard 2.0.</span></span>

<span data-ttu-id="86f52-144">如果使用 COM API，则可能需要将字符串作为 `BSTR` 进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-144">If you're working with COM APIs, you may need to marshal a string as a `BSTR`.</span></span> <span data-ttu-id="86f52-145">使用 <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> 值可以将字符串作为 `BSTR` 进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-145">Using the <xref:System.Runtime.InteropServices.UnmanagedType.BStr?displayProperty=nameWithType> value, you can marshal a string as a `BSTR`.</span></span>

```csharp
public struct BString
{
    [MarshalAs(UnmanagedType.BStr)]
    public string str;
}
```

```cpp
struct BString
{
    BSTR str;
};
```

<span data-ttu-id="86f52-146">使用基于 WinRT 的 API 时，可能需要将字符串作为 `HSTRING` 进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-146">When using a WinRT-based API, you may need to marshal a string as an `HSTRING`.</span></span>  <span data-ttu-id="86f52-147">使用 <xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> 值可以将字符串作为 `HSTRING` 进行封送。</span><span class="sxs-lookup"><span data-stu-id="86f52-147">Using the <xref:System.Runtime.InteropServices.UnmanagedType.HString?displayProperty=nameWithType> value, you can marshal a string as a `HSTRING`.</span></span>

```csharp
public struct HString
{
    [MarshalAs(UnmanagedType.HString)]
    public string str;
}
```

```cpp
struct BString
{
    HSTRING str;
};
```

<span data-ttu-id="86f52-148">如果 API 要求你将字符串就地传入结构，则可以使用 <xref:System.Runtime.InteropServices.UnmanagedType.ByValTStr?displayProperty=nameWithType> 值。</span><span class="sxs-lookup"><span data-stu-id="86f52-148">If your API requires you to pass the string in-place in the structure, you can use the <xref:System.Runtime.InteropServices.UnmanagedType.ByValTStr?displayProperty=nameWithType> value.</span></span> <span data-ttu-id="86f52-149">务必注意，通过 `ByValTStr` 封送的字符串的编码由 `CharSet` 属性确定。</span><span class="sxs-lookup"><span data-stu-id="86f52-149">Do note that the encoding for a string marshalled by `ByValTStr` is determined from the `CharSet` attribute.</span></span> <span data-ttu-id="86f52-150">此外，还需要通过 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> 字段传递字符串长度。</span><span class="sxs-lookup"><span data-stu-id="86f52-150">Additionally, it requires that a string length is passed by the <xref:System.Runtime.InteropServices.MarshalAsAttribute.SizeConst?displayProperty=nameWithType> field.</span></span>

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Ansi)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char str[4];
};
```

```csharp
[StructLayout(LayoutKind.Sequential, CharSet = CharSet.Unicode)]
public struct DefaultString
{
    [MarshalAs(UnmanagedType.ByValTStr, SizeConst = 4)]
    public string str;
}
```

```cpp
struct DefaultString
{
    char16_t str[4]; // Could also be wchar_t[4] on Windows.
};
```

## <a name="customizing-decimal-field-marshalling"></a><span data-ttu-id="86f52-151">自定义十进制字段封送</span><span class="sxs-lookup"><span data-stu-id="86f52-151">Customizing decimal field marshalling</span></span>

<span data-ttu-id="86f52-152">如果在 Windows 上操作，则可能会遇到一些使用本机 [`CY` 或 `CURRENCY`](/windows/desktop/api/wtypes/ns-wtypes-tagcy) 结构的 API。</span><span class="sxs-lookup"><span data-stu-id="86f52-152">If you're working on Windows, you might encounter some APIs that use the native [`CY` or `CURRENCY`](/windows/desktop/api/wtypes/ns-wtypes-tagcy) structure.</span></span> <span data-ttu-id="86f52-153">默认情况下，.NET `decimal` 类型会封送到本机 [`DECIMAL`](/windows/desktop/api/wtypes/ns-wtypes-tagdec) 结构。</span><span class="sxs-lookup"><span data-stu-id="86f52-153">By default, the .NET `decimal` type marshals to the native [`DECIMAL`](/windows/desktop/api/wtypes/ns-wtypes-tagdec) structure.</span></span> <span data-ttu-id="86f52-154">但是，可以使用包含 <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=nameWithType> 值的 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 来指示封送处理程序将 `decimal` 值转换为本机 `CY` 值。</span><span class="sxs-lookup"><span data-stu-id="86f52-154">However, you can use a <xref:System.Runtime.InteropServices.MarshalAsAttribute> with the <xref:System.Runtime.InteropServices.UnmanagedType.Currency?displayProperty=nameWithType> value to instruct the marshaler to convert a `decimal` value to a native `CY` value.</span></span>

```csharp
public struct Currency
{
    [MarshalAs(UnmanagedType.Currency)]
    public decimal dec;
}
```

```cpp
struct Currency
{
    CY dec;
};
```

## <a name="marshalling-systemobjects"></a><span data-ttu-id="86f52-155">封送 `System.Object`</span><span class="sxs-lookup"><span data-stu-id="86f52-155">Marshalling `System.Object`s</span></span>

<span data-ttu-id="86f52-156">在 Windows 上，可以将类型为 `object` 的字段封送到本机代码。</span><span class="sxs-lookup"><span data-stu-id="86f52-156">On Windows, you can marshal `object`-typed fields to native code.</span></span> <span data-ttu-id="86f52-157">可以将这些字段封送到以下三个类型之一：</span><span class="sxs-lookup"><span data-stu-id="86f52-157">You can marshal these fields to one of three types:</span></span>
- [`VARIANT`](/windows/desktop/api/oaidl/ns-oaidl-tagvariant)
- [`IUnknown*`](/windows/desktop/api/unknwn/nn-unknwn-iunknown)
- <span data-ttu-id="86f52-158">[`IDispatch*`](/windows/desktop/api/oaidl/nn-oaidl-idispatch)。</span><span class="sxs-lookup"><span data-stu-id="86f52-158">[`IDispatch*`](/windows/desktop/api/oaidl/nn-oaidl-idispatch).</span></span> 

<span data-ttu-id="86f52-159">默认情况下，将类型为 `object` 的字段封送到用来包装对象的 `IUnknown*`。</span><span class="sxs-lookup"><span data-stu-id="86f52-159">By default, an `object`-typed field will be marshalled to an `IUnknown*` that wraps the object.</span></span>

```csharp
public struct ObjectDefault
{
    public object obj;
}
```

```cpp
struct ObjectDefault
{
    IUnknown* obj;
};
```

<span data-ttu-id="86f52-160">如果要将对象字段封送到 `IDispatch*`，请添加包含 <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType> 值的 <xref:System.Runtime.InteropServices.MarshalAsAttribute>。</span><span class="sxs-lookup"><span data-stu-id="86f52-160">If you want to marshal an object field to an `IDispatch*`, add a <xref:System.Runtime.InteropServices.MarshalAsAttribute> with the <xref:System.Runtime.InteropServices.UnmanagedType.IDispatch?displayProperty=nameWithType> value.</span></span>

```csharp
public struct ObjectDispatch
{
    [MarshalAs(UnmanagedType.IDispatch)]
    public object obj;
}
```

```cpp
struct ObjectDispatch
{
    IDispatch* obj;
};
```

<span data-ttu-id="86f52-161">如果要将其作为 `VARIANT` 进行封送，请添加包含 <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> 值的 <xref:System.Runtime.InteropServices.MarshalAsAttribute>。</span><span class="sxs-lookup"><span data-stu-id="86f52-161">If you want to marshal it as a `VARIANT`, add a <xref:System.Runtime.InteropServices.MarshalAsAttribute> with the <xref:System.Runtime.InteropServices.UnmanagedType.Struct?displayProperty=nameWithType> value.</span></span>

```csharp
public struct ObjectVariant
{
    [MarshalAs(UnmanagedType.Struct)]
    public object obj;
}
```

```cpp
struct ObjectVariant
{
    VARIANT obj;
};
```

<span data-ttu-id="86f52-162">下表介绍了如何将 `obj` 字段的不同运行时类型映射到存储在 `VARIANT` 中的各种类型：</span><span class="sxs-lookup"><span data-stu-id="86f52-162">The following table describes how different runtime types of the `obj` field map to the various types stored in a `VARIANT`:</span></span>

| <span data-ttu-id="86f52-163">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="86f52-163">.NET Type</span></span> | <span data-ttu-id="86f52-164">VARIANT 类型</span><span class="sxs-lookup"><span data-stu-id="86f52-164">VARIANT Type</span></span> | | <span data-ttu-id="86f52-165">.NET 类型</span><span class="sxs-lookup"><span data-stu-id="86f52-165">.NET Type</span></span> | <span data-ttu-id="86f52-166">VARIANT 类型</span><span class="sxs-lookup"><span data-stu-id="86f52-166">VARIANT Type</span></span> |
|------------|--------------|-|----------|--------------|
|  `byte`  | `VT_UI1` |     | `System.Runtime.InteropServices.BStrWrapper` | `VT_BSTR` |
| `sbyte`  | `VT_I1`  |     | `object`  | `VT_DISPATCH` |
| `short`  | `VT_I2`  |     | `System.Runtime.InteropServices.UnknownWrapper` | `VT_UNKNOWN` |
| `ushort` | `VT_UI2` |     | `System.Runtime.InteropServices.DispatchWrapper` | `VT_DISPATCH` |
| `int`    | `VT_I4`  |     | `System.Reflection.Missing` | `VT_ERROR` |
| `uint`   | `VT_UI4` |     | `(object)null` | `VT_EMPTY` |
| `long`   | `VT_I8`  |     | `bool` | `VT_BOOL` |
| `ulong`  | `VT_UI8` |     | `System.DateTime` | `VT_DATE` |
| `float`  | `VT_R4`  |     | `decimal` | `VT_DECIMAL` |
| `double` | `VT_R8`  |     | `System.Runtime.InteropServices.CurrencyWrapper` | `VT_CURRENCY` |
| `char`   | `VT_UI2` |     | `System.DBNull` | `VT_NULL` |
| `string` | `VT_BSTR`|