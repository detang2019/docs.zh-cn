---
title: IMetaDataImport::GetMemberProps 方法
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetMemberProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetMemberProps
helpviewer_keywords:
- IMetaDataImport::GetMemberProps method [.NET Framework metadata]
- GetMemberProps method [.NET Framework metadata]
ms.assetid: 42790918-4142-4938-b8f4-a56979a55846
topic_type:
- apiref
ms.openlocfilehash: bc5bbba2fa4a95955e52a2e083a2097178b5d96a
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437515"
---
# <a name="imetadataimportgetmemberprops-method"></a>IMetaDataImport::GetMemberProps 方法
获取存储在指定的成员定义的元数据中的信息，这些信息包括指定的元数据标记所引用的 <xref:System.Type> 成员的名称、二进制签名和相对虚拟地址。 这是一个简单的帮助器方法：如果*mb*是 MethodDef，则调用**GetMethodProps** ;如果*mb*是 FieldDef，则调用**GetFieldProps** 。 有关详细信息，请参阅其他方法。 
  
## <a name="syntax"></a>语法  
  
```cpp  
HRESULT GetMemberProps (  
   [in]  mdToken           mb,   
   [out] mdTypeDef         *pClass,  
   [out] LPWSTR            szMember,   
   [in]  ULONG             cchMember,   
   [out] ULONG             *pchMember,   
   [out] DWORD             *pdwAttr,  
   [out] PCCOR_SIGNATURE   *ppvSigBlob,   
   [out] ULONG             *pcbSigBlob,   
   [out] ULONG             *pulCodeRVA,   
   [out] DWORD             *pdwImplFlags,   
   [out] DWORD             *pdwCPlusTypeFlag,   
   [out] UVCP_CONSTANT     *ppValue,  
   [out] ULONG             *pcchValue  
);  
```  
  
## <a name="parameters"></a>参数  
 `mb`  
 中引用要为其获取关联元数据的成员的标记。  
  
 `pClass`  
 弄指向表示成员的类的元数据标记的指针。  
  
 `szMember`  
 弄成员的名称。  
  
 `cchMember`  
 中`szMember` 缓冲区的大小（以宽字符为大小）。  
  
 `pchMember`  
 弄返回名称的大小（以宽字符为大小）。  
  
 `pdwAttr`  
 弄应用于成员的任何标志值。  
  
 `ppvSigBlob`  
 弄指向成员的二进制元数据签名的指针。  
  
 `pcbSigBlob`  
 弄`ppvSigBlob`的大小（以字节为单位）。  
  
 `pulCodeRVA`  
 弄指向成员的相对虚拟地址的指针。  
  
 `pdwImplFlags`  
 弄与成员关联的任何方法实现标志。  
  
 `pdwCPlusTypeFlag`  
 弄标记 <xref:System.ValueType>的标志。 这是 `ELEMENT_TYPE_*` 值之一。
  
 `ppValue`  
 弄此成员返回的常量字符串值。  
  
 `pcchValue`  
 弄`ppValue`的大小（以字符为大小）; 如果 `ppValue` 不包含字符串，则为零。  
  
## <a name="requirements"></a>要求  
 **平台：** 请参阅[系统要求](../../../../docs/framework/get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为资源包括在 Mscoree.dll 中  
  
 **.NET Framework 版本：** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [IMetaDataImport 接口](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 接口](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
