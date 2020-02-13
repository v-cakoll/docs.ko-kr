---
title: <inheritdoc> - C# 프로그래밍 가이드
ms.date: 01/21/2020
f1_keywords:
- inheritdoc
- <inheritdoc>
helpviewer_keywords:
- <inheritdoc> C# XML tag
- inheritdoc C# XML tag
ms.assetid: 46d329b1-5b84-4537-9e17-73ca97313e4e
ms.openlocfilehash: d660cb1739733c4e98ae0b7939476fe74e6cf200
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794836"
---
# <a name="inheritdoc-c-programming-guide"></a><span data-ttu-id="5c8e1-102">\<inheritdoc>(C# 프로그래밍 가이드)</span><span class="sxs-lookup"><span data-stu-id="5c8e1-102">\<inheritdoc> (C# Programming Guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="5c8e1-103">구문</span><span class="sxs-lookup"><span data-stu-id="5c8e1-103">Syntax</span></span>  
  
```xml  
<inheritdoc/> 
```  

## <a name="inheritdoc"></a><span data-ttu-id="5c8e1-104">InheritDoc</span><span class="sxs-lookup"><span data-stu-id="5c8e1-104">InheritDoc</span></span>

<span data-ttu-id="5c8e1-105">기본 클래스, 인터페이스 및 유사한 메서드에서 XML 주석을 상속합니다.</span><span class="sxs-lookup"><span data-stu-id="5c8e1-105">Inherit XML comments from base classes, interfaces, and similar methods.</span></span> <span data-ttu-id="5c8e1-106">이렇게 하면 중복 XML 주석을 복사하여 붙여 넣을 필요가 없으며 XML 주석이 자동으로 동기화됩니다.</span><span class="sxs-lookup"><span data-stu-id="5c8e1-106">This eliminates unwanted copying and pasting of duplicate XML comments and automatically keeps XML comments synchronized.</span></span> 
  
## <a name="remarks"></a><span data-ttu-id="5c8e1-107">설명</span><span class="sxs-lookup"><span data-stu-id="5c8e1-107">Remarks</span></span>  
<span data-ttu-id="5c8e1-108">기본 클래스 또는 인터페이스에 XML 주석을 추가하고 InheritDoc가 주석을 구현 클래스에 복사하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="5c8e1-108">Add your XML comments in base classes or interfaces and let InheritDoc copy the comments to implementing classes.</span></span>

<span data-ttu-id="5c8e1-109">동기 메서드에 XML 주석을 추가하고 InheritDoc가 동일한 메서드의 비동기 버전에 주석을 복사하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="5c8e1-109">Add your XML comments to your synchronous methods and let InheritDoc copy the comments to your asynchronous versions of the same methods.</span></span>  

<span data-ttu-id="5c8e1-110">특정 멤버에서 주석을 복사하려면 `cref` 특성을 사용하여 멤버를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5c8e1-110">If you want to copy the comments from a specific member you can use the `cref` attribute to specify the member.</span></span>
  
## <a name="examples"></a><span data-ttu-id="5c8e1-111">예</span><span class="sxs-lookup"><span data-stu-id="5c8e1-111">Examples</span></span>
[!code-csharp[csProgGuideDocComments#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#16)]  

[!code-csharp[csProgGuideDocComments#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#17)]  

## <a name="see-also"></a><span data-ttu-id="5c8e1-112">참조</span><span class="sxs-lookup"><span data-stu-id="5c8e1-112">See also</span></span>

- [<span data-ttu-id="5c8e1-113">C# 프로그래밍 가이드</span><span class="sxs-lookup"><span data-stu-id="5c8e1-113">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="5c8e1-114">문서 주석에 대한 권장 태그</span><span class="sxs-lookup"><span data-stu-id="5c8e1-114">Recommended Tags for Documentation Comments</span></span>](./recommended-tags-for-documentation-comments.md)