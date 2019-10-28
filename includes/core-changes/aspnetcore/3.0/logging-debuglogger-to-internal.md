---
ms.openlocfilehash: 958dede03e1c15f69f4ee676f13713ff43c29e96
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394394"
---
### <a name="logging-debuglogger-class-made-internal"></a><span data-ttu-id="6ef4a-101">로깅: DebugLogger 클래스를 내부적으로 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-101">Logging: DebugLogger class made internal</span></span>

<span data-ttu-id="6ef4a-102">ASP.NET Core 3.0 이전에는 `DebugLogger`의 액세스 한정자가 `public`였습니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-102">Prior to ASP.NET Core 3.0, `DebugLogger`'s access modifier was `public`.</span></span> <span data-ttu-id="6ef4a-103">ASP.NET Core 3.0에서 액세스 한정자가 `internal`로 변경되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-103">In ASP.NET Core 3.0, the access modifier changed to `internal`.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="6ef4a-104">도입된 버전</span><span class="sxs-lookup"><span data-stu-id="6ef4a-104">Version introduced</span></span>

<span data-ttu-id="6ef4a-105">3.0</span><span class="sxs-lookup"><span data-stu-id="6ef4a-105">3.0</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="6ef4a-106">변경 이유</span><span class="sxs-lookup"><span data-stu-id="6ef4a-106">Reason for change</span></span>

<span data-ttu-id="6ef4a-107">변경 내용은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-107">The change is being made to:</span></span>

* <span data-ttu-id="6ef4a-108">`ConsoleLogger`와 같은 다른 로거 구현과 일관성을 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-108">Enforce consistency with other logger implementations such as `ConsoleLogger`.</span></span>
* <span data-ttu-id="6ef4a-109">API 표면을 줄입니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-109">Reduce the API surface.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="6ef4a-110">권장 작업</span><span class="sxs-lookup"><span data-stu-id="6ef4a-110">Recommended action</span></span>

<span data-ttu-id="6ef4a-111"><xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions.AddDebug%2A> `ILoggingBuilder` 확장 메서드를 사용하여 디버그 로깅을 사용하도록 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-111">Use the <xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions.AddDebug%2A> `ILoggingBuilder` extension method to enable debug logging.</span></span> <span data-ttu-id="6ef4a-112">서비스를 수동으로 동록해야 하는 경우에도 <xref:Microsoft.Extensions.Logging.Debug.DebugLoggerProvider>는 여전히 `public`입니다.</span><span class="sxs-lookup"><span data-stu-id="6ef4a-112"><xref:Microsoft.Extensions.Logging.Debug.DebugLoggerProvider> is also still `public` in the event the service needs to be registered manually.</span></span>

#### <a name="category"></a><span data-ttu-id="6ef4a-113">범주</span><span class="sxs-lookup"><span data-stu-id="6ef4a-113">Category</span></span>

<span data-ttu-id="6ef4a-114">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6ef4a-114">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="6ef4a-115">영향을 받는 API</span><span class="sxs-lookup"><span data-stu-id="6ef4a-115">Affected APIs</span></span>

<xref:Microsoft.Extensions.Logging.Debug.DebugLogger?displayProperty=nameWithType>

<!--

#### Affected APIs

`T:Microsoft.Extensions.Logging.Debug.DebugLogger`

-->