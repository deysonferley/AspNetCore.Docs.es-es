---
title: Uso de ASP.NET Core con HTTP/2 en IIS
author: rick-anderson
description: Obtenga información sobre cómo usar las características de HTTP/2 con IIS.
monikerRange: '>= aspnetcore-5.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
no-loc:
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: host-and-deploy/iis/protocols
ms.openlocfilehash: 1c3748eeead1b8cccdb6112150cf90fb5597b689
ms.sourcegitcommit: e519d95d17443abafba8f712ac168347b15c8b57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91654352"
---
# <a name="use-aspnet-core-with-http2-on-iis"></a><span data-ttu-id="9e9fd-103">Uso de ASP.NET Core con HTTP/2 en IIS</span><span class="sxs-lookup"><span data-stu-id="9e9fd-103">Use ASP.NET Core with HTTP/2 on IIS</span></span>

<span data-ttu-id="9e9fd-104">Por [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="9e9fd-104">By [Justin Kotalik](https://github.com/jkotalik)</span></span>

<span data-ttu-id="9e9fd-105">[HTTP/2](https://httpwg.org/specs/rfc7540.html) es compatible con ASP.NET Core en los escenarios de implementación de IIS siguientes:</span><span class="sxs-lookup"><span data-stu-id="9e9fd-105">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following IIS deployment scenarios:</span></span>

* <span data-ttu-id="9e9fd-106">Windows 2016 o posterior; Windows 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="9e9fd-106">Windows Server 2016 or later / Windows 10 or later</span></span>
* <span data-ttu-id="9e9fd-107">IIS 10 o posterior</span><span class="sxs-lookup"><span data-stu-id="9e9fd-107">IIS 10 or later</span></span>
* <span data-ttu-id="9e9fd-108">Conexión con TLS 1.2 o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="9e9fd-108">TLS 1.2 or later connection</span></span>
* <span data-ttu-id="9e9fd-109">Al [hospedar fuera del proceso](xref:host-and-deploy/iis/index#out-of-process-hosting-model): Las conexiones de servidor perimetral de acceso público usan HTTP/2, pero la conexión de proxy inverso al [servidor de Kestrel](xref:fundamentals/servers/kestrel) usa HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-109">When [hosting out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model): Public-facing edge server connections use HTTP/2, but the reverse proxy connection to the [Kestrel server](xref:fundamentals/servers/kestrel) uses HTTP/1.1.</span></span>

<span data-ttu-id="9e9fd-110">Para una implementación dentro del proceso cuando se establece una conexión HTTP/2, [`HttpRequest.Protocol`](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-110">For an in-process deployment when an HTTP/2 connection is established, [`HttpRequest.Protocol`](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span> <span data-ttu-id="9e9fd-111">Para una implementación fuera del proceso cuando se establece una conexión HTTP/2, [`HttpRequest.Protocol`](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/1.1`.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-111">For an out-of-process deployment when an HTTP/2 connection is established, [`HttpRequest.Protocol`](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

<span data-ttu-id="9e9fd-112">Para obtener más información sobre los modelos de hospedaje en proceso y fuera de proceso, consulte <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-112">For more information on the in-process and out-of-process hosting models, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="9e9fd-113">HTTP/2 está habilitado de forma predeterminada para las conexiones HTTPS/TLS.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-113">HTTP/2 is enabled by default for HTTPS/TLS connections.</span></span> <span data-ttu-id="9e9fd-114">Las conexiones vuelven a HTTP/1.1 si no se establece una conexión HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-114">Connections fall back to HTTP/1.1 if an HTTP/2 connection isn't established.</span></span> <span data-ttu-id="9e9fd-115">Para más información sobre la configuración HTTP/2 con implementaciones de IIS, vea [HTTP/2 en IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span><span class="sxs-lookup"><span data-stu-id="9e9fd-115">For more information on HTTP/2 configuration with IIS deployments, see [HTTP/2 on IIS](/iis/get-started/whats-new-in-iis-10/http2-on-iis).</span></span>

## <a name="advanced-http2-features-to-support-grpc"></a><span data-ttu-id="9e9fd-116">Características avanzadas de HTTP/2 para admitir gRPC</span><span class="sxs-lookup"><span data-stu-id="9e9fd-116">Advanced HTTP/2 features to support gRPC</span></span>

<span data-ttu-id="9e9fd-117">Las características adicionales de HTTP/2 en IIS admiten gRPC, incluida la compatibilidad con los finalizadores de respuesta y el envío de marcos de restablecimiento.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-117">Additional HTTP/2 features in IIS support gRPC, including support for response trailers and sending reset frames.</span></span>

<span data-ttu-id="9e9fd-118">Requisitos para ejecutar gRPC en IIS:</span><span class="sxs-lookup"><span data-stu-id="9e9fd-118">Requirements to run gRPC on IIS:</span></span>

* <span data-ttu-id="9e9fd-119">Hospedaje dentro del proceso.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-119">In-process hosting.</span></span>
* <span data-ttu-id="9e9fd-120">Windows 10, compilación del sistema operativo 20300.1000 o posterior.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-120">Windows 10, OS Build 20300.1000 or later.</span></span> <span data-ttu-id="9e9fd-121">Puede requerir el uso de compilaciones de Windows Insider.</span><span class="sxs-lookup"><span data-stu-id="9e9fd-121">May require use of Windows Insider Builds.</span></span>
* <span data-ttu-id="9e9fd-122">Conexión con TLS 1.2 o una versión posterior</span><span class="sxs-lookup"><span data-stu-id="9e9fd-122">TLS 1.2 or later connection</span></span>

### <a name="trailers"></a><span data-ttu-id="9e9fd-123">Clips finales</span><span class="sxs-lookup"><span data-stu-id="9e9fd-123">Trailers</span></span>

[!INCLUDE[](~/includes/trailers.md)]

### <a name="reset"></a><span data-ttu-id="9e9fd-124">Reset</span><span class="sxs-lookup"><span data-stu-id="9e9fd-124">Reset</span></span>

[!INCLUDE[](~/includes/reset.md)]