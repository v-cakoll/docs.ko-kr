---
title: Newtonsoft.json에서 System.object로 마이그레이션-.NET
author: tdykstra
ms.author: tdykstra
ms.date: 01/10/2020
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 8b3ffc885691264548a19f694d159ce07aba7550
ms.sourcegitcommit: dfad244ba549702b649bfef3bb057e33f24a8fb2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75904697"
---
# <a name="how-to-migrate-from-newtonsoftjson-to-systemtextjson"></a><span data-ttu-id="c6264-102">Newtonsoft.json에서 System.object로 마이그레이션하는 방법</span><span class="sxs-lookup"><span data-stu-id="c6264-102">How to migrate from Newtonsoft.Json to System.Text.Json</span></span>

<span data-ttu-id="c6264-103">이 문서에서는 [newtonsoft.json](https://www.newtonsoft.com/json) 에서 <xref:System.Text.Json>로 마이그레이션하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-103">This article shows how to migrate from [Newtonsoft.Json](https://www.newtonsoft.com/json) to <xref:System.Text.Json>.</span></span>

 <span data-ttu-id="c6264-104">`System.Text.Json`는 주로 성능, 보안 및 표준 준수에 중점을 둘 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-104">`System.Text.Json` focuses primarily on performance, security, and standards compliance.</span></span> <span data-ttu-id="c6264-105">기본 동작에는 몇 가지 주요 차이점이 있으며 `Newtonsoft.Json`기능 패리티를 목표로 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-105">It has some key differences in default behavior and doesn't aim to have feature parity with `Newtonsoft.Json`.</span></span> <span data-ttu-id="c6264-106">일부 시나리오에서는 `System.Text.Json`에 기본 제공 기능이 없지만 권장 해결 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-106">For some scenarios, `System.Text.Json` has no built-in functionality, but there are recommended workarounds.</span></span> <span data-ttu-id="c6264-107">다른 시나리오의 경우 해결 방법은 실용적이 지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-107">For other scenarios, workarounds are impractical.</span></span> <span data-ttu-id="c6264-108">응용 프로그램이 누락 된 기능에 종속 된 경우 시나리오에 대 한 지원을 추가할 수 있는지 확인 하는 [문제](https://github.com/dotnet/runtime/issues/new) 를 확인 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-108">If your application depends on a missing feature, consider [filing an issue](https://github.com/dotnet/runtime/issues/new) to find out if support for your scenario can be added.</span></span>

<!-- For information about which features might be added in future releases, see the [Roadmap](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md). [Restore this when the roadmap is updated.]-->

<span data-ttu-id="c6264-109">이 문서의 대부분은 <xref:System.Text.Json.JsonSerializer> API를 사용 하는 방법에 대 한 것 이지만 <xref:System.Text.Json.JsonDocument> (문서 개체 모델 또는 DOM을 나타냄), <xref:System.Text.Json.Utf8JsonReader>및 <xref:System.Text.Json.Utf8JsonWriter> 유형을 사용 하는 방법에 대 한 지침도 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-109">Most of this article is about how to use the <xref:System.Text.Json.JsonSerializer> API, but it also includes guidance on how to use the <xref:System.Text.Json.JsonDocument> (which represents the Document Object Model or DOM), <xref:System.Text.Json.Utf8JsonReader>, and <xref:System.Text.Json.Utf8JsonWriter> types.</span></span> <span data-ttu-id="c6264-110">문서는 다음과 같은 순서로 섹션으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-110">The article is organized into sections in the following order:</span></span>

* [<span data-ttu-id="c6264-111">Newtonsoft.json에 비해 **기본** JsonSerializer 동작의 차이점</span><span class="sxs-lookup"><span data-stu-id="c6264-111">Differences in **default** JsonSerializer behavior compared to Newtonsoft.Json</span></span>](#differences-in-default-jsonserializer-behavior-compared-to-newtonsoftjson)
* [<span data-ttu-id="c6264-112">해결 방법이 필요한 JsonSerializer를 사용 하는 시나리오</span><span class="sxs-lookup"><span data-stu-id="c6264-112">Scenarios using JsonSerializer that require workarounds</span></span>](#scenarios-using-jsonserializer-that-require-workarounds)
* [<span data-ttu-id="c6264-113">JsonSerializer 현재 지원 하지 않는 시나리오</span><span class="sxs-lookup"><span data-stu-id="c6264-113">Scenarios that JsonSerializer currently doesn't support</span></span>](#scenarios-that-jsonserializer-currently-doesnt-support)
* [<span data-ttu-id="c6264-114">JToken과 같은 JsonDocument 및 JsonElement (예: Jtoken, Jtoken)</span><span class="sxs-lookup"><span data-stu-id="c6264-114">JsonDocument and JsonElement compared to JToken (like JObject, JArray)</span></span>](#jsondocument-and-jsonelement-compared-to-jtoken-like-jobject-jarray)
* [<span data-ttu-id="c6264-115">Utf8JsonReader와 JsonTextReader 비교</span><span class="sxs-lookup"><span data-stu-id="c6264-115">Utf8JsonReader compared to JsonTextReader</span></span>](#utf8jsonreader-compared-to-jsontextreader)
* [<span data-ttu-id="c6264-116">Utf8JsonWriter와 JsonTextWriter 비교</span><span class="sxs-lookup"><span data-stu-id="c6264-116">Utf8JsonWriter compared to JsonTextWriter</span></span>](#utf8jsonwriter-compared-to-jsontextwriter)

## <a name="differences-in-default-jsonserializer-behavior-compared-to-newtonsoftjson"></a><span data-ttu-id="c6264-117">Newtonsoft.json에 비해 기본 JsonSerializer 동작의 차이점</span><span class="sxs-lookup"><span data-stu-id="c6264-117">Differences in default JsonSerializer behavior compared to Newtonsoft.Json</span></span>

<span data-ttu-id="c6264-118"><xref:System.Text.Json>은 기본적으로 엄격 하며, 호출자를 대신 하 여 추측 또는 해석을 방지 하 여 결정적 동작을 강조 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-118"><xref:System.Text.Json> is strict by default and avoids any guessing or interpretation on the caller's behalf, emphasizing deterministic behavior.</span></span> <span data-ttu-id="c6264-119">라이브러리는 이러한 방식으로 성능 및 보안을 위해 의도적으로 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-119">The library is intentionally designed this way for performance and security.</span></span> <span data-ttu-id="c6264-120">`Newtonsoft.Json`은 기본적으로 유연 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-120">`Newtonsoft.Json` is flexible by default.</span></span> <span data-ttu-id="c6264-121">이러한 기본적인 디자인의 차이점은 기본 동작의 다음과 같은 몇 가지 중요 한 차이점입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-121">This fundamental difference in design is behind many of the following specific differences in default behavior.</span></span>

### <a name="case-insensitive-deserialization"></a><span data-ttu-id="c6264-122">대/소문자를 구분 하지 않는 deserialization</span><span class="sxs-lookup"><span data-stu-id="c6264-122">Case-insensitive deserialization</span></span> 

<span data-ttu-id="c6264-123">Deserialization을 수행 하는 동안 `Newtonsoft.Json`는 기본적으로 대/소문자를 구분 하지 않는 속성 이름을 비교 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-123">During deserialization, `Newtonsoft.Json` does case-insensitive property name matching by default.</span></span> <span data-ttu-id="c6264-124"><xref:System.Text.Json> 기본값은 대/소문자를 구분 하며,이는 정확히 일치 하는 항목을 수행 하기 때문에 더 나은 성능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-124">The <xref:System.Text.Json> default is case-sensitive, which gives better performance since it's doing an exact match.</span></span> <span data-ttu-id="c6264-125">대/소문자를 구분 하지 않는 일치를 수행 하는 방법은 대/소문자를 구분 하지 않는 [속성 일치](system-text-json-how-to.md#case-insensitive-property-matching)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-125">For information about how to do case-insensitive matching, see [Case-insensitive property matching](system-text-json-how-to.md#case-insensitive-property-matching).</span></span>

<span data-ttu-id="c6264-126">ASP.NET Core를 사용 하 여 간접적으로 `System.Text.Json`를 사용 하는 경우 `Newtonsoft.Json`와 같은 동작을 수행 하기 위해 아무것도 수행할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-126">If you're using `System.Text.Json` indirectly by using ASP.NET Core, you don't need to do anything to get behavior like `Newtonsoft.Json`.</span></span> <span data-ttu-id="c6264-127">ASP.NET Core는 [카멜식 대/소문자 속성 이름](system-text-json-how-to.md#use-camel-case-for-all-json-property-names) 및 대/소문자를 구분 하지 않는 일치를 `System.Text.Json`사용 하는 경우 해당 설정을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-127">ASP.NET Core specifies the settings for [camel-casing property names](system-text-json-how-to.md#use-camel-case-for-all-json-property-names) and case-insensitive matching when it uses `System.Text.Json`.</span></span>

### <a name="comments"></a><span data-ttu-id="c6264-128">설명</span><span class="sxs-lookup"><span data-stu-id="c6264-128">Comments</span></span>

<span data-ttu-id="c6264-129">Deserialization을 수행 하는 동안 `Newtonsoft.Json`는 기본적으로 JSON의 주석을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-129">During deserialization, `Newtonsoft.Json` ignores comments in the JSON by default.</span></span> <span data-ttu-id="c6264-130"><xref:System.Text.Json> 기본값은 [RFC 8259](https://tools.ietf.org/html/rfc8259) 사양에 포함 되지 않기 때문에 주석에 대 한 예외를 throw 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-130">The <xref:System.Text.Json> default is to throw exceptions for comments because the [RFC 8259](https://tools.ietf.org/html/rfc8259) specification doesn't include them.</span></span> <span data-ttu-id="c6264-131">주석을 허용 하는 방법에 대 한 자세한 내용은 [주석 및 후행 쉼표 허용](system-text-json-how-to.md#allow-comments-and-trailing-commas)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-131">For information about how to allow comments, see [Allow comments and trailing commas](system-text-json-how-to.md#allow-comments-and-trailing-commas).</span></span>

### <a name="trailing-commas"></a><span data-ttu-id="c6264-132">후행 쉼표</span><span class="sxs-lookup"><span data-stu-id="c6264-132">Trailing commas</span></span>

<span data-ttu-id="c6264-133">Deserialization을 수행 하는 동안 `Newtonsoft.Json`는 기본적으로 후행 쉼표를 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-133">During deserialization, `Newtonsoft.Json` ignores trailing commas by default.</span></span> <span data-ttu-id="c6264-134">또한 여러 개의 후행 쉼표를 무시 합니다 (예: `[{"Color":"Red"},{"Color":"Green"},,]`).</span><span class="sxs-lookup"><span data-stu-id="c6264-134">It also ignores multiple trailing commas (for example, `[{"Color":"Red"},{"Color":"Green"},,]`).</span></span> <span data-ttu-id="c6264-135"><xref:System.Text.Json> 기본값은 [RFC 8259](https://tools.ietf.org/html/rfc8259) 사양에서 허용 하지 않으므로 후행 쉼표에 대 한 예외를 throw 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-135">The <xref:System.Text.Json> default is to throw exceptions for trailing commas because the [RFC 8259](https://tools.ietf.org/html/rfc8259) specification doesn't allow them.</span></span> <span data-ttu-id="c6264-136">`System.Text.Json` 허용 하는 방법에 대 한 자세한 내용은 [주석 및 후행 쉼표 허용](system-text-json-how-to.md#allow-comments-and-trailing-commas)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-136">For information about how to make `System.Text.Json` accept them, see [Allow comments and trailing commas](system-text-json-how-to.md#allow-comments-and-trailing-commas).</span></span> <span data-ttu-id="c6264-137">후행 쉼표를 여러 개 허용 하는 방법은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-137">There's no way to allow multiple trailing commas.</span></span>

### <a name="json-strings-property-names-and-string-values"></a><span data-ttu-id="c6264-138">JSON 문자열 (속성 이름 및 문자열 값)</span><span class="sxs-lookup"><span data-stu-id="c6264-138">JSON strings (property names and string values)</span></span>

<span data-ttu-id="c6264-139">Deserialization을 수행 하는 동안 `Newtonsoft.Json` 큰따옴표, 작은따옴표 또는 따옴표 없이 속성 이름을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-139">During deserialization, `Newtonsoft.Json` accepts property names surrounded by double quotes, single quotes, or without quotes.</span></span> <span data-ttu-id="c6264-140">큰따옴표 또는 작은따옴표로 묶은 문자열 값을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-140">It accepts string values surrounded by double quotes or single quotes.</span></span> <span data-ttu-id="c6264-141">예를 들어 `Newtonsoft.Json`는 다음 JSON을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-141">For example, `Newtonsoft.Json` accepts the following JSON:</span></span>

```json
{
  "name1": "value",
  'name2': "value",
  name3: 'value'
}
```

<span data-ttu-id="c6264-142">`System.Text.Json`는 [RFC 8259](https://tools.ietf.org/html/rfc8259) 사양에서 필요 하며 유효한 JSON으로 간주 되는 유일한 형식 이므로 속성 이름과 문자열 값을 큰따옴표로만 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-142">`System.Text.Json` only accepts property names and string values in double quotes because that format is required by the [RFC 8259](https://tools.ietf.org/html/rfc8259) specification and is the only format considered valid JSON.</span></span>

<span data-ttu-id="c6264-143">작은따옴표로 묶인 값을 사용 하면 다음과 같은 메시지와 함께 [JsonException](xref:System.Text.Json.JsonException) 이 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-143">A value enclosed in single quotes results in a [JsonException](xref:System.Text.Json.JsonException) with the following message:</span></span>

```
''' is an invalid start of a value.
```

### <a name="non-string-values-for-string-properties"></a><span data-ttu-id="c6264-144">문자열 속성에 대 한 문자열이 아닌 값</span><span class="sxs-lookup"><span data-stu-id="c6264-144">Non-string values for string properties</span></span>

<span data-ttu-id="c6264-145">문자열 형식의 속성에 대 한 deserialization을 위해 숫자, 리터럴 `true` 및 `false`와 같은 문자열이 아닌 값을 `Newtonsoft.Json` 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-145">`Newtonsoft.Json` accepts non-string values, such as a number or the literals `true` and `false`, for deserialization to properties of type string.</span></span> <span data-ttu-id="c6264-146">다음 클래스를 성공적으로 deserialize `Newtonsoft.Json` JSON의 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-146">Here's an example of JSON that `Newtonsoft.Json` successfully deserializes to the following class:</span></span>

```json
{
  "String1": 1,
  "String2": true,
  "String3": false
}
```

```csharp
public class ExampleClass
{
    public string String1 { get; set; }
    public string String2 { get; set; }
    public string String3 { get; set; }
}
```

<span data-ttu-id="c6264-147">`System.Text.Json`은 문자열이 아닌 값을 문자열 속성으로 deserialize 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-147">`System.Text.Json` doesn't deserialize non-string values into string properties.</span></span> <span data-ttu-id="c6264-148">문자열 필드에 대해 문자열이 아닌 값을 받으면 다음과 같은 메시지와 함께 [JsonException](xref:System.Text.Json.JsonException) 이 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-148">A non-string value received for a string field results in a [JsonException](xref:System.Text.Json.JsonException) with the following message:</span></span>

```
The JSON value could not be converted to System.String.
```

### <a name="converter-registration-precedence"></a><span data-ttu-id="c6264-149">변환기 등록 우선 순위</span><span class="sxs-lookup"><span data-stu-id="c6264-149">Converter registration precedence</span></span>

<span data-ttu-id="c6264-150">사용자 지정 변환기에 대 한 `Newtonsoft.Json` 등록 우선 순위는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-150">The `Newtonsoft.Json` registration precedence for custom converters is as follows:</span></span>

* <span data-ttu-id="c6264-151">속성의 특성</span><span class="sxs-lookup"><span data-stu-id="c6264-151">Attribute on property</span></span>
* <span data-ttu-id="c6264-152">형식의 특성</span><span class="sxs-lookup"><span data-stu-id="c6264-152">Attribute on type</span></span>
* <span data-ttu-id="c6264-153">[변환기](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonSerializerSettings_Converters.htm) 컬렉션</span><span class="sxs-lookup"><span data-stu-id="c6264-153">[Converters](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonSerializerSettings_Converters.htm) collection</span></span>

<span data-ttu-id="c6264-154">이 순서는 `Converters` 컬렉션의 사용자 지정 변환기가 형식 수준에서 특성을 적용 하 여 등록 된 변환기에 의해 재정의 됨을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-154">This order means that a custom converter in the `Converters` collection is overridden by a converter that is registered by applying an attribute at the type level.</span></span> <span data-ttu-id="c6264-155">이러한 등록은 모두 속성 수준에서 특성에 의해 재정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-155">Both of those registrations are overridden by an attribute at the property level.</span></span>

<span data-ttu-id="c6264-156">사용자 지정 변환기에 대 한 <xref:System.Text.Json> 등록 우선 순위는 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-156">The <xref:System.Text.Json> registration precedence for custom converters is different:</span></span>

* <span data-ttu-id="c6264-157">속성의 특성</span><span class="sxs-lookup"><span data-stu-id="c6264-157">Attribute on property</span></span>
* <span data-ttu-id="c6264-158"><xref:System.Text.Json.JsonSerializerOptions.Converters> 컬렉션</span><span class="sxs-lookup"><span data-stu-id="c6264-158"><xref:System.Text.Json.JsonSerializerOptions.Converters> collection</span></span>
* <span data-ttu-id="c6264-159">형식의 특성</span><span class="sxs-lookup"><span data-stu-id="c6264-159">Attribute on type</span></span>

<span data-ttu-id="c6264-160">여기서 차이점은 `Converters` 컬렉션의 사용자 지정 변환기가 형식 수준에서 특성을 재정의 한다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-160">The difference here is that a custom converter in the `Converters` collection overrides an attribute at the type level.</span></span> <span data-ttu-id="c6264-161">이 우선 순위를 설정 하는 것은 런타임 변경 시 디자인 타임 선택 항목을 재정의 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-161">The intention behind this order of precedence is to make run-time changes override design-time choices.</span></span> <span data-ttu-id="c6264-162">우선 순위를 변경할 수 있는 방법은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-162">There's no way to change the precedence.</span></span>

<span data-ttu-id="c6264-163">사용자 지정 변환기 등록에 대 한 자세한 내용은 [사용자 지정 변환기 등록](system-text-json-converters-how-to.md#register-a-custom-converter)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-163">For more information about custom converter registration, see [Register a custom converter](system-text-json-converters-how-to.md#register-a-custom-converter).</span></span>

### <a name="character-escaping"></a><span data-ttu-id="c6264-164">문자 이스케이프</span><span class="sxs-lookup"><span data-stu-id="c6264-164">Character escaping</span></span>

<span data-ttu-id="c6264-165">Serialization 중에는 문자를 이스케이프 하지 않고 문자를 허용 하는 것에 대 한 `Newtonsoft.Json` 비교적 허용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-165">During serialization, `Newtonsoft.Json` is relatively permissive about letting characters through without escaping them.</span></span> <span data-ttu-id="c6264-166">즉, `xxxx` 문자 코드 포인트 인 `\uxxxx`로 대체 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-166">That is, it doesn't replace them with `\uxxxx` where `xxxx` is the character's code point.</span></span> <span data-ttu-id="c6264-167">이를 이스케이프 처리 하는 경우에는 문자 앞에 `\`을 내보내면 됩니다. 예를 들어 `"` `\"`됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-167">Where it does escape them, it does so by emitting a `\` before the character (for example, `"` becomes `\"`).</span></span> <span data-ttu-id="c6264-168"><xref:System.Text.Json>은 기본적으로 더 많은 문자를 이스케이프 하 여 XSS (교차 사이트 스크립팅) 또는 정보 공개 공격에 대해 심층 방어 보호를 제공 하며,이를 위해 6 문자 시퀀스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-168"><xref:System.Text.Json> escapes more characters by default to provide defense-in-depth protections against cross-site scripting (XSS) or information-disclosure attacks and does so by using the six-character sequence.</span></span> <span data-ttu-id="c6264-169">`System.Text.Json`은 기본적으로 ASCII가 아닌 모든 문자를 이스케이프 하므로 `Newtonsoft.Json`에서 `StringEscapeHandling.EscapeNonAscii`를 사용 하는 경우에는 아무것도 수행할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-169">`System.Text.Json` escapes all non-ASCII characters by default, so you don't need to do anything if you're using `StringEscapeHandling.EscapeNonAscii` in `Newtonsoft.Json`.</span></span> <span data-ttu-id="c6264-170">또한 `System.Text.Json`는 기본적으로 HTML 구분 문자를 이스케이프 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-170">`System.Text.Json` also escapes HTML-sensitive characters, by default.</span></span> <span data-ttu-id="c6264-171">기본 `System.Text.Json` 동작을 재정의 하는 방법에 대 한 자세한 내용은 [문자 인코딩 사용자 지정](system-text-json-how-to.md#customize-character-encoding)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-171">For information about how to override the default `System.Text.Json` behavior, see [Customize character encoding](system-text-json-how-to.md#customize-character-encoding).</span></span>

### <a name="deserialization-of-object-properties"></a><span data-ttu-id="c6264-172">개체 속성 Deserialization</span><span class="sxs-lookup"><span data-stu-id="c6264-172">Deserialization of object properties</span></span>

<span data-ttu-id="c6264-173">`Newtonsoft.Json`에서 POCOs의 속성 또는 `Dictionary<string, object>`형식의 사전에 `object` 속성을 deserialize 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-173">When `Newtonsoft.Json` deserializes to `object` properties in POCOs or in dictionaries of type `Dictionary<string, object>`, it:</span></span>

* <span data-ttu-id="c6264-174">JSON 페이로드의 기본 값 형식 (`null`제외)을 유추 하 고 저장 된 `string`, `long`, `double`, `boolean`또는 `DateTime`를 boxed 개체로 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-174">Infers the type of primitive values in the JSON payload (other than `null`) and returns the stored `string`, `long`, `double`, `boolean`, or `DateTime` as a boxed object.</span></span> <span data-ttu-id="c6264-175">*기본 값* 은 json number, string, `true`, `false`, `null`등의 단일 json 값입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-175">*Primitive values* are single JSON values such as a JSON number, string, `true`, `false`, or `null`.</span></span>
* <span data-ttu-id="c6264-176">JSON 페이로드의 복합 값에 대 한 `JObject` 또는 `JArray`을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-176">Returns a `JObject` or `JArray` for complex values in the JSON payload.</span></span> <span data-ttu-id="c6264-177">*복합 값* 은 중괄호 (`{}`) 내에 있는 JSON 키-값 쌍의 컬렉션 이거나 대괄호 안에 있는 값 목록 (`[]`)입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-177">*Complex values* are collections of JSON key-value pairs within braces (`{}`) or lists of values within brackets (`[]`).</span></span> <span data-ttu-id="c6264-178">중괄호 또는 대괄호 안의 속성 및 값에는 추가 속성이 나 값이 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-178">The properties and values within the braces or brackets can have additional properties or values.</span></span>
* <span data-ttu-id="c6264-179">페이로드에 `null` JSON 리터럴이 있는 경우 null 참조를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-179">Returns a null reference when the payload has the `null` JSON literal.</span></span>

<span data-ttu-id="c6264-180"><xref:System.Text.Json>은 `System.Object` 속성 또는 사전 값 내에서 기본 값과 복합 값 모두에 대해 boxed `JsonElement`를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-180"><xref:System.Text.Json> stores a boxed `JsonElement` for both primitive and complex values within the `System.Object` property or dictionary value.</span></span> <span data-ttu-id="c6264-181">그러나 `Newtonsoft.Json`와 동일 하 게 `null`를 처리 하 고 페이로드에 `null` JSON 리터럴이 있는 경우 null 참조를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-181">However, it treats `null` the same as `Newtonsoft.Json` and returns a null reference when the payload has the `null` JSON literal in it.</span></span>

<span data-ttu-id="c6264-182">`object` 속성에 대 한 형식 유추를 구현 하려면 [사용자 지정 변환기를 작성 하는 방법](system-text-json-converters-how-to.md#deserialize-inferred-types-to-object-properties)의 예제와 같은 변환기를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-182">To implement type inference for `object` properties, create a converter like the example in [How to write custom converters](system-text-json-converters-how-to.md#deserialize-inferred-types-to-object-properties).</span></span>

### <a name="maximum-depth"></a><span data-ttu-id="c6264-183">최대 깊이</span><span class="sxs-lookup"><span data-stu-id="c6264-183">Maximum depth</span></span>

<span data-ttu-id="c6264-184">`Newtonsoft.Json`은 기본적으로 최대 깊이 제한이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-184">`Newtonsoft.Json` doesn't have a maximum depth limit by default.</span></span> <span data-ttu-id="c6264-185"><xref:System.Text.Json>의 경우 기본 제한은 64이 고 <xref:System.Text.Json.JsonSerializerOptions.MaxDepth?displayProperty=nameWithType>을 설정 하 여 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-185">For <xref:System.Text.Json> there's a default limit  of 64, and it's configurable by setting <xref:System.Text.Json.JsonSerializerOptions.MaxDepth?displayProperty=nameWithType>.</span></span>

### <a name="stack-type-handling"></a><span data-ttu-id="c6264-186">스택 형식 처리</span><span class="sxs-lookup"><span data-stu-id="c6264-186">Stack type handling</span></span>

<span data-ttu-id="c6264-187"><xref:System.Text.Json>에서 스택 내용의 순서는 serialize 될 때 반전 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-187">In <xref:System.Text.Json>, the order of a stack's contents is reversed when it's serialized.</span></span> <span data-ttu-id="c6264-188">이 동작은 다음 형식과 인터페이스에서 파생 되는 사용자 정의 형식에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-188">This behavior applies to the following types and interface and user-defined types that derive from them:</span></span>

* <xref:System.Collections.Stack>
* <xref:System.Collections.Generic.Stack%601>
* <xref:System.Collections.Immutable.ImmutableStack%601>
* <xref:System.Collections.Immutable.IImmutableStack%601>

<span data-ttu-id="c6264-189">스택 내용을 동일한 순서로 유지 하기 위해 사용자 지정 변환기를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-189">A custom converter could be implemented to keep stack contents in the same order.</span></span>

### <a name="omit-null-value-properties"></a><span data-ttu-id="c6264-190">Null 값 속성 생략</span><span class="sxs-lookup"><span data-stu-id="c6264-190">Omit null-value properties</span></span>

<span data-ttu-id="c6264-191">`Newtonsoft.Json`에는 null 값 속성이 serialization에서 제외 되도록 하는 전역 설정이 있습니다. [Nullvaluehandling. 무시](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_NullValueHandling.htm)합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-191">`Newtonsoft.Json` has a global setting that causes null-value properties to be excluded from serialization: [NullValueHandling.Ignore](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_NullValueHandling.htm).</span></span> <span data-ttu-id="c6264-192"><xref:System.Text.Json>의 해당 옵션은 <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues%2A>입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-192">The corresponding option in <xref:System.Text.Json> is <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues%2A>.</span></span>

## <a name="scenarios-using-jsonserializer-that-require-workarounds"></a><span data-ttu-id="c6264-193">해결 방법이 필요한 JsonSerializer를 사용 하는 시나리오</span><span class="sxs-lookup"><span data-stu-id="c6264-193">Scenarios using JsonSerializer that require workarounds</span></span>

<span data-ttu-id="c6264-194">다음 시나리오는 기본 제공 기능에서 지원 되지 않지만 문제 해결을 위해 샘플 코드가 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-194">The following scenarios aren't supported by built-in functionality, but sample code is provided for workarounds.</span></span> <span data-ttu-id="c6264-195">대부분의 해결 방법은 [사용자 지정 변환기](system-text-json-converters-how-to.md)를 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-195">Most of the workarounds require that you implement [custom converters](system-text-json-converters-how-to.md).</span></span>

### <a name="specify-date-format"></a><span data-ttu-id="c6264-196">날짜 형식 지정</span><span class="sxs-lookup"><span data-stu-id="c6264-196">Specify date format</span></span>

<span data-ttu-id="c6264-197">`Newtonsoft.Json` `DateTime` 및 `DateTimeOffset` 형식의 속성을 serialize 및 deserialize 하는 방법을 제어 하는 여러 가지 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-197">`Newtonsoft.Json` provides several ways to control how properties of `DateTime` and `DateTimeOffset` types are serialized and deserialized:</span></span>

* <span data-ttu-id="c6264-198">`DateTimeZoneHandling` 설정을 사용 하 여 모든 `DateTime` 값을 UTC 날짜로 serialize 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-198">The `DateTimeZoneHandling` setting can be used to serialize all `DateTime` values as UTC dates.</span></span>
* <span data-ttu-id="c6264-199">`DateFormatString` 설정 및 `DateTime` 변환기를 사용 하 여 날짜 문자열의 형식을 사용자 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-199">The `DateFormatString` setting and `DateTime` converters can be used to customize the format of date strings.</span></span>

<span data-ttu-id="c6264-200"><xref:System.Text.Json>기본적으로 지원 되는 유일한 형식은 ISO 8601-1:2019입니다 .이는 널리 사용 되 고 명확 하 게 도입 되었으며 라운드트립을 정확 하 게 수행 하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-200">In <xref:System.Text.Json>, the only format that has built-in support is ISO 8601-1:2019 since it's widely adopted, unambiguous, and makes round trips precisely.</span></span> <span data-ttu-id="c6264-201">다른 형식을 사용 하려면 사용자 지정 변환기를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-201">To use any other format, create a custom converter.</span></span> <span data-ttu-id="c6264-202">자세한 내용은 [system.object의 DateTime 및 DateTimeOffset 지원](../datetime/system-text-json-support.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-202">For more information, see [DateTime and DateTimeOffset support in System.Text.Json](../datetime/system-text-json-support.md).</span></span>

### <a name="quoted-numbers"></a><span data-ttu-id="c6264-203">따옴표 붙은 숫자</span><span class="sxs-lookup"><span data-stu-id="c6264-203">Quoted numbers</span></span>

<span data-ttu-id="c6264-204">JSON 문자열로 표시 되는 숫자를 직렬화 하거나 deserialize 할 수 `Newtonsoft.Json` (따옴표로 묶인).</span><span class="sxs-lookup"><span data-stu-id="c6264-204">`Newtonsoft.Json` can serialize or deserialize numbers represented by JSON strings (surrounded by quotes).</span></span> <span data-ttu-id="c6264-205">예를 들어, `{"DegreesCelsius":23}`대신 `{"DegreesCelsius":"23"}`를 수락할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-205">For example, it can accept: `{"DegreesCelsius":"23"}` instead of `{"DegreesCelsius":23}`.</span></span> <span data-ttu-id="c6264-206"><xref:System.Text.Json>에서 해당 동작을 사용 하도록 설정 하려면 다음 예제와 같이 사용자 지정 변환기를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-206">To enable that behavior in <xref:System.Text.Json>, implement a custom converter like the following example.</span></span> <span data-ttu-id="c6264-207">변환기는 `long`으로 정의 된 속성을 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-207">The converter handles properties defined as `long`:</span></span>

* <span data-ttu-id="c6264-208">JSON 문자열로 serialize 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-208">It serializes them as JSON strings.</span></span> 
* <span data-ttu-id="c6264-209">Deserialize 하는 동안 따옴표 안의 JSON 숫자 및 숫자를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-209">It accepts JSON numbers and numbers within quotes while deserializing.</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/LongToStringConverter.cs)]

<span data-ttu-id="c6264-210">개별 `long` 속성에서 [특성을 사용](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property) 하거나 <xref:System.Text.Json.JsonSerializerOptions.Converters> 컬렉션에 [변환기를 추가](system-text-json-converters-how-to.md#registration-sample---converters-collection) 하 여이 사용자 지정 변환기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-210">Register this custom converter by [using an attribute](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property) on individual `long` properties or by [adding the converter](system-text-json-converters-how-to.md#registration-sample---converters-collection) to the <xref:System.Text.Json.JsonSerializerOptions.Converters> collection.</span></span>

### <a name="dictionary-with-non-string-key"></a><span data-ttu-id="c6264-211">문자열이 아닌 키를 포함 하는 사전</span><span class="sxs-lookup"><span data-stu-id="c6264-211">Dictionary with non-string key</span></span>

<span data-ttu-id="c6264-212">`Newtonsoft.Json`는 `Dictionary<TKey, TValue>`형식의 컬렉션을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-212">`Newtonsoft.Json` supports collections of type `Dictionary<TKey, TValue>`.</span></span> <span data-ttu-id="c6264-213"><xref:System.Text.Json>의 사전 컬렉션에 대 한 기본 제공 지원은 `Dictionary<string, TValue>`으로 제한 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-213">The built-in support for dictionary collections in <xref:System.Text.Json> is limited to `Dictionary<string, TValue>`.</span></span> <span data-ttu-id="c6264-214">즉, 키는 문자열 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-214">That is, the key must be a string.</span></span>

<span data-ttu-id="c6264-215">정수 또는 다른 형식을 키로 사용 하 여 사전을 지원 하려면 [사용자 지정 변환기를 작성 하는 방법](system-text-json-converters-how-to.md#support-dictionary-with-non-string-key)의 예제와 같은 변환기를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-215">To support a dictionary with an integer or some other type as the key, create a converter like the example in [How to write custom converters](system-text-json-converters-how-to.md#support-dictionary-with-non-string-key).</span></span>

### <a name="polymorphic-serialization"></a><span data-ttu-id="c6264-216">다형 serialization</span><span class="sxs-lookup"><span data-stu-id="c6264-216">Polymorphic serialization</span></span>

<span data-ttu-id="c6264-217">`Newtonsoft.Json`는 다형성 serialization을 자동으로 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-217">`Newtonsoft.Json` automatically does polymorphic serialization.</span></span> <span data-ttu-id="c6264-218"><xref:System.Text.Json>의 제한 된 다형성 serialization 기능에 대 한 자세한 내용은 [파생 클래스의 속성 직렬화](system-text-json-how-to.md#serialize-properties-of-derived-classes)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-218">For information about the limited polymorphic serialization capabilities of <xref:System.Text.Json>, see [Serialize properties of derived classes](system-text-json-how-to.md#serialize-properties-of-derived-classes).</span></span>

<span data-ttu-id="c6264-219">`object`형식으로 파생 클래스를 포함할 수 있는 속성을 정의 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-219">The workaround described there is to define properties that may contain derived classes as type `object`.</span></span> <span data-ttu-id="c6264-220">이렇게 할 수 없는 경우 [사용자 지정 변환기를 작성 하는 방법](system-text-json-converters-how-to.md#support-polymorphic-deserialization)의 예제와 같이 전체 상속 형식 계층 구조에 대 한 `Write` 메서드를 사용 하 여 변환기를 만드는 것이 또 다른 옵션입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-220">If that isn't possible, another option is to create a converter with a `Write` method for the whole inheritance type hierarchy like the example in [How to write custom converters](system-text-json-converters-how-to.md#support-polymorphic-deserialization).</span></span>

### <a name="polymorphic-deserialization"></a><span data-ttu-id="c6264-221">다형 deserialization</span><span class="sxs-lookup"><span data-stu-id="c6264-221">Polymorphic deserialization</span></span>

<span data-ttu-id="c6264-222">`Newtonsoft.Json`에는 직렬화 하는 동안 JSON에 형식 이름 메타 데이터를 추가 하는 `TypeNameHandling` 설정이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-222">`Newtonsoft.Json` has a `TypeNameHandling` setting that adds type name metadata to the JSON while serializing.</span></span> <span data-ttu-id="c6264-223">Deserialize 하는 동안 메타 데이터를 사용 하 여 다형 deserialization을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-223">It uses the metadata while deserializing to do polymorphic deserialization.</span></span> <span data-ttu-id="c6264-224"><xref:System.Text.Json>는 다형성 [serialization](system-text-json-how-to.md#serialize-properties-of-derived-classes) 의 제한 된 범위를 수행할 수 있지만 다형성 deserialization은 수행할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-224"><xref:System.Text.Json> can do a limited range of [polymorphic serialization](system-text-json-how-to.md#serialize-properties-of-derived-classes) but not polymorphic deserialization.</span></span>

<span data-ttu-id="c6264-225">다형 deserialization을 지원 하려면 [사용자 지정 변환기를 작성 하는 방법](system-text-json-converters-how-to.md#support-polymorphic-deserialization)의 예제와 같은 변환기를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-225">To support polymorphic deserialization, create a converter like the example in [How to write custom converters](system-text-json-converters-how-to.md#support-polymorphic-deserialization).</span></span>

### <a name="required-properties"></a><span data-ttu-id="c6264-226">필수 속성</span><span class="sxs-lookup"><span data-stu-id="c6264-226">Required properties</span></span>

<span data-ttu-id="c6264-227">Deserialization을 수행 하는 동안 대상 형식의 속성 중 하나에 대해 JSON에서 값을 받지 못한 경우에는 <xref:System.Text.Json> 예외를 throw 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-227">During deserialization, <xref:System.Text.Json> doesn't throw an exception if no value is received in the JSON for one of the properties of the target type.</span></span> <span data-ttu-id="c6264-228">예를 들어 `WeatherForecast` 클래스가 있는 경우 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-228">For example, if you have a `WeatherForecast` class:</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

<span data-ttu-id="c6264-229">다음 JSON은 오류 없이 deserialize 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-229">The following JSON is deserialized without error:</span></span>

```json
{
    "TemperatureCelsius": 25,
    "Summary": "Hot"
}
```

<span data-ttu-id="c6264-230">JSON에 `Date` 속성이 없는 경우 deserialization이 실패 하도록 하려면 사용자 지정 변환기를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-230">To make deserialization fail if no `Date` property is in the JSON, implement a custom converter.</span></span> <span data-ttu-id="c6264-231">다음 샘플 변환기 코드는 deserialization이 완료 된 후 `Date` 속성이 설정 되지 않은 경우 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-231">The following sample converter code throws an exception if the `Date` property isn't set after deserialization is complete:</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecastRequiredPropertyConverter.cs)]

<span data-ttu-id="c6264-232">[POCO 클래스에서 특성을 사용](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type) 하거나 <xref:System.Text.Json.JsonSerializerOptions.Converters> 컬렉션에 [변환기를 추가](system-text-json-converters-how-to.md#registration-sample---converters-collection) 하 여이 사용자 지정 변환기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-232">Register this custom converter by [using an attribute on the POCO class](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type) or by [adding the converter](system-text-json-converters-how-to.md#registration-sample---converters-collection) to the <xref:System.Text.Json.JsonSerializerOptions.Converters> collection.</span></span>

<span data-ttu-id="c6264-233">이 패턴을 따르는 경우 <xref:System.Text.Json.JsonSerializer.Serialize%2A> 또는 <xref:System.Text.Json.JsonSerializer.Deserialize%2A>를 재귀적으로 호출 하는 경우 options 개체를 전달 하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-233">If you follow this pattern, don't pass in the options object when recursively calling <xref:System.Text.Json.JsonSerializer.Serialize%2A> or <xref:System.Text.Json.JsonSerializer.Deserialize%2A>.</span></span> <span data-ttu-id="c6264-234">Options 개체는 <xref:System.Text.Json.JsonSerializerOptions.Converters%2A> 컬렉션을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-234">The options object contains the <xref:System.Text.Json.JsonSerializerOptions.Converters%2A> collection.</span></span> <span data-ttu-id="c6264-235">`Serialize` 또는 `Deserialize`에 전달 하는 경우 사용자 지정 변환기가 자신을 호출 하 여 스택 오버플로 예외가 발생 하는 무한 루프를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-235">If you pass it in to `Serialize` or `Deserialize`, the custom converter calls into itself, making an infinite loop that results in a stack overflow exception.</span></span> <span data-ttu-id="c6264-236">기본 옵션이 적절 하지 않은 경우 필요한 설정을 사용 하 여 옵션의 새 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-236">If the default options are not feasible, create a new instance of the options with the settings that you need.</span></span> <span data-ttu-id="c6264-237">이 접근 방식은 각 새 인스턴스를 독립적으로 캐시 하므로 속도가 느립니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-237">This approach will be slow since each new instance caches independently.</span></span>

<span data-ttu-id="c6264-238">앞의 변환기 코드는 단순화 된 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-238">The preceding converter code is a simplified example.</span></span> <span data-ttu-id="c6264-239">특성 (예: [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute) ) 또는 다른 옵션 (예: 사용자 지정 인코더)을 처리 해야 하는 경우에는 추가 논리가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-239">Additional logic would be required if you need to handle attributes (such as [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute) or different options (such as custom encoders).</span></span> <span data-ttu-id="c6264-240">또한 예제 코드는 생성자에서 기본값이 설정 된 속성을 처리 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-240">Also, the example code doesn't handle properties for which a default value is set in the constructor.</span></span> <span data-ttu-id="c6264-241">이 방법은 다음과 같은 시나리오를 구분 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-241">And this approach doesn't differentiate between the following scenarios:</span></span>

* <span data-ttu-id="c6264-242">JSON에서 속성이 누락 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-242">A property is missing from the JSON.</span></span>
* <span data-ttu-id="c6264-243">Nullable이 아닌 형식에 대 한 속성은 JSON에 있지만 값은 `int`의 경우 0과 같이 형식에 대 한 기본값입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-243">A property for a non-nullable type is present in the JSON, but the value is the default for the type, such as zero for an `int`.</span></span>
* <span data-ttu-id="c6264-244">Nullable 형식의 속성은 JSON에 있지만 값은 null입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-244">A property for a nullable type is present in the JSON, but the value is null.</span></span>

### <a name="deserialize-null-to-non-nullable-type"></a><span data-ttu-id="c6264-245">Null을 허용 하지 않는 형식으로 Deserialize 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-245">Deserialize null to non-nullable type</span></span> 

<span data-ttu-id="c6264-246">다음 시나리오에서는 `Newtonsoft.Json` 예외를 throw 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-246">`Newtonsoft.Json` doesn't throw an exception in the following scenario:</span></span>

* <span data-ttu-id="c6264-247">`NullValueHandling` `Ignore`로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-247">`NullValueHandling` is set to `Ignore`, and</span></span>
* <span data-ttu-id="c6264-248">Deserialization을 수행 하는 동안 JSON은 null을 허용 하지 않는 형식에 대해 null 값을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-248">During deserialization, the JSON contains a null value for a non-nullable type.</span></span>

<span data-ttu-id="c6264-249">동일한 시나리오에서 <xref:System.Text.Json>는 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-249">In the same scenario, <xref:System.Text.Json> does throw an exception.</span></span> <span data-ttu-id="c6264-250">해당 하는 null 처리 설정은 <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues?displayProperty=nameWithType>입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-250">(The corresponding null handling setting is <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues?displayProperty=nameWithType>.)</span></span>

<span data-ttu-id="c6264-251">대상 유형을 소유 하는 경우 가장 좋은 해결 방법은 해당 속성을 nullable nullable로 설정 하는 것입니다 (예: `int` `int?`로 변경).</span><span class="sxs-lookup"><span data-stu-id="c6264-251">If you own the target type, the best workaround is to make the property in question nullable (for example, change `int` to `int?`).</span></span>

<span data-ttu-id="c6264-252">또 다른 해결 방법은 `DateTimeOffset` 형식에 대해 null 값을 처리 하는 다음 예제와 같이 형식에 대 한 변환기를 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-252">Another workaround is to make a converter for the type, such as the following example that handles null values for `DateTimeOffset` types:</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/DateTimeOffsetNullHandlingConverter.cs)]

<span data-ttu-id="c6264-253">[속성의 특성을 사용](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property) 하거나 <xref:System.Text.Json.JsonSerializerOptions.Converters> 컬렉션에 [변환기를 추가](system-text-json-converters-how-to.md#registration-sample---converters-collection) 하 여이 사용자 지정 변환기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-253">Register this custom converter by [using an attribute on the property](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property) or by [adding the converter](system-text-json-converters-how-to.md#registration-sample---converters-collection) to the <xref:System.Text.Json.JsonSerializerOptions.Converters> collection.</span></span>

<span data-ttu-id="c6264-254">**참고:** 위의 변환기는 기본값을 지정 하는 POCOs에 대해 `Newtonsoft.Json`는 것과는 **다른 방식으로 null 값을 처리** 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-254">**Note:** The preceding converter **handles null values differently** than `Newtonsoft.Json` does for POCOs that specify default values.</span></span> <span data-ttu-id="c6264-255">예를 들어 다음 코드는 대상 개체를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-255">For example, suppose the following code represents your target object:</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWFWithDefault)]

<span data-ttu-id="c6264-256">그리고 앞의 변환기를 사용 하 여 다음 JSON을 deserialize 한다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-256">And suppose the following JSON is deserialized by using the preceding converter:</span></span>

```json
{
  "Date": null,
  "TemperatureCelsius": 25,
  "Summary": null
}
```

<span data-ttu-id="c6264-257">Deserialization 후 `Date` 속성에 1/1/0001 (`default(DateTimeOffset)`)가 있습니다. 즉, 생성자에 설정 된 값을 덮어씁니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-257">After deserialization, the `Date` property has 1/1/0001 (`default(DateTimeOffset)`), that is, the value set in the constructor is overwritten.</span></span> <span data-ttu-id="c6264-258">동일한 POCO 및 JSON을 지정 하는 경우 `Newtonsoft.Json` deserialization은 `Date` 속성에 1/1/2001을 남겨 둡니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-258">Given the same POCO and JSON, `Newtonsoft.Json` deserialization would leave 1/1/2001 in the `Date` property.</span></span>

### <a name="deserialize-to-immutable-classes-and-structs"></a><span data-ttu-id="c6264-259">변경할 수 없는 클래스 및 구조체로 Deserialize</span><span class="sxs-lookup"><span data-stu-id="c6264-259">Deserialize to immutable classes and structs</span></span>

<span data-ttu-id="c6264-260">매개 변수가 있는 생성자를 사용할 수 있기 때문에 변경 불가능 한 클래스 및 구조체로 deserialize 할 수 `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="c6264-260">`Newtonsoft.Json` can deserialize to immutable classes and structs because it can use constructors that have parameters.</span></span> <span data-ttu-id="c6264-261"><xref:System.Text.Json>은 매개 변수가 없는 public 생성자만 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-261"><xref:System.Text.Json> supports only public parameterless constructors.</span></span> <span data-ttu-id="c6264-262">이 문제를 해결 하기 위해 사용자 지정 변환기에서 매개 변수가 있는 생성자를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-262">As a workaround, you can call a constructor with parameters in a custom converter.</span></span>

<span data-ttu-id="c6264-263">여러 생성자 매개 변수를 사용 하는 변경할 수 없는 구조체는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-263">Here's an immutable struct with multiple constructor parameters:</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ImmutablePoint.cs#ImmutablePoint)]

<span data-ttu-id="c6264-264">이 구조체를 serialize 하 고 deserialize 하는 변환기는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-264">And here's a converter that serializes and deserializes this struct:</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ImmutablePointConverter.cs)]

<span data-ttu-id="c6264-265"><xref:System.Text.Json.JsonSerializerOptions.Converters> 컬렉션에 변환기를 [추가](system-text-json-converters-how-to.md#registration-sample---converters-collection) 하 여이 사용자 지정 변환기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-265">Register this custom converter by [adding the converter](system-text-json-converters-how-to.md#registration-sample---converters-collection) to the <xref:System.Text.Json.JsonSerializerOptions.Converters> collection.</span></span>

<span data-ttu-id="c6264-266">개방형 제네릭 속성을 처리 하는 비슷한 변환기의 예제는 [키-값 쌍에 대 한 기본 제공 변환기](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters/JsonValueConverterKeyValuePair.cs)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-266">For an example of a similar converter that handles open generic properties, see the [built-in converter for key-value pairs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters/JsonValueConverterKeyValuePair.cs).</span></span>

### <a name="specify-constructor-to-use"></a><span data-ttu-id="c6264-267">사용할 생성자를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-267">Specify constructor to use</span></span>

<span data-ttu-id="c6264-268">`Newtonsoft.Json` `[JsonConstructor]` 특성을 사용 하면 POCO로 deserialize 할 때 호출할 생성자를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-268">The `Newtonsoft.Json` `[JsonConstructor]` attribute lets you specify which constructor to call when deserializing to a POCO.</span></span> <span data-ttu-id="c6264-269"><xref:System.Text.Json>는 매개 변수가 없는 생성자만 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-269"><xref:System.Text.Json> supports only parameterless constructors.</span></span> <span data-ttu-id="c6264-270">이 문제를 해결 하려면 사용자 지정 변환기에서 필요한 생성자를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-270">As a workaround, you can call whichever constructor you need in a custom converter.</span></span> <span data-ttu-id="c6264-271">[변경할 수 없는 클래스 및 구조체로 Deserialize 하](#deserialize-to-immutable-classes-and-structs)는 예제를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-271">See the example for [Deserialize to immutable classes and structs](#deserialize-to-immutable-classes-and-structs).</span></span>

### <a name="conditionally-ignore-a-property"></a><span data-ttu-id="c6264-272">조건부로 속성 무시</span><span class="sxs-lookup"><span data-stu-id="c6264-272">Conditionally ignore a property</span></span>

<span data-ttu-id="c6264-273">`Newtonsoft.Json`는 serialization 또는 deserialization에서 속성을 조건부로 무시 하는 여러 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-273">`Newtonsoft.Json` has several ways to conditionally ignore a property on serialization or deserialization:</span></span>

* <span data-ttu-id="c6264-274">`DefaultContractResolver`를 사용 하 여 임의 조건에 따라 포함 하거나 제외할 속성을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-274">`DefaultContractResolver` lets you select properties to include or exclude, based on arbitrary criteria.</span></span> 
* <span data-ttu-id="c6264-275">`JsonSerializerSettings`의 `NullValueHandling` 및 `DefaultValueHandling` 설정을 사용 하 여 모든 null 값 또는 기본 값 속성을 무시 하도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-275">The `NullValueHandling` and `DefaultValueHandling` settings on `JsonSerializerSettings` let you specify that all null-value or default-value properties should be ignored.</span></span>
* <span data-ttu-id="c6264-276">`[JsonProperty]` 특성의 `NullValueHandling` 및 `DefaultValueHandling` 설정을 사용 하면 null 또는 기본값으로 설정 된 경우 무시 해야 하는 개별 속성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-276">The `NullValueHandling` and `DefaultValueHandling` settings on the `[JsonProperty]` attribute let you specify individual properties that should be ignored when set to null or the default value.</span></span>

<span data-ttu-id="c6264-277"><xref:System.Text.Json>는 serialize 하는 동안 속성을 생략 하는 다음과 같은 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-277"><xref:System.Text.Json> provides the following ways to omit properties while serializing:</span></span>

* <span data-ttu-id="c6264-278">속성의 [[JsonIgnore]](system-text-json-how-to.md#exclude-individual-properties) 특성을 지정 하면 serialization 중에 JSON에서 속성이 생략 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-278">The [[JsonIgnore]](system-text-json-how-to.md#exclude-individual-properties) attribute on a property causes the property to be omitted from the JSON during serialization.</span></span>
* <span data-ttu-id="c6264-279">[Ignorenullvalues](system-text-json-how-to.md#exclude-all-null-value-properties) 전역 옵션을 사용 하면 모든 null 값 속성을 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-279">The [IgnoreNullValues](system-text-json-how-to.md#exclude-all-null-value-properties) global option lets you exclude all null-value properties.</span></span>
* <span data-ttu-id="c6264-280">[Ignorreadonly 속성](system-text-json-how-to.md#exclude-all-read-only-properties) 전역 옵션을 사용 하면 모든 읽기 전용 속성을 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-280">The [IgnoreReadOnlyProperties](system-text-json-how-to.md#exclude-all-read-only-properties) global option lets you exclude all read-only properties.</span></span>

<span data-ttu-id="c6264-281">이러한 옵션을 사용할 수 **없습니다** .</span><span class="sxs-lookup"><span data-stu-id="c6264-281">These options **don't** let you:</span></span>

* <span data-ttu-id="c6264-282">형식에 대 한 기본값이 있는 모든 속성을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-282">Ignore all properties that have the default value for the type.</span></span>
* <span data-ttu-id="c6264-283">형식에 대 한 기본값이 있는 선택한 속성을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-283">Ignore selected properties that have the default value for the type.</span></span>
* <span data-ttu-id="c6264-284">해당 값이 null 인 경우 선택한 속성을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-284">Ignore selected properties if their value is null.</span></span>
* <span data-ttu-id="c6264-285">런타임에 평가 되는 임의의 조건에 따라 선택한 속성을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-285">Ignore selected properties based on arbitrary criteria evaluated at run time.</span></span> 

<span data-ttu-id="c6264-286">이 기능을 위해 사용자 지정 변환기를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-286">For that functionality, you can write a custom converter.</span></span> <span data-ttu-id="c6264-287">다음은이 접근 방식을 보여 주는 샘플 POCO 및 사용자 지정 변환기입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-287">Here's a sample POCO and a custom converter for it that illustrates this approach:</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecast.cs?name=SnippetWF)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecastRuntimeIgnoreConverter.cs)]

<span data-ttu-id="c6264-288">변환기를 설정 하면 해당 값이 null, 빈 문자열 또는 "N/A" 인 경우 serialization에서 `Summary` 속성이 생략 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-288">The converter causes the `Summary` property to be omitted from serialization if its value is null, an empty string, or "N/A".</span></span> 

<span data-ttu-id="c6264-289">[클래스에서 특성을 사용](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type) 하거나 <xref:System.Text.Json.JsonSerializerOptions.Converters> 컬렉션에 [변환기를 추가](system-text-json-converters-how-to.md#registration-sample---converters-collection) 하 여이 사용자 지정 변환기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-289">Register this custom converter by [using an attribute on the class](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type) or by [adding the converter](system-text-json-converters-how-to.md#registration-sample---converters-collection) to the <xref:System.Text.Json.JsonSerializerOptions.Converters> collection.</span></span>

<span data-ttu-id="c6264-290">이 방법에는 다음과 같은 경우 추가 논리가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-290">This approach requires additional logic if:</span></span>

* <span data-ttu-id="c6264-291">POCO에는 복잡 한 속성이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-291">The POCO includes complex properties.</span></span>
* <span data-ttu-id="c6264-292">`[JsonIgnore]` 또는 사용자 지정 인코더와 같은 옵션과 같은 특성을 처리 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-292">You need to handle attributes such as `[JsonIgnore]` or options such as custom encoders.</span></span>

### <a name="callbacks"></a><span data-ttu-id="c6264-293">콜백</span><span class="sxs-lookup"><span data-stu-id="c6264-293">Callbacks</span></span>

<span data-ttu-id="c6264-294">`Newtonsoft.Json`를 사용 하면 serialization 또는 deserialization 프로세스의 여러 지점에서 사용자 지정 코드를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-294">`Newtonsoft.Json` lets you execute custom code at several points in the serialization or deserialization process:</span></span>

* <span data-ttu-id="c6264-295">OnDeserializing (개체 deserialize를 시작할 때)</span><span class="sxs-lookup"><span data-stu-id="c6264-295">OnDeserializing (when beginning to deserialize an object)</span></span>
* <span data-ttu-id="c6264-296">OnDeserialized 됨 (개체 deserialize 완료 시)</span><span class="sxs-lookup"><span data-stu-id="c6264-296">OnDeserialized (when finished deserializing an object)</span></span>
* <span data-ttu-id="c6264-297">OnSerializing (개체 직렬화를 시작할 때)</span><span class="sxs-lookup"><span data-stu-id="c6264-297">OnSerializing (when beginning to serialize an object)</span></span>
* <span data-ttu-id="c6264-298">OnSerialized 됨 (개체 직렬화를 완료 한 경우)</span><span class="sxs-lookup"><span data-stu-id="c6264-298">OnSerialized (when finished serializing an object)</span></span>

<span data-ttu-id="c6264-299"><xref:System.Text.Json>에서 사용자 지정 변환기를 작성 하 여 콜백을 시뮬레이션할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-299">In <xref:System.Text.Json>, you can simulate callbacks by writing a custom converter.</span></span> <span data-ttu-id="c6264-300">다음 예제에서는 POCO의 사용자 지정 변환기를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-300">The following example shows a custom converter for a POCO.</span></span> <span data-ttu-id="c6264-301">변환기에는 `Newtonsoft.Json` 콜백에 해당 하는 각 지점에서 메시지를 표시 하는 코드가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-301">The converter includes code that displays a message at each point that corresponds to a `Newtonsoft.Json` callback.</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/WeatherForecastCallbacksConverter.cs)]

<span data-ttu-id="c6264-302">[클래스에서 특성을 사용](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type) 하거나 <xref:System.Text.Json.JsonSerializerOptions.Converters> 컬렉션에 [변환기를 추가](system-text-json-converters-how-to.md#registration-sample---converters-collection) 하 여이 사용자 지정 변환기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-302">Register this custom converter by [using an attribute on the class](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type) or by [adding the converter](system-text-json-converters-how-to.md#registration-sample---converters-collection) to the <xref:System.Text.Json.JsonSerializerOptions.Converters> collection.</span></span>

<span data-ttu-id="c6264-303">이전 샘플을 따르는 사용자 지정 변환기를 사용 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="c6264-303">If you use a custom converter that follows the preceding sample:</span></span>

* <span data-ttu-id="c6264-304">`OnDeserializing` 코드는 새 POCO 인스턴스에 액세스할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-304">The `OnDeserializing` code doesn't have access to the new POCO instance.</span></span> <span data-ttu-id="c6264-305">Deserialization을 시작할 때 새 POCO 인스턴스를 조작 하려면 POCO 생성자에 해당 코드를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-305">To manipulate the new POCO instance at the start of deserialization, put that code in the POCO constructor.</span></span>
* <span data-ttu-id="c6264-306">`Serialize` 또는 `Deserialize`를 재귀적으로 호출 하는 경우 options 개체를 전달 하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-306">Don't pass in the options object when recursively calling `Serialize` or `Deserialize`.</span></span> <span data-ttu-id="c6264-307">Options 개체는 `Converters` 컬렉션을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-307">The options object contains the `Converters` collection.</span></span> <span data-ttu-id="c6264-308">`Serialize` 또는 `Deserialize`에 전달 하는 경우 변환기가 사용 되어 스택 오버플로 예외가 발생 하는 무한 루프를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-308">If you pass it in to `Serialize` or `Deserialize`, the converter will be used, making an infinite loop that results in a stack overflow exception.</span></span>

## <a name="scenarios-that-jsonserializer-currently-doesnt-support"></a><span data-ttu-id="c6264-309">JsonSerializer 현재 지원 하지 않는 시나리오</span><span class="sxs-lookup"><span data-stu-id="c6264-309">Scenarios that JsonSerializer currently doesn't support</span></span>

<span data-ttu-id="c6264-310">해결 방법은 다음과 같은 시나리오에서 가능 하지만 일부는 구현 하기가 비교적 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-310">Workarounds are possible for the following scenarios, but some of them would be relatively difficult to implement.</span></span> <span data-ttu-id="c6264-311">이 문서에서는 이러한 시나리오에 대 한 해결 방법에 대 한 코드 샘플을 제공 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-311">This article doesn't provide code samples for workarounds for these scenarios.</span></span>

<span data-ttu-id="c6264-312">`System.Text.Json`에 해당 하는 항목이 없는 `Newtonsoft.Json` 기능에 대 한 완전 한 목록은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-312">This is not an exhaustive list of `Newtonsoft.Json` features that have no equivalents in `System.Text.Json`.</span></span> <span data-ttu-id="c6264-313">이 목록에는 [GitHub 문제](https://github.com/dotnet/runtime/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-System.Text.Json) 또는 [stackoverflow](https://stackoverflow.com/questions/tagged/system.text.json) 게시물에서 요청 된 많은 시나리오가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-313">The list includes many of the scenarios that have been requested in [GitHub issues](https://github.com/dotnet/runtime/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-System.Text.Json) or [StackOverflow](https://stackoverflow.com/questions/tagged/system.text.json) posts.</span></span>

<span data-ttu-id="c6264-314">이러한 시나리오 중 하나에 대 한 해결 방법을 구현 하 고 코드를 공유할 수 있는 경우 페이지 맨 아래에 있는 "**이 페이지**" 단추를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-314">If you implement a workaround for one of these scenarios and can share the code, select the "**This page**" button at the bottom of the page.</span></span> <span data-ttu-id="c6264-315">그러면 GitHub 문제를 만들어 페이지 아래쪽에 나열 된 문제에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-315">That creates a GitHub issue and adds it to the issues that are listed at the bottom of the page.</span></span>

### <a name="types-without-built-in-support"></a><span data-ttu-id="c6264-316">기본 제공 지원이 없는 형식</span><span class="sxs-lookup"><span data-stu-id="c6264-316">Types without built-in support</span></span>

<span data-ttu-id="c6264-317"><xref:System.Text.Json>는 다음 형식에 대 한 기본 제공 지원을 제공 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-317"><xref:System.Text.Json> doesn't provide built-in support for the following types:</span></span>

* <span data-ttu-id="c6264-318"><xref:System.Data.DataTable> 및 관련 형식</span><span class="sxs-lookup"><span data-stu-id="c6264-318"><xref:System.Data.DataTable> and related types</span></span>
* <span data-ttu-id="c6264-319">F#[구분 된 공용 구조체](../../fsharp/language-reference/discriminated-unions.md), [레코드 형식](../../fsharp/language-reference/records.md)및 [익명 레코드 형식과](../../fsharp/language-reference/anonymous-records.md)같은 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-319">F# types, such as [discriminated unions](../../fsharp/language-reference/discriminated-unions.md), [record types](../../fsharp/language-reference/records.md), and [anonymous record types](../../fsharp/language-reference/anonymous-records.md).</span></span>
* <span data-ttu-id="c6264-320"><xref:System.Collections.Specialized> 네임 스페이스의 컬렉션 형식</span><span class="sxs-lookup"><span data-stu-id="c6264-320">Collection types in the <xref:System.Collections.Specialized> namespace</span></span>
* <xref:System.Dynamic.ExpandoObject>
* <xref:System.TimeZoneInfo>
* <xref:System.Numerics.BigInteger>
* <xref:System.TimeSpan>
* <xref:System.DBNull>
* <xref:System.Type>
* <span data-ttu-id="c6264-321"><xref:System.ValueTuple> 및 연결 된 제네릭 형식</span><span class="sxs-lookup"><span data-stu-id="c6264-321"><xref:System.ValueTuple> and its associated generic types</span></span>

<span data-ttu-id="c6264-322">기본 제공 지원이 없는 형식에 대해 사용자 지정 변환기를 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-322">Custom converters can be implemented for types that don't have built-in support.</span></span>

### <a name="public-and-non-public-fields"></a><span data-ttu-id="c6264-323">Public 및 public이 아닌 필드</span><span class="sxs-lookup"><span data-stu-id="c6264-323">Public and non-public fields</span></span>

<span data-ttu-id="c6264-324">`Newtonsoft.Json` 필드 및 속성을 serialize 및 deserialize 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-324">`Newtonsoft.Json` can serialize and deserialize fields as well as properties.</span></span> <span data-ttu-id="c6264-325"><xref:System.Text.Json>는 공용 속성 에서만 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-325"><xref:System.Text.Json> only works with public properties.</span></span> <span data-ttu-id="c6264-326">사용자 지정 변환기는이 기능을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-326">Custom converters can provide this functionality.</span></span>

### <a name="internal-and-private-property-setters-and-getters"></a><span data-ttu-id="c6264-327">내부 및 개인 속성 setter 및 getter</span><span class="sxs-lookup"><span data-stu-id="c6264-327">Internal and private property setters and getters</span></span>

<span data-ttu-id="c6264-328">`Newtonsoft.Json`는 `JsonProperty` 특성을 통해 전용 및 내부 속성 setter 및 getter를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-328">`Newtonsoft.Json` can use private and internal property setters and getters via the `JsonProperty` attribute.</span></span> <span data-ttu-id="c6264-329"><xref:System.Text.Json>는 공용 setter만 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-329"><xref:System.Text.Json> supports only public setters.</span></span> <span data-ttu-id="c6264-330">사용자 지정 변환기는이 기능을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-330">Custom converters can provide this functionality.</span></span>

### <a name="preserve-object-references-and-handle-loops"></a><span data-ttu-id="c6264-331">개체 참조 유지 및 루프 처리</span><span class="sxs-lookup"><span data-stu-id="c6264-331">Preserve object references and handle loops</span></span>

<span data-ttu-id="c6264-332">기본적으로 `Newtonsoft.Json`는 값으로 직렬화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-332">By default, `Newtonsoft.Json` serializes by value.</span></span> <span data-ttu-id="c6264-333">예를 들어 개체가 동일한 `Person` 개체에 대 한 참조를 포함 하는 두 개의 속성을 포함 하는 경우 해당 `Person` 개체의 속성 값이 JSON에서 중복 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-333">For example, if an object contains two properties that contain a reference to the same `Person` object, the values of that `Person` object's properties are duplicated in the JSON.</span></span>

<span data-ttu-id="c6264-334">`Newtonsoft.Json`에는 참조로 serialize 할 수 있는 `JsonSerializerSettings` `PreserveReferencesHandling` 설정이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-334">`Newtonsoft.Json` has a `PreserveReferencesHandling` setting on `JsonSerializerSettings` that lets you serialize by reference:</span></span>

* <span data-ttu-id="c6264-335">첫 번째 `Person` 개체에 대해 만든 JSON에 식별자 메타 데이터가 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-335">An identifier metadata is added to the JSON created for the first `Person` object.</span></span>
* <span data-ttu-id="c6264-336">두 번째 `Person` 개체에 대해 만든 JSON에는 속성 값 대신 해당 식별자에 대 한 참조가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-336">The JSON that is created for the second `Person` object contains a reference to that identifier instead of property values.</span></span>

<span data-ttu-id="c6264-337">또한 `Newtonsoft.Json`에는 예외를 throw 하는 대신 순환 참조를 무시할 수 있는 `ReferenceLoopHandling` 설정이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-337">`Newtonsoft.Json` also has a `ReferenceLoopHandling` setting that lets you ignore circular references rather than throw an exception.</span></span>

<span data-ttu-id="c6264-338"><xref:System.Text.Json>는 값으로 직렬화만 지원 하 고 순환 참조에 대해 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-338"><xref:System.Text.Json> only supports serialization by value and throws an exception for circular references.</span></span>

### <a name="systemruntimeserialization-attributes"></a><span data-ttu-id="c6264-339">System.object. 직렬화 특성</span><span class="sxs-lookup"><span data-stu-id="c6264-339">System.Runtime.Serialization attributes</span></span>

<span data-ttu-id="c6264-340"><xref:System.Text.Json>은 `DataMemberAttribute` 및 `IgnoreDataMemberAttribute`과 같은 `System.Runtime.Serialization` 네임 스페이스의 특성을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-340"><xref:System.Text.Json> doesn't support attributes from the `System.Runtime.Serialization` namespace, such as `DataMemberAttribute` and `IgnoreDataMemberAttribute`.</span></span>

### <a name="octal-numbers"></a><span data-ttu-id="c6264-341">8 진수</span><span class="sxs-lookup"><span data-stu-id="c6264-341">Octal numbers</span></span>

<span data-ttu-id="c6264-342">`Newtonsoft.Json`는 앞에 오는 0을 8 진수로 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-342">`Newtonsoft.Json` treats numbers with a leading zero as octal numbers.</span></span> <span data-ttu-id="c6264-343"><xref:System.Text.Json> [RFC 8259](https://tools.ietf.org/html/rfc8259) 사양에서 허용 하지 않으므로 선행 0을 허용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-343"><xref:System.Text.Json> doesn't allow leading zeroes because the [RFC 8259](https://tools.ietf.org/html/rfc8259) specification doesn't allow them.</span></span>

### <a name="populate-existing-objects"></a><span data-ttu-id="c6264-344">기존 개체 채우기</span><span class="sxs-lookup"><span data-stu-id="c6264-344">Populate existing objects</span></span>

<span data-ttu-id="c6264-345">`Newtonsoft.Json`의 `JsonConvert.PopulateObject` 메서드는 새 인스턴스를 만드는 대신 JSON 문서를 클래스의 기존 인스턴스로 deserialize 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-345">The `JsonConvert.PopulateObject` method in `Newtonsoft.Json` deserializes a JSON document to an existing instance of a class, instead of creating a new instance.</span></span> <span data-ttu-id="c6264-346"><xref:System.Text.Json>는 항상 기본 public 매개 변수가 없는 생성자를 사용 하 여 대상 형식의 새 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-346"><xref:System.Text.Json> always creates a new instance of the target type by using the default public parameterless constructor.</span></span> <span data-ttu-id="c6264-347">사용자 지정 변환기는 기존 인스턴스로 deserialize 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-347">Custom converters can deserialize to an existing instance.</span></span>

### <a name="reuse-rather-than-replace-properties"></a><span data-ttu-id="c6264-348">속성 바꾸기 대신 다시 사용</span><span class="sxs-lookup"><span data-stu-id="c6264-348">Reuse rather than replace properties</span></span>

<span data-ttu-id="c6264-349">`Newtonsoft.Json` `ObjectCreationHandling` 설정을 사용 하면 deserialization 중에 속성의 개체를 대체 하는 대신 다시 사용 하도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-349">The `Newtonsoft.Json` `ObjectCreationHandling` setting lets you specify that objects in properties should be reused rather than replaced during deserialization.</span></span> <span data-ttu-id="c6264-350"><xref:System.Text.Json>는 항상 속성에서 개체를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-350"><xref:System.Text.Json> always replaces objects in properties.</span></span>  <span data-ttu-id="c6264-351">사용자 지정 변환기는이 기능을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-351">Custom converters can provide this functionality.</span></span>

### <a name="add-to-collections-without-setters"></a><span data-ttu-id="c6264-352">Setter를 사용 하지 않고 컬렉션에 추가</span><span class="sxs-lookup"><span data-stu-id="c6264-352">Add to collections without setters</span></span>

<span data-ttu-id="c6264-353">Deserialization을 수행 하는 동안 속성에 setter가 없는 경우에도 `Newtonsoft.Json` 개체를 컬렉션에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-353">During deserialization, `Newtonsoft.Json` adds objects to a collection even if the property has no setter.</span></span> <span data-ttu-id="c6264-354"><xref:System.Text.Json>는 setter가 없는 속성을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-354"><xref:System.Text.Json> ignores properties that don't have setters.</span></span> <span data-ttu-id="c6264-355">사용자 지정 변환기는이 기능을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-355">Custom converters can provide this functionality.</span></span>

### <a name="missingmemberhandling"></a><span data-ttu-id="c6264-356">MissingMemberHandling</span><span class="sxs-lookup"><span data-stu-id="c6264-356">MissingMemberHandling</span></span>

<span data-ttu-id="c6264-357">JSON에 대상 형식에 없는 속성이 포함 된 경우 deserialization 하는 동안 예외를 throw 하도록 `Newtonsoft.Json`를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-357">`Newtonsoft.Json` can be configured to throw exceptions during deserialization if the JSON includes properties that are missing in the target type.</span></span> <span data-ttu-id="c6264-358"><xref:System.Text.Json>는 [[JsonExtensionData] 특성](system-text-json-how-to.md#handle-overflow-json)을 사용 하는 경우를 제외 하 고 JSON의 추가 속성을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-358"><xref:System.Text.Json> ignores extra properties in the JSON, except when you use the [[JsonExtensionData] attribute](system-text-json-how-to.md#handle-overflow-json).</span></span> <span data-ttu-id="c6264-359">누락 된 멤버 기능에 대 한 해결 방법은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-359">There's no workaround for the missing member feature.</span></span>

### <a name="tracewriter"></a><span data-ttu-id="c6264-360">TraceWriter</span><span class="sxs-lookup"><span data-stu-id="c6264-360">TraceWriter</span></span>

<span data-ttu-id="c6264-361">`Newtonsoft.Json`를 사용 하면 `TraceWriter`를 사용 하 여 serialization 또는 deserialization에 의해 생성 된 로그를 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-361">`Newtonsoft.Json` lets you debug by using a `TraceWriter` to view logs that are generated by serialization or deserialization.</span></span> <span data-ttu-id="c6264-362"><xref:System.Text.Json>는 로깅을 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-362"><xref:System.Text.Json> doesn't do logging.</span></span>

## <a name="jsondocument-and-jsonelement-compared-to-jtoken-like-jobject-jarray"></a><span data-ttu-id="c6264-363">JToken과 같은 JsonDocument 및 JsonElement (예: Jtoken, Jtoken)</span><span class="sxs-lookup"><span data-stu-id="c6264-363">JsonDocument and JsonElement compared to JToken (like JObject, JArray)</span></span>

<span data-ttu-id="c6264-364"><xref:System.Text.Json.JsonDocument?displayProperty=fullName>는 기존 JSON 페이로드에서 **읽기** 전용 문서 개체 모델 (DOM)를 구문 분석 하 고 작성 하는 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-364"><xref:System.Text.Json.JsonDocument?displayProperty=fullName> provides the ability to parse and build a **read-only** Document Object Model (DOM) from existing JSON payloads.</span></span> <span data-ttu-id="c6264-365">DOM은 JSON 페이로드의 데이터에 대 한 임의 액세스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-365">The DOM provides random access to data in a JSON payload.</span></span> <span data-ttu-id="c6264-366">페이로드를 구성 하는 JSON 요소는 <xref:System.Text.Json.JsonElement> 형식을 통해 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-366">The JSON elements that compose the payload can be accessed via the <xref:System.Text.Json.JsonElement> type.</span></span> <span data-ttu-id="c6264-367">`JsonElement` 형식은 JSON 텍스트를 일반적인 .NET 형식으로 변환 하는 Api를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-367">The `JsonElement` type provides APIs to convert JSON text to common .NET types.</span></span> <span data-ttu-id="c6264-368">`JsonDocument` <xref:System.Text.Json.JsonDocument.RootElement> 속성을 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-368">`JsonDocument` exposes a <xref:System.Text.Json.JsonDocument.RootElement> property.</span></span>

### <a name="jsondocument-is-idisposable"></a><span data-ttu-id="c6264-369">JsonDocument는 IDisposable입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-369">JsonDocument is IDisposable</span></span>

<span data-ttu-id="c6264-370">`JsonDocument`는 풀링된 버퍼에 데이터의 메모리 내 뷰를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-370">`JsonDocument` builds an in-memory view of the data into a pooled buffer.</span></span> <span data-ttu-id="c6264-371">따라서 `Newtonsoft.Json`에서 `JObject` 또는 `JArray`와 달리 `JsonDocument` 형식은 `IDisposable`를 구현 하며 using 블록 내에서 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-371">Therefore, unlike `JObject` or `JArray` from `Newtonsoft.Json`, the `JsonDocument` type implements `IDisposable` and needs to be used inside a using block.</span></span> 

<span data-ttu-id="c6264-372">수명 소유권을 양도 하 고 호출자에 게 책임을 삭제 하려는 경우에만 API에서 `JsonDocument`을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-372">Only return a `JsonDocument` from your API if you want to transfer lifetime ownership and dispose responsibility to the caller.</span></span> <span data-ttu-id="c6264-373">대부분의 시나리오에서는 필요 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-373">In most scenarios, that isn't necessary.</span></span> <span data-ttu-id="c6264-374">호출자가 전체 JSON 문서를 사용 해야 하는 경우 <xref:System.Text.Json.JsonElement><xref:System.Text.Json.JsonDocument.RootElement%2A><xref:System.Text.Json.JsonElement.Clone%2A> 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-374">If the caller needs to work with the entire JSON document, return the <xref:System.Text.Json.JsonElement.Clone%2A> of the <xref:System.Text.Json.JsonDocument.RootElement%2A>, which is a <xref:System.Text.Json.JsonElement>.</span></span> <span data-ttu-id="c6264-375">호출자가 JSON 문서 내의 특정 요소를 사용 해야 하는 경우 해당 <xref:System.Text.Json.JsonElement>의 <xref:System.Text.Json.JsonElement.Clone%2A> 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-375">If the caller needs to work with a particular element within the JSON document, return the <xref:System.Text.Json.JsonElement.Clone%2A> of that <xref:System.Text.Json.JsonElement>.</span></span> <span data-ttu-id="c6264-376">`Clone`하지 않고 `RootElement` 또는 하위 요소를 직접 반환 하는 경우 호출자는이를 소유 하는 `JsonDocument` 삭제 된 후 반환 된 `JsonElement`에 액세스할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-376">If you return the `RootElement` or a sub-element directly without making a `Clone`, the caller won't be able to access the returned `JsonElement` after the `JsonDocument` that owns it is disposed.</span></span>

<span data-ttu-id="c6264-377">`Clone`해야 하는 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-377">Here's an example that requires you to make a `Clone`:</span></span>

```csharp
public JsonElement LookAndLoad(JsonElement source)
{
    string json = File.ReadAllText(source.GetProperty("fileName").GetString());
   
    using (JsonDocument doc = JsonDocument.Parse(json))
    {
        return doc.RootElement.Clone();
    }
}
```

<span data-ttu-id="c6264-378">위의 코드에는 `fileName` 속성을 포함 하는 `JsonElement` 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-378">The preceding code expects a `JsonElement` that contains a `fileName` property.</span></span> <span data-ttu-id="c6264-379">JSON 파일을 열고 `JsonDocument`를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-379">It opens the JSON file and creates a `JsonDocument`.</span></span> <span data-ttu-id="c6264-380">메서드는 호출자가 전체 문서를 사용 하 고 있는 것으로 가정 하 여 `RootElement``Clone` 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-380">The method assumes that the caller wants to work with the entire document, so it returns the `Clone` of the `RootElement`.</span></span> 

<span data-ttu-id="c6264-381">`JsonElement` 받고 하위 요소를 반환 하는 경우 하위 요소의 `Clone`를 반환할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-381">If you receive a `JsonElement` and are returning a sub-element, it's not necessary to return a `Clone` of the sub-element.</span></span> <span data-ttu-id="c6264-382">호출자는 전달 된 `JsonElement`이 속한 `JsonDocument`의 활성 상태를 유지 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-382">The caller is responsible for keeping alive the `JsonDocument` that the passed-in `JsonElement` belongs to.</span></span> <span data-ttu-id="c6264-383">예를 들면 다음과 같습니다.:</span><span class="sxs-lookup"><span data-stu-id="c6264-383">For example:</span></span>

```csharp
public JsonElement ReturnFileName(JsonElement source)
{
   return source.GetProperty("fileName");
}
```

### <a name="jsondocument-is-read-only"></a><span data-ttu-id="c6264-384">JsonDocument는 읽기 전용입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-384">JsonDocument is read-only</span></span>

<span data-ttu-id="c6264-385"><xref:System.Text.Json> DOM은 JSON 요소를 추가, 제거 또는 수정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-385">The <xref:System.Text.Json> DOM can't add, remove, or modify JSON elements.</span></span> <span data-ttu-id="c6264-386">이러한 방식으로 성능을 향상 하 고 일반적인 JSON 페이로드 크기를 구문 분석 하기 위한 할당 (< 1mb)을 줄일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-386">It's designed this way for performance and to reduce allocations for parsing common JSON payload sizes (that is, < 1 MB).</span></span> <span data-ttu-id="c6264-387">현재 시나리오에서 수정 가능한 DOM을 사용 하는 경우 다음 해결 방법 중 하나를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-387">If your scenario currently uses a modifiable DOM, one of the following workarounds might be feasible:</span></span>

* <span data-ttu-id="c6264-388">`JsonDocument`를 처음부터 새로 작성 하려면 (즉, `Parse` 메서드에 기존 JSON 페이로드를 전달 하지 않고), `Utf8JsonWriter`를 사용 하 여 JSON 텍스트를 작성 하 고이의 출력을 구문 분석 하 여 새 `JsonDocument`를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-388">To build a `JsonDocument` from scratch (that is, without passing in an existing JSON payload to the `Parse` method), write the JSON text by using the `Utf8JsonWriter` and parse the output from that to make a new `JsonDocument`.</span></span>
* <span data-ttu-id="c6264-389">기존 `JsonDocument`을 수정 하려면이를 사용 하 여 JSON 텍스트를 작성 하 고, 작성 하는 동안 변경 하 고, 새 `JsonDocument`을 만들 때의 출력을 구문 분석 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-389">To modify an existing `JsonDocument`, use it to write JSON text, making changes while you write, and parse the output from that to make a new `JsonDocument`.</span></span>
* <span data-ttu-id="c6264-390">`Newtonsoft.Json`에서 `JObject.Merge` 또는 `JContainer.Merge` Api에 해당 하는 기존 JSON 문서를 병합 하려면 [이 GitHub 문제](https://github.com/dotnet/corefx/issues/42466#issuecomment-570475853)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-390">To merge existing JSON documents, equivalent to the `JObject.Merge` or `JContainer.Merge` APIs from `Newtonsoft.Json`, see [this GitHub issue](https://github.com/dotnet/corefx/issues/42466#issuecomment-570475853).</span></span>

### <a name="jsonelement-is-a-union-struct"></a><span data-ttu-id="c6264-391">JsonElement는 union 구조체입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-391">JsonElement is a union struct</span></span>

<span data-ttu-id="c6264-392">`JsonDocument`은 모든 JSON 요소를 포함 하는 공용 구조체 형식인 <xref:System.Text.Json.JsonElement>형식의 속성으로 `RootElement`를 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-392">`JsonDocument` exposes the `RootElement` as a property of type <xref:System.Text.Json.JsonElement>, which is a union, struct type that encompasses any JSON element.</span></span> <span data-ttu-id="c6264-393">`Newtonsoft.Json` `JObject`,`JArray`, `JToken`등의 전용 계층적 유형을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-393">`Newtonsoft.Json` uses dedicated hierarchical types like `JObject`,`JArray`, `JToken`, and so forth.</span></span> <span data-ttu-id="c6264-394">`JsonElement`는 검색 및 열거할 수 있는 기능으로, `JsonElement`를 사용 하 여 JSON 요소를 .NET 형식으로 구체화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-394">`JsonElement` is what you can search and enumerate over, and you can use `JsonElement` to materialize JSON elements into .NET types.</span></span>

### <a name="how-to-search-a-jsondocument-and-jsonelement-for-sub-elements"></a><span data-ttu-id="c6264-395">하위 요소에 대해 JsonDocument 및 JsonElement를 검색 하는 방법</span><span class="sxs-lookup"><span data-stu-id="c6264-395">How to search a JsonDocument and JsonElement for sub-elements</span></span>

<span data-ttu-id="c6264-396">`JObject` 또는 `Newtonsoft.Json` `JArray`를 사용 하 여 JSON 토큰을 검색 하는 것은 일부 사전에서 조회 되기 때문에 비교적 빠릅니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-396">Searches for JSON tokens using `JObject` or `JArray` from `Newtonsoft.Json` tend to be relatively fast because they're lookups in some dictionary.</span></span> <span data-ttu-id="c6264-397">비교 하 여 `JsonElement` 검색은 속성을 순차적으로 검색 해야 하므로 상대적으로 느립니다 (예: `TryGetProperty`를 사용 하는 경우).</span><span class="sxs-lookup"><span data-stu-id="c6264-397">By comparison, searches on `JsonElement` require a sequential search of the properties and hence is relatively slow (for example when using `TryGetProperty`).</span></span> <span data-ttu-id="c6264-398"><xref:System.Text.Json>은 조회 시간이 아니라 초기 구문 분석 시간을 최소화 하도록 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-398"><xref:System.Text.Json> is designed to minimize initial parse time rather than lookup time.</span></span> <span data-ttu-id="c6264-399">따라서 `JsonDocument` 개체를 검색할 때 성능을 최적화 하려면 다음 방법을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-399">Therefore, use the following approaches to optimize performance when searching through a `JsonDocument` object:</span></span>

* <span data-ttu-id="c6264-400">사용자 고유의 인덱싱 또는 루프를 수행 하는 대신 기본 제공 열거자 (<xref:System.Text.Json.JsonElement.EnumerateArray%2A> 및 <xref:System.Text.Json.JsonElement.EnumerateObject%2A>)를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-400">Use the built-in enumerators (<xref:System.Text.Json.JsonElement.EnumerateArray%2A> and <xref:System.Text.Json.JsonElement.EnumerateObject%2A>) rather than doing your own indexing or loops.</span></span>
* <span data-ttu-id="c6264-401">`RootElement`를 사용 하 여 모든 속성을 통해 전체 `JsonDocument`에서 순차적 검색을 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-401">Don't do a sequential search on the whole `JsonDocument` through every property by using `RootElement`.</span></span> <span data-ttu-id="c6264-402">대신, 알려진 JSON 데이터 구조를 기반으로 중첩 된 JSON 개체를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-402">Instead, search on nested JSON objects based on the known structure of the JSON data.</span></span> <span data-ttu-id="c6264-403">예를 들어 `Student` 개체에서 `Grade` 속성을 찾고 있는 경우 `JsonElement` 속성을 검색 하는 모든 `Grade` 개체를 검색 하는 대신 `Student` 개체를 반복 하 고 각 개체에 대 한 `Grade` 값을 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-403">For example, if you're looking for a `Grade` property in `Student` objects, loop through the `Student` objects and get the value of `Grade` for each, rather than searching through all `JsonElement` objects looking for `Grade` properties.</span></span> <span data-ttu-id="c6264-404">후자를 수행 하면 동일한 데이터에 대해 불필요 한 패스가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-404">Doing the latter will result in unnecessary passes over the same data.</span></span>

<span data-ttu-id="c6264-405">코드 예제는 [데이터 액세스에 JsonDocument 사용](system-text-json-how-to.md#use-jsondocument-for-access-to-data)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-405">For a code example, see [Use JsonDocument for access to data](system-text-json-how-to.md#use-jsondocument-for-access-to-data).</span></span>

## <a name="utf8jsonreader-compared-to-jsontextreader"></a><span data-ttu-id="c6264-406">Utf8JsonReader와 JsonTextReader 비교</span><span class="sxs-lookup"><span data-stu-id="c6264-406">Utf8JsonReader compared to JsonTextReader</span></span>

<span data-ttu-id="c6264-407"><xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName>는 u t f-8로 인코딩된 JSON 텍스트에 대 한 고성능, 낮은 할당, 전방 전용 판독기로, [ReadOnlySpan\<바이트 >](xref:System.ReadOnlySpan%601) 또는 [ReadOnlySequence\<바이트 >](xref:System.Buffers.ReadOnlySequence%601)에서 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-407"><xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName> is a high-performance, low allocation, forward-only reader for UTF-8 encoded JSON text, read from a [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601) or [ReadOnlySequence\<byte>](xref:System.Buffers.ReadOnlySequence%601).</span></span> <span data-ttu-id="c6264-408">`Utf8JsonReader`는 사용자 지정 파서 및 deserializers를 빌드하는 데 사용할 수 있는 하위 수준 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-408">The `Utf8JsonReader` is a low-level type that can be used to build custom parsers and deserializers.</span></span>

<span data-ttu-id="c6264-409">다음 섹션에서는 `Utf8JsonReader`사용을 위한 권장 프로그래밍 패턴에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-409">The following sections explain recommended programming patterns for using `Utf8JsonReader`.</span></span>

### <a name="utf8jsonreader-is-a-ref-struct"></a><span data-ttu-id="c6264-410">Utf8JsonReader는 ref 구조체입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-410">Utf8JsonReader is a ref struct</span></span>

<span data-ttu-id="c6264-411">`Utf8JsonReader` 형식은 *ref 구조체*이므로 [특정 제한 사항이](../../csharp/language-reference/keywords/ref.md#ref-struct-types)있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-411">Because the `Utf8JsonReader` type is a *ref struct*, it has [certain limitations](../../csharp/language-reference/keywords/ref.md#ref-struct-types).</span></span> <span data-ttu-id="c6264-412">예를 들어 ref 구조체가 아닌 클래스 또는 구조체에 필드로 저장할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-412">For example, it can't be stored as a field on a class or struct other than a ref struct.</span></span> <span data-ttu-id="c6264-413">고성능을 얻기 위해이 형식은 입력 [ReadOnlySpan\<바이트 >](xref:System.ReadOnlySpan%601)를 캐시 해야 하므로 해당 형식이 ref 구조체 이기 때문에 `ref struct` 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-413">To achieve high performance, this type must be a `ref struct` since it needs to cache the input [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601), which itself is a ref struct.</span></span> <span data-ttu-id="c6264-414">또한이 형식은 상태를 유지 하기 때문에 변경 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-414">In addition, this type is mutable since it holds state.</span></span> <span data-ttu-id="c6264-415">따라서 값 **이 아닌 ref로 전달** 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-415">Therefore, **pass it by ref** rather than by value.</span></span> <span data-ttu-id="c6264-416">값으로 전달 하면 구조체 복사본이 생성 되 고 상태 변경 내용이 호출자에 게 표시 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-416">Passing it by value would result in a struct copy and the state changes would not be visible to the caller.</span></span> <span data-ttu-id="c6264-417">`Newtonsoft.Json` `JsonTextReader`은 클래스 이므로 `Newtonsoft.Json`와 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-417">This differs from `Newtonsoft.Json` since the `Newtonsoft.Json` `JsonTextReader` is a class.</span></span> <span data-ttu-id="c6264-418">Ref 구조체를 사용 하는 방법에 대 한 자세한 내용은 [안전 하 고 C# 효율적인 코드 작성](../../csharp/write-safe-efficient-code.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-418">For more information about how to use ref structs, see [Write safe and efficient C# code](../../csharp/write-safe-efficient-code.md).</span></span>

### <a name="read-utf-8-text"></a><span data-ttu-id="c6264-419">UTF-8 텍스트 읽기</span><span class="sxs-lookup"><span data-stu-id="c6264-419">Read UTF-8 text</span></span>

<span data-ttu-id="c6264-420">`Utf8JsonReader`사용 하는 동안 최상의 성능을 얻으려면 UTF-16 문자열이 아닌 u t f-8 텍스트로 이미 인코드된 JSON 페이로드를 읽어 보십시오.</span><span class="sxs-lookup"><span data-stu-id="c6264-420">To achieve the best possible performance while using the `Utf8JsonReader`, read JSON payloads already encoded as UTF-8 text rather than as UTF-16 strings.</span></span> <span data-ttu-id="c6264-421">코드 예제를 보려면 Utf8JsonReader를 [사용 하 여 데이터 필터링](system-text-json-how-to.md#filter-data-using-utf8jsonreader)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-421">For a code example, see [Filter data using Utf8JsonReader](system-text-json-how-to.md#filter-data-using-utf8jsonreader).</span></span>

### <a name="read-with-a-stream-or-pipereader"></a><span data-ttu-id="c6264-422">Stream 또는 PipeReader를 사용 하 여 읽기</span><span class="sxs-lookup"><span data-stu-id="c6264-422">Read with a Stream or PipeReader</span></span>

<span data-ttu-id="c6264-423">`Utf8JsonReader`는 u t f-8로 인코딩된 [ReadOnlySpan\<byte >](xref:System.ReadOnlySpan%601) 또는 [ReadOnlySequence\<바이트 >](xref:System.Buffers.ReadOnlySequence%601) (<xref:System.IO.Pipelines.PipeReader>에서 읽은 결과)에서 읽기를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-423">The `Utf8JsonReader` supports reading from a UTF-8 encoded [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601) or [ReadOnlySequence\<byte>](xref:System.Buffers.ReadOnlySequence%601) (which is the result of reading from a <xref:System.IO.Pipelines.PipeReader>).</span></span>

<span data-ttu-id="c6264-424">동기 읽기의 경우 스트림의 끝에서 바이트 배열로 JSON 페이로드를 읽은 후 판독기에 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-424">For synchronous reading, you could read the JSON payload until the end of the stream into a byte array and pass that into the reader.</span></span> <span data-ttu-id="c6264-425">U t f-16으로 인코딩된 문자열에서 읽으려면 <xref:System.Text.Encoding.UTF8>를 호출 합니다.<xref:System.Text.Encoding.GetBytes%2A></span><span class="sxs-lookup"><span data-stu-id="c6264-425">For reading from a string (which is encoded as UTF-16), call <xref:System.Text.Encoding.UTF8>.<xref:System.Text.Encoding.GetBytes%2A></span></span> <span data-ttu-id="c6264-426">먼저 문자열을 u t f-8로 인코딩된 바이트 배열로 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-426">to first transcode the string to a UTF-8 encoded byte array.</span></span> <span data-ttu-id="c6264-427">그런 다음 `Utf8JsonReader`에 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-427">Then pass that to the `Utf8JsonReader`.</span></span> 

<span data-ttu-id="c6264-428">`Utf8JsonReader`는 입력을 JSON 텍스트로 간주 하므로 UTF-8 BOM (바이트 순서 표시)은 잘못 된 JSON으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-428">Since the `Utf8JsonReader` considers the input to be JSON text, a UTF-8 byte order mark (BOM) is considered invalid JSON.</span></span> <span data-ttu-id="c6264-429">호출자는 데이터를 판독기에 전달 하기 전에 필터링 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-429">The caller needs to filter that out before passing the data to the reader.</span></span>

<span data-ttu-id="c6264-430">코드 예제는 [Utf8JsonReader 사용](system-text-json-how-to.md#use-utf8jsonreader)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-430">For code examples, see [Use Utf8JsonReader](system-text-json-how-to.md#use-utf8jsonreader).</span></span>

### <a name="read-with-multi-segment-readonlysequence"></a><span data-ttu-id="c6264-431">다중 세그먼트 ReadOnlySequence를 사용 하 여 읽기</span><span class="sxs-lookup"><span data-stu-id="c6264-431">Read with multi-segment ReadOnlySequence</span></span>

<span data-ttu-id="c6264-432">JSON 입력이 [ReadOnlySpan\<바이트 >](xref:System.ReadOnlySpan%601)경우 읽기 루프를 진행할 때 판독기의 `ValueSpan` 속성에서 각 json 요소에 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-432">If your JSON input is a [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601), each JSON element can be accessed from the `ValueSpan` property on the reader as you go through the read loop.</span></span> <span data-ttu-id="c6264-433">그러나 입력이 [ReadOnlySequence\<바이트 >](xref:System.Buffers.ReadOnlySequence%601) (<xref:System.IO.Pipelines.PipeReader>에서 읽은 결과) 인 경우 일부 JSON 요소는 `ReadOnlySequence<byte>` 개체의 여러 세그먼트를 걸쳐 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-433">However, if your input is a [ReadOnlySequence\<byte>](xref:System.Buffers.ReadOnlySequence%601) (which is the result of reading from a <xref:System.IO.Pipelines.PipeReader>), some JSON elements might straddle multiple segments of the `ReadOnlySequence<byte>` object.</span></span> <span data-ttu-id="c6264-434">이러한 요소는 연속 메모리 블록의 <xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A>에서 액세스할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-434">These elements would not be accessible from <xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A> in a contiguous memory block.</span></span> <span data-ttu-id="c6264-435">대신 다중 `ReadOnlySequence<byte>` 세그먼트가 입력으로 사용 될 때마다 판독기에서 <xref:System.Text.Json.Utf8JsonReader.HasValueSequence%2A> 속성을 폴링하여 현재 JSON 요소에 액세스 하는 방법을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-435">Instead, whenever you have a multi-segment `ReadOnlySequence<byte>` as input, poll the <xref:System.Text.Json.Utf8JsonReader.HasValueSequence%2A> property on the reader to figure out how to access the current JSON element.</span></span> <span data-ttu-id="c6264-436">권장 패턴은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-436">Here's a recommended pattern:</span></span>

```csharp
while (reader.Read())
{
    switch (reader.TokenType)
    {
        // ...
        ReadOnlySpan<byte> jsonElement = reader.HasValueSequence ?
            reader.ValueSequence.ToArray() :
            reader.ValueSpan;
        // ...
    }
}
```

### <a name="use-valuetextequals-for-property-name-lookups"></a><span data-ttu-id="c6264-437">속성 이름 조회에 대 한 사용</span><span class="sxs-lookup"><span data-stu-id="c6264-437">Use ValueTextEquals for property name lookups</span></span>

<span data-ttu-id="c6264-438">속성 이름 조회를 위해 <xref:System.MemoryExtensions.SequenceEqual%2A>를 호출 하 여 바이트 단위 비교를 수행 하려면 <xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A>를 사용 하지 마세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-438">Don't use <xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A> to do byte-by-byte comparisons by calling <xref:System.MemoryExtensions.SequenceEqual%2A> for property name lookups.</span></span> <span data-ttu-id="c6264-439">해당 메서드가 JSON에서 이스케이프 된 모든 문자를 unescapes 하므로 대신 <xref:System.Text.Json.Utf8JsonReader.ValueTextEquals%2A>를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-439">Call <xref:System.Text.Json.Utf8JsonReader.ValueTextEquals%2A> instead, because that method unescapes any characters that are escaped in the JSON.</span></span> <span data-ttu-id="c6264-440">"Name" 이라는 속성을 검색 하는 방법을 보여 주는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-440">Here's an example that shows how to search for a property that is named "name":</span></span>

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ValueTextEqualsExample.cs?name=SnippetDefineUtf8Var)]

[!code-csharp[](~/samples/snippets/core/system-text-json/csharp/ValueTextEqualsExample.cs?name=SnippetUseUtf8Var&highlight=11)]

### <a name="read-null-values-into-nullable-value-types"></a><span data-ttu-id="c6264-441">Null 값을 nullable 값 형식으로 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-441">Read null values into nullable value types</span></span>

<span data-ttu-id="c6264-442">`Newtonsoft.Json`는 `TokenType`를 반환 하 여 `Null` `bool?`를 처리 하는 `ReadAsBoolean`와 같은 <xref:System.Nullable%601>를 반환 하는 Api를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-442">`Newtonsoft.Json` provides APIs that return <xref:System.Nullable%601>, such as `ReadAsBoolean`, which handles a `Null` `TokenType` for you by returning a `bool?`.</span></span> <span data-ttu-id="c6264-443">기본 제공 `System.Text.Json` Api는 null을 허용 하지 않는 값 형식만 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-443">The built-in `System.Text.Json` APIs return only non-nullable value types.</span></span> <span data-ttu-id="c6264-444">예를 들어 <xref:System.Text.Json.Utf8JsonReader.GetBoolean%2A?displayProperty=nameWithType>은 `bool`을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-444">For example, <xref:System.Text.Json.Utf8JsonReader.GetBoolean%2A?displayProperty=nameWithType> returns a `bool`.</span></span> <span data-ttu-id="c6264-445">JSON에서 `Null` 발견 되 면 예외를 throw 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-445">It throws an exception if it finds `Null` in the JSON.</span></span> <span data-ttu-id="c6264-446">다음 예제에서는 null을 처리 하는 두 가지 방법, 즉 nullable 값 형식을 반환 하 고 기본값을 반환 하 여 null을 처리 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-446">The following examples show two ways to handle nulls, one by returning a nullable value type and one by returning the default value:</span></span>

```csharp
public bool? ReadAsNullableBoolean()
{
    _reader.Read();
    if (_reader.TokenType == JsonTokenType.Null)
    {
        return null;
    }
    if (_reader.TokenType != JsonTokenType.True && _reader.TokenType != JsonTokenType.False)
    {
        throw new JsonException();
    }
    return _reader.GetBoolean();
}
```

```csharp
public bool ReadAsBoolean(bool defaultValue)
{
    _reader.Read();
    if (_reader.TokenType == JsonTokenType.Null)
    {
        return defaultValue;
    }
    if (_reader.TokenType != JsonTokenType.True && _reader.TokenType != JsonTokenType.False)
    {
        throw new JsonException();
    }
    return _reader.GetBoolean();
}
```

### <a name="multi-targeting"></a><span data-ttu-id="c6264-447">멀티 타기팅</span><span class="sxs-lookup"><span data-stu-id="c6264-447">Multi-targeting</span></span>

<span data-ttu-id="c6264-448">특정 대상 프레임 워크에 대 한 `Newtonsoft.Json`를 계속 사용 해야 하는 경우 다중 대상을 지정할 수 있으며 두 가지 구현을 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-448">If you need to continue to use `Newtonsoft.Json` for certain target frameworks, you can multi-target and have two implementations.</span></span> <span data-ttu-id="c6264-449">그러나이 방법은 간단 하지 않으며 일부 `#ifdefs` 및 원본 복제가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-449">However, this is not trivial and would require some `#ifdefs` and source duplication.</span></span> <span data-ttu-id="c6264-450">가능한 한 많은 코드를 공유 하는 한 가지 방법은 `Utf8JsonReader` 및 `Newtonsoft.Json` `JsonTextReader`주위에 `ref struct` 래퍼를 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-450">One way to share as much code as possible is to create a `ref struct` wrapper around `Utf8JsonReader` and `Newtonsoft.Json` `JsonTextReader`.</span></span> <span data-ttu-id="c6264-451">이 래퍼는 동작 차이를 격리 하면서 공개 노출 영역을 통합 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-451">This wrapper would unify the public surface area while isolating the behavioral differences.</span></span> <span data-ttu-id="c6264-452">이렇게 하면 새 형식을 참조로 전달 하는 것과 함께 주로 형식 생성에 대 한 변경 내용을 격리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-452">This lets you isolate the changes mainly to the construction of the type, along with passing the new type around by reference.</span></span> <span data-ttu-id="c6264-453">[DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/) 라이브러리가 따르는 패턴은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-453">This is the pattern that the [Microsoft.Extensions.DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/) library follows:</span></span>

* [<span data-ttu-id="c6264-454">UnifiedJsonReader.JsonTextReader.cs</span><span class="sxs-lookup"><span data-stu-id="c6264-454">UnifiedJsonReader.JsonTextReader.cs</span></span>](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonReader.JsonTextReader.cs)
* [<span data-ttu-id="c6264-455">UnifiedJsonReader.Utf8JsonReader.cs</span><span class="sxs-lookup"><span data-stu-id="c6264-455">UnifiedJsonReader.Utf8JsonReader.cs</span></span>](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonReader.Utf8JsonReader.cs)

## <a name="utf8jsonwriter-compared-to-jsontextwriter"></a><span data-ttu-id="c6264-456">Utf8JsonWriter와 JsonTextWriter 비교</span><span class="sxs-lookup"><span data-stu-id="c6264-456">Utf8JsonWriter compared to JsonTextWriter</span></span>

<span data-ttu-id="c6264-457"><xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName>는 `String`, `Int32`및 `DateTime`같은 일반적인 .NET 유형에 서 UTF-8 인코딩 JSON 텍스트를 작성 하는 고성능 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-457"><xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName> is a high-performance way to write UTF-8 encoded JSON text from common .NET types like `String`, `Int32`, and `DateTime`.</span></span> <span data-ttu-id="c6264-458">기록기는 사용자 지정 serializer를 빌드하는 데 사용할 수 있는 하위 수준 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-458">The writer is a low-level type that can be used to build custom serializers.</span></span>

<span data-ttu-id="c6264-459">다음 섹션에서는 `Utf8JsonWriter`사용을 위한 권장 프로그래밍 패턴에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-459">The following sections explain recommended programming patterns for using `Utf8JsonWriter`.</span></span>

### <a name="write-with-utf-8-text"></a><span data-ttu-id="c6264-460">UTF-8 텍스트를 사용 하 여 쓰기</span><span class="sxs-lookup"><span data-stu-id="c6264-460">Write with UTF-8 text</span></span>

<span data-ttu-id="c6264-461">`Utf8JsonWriter`사용 하는 동안 최상의 성능을 얻으려면 UTF-16 문자열이 아닌 u t f-8 텍스트로 이미 인코드된 JSON 페이로드를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-461">To achieve the best possible performance while using the `Utf8JsonWriter`, write JSON payloads already encoded as UTF-8 text rather than as UTF-16 strings.</span></span> <span data-ttu-id="c6264-462"><xref:System.Text.Json.JsonEncodedText>를 사용 하 여 알려진 문자열 속성 이름 및 값을 캐시 하 고 정적으로 미리 인코딩하고 UTF-16 문자열 리터럴을 사용 하는 대신 작성기에 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-462">Use <xref:System.Text.Json.JsonEncodedText> to cache and pre-encode known string property names and values as statics, and pass those to the writer, rather than using UTF-16 string literals.</span></span> <span data-ttu-id="c6264-463">이는 u t f-8 바이트 배열을 캐시 하 고 사용 하는 것 보다 빠릅니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-463">This is faster than caching and using UTF-8 byte arrays.</span></span>

<span data-ttu-id="c6264-464">이 방법은 사용자 지정 이스케이프를 수행 해야 하는 경우에도 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-464">This approach also works if you need to do custom escaping.</span></span> <span data-ttu-id="c6264-465">`System.Text.Json` 문자열을 작성 하는 동안 이스케이프를 사용 하지 않도록 설정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-465">`System.Text.Json` doesn't let you disable escaping while writing a string.</span></span> <span data-ttu-id="c6264-466">그러나 사용자 지정 <xref:System.Text.Encodings.Web.JavaScriptEncoder>를 작성기에 옵션으로 전달 하거나 `JavascriptEncoder`를 사용 하 여 이스케이프를 수행 하는 고유한 `JsonEncodedText` 만든 다음 문자열 대신 `JsonEncodedText`를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-466">However, you could pass in your own custom <xref:System.Text.Encodings.Web.JavaScriptEncoder> as an option to the writer, or create your own `JsonEncodedText` that uses your `JavascriptEncoder` to do the escaping, and then write the `JsonEncodedText` instead of the string.</span></span> <span data-ttu-id="c6264-467">자세한 내용은 [문자 인코딩 사용자 지정](system-text-json-how-to.md#customize-character-encoding)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-467">For more information, see [Customize character encoding](system-text-json-how-to.md#customize-character-encoding).</span></span>

### <a name="write-raw-values"></a><span data-ttu-id="c6264-468">원시 값 쓰기</span><span class="sxs-lookup"><span data-stu-id="c6264-468">Write raw values</span></span>

<span data-ttu-id="c6264-469">`Newtonsoft.Json` `WriteRawValue` 메서드는 값이 필요한 원시 JSON을 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-469">The `Newtonsoft.Json` `WriteRawValue` method writes raw JSON where a value is expected.</span></span> <span data-ttu-id="c6264-470"><xref:System.Text.Json>에는 직접적인 대응 기능이 없지만, 올바른 JSON만 작성 되도록 하는 해결 방법은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-470"><xref:System.Text.Json> has no direct equivalent, but here's a workaround that ensures only valid JSON is written:</span></span>

```csharp
using JsonDocument doc = JsonDocument.Parse(string);
doc.WriteTo(writer);
```

### <a name="customize-character-escaping"></a><span data-ttu-id="c6264-471">문자 이스케이프 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="c6264-471">Customize character escaping</span></span>

<span data-ttu-id="c6264-472">`JsonTextWriter` [StringEscapeHandling](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_StringEscapeHandling.htm) 설정은 ASCII가 아닌 문자 **또는** HTML 문자를 모두 이스케이프 하는 옵션을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-472">The [StringEscapeHandling](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_StringEscapeHandling.htm) setting of `JsonTextWriter` offers options to escape all non-ASCII characters **or** HTML characters.</span></span> <span data-ttu-id="c6264-473">기본적으로 `Utf8JsonWriter`는 ASCII가 아닌 문자 **및** HTML 문자를 모두 이스케이프 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-473">By default, `Utf8JsonWriter` escapes all non-ASCII **and** HTML characters.</span></span> <span data-ttu-id="c6264-474">이러한 이스케이프는 심층 방어 보안을 위해 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-474">This escaping is done for defense-in-depth security reasons.</span></span> <span data-ttu-id="c6264-475">다른 이스케이프 정책을 지정 하려면 <xref:System.Text.Encodings.Web.JavaScriptEncoder> 만들고 <xref:System.Text.Json.JsonWriterOptions.Encoder?displayProperty=nameWithType>를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-475">To specify a different escaping policy, create a <xref:System.Text.Encodings.Web.JavaScriptEncoder> and set <xref:System.Text.Json.JsonWriterOptions.Encoder?displayProperty=nameWithType>.</span></span> <span data-ttu-id="c6264-476">자세한 내용은 [문자 인코딩 사용자 지정](system-text-json-how-to.md#customize-character-encoding)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c6264-476">For more information, see [Customize character encoding](system-text-json-how-to.md#customize-character-encoding).</span></span>

### <a name="customize-json-format"></a><span data-ttu-id="c6264-477">JSON 형식 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="c6264-477">Customize JSON format</span></span>

<span data-ttu-id="c6264-478">`JsonTextWriter`에는 다음과 같은 설정이 포함 됩니다. `Utf8JsonWriter`에는 해당 항목이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-478">`JsonTextWriter` includes the following settings, for which `Utf8JsonWriter` has no equivalent:</span></span>

* <span data-ttu-id="c6264-479">[들여쓰기](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_Indentation.htm) -들여쓸 문자 수를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-479">[Indentation](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_Indentation.htm) - Specifies how many characters to indent.</span></span> <span data-ttu-id="c6264-480">`Utf8JsonWriter` 항상 2 자 들여쓰기를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-480">`Utf8JsonWriter` always does 2-character indentation.</span></span>
* <span data-ttu-id="c6264-481">[Indentchar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_IndentChar.htm) -들여쓰기에 사용할 문자를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-481">[IndentChar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_IndentChar.htm) - Specifies the character to use for indentation.</span></span>  <span data-ttu-id="c6264-482">`Utf8JsonWriter`은 항상 공백을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-482">`Utf8JsonWriter` always uses whitespace.</span></span>
* <span data-ttu-id="c6264-483">[QuoteChar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteChar.htm) -문자열 값을 묶는 데 사용할 문자를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-483">[QuoteChar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteChar.htm) - Specifies the character to use to surround string values.</span></span>  <span data-ttu-id="c6264-484">`Utf8JsonWriter` 항상 큰따옴표를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-484">`Utf8JsonWriter` always uses double quotes.</span></span>
* <span data-ttu-id="c6264-485">[QuoteName](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteName.htm) -속성 이름을 따옴표로 묶을 지 여부를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-485">[QuoteName](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteName.htm) - Specifies whether or not to surround property names with quotes.</span></span>  <span data-ttu-id="c6264-486">`Utf8JsonWriter` 항상 따옴표로 묶습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-486">`Utf8JsonWriter` always surrounds them with quotes.</span></span>

<span data-ttu-id="c6264-487">이러한 방법으로 `Utf8JsonWriter`에서 생성 된 JSON을 사용자 지정할 수 있는 해결 방법은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-487">There are no workarounds that would let you customize the JSON produced by `Utf8JsonWriter` in these ways.</span></span>

### <a name="write-null-values"></a><span data-ttu-id="c6264-488">Null 값 쓰기</span><span class="sxs-lookup"><span data-stu-id="c6264-488">Write null values</span></span>

<span data-ttu-id="c6264-489">`Utf8JsonWriter`를 사용 하 여 null 값을 쓰려면를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-489">To write null values by using `Utf8JsonWriter`, call:</span></span>

* <span data-ttu-id="c6264-490">값으로 null을 사용 하 여 키-값 쌍을 쓰려면 <xref:System.Text.Json.Utf8JsonWriter.WriteNull%2A> 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-490"><xref:System.Text.Json.Utf8JsonWriter.WriteNull%2A> to write a key-value pair with null as the value.</span></span>
* <span data-ttu-id="c6264-491">null을 JSON 배열의 요소로 쓰려면 <xref:System.Text.Json.Utf8JsonWriter.WriteNullValue%2A> 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-491"><xref:System.Text.Json.Utf8JsonWriter.WriteNullValue%2A> to write null as an element of a JSON array.</span></span>

<span data-ttu-id="c6264-492">문자열 속성의 경우 문자열이 null 이면 <xref:System.Text.Json.Utf8JsonWriter.WriteString%2A> 및 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A>은 `WriteNull` 및 `WriteNullValue`와 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-492">For a string property, if the string is null, <xref:System.Text.Json.Utf8JsonWriter.WriteString%2A> and <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A> are equivalent to `WriteNull` and `WriteNullValue`.</span></span>

### <a name="write-timespan-uri-or-char-values"></a><span data-ttu-id="c6264-493">Timespan, Uri 또는 char 값 쓰기</span><span class="sxs-lookup"><span data-stu-id="c6264-493">Write Timespan, Uri, or char values</span></span>

<span data-ttu-id="c6264-494">`JsonTextWriter`는 [TimeSpan](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_18.htm), [Uri](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_22.htm)및 [char](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_3.htm) 값에 대 한 `WriteValue` 메서드를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-494">`JsonTextWriter` provides `WriteValue` methods for [TimeSpan](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_18.htm), [Uri](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_22.htm), and [char](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_3.htm) values.</span></span> <span data-ttu-id="c6264-495">`Utf8JsonWriter`에는 동일한 메서드가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-495">`Utf8JsonWriter` doesn't have equivalent methods.</span></span> <span data-ttu-id="c6264-496">대신 `ToString()`를 호출 하 여 이러한 값을 문자열로 포맷 하 고 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A>를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-496">Instead, format these values as strings (by calling `ToString()`, for example) and call <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A>.</span></span>

### <a name="multi-targeting"></a><span data-ttu-id="c6264-497">멀티 타기팅</span><span class="sxs-lookup"><span data-stu-id="c6264-497">Multi-targeting</span></span>

<span data-ttu-id="c6264-498">특정 대상 프레임 워크에 대 한 `Newtonsoft.Json`를 계속 사용 해야 하는 경우 다중 대상을 지정할 수 있으며 두 가지 구현을 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-498">If you need to continue to use `Newtonsoft.Json` for certain target frameworks, you can multi-target and have two implementations.</span></span> <span data-ttu-id="c6264-499">그러나이 방법은 간단 하지 않으며 일부 `#ifdefs` 및 원본 복제가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-499">However, this is not trivial and would require some `#ifdefs` and source duplication.</span></span> <span data-ttu-id="c6264-500">가능한 한 많은 코드를 공유 하는 한 가지 방법은 `Utf8JsonWriter`와 `Newtonsoft` `JsonTextWriter`에 대 한 래퍼를 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-500">One way to share as much code as possible is to create a wrapper around `Utf8JsonWriter` and `Newtonsoft` `JsonTextWriter`.</span></span> <span data-ttu-id="c6264-501">이 래퍼는 동작 차이를 격리 하면서 공개 노출 영역을 통합 합니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-501">This wrapper would unify the public surface area while isolating the behavioral differences.</span></span> <span data-ttu-id="c6264-502">이를 통해 주로 형식 생성에 대 한 변경 내용을 격리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-502">This lets you isolate the changes mainly to the construction of the type.</span></span> <span data-ttu-id="c6264-503">[DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/) 라이브러리는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="c6264-503">[Microsoft.Extensions.DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/) library follows:</span></span>

* [<span data-ttu-id="c6264-504">UnifiedJsonWriter.JsonTextWriter.cs</span><span class="sxs-lookup"><span data-stu-id="c6264-504">UnifiedJsonWriter.JsonTextWriter.cs</span></span>](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonWriter.JsonTextWriter.cs)
* [<span data-ttu-id="c6264-505">UnifiedJsonWriter.Utf8JsonWriter.cs</span><span class="sxs-lookup"><span data-stu-id="c6264-505">UnifiedJsonWriter.Utf8JsonWriter.cs</span></span>](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonWriter.Utf8JsonWriter.cs)

## <a name="additional-resources"></a><span data-ttu-id="c6264-506">추가 자료</span><span class="sxs-lookup"><span data-stu-id="c6264-506">Additional resources</span></span>

<!-- * [System.Text.Json roadmap](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/roadmap/README.md)[Restore this when the roadmap is updated.]-->
* [<span data-ttu-id="c6264-507">System.object 개요</span><span class="sxs-lookup"><span data-stu-id="c6264-507">System.Text.Json overview</span></span>](system-text-json-overview.md)
* [<span data-ttu-id="c6264-508">System.object를 사용 하는 방법</span><span class="sxs-lookup"><span data-stu-id="c6264-508">How to use System.Text.Json</span></span>](system-text-json-how-to.md)
* [<span data-ttu-id="c6264-509">사용자 지정 변환기를 작성 하는 방법</span><span class="sxs-lookup"><span data-stu-id="c6264-509">How to write custom converters</span></span>](system-text-json-converters-how-to.md)
* [<span data-ttu-id="c6264-510">System.object의 DateTime 및 DateTimeOffset 지원</span><span class="sxs-lookup"><span data-stu-id="c6264-510">DateTime and DateTimeOffset support in System.Text.Json</span></span>](../datetime/system-text-json-support.md)
* [<span data-ttu-id="c6264-511">System.object API 참조</span><span class="sxs-lookup"><span data-stu-id="c6264-511">System.Text.Json API reference</span></span>](xref:System.Text.Json)
* [<span data-ttu-id="c6264-512">System.string API 참조</span><span class="sxs-lookup"><span data-stu-id="c6264-512">System.Text.Json.Serialization API reference</span></span>](xref:System.Text.Json.Serialization)