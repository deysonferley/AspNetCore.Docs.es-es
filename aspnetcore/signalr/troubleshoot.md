---
title: Solución de problemas de conexión de ASP.NET Core SignalR
author: bradygaster
description: Solución de problemas de conexión de ASP.NET Core SignalR .
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/08/2020
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
uid: signalr/troubleshoot
ms.openlocfilehash: 2ffe8bcf4bef5dcb9fc74513c7507e5cb3a6b7cb
ms.sourcegitcommit: fad0cd264c9d07a48a8c6ba1690807e0f8728898
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92379568"
---
# <a name="troubleshoot-connection-errors"></a><span data-ttu-id="de125-103">Solución de problemas de conexión</span><span class="sxs-lookup"><span data-stu-id="de125-103">Troubleshoot connection errors</span></span>

<span data-ttu-id="de125-104">En esta sección se proporciona ayuda con los errores que se pueden producir al intentar establecer una conexión con un SignalR concentrador de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="de125-104">This section provides help with errors that can occur when trying to establish a connection to a ASP.NET Core SignalR hub.</span></span>

### <a name="response-code-404"></a><span data-ttu-id="de125-105">Código de respuesta 404</span><span class="sxs-lookup"><span data-stu-id="de125-105">Response code 404</span></span>

<span data-ttu-id="de125-106">Al usar WebSockets y `skipNegotiation = true`</span><span class="sxs-lookup"><span data-stu-id="de125-106">When using WebSockets and `skipNegotiation = true`</span></span>
```log
WebSocket connection to 'wss://xxx/HubName' failed: Error during WebSocket handshake: Unexpected response code: 404
```

* <span data-ttu-id="de125-107">Cuando se usan varios servidores sin sesiones permanentes, la conexión puede iniciarse en un servidor y, a continuación, cambiar a otro servidor.</span><span class="sxs-lookup"><span data-stu-id="de125-107">When using multiple servers without sticky sessions, the connection can start on one server and then switch to another server.</span></span> <span data-ttu-id="de125-108">El otro servidor no es consciente de la conexión anterior.</span><span class="sxs-lookup"><span data-stu-id="de125-108">The other server is not aware of the previous connection.</span></span>
* <span data-ttu-id="de125-109">Compruebe que el cliente se está conectando al extremo correcto.</span><span class="sxs-lookup"><span data-stu-id="de125-109">Verify the client is connecting to the correct endpoint.</span></span> <span data-ttu-id="de125-110">Por ejemplo, el servidor se hospeda en `http://127.0.0.1:5000/hub/myHub` y el cliente intenta conectarse a `http://127.0.0.1:5000/myHub` .</span><span class="sxs-lookup"><span data-stu-id="de125-110">For example, the server is hosted at `http://127.0.0.1:5000/hub/myHub` and client is trying to connect to `http://127.0.0.1:5000/myHub`.</span></span>
* <span data-ttu-id="de125-111">Si la conexión usa el identificador y tarda demasiado tiempo en enviar una solicitud al servidor después de negociar, el servidor:</span><span class="sxs-lookup"><span data-stu-id="de125-111">If the connection uses the ID and takes too long to send a request to the server after the negotiate, the server:</span></span>

  * <span data-ttu-id="de125-112">Elimina el identificador.</span><span class="sxs-lookup"><span data-stu-id="de125-112">Deletes the ID.</span></span>
  * <span data-ttu-id="de125-113">Devuelve un 404.</span><span class="sxs-lookup"><span data-stu-id="de125-113">Returns a 404.</span></span>

### <a name="response-code-400-or-503"></a><span data-ttu-id="de125-114">Código de respuesta 400 o 503</span><span class="sxs-lookup"><span data-stu-id="de125-114">Response code 400 or 503</span></span>

<span data-ttu-id="de125-115">Para el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="de125-115">For the following error:</span></span>

```log
WebSocket connection to 'wss://xxx/HubName' failed: Error during WebSocket handshake: Unexpected response code: 400

Error: Failed to start the connection: Error: There was an error with the transport.
```

<span data-ttu-id="de125-116">Este error se debe normalmente a un cliente que usa solo el transporte de WebSockets, pero el protocolo WebSockets no está habilitado en el servidor.</span><span class="sxs-lookup"><span data-stu-id="de125-116">This error is usually caused by a client using only the WebSockets transport but the WebSockets protocol is not enabled on the server.</span></span>

### <a name="response-code-307"></a><span data-ttu-id="de125-117">Código de respuesta 307</span><span class="sxs-lookup"><span data-stu-id="de125-117">Response code 307</span></span>

<span data-ttu-id="de125-118">Al usar WebSockets y `skipNegotiation = true`</span><span class="sxs-lookup"><span data-stu-id="de125-118">When using WebSockets and `skipNegotiation = true`</span></span>
```log
WebSocket connection to 'ws://xxx/HubName' failed: Error during WebSocket handshake: Unexpected response code: 307
```

<span data-ttu-id="de125-119">Este error también puede producirse durante la solicitud Negotiate.</span><span class="sxs-lookup"><span data-stu-id="de125-119">This error can also happen during the negotiate request.</span></span>

<span data-ttu-id="de125-120">Causa común:</span><span class="sxs-lookup"><span data-stu-id="de125-120">Common cause:</span></span>
* <span data-ttu-id="de125-121">La aplicación está configurada para exigir HTTPS llamando a `UseHttpsRedirection` en `Startup` o aplica https a través de la regla de reescritura de URL.</span><span class="sxs-lookup"><span data-stu-id="de125-121">App is configured to enforce HTTPS by calling `UseHttpsRedirection` in `Startup`, or enforces HTTPS via URL rewrite rule.</span></span>

<span data-ttu-id="de125-122">Solución posible:</span><span class="sxs-lookup"><span data-stu-id="de125-122">Possible solution:</span></span>
* <span data-ttu-id="de125-123">Cambie la dirección URL en el lado cliente de "http" a "https".</span><span class="sxs-lookup"><span data-stu-id="de125-123">Change the URL on the client side from "http" to "https".</span></span> `.withUrl("https://xxx/HubName")`

### <a name="response-code-405"></a><span data-ttu-id="de125-124">Código de respuesta 405</span><span class="sxs-lookup"><span data-stu-id="de125-124">Response code 405</span></span>

<span data-ttu-id="de125-125">Código de estado http 405-método no permitido</span><span class="sxs-lookup"><span data-stu-id="de125-125">Http status code 405 - Method Not Allowed</span></span>

* <span data-ttu-id="de125-126">La aplicación no tiene [CORS](xref:signalr/security#cross-origin-resource-sharing) habilitado</span><span class="sxs-lookup"><span data-stu-id="de125-126">The app doesn't have [CORS](xref:signalr/security#cross-origin-resource-sharing) enabled</span></span>

### <a name="response-code-0"></a><span data-ttu-id="de125-127">Código de respuesta 0</span><span class="sxs-lookup"><span data-stu-id="de125-127">Response code 0</span></span>

<span data-ttu-id="de125-128">Código de estado http 0: normalmente un problema de [CORS](xref:signalr/security#cross-origin-resource-sharing) , no se proporciona código de estado</span><span class="sxs-lookup"><span data-stu-id="de125-128">Http status code 0 - Usually a [CORS](xref:signalr/security#cross-origin-resource-sharing) issue, no status code is given</span></span>

```log
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at http://localhost:5000/default/negotiate?negotiateVersion=1. (Reason: CORS header 'Access-Control-Allow-Origin' missing).
```

* <span data-ttu-id="de125-129">Agregue los orígenes esperados a `.WithOrigins(...)`</span><span class="sxs-lookup"><span data-stu-id="de125-129">Add the expected origins to `.WithOrigins(...)`</span></span>

```log
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at http://localhost:5000/default/negotiate?negotiateVersion=1. (Reason: expected 'true' in CORS header 'Access-Control-Allow-Credentials').
```

* <span data-ttu-id="de125-130">Agregue `.AllowCredentials()` a la Directiva de CORS.</span><span class="sxs-lookup"><span data-stu-id="de125-130">Add `.AllowCredentials()` to your CORS policy.</span></span> <span data-ttu-id="de125-131">No se puede usar `.AllowAnyOrigin()` o `.WithOrigins("*")` con esta opción</span><span class="sxs-lookup"><span data-stu-id="de125-131">Cannot use `.AllowAnyOrigin()` or `.WithOrigins("*")` with this option</span></span>

### <a name="response-code-413"></a><span data-ttu-id="de125-132">Código de respuesta 413</span><span class="sxs-lookup"><span data-stu-id="de125-132">Response code 413</span></span>

<span data-ttu-id="de125-133">Código de estado http 413-carga demasiado grande</span><span class="sxs-lookup"><span data-stu-id="de125-133">Http status code 413 - Payload Too Large</span></span>

<span data-ttu-id="de125-134">Esto suele deberse a que hay un token de acceso que está por encima de 4k.</span><span class="sxs-lookup"><span data-stu-id="de125-134">This is often caused by having an access token that is over 4k.</span></span>

* <span data-ttu-id="de125-135">Si usa el servicio de Azure SignalR , reduzca el tamaño del token mediante la personalización de las notificaciones que se envían a través del servicio con:</span><span class="sxs-lookup"><span data-stu-id="de125-135">If using the Azure SignalR Service, reduce the token size by customizing the claims being sent through the Service with:</span></span>
```csharp
.AddAzureSignalR(options =>
{
    options.ClaimsProvider = context => context.User.Claims;
});
```

### <a name="transient-network-failures"></a><span data-ttu-id="de125-136">Errores de red transitorios</span><span class="sxs-lookup"><span data-stu-id="de125-136">Transient network failures</span></span>

<span data-ttu-id="de125-137">Los errores de red transitorios pueden cerrar la SignalR conexión.</span><span class="sxs-lookup"><span data-stu-id="de125-137">Transient network failures may close the SignalR connection.</span></span> <span data-ttu-id="de125-138">El servidor puede interpretar la conexión cerrada como desconexión de cliente correcta.</span><span class="sxs-lookup"><span data-stu-id="de125-138">The server may interpret the closed connection as a graceful client disconnect.</span></span> <span data-ttu-id="de125-139">Para obtener más información sobre por qué un cliente se desconecta en esos casos [, recopile los registros del cliente y del servidor](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="de125-139">To get more info on why a client disconnected in those cases [gather logs from the client and server](xref:signalr/diagnostics).</span></span>