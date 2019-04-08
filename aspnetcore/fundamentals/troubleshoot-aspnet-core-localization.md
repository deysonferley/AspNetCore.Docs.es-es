---
title: Solución de problemas de localización de ASP.NET Core
author: hishamco
description: Obtenga información sobre cómo diagnosticar problemas con la localización en aplicaciones de ASP.NET Core.
ms.date: 01/24/2019
uid: fundamentals/troubleshoot-aspnet-core-localization
ms.openlocfilehash: 73405f539c89d79096e7b536407cd9730679d478
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58489002"
---
# <a name="troubleshoot-aspnet-core-localization"></a><span data-ttu-id="a389d-103">Solución de problemas de localización de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a389d-103">Troubleshoot ASP.NET Core Localization</span></span>

<span data-ttu-id="a389d-104">Por [Hisham Bin Ateya](https://github.com/hishamco)</span><span class="sxs-lookup"><span data-stu-id="a389d-104">By [Hisham Bin Ateya](https://github.com/hishamco)</span></span>

<span data-ttu-id="a389d-105">En este artículo se proporcionan instrucciones sobre cómo diagnosticar problemas de localización en aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a389d-105">This article provides instructions on how to diagnose ASP.NET Core app localization issues.</span></span>

## <a name="localization-configuration-issues"></a><span data-ttu-id="a389d-106">Problemas de configuración de localización</span><span class="sxs-lookup"><span data-stu-id="a389d-106">Localization configuration issues</span></span>

<span data-ttu-id="a389d-107">**Orden del middleware de localización**</span><span class="sxs-lookup"><span data-stu-id="a389d-107">**Localization middleware order**</span></span>  
<span data-ttu-id="a389d-108">Es posible que la aplicación no esté localizada porque el middleware de localización no esté en el orden esperado.</span><span class="sxs-lookup"><span data-stu-id="a389d-108">The app may not localize because the localization middleware isn't ordered as expected.</span></span>

<span data-ttu-id="a389d-109">Para resolver este problema, asegúrese de que el middleware de localización esté registrado antes del middleware de MVC.</span><span class="sxs-lookup"><span data-stu-id="a389d-109">To resolve this issue, ensure that localization middleware is registered before MVC middleware.</span></span> <span data-ttu-id="a389d-110">En caso contrario, el middleware de localización no se aplicará.</span><span class="sxs-lookup"><span data-stu-id="a389d-110">Otherwise, the localization middleware isn't applied.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddMvc();
}
```

<span data-ttu-id="a389d-111">**Ruta de acceso a los recursos de localización no encontrada**</span><span class="sxs-lookup"><span data-stu-id="a389d-111">**Localization resources path not found**</span></span>

<span data-ttu-id="a389d-112">**Error de coincidencia entre las referencias culturales admitidas en RequestCultureProvider y las registradas**</span><span class="sxs-lookup"><span data-stu-id="a389d-112">**Supported Cultures in RequestCultureProvider don't match with registered once**</span></span>  

## <a name="resource-file-naming-issues"></a><span data-ttu-id="a389d-113">Problemas con la nomenclatura de los archivos de recursos</span><span class="sxs-lookup"><span data-stu-id="a389d-113">Resource file naming issues</span></span>

<span data-ttu-id="a389d-114">ASP.NET Core ha predefinido reglas y directrices para la nomenclatura de archivos de recursos de localización, que se describen detalladamente [aquí](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span><span class="sxs-lookup"><span data-stu-id="a389d-114">ASP.NET Core has predefined rules and guidelines for localization resources file naming, which are described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span></span>

## <a name="missing-resources"></a><span data-ttu-id="a389d-115">Recursos no disponibles</span><span class="sxs-lookup"><span data-stu-id="a389d-115">Missing resources</span></span>

<span data-ttu-id="a389d-116">Estas son algunas de las causas que suelen provocar que no se encuentren los archivos:</span><span class="sxs-lookup"><span data-stu-id="a389d-116">Common causes of resources not being found include:</span></span>

- <span data-ttu-id="a389d-117">Los nombres de los recursos están mal escritos, bien en el archivo `resx`, bien en la solicitud del localizador.</span><span class="sxs-lookup"><span data-stu-id="a389d-117">Resource names are misspelled in either the `resx` file or the localizer request.</span></span>
- <span data-ttu-id="a389d-118">Falta el recurso en `resx` para algunos idiomas, pero existe para otros.</span><span class="sxs-lookup"><span data-stu-id="a389d-118">The resource is missing from the `resx` for some languages, but exists in others.</span></span>
- <span data-ttu-id="a389d-119">Si sigue teniendo problemas, compruebe los mensajes del registro de localización (que están en el nivel de registro `Debug`) para obtener información detallada sobre los recursos que faltan.</span><span class="sxs-lookup"><span data-stu-id="a389d-119">If you're still having trouble, check the localization log messages (which are at `Debug` log level) for more details about the missing resources.</span></span>

<span data-ttu-id="a389d-120">_**Sugerencia**: Si usa `CookieRequestCultureProvider`, compruebe que no se usen comillas simples con las referencias culturales dentro del valor de la cookie de localización. Por ejemplo, `c='en-UK'|uic='en-US'` es un valor de cookie no válido, pero `c=en-UK|uic=en-US` es válido._</span><span class="sxs-lookup"><span data-stu-id="a389d-120">_**Hint:** When using `CookieRequestCultureProvider`, verify single quotes are not used with the cultures inside the localization cookie value. For example, `c='en-UK'|uic='en-US'` is an invalid cookie value, while `c=en-UK|uic=en-US` is a valid._</span></span>

## <a name="resources--class-libraries-issues"></a><span data-ttu-id="a389d-121">Problemas de las bibliotecas de clases y recursos</span><span class="sxs-lookup"><span data-stu-id="a389d-121">Resources & Class Libraries issues</span></span>

<span data-ttu-id="a389d-122">De manera predeterminada, ASP.NET Core proporciona un método para permitir que las bibliotecas de clases encuentren los archivos de recursos mediante [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="a389d-122">ASP.NET Core by default provides a way to allow the class libraries to find their resource files via [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="a389d-123">Estos son algunos de los problemas habituales con las bibliotecas de clases:</span><span class="sxs-lookup"><span data-stu-id="a389d-123">Common issues with class libraries include:</span></span>
- <span data-ttu-id="a389d-124">Falta `ResourceLocationAttribute` en la biblioteca de clases, lo que impide que `ResourceManagerStringLocalizerFactory` detecte los recursos.</span><span class="sxs-lookup"><span data-stu-id="a389d-124">Missing the `ResourceLocationAttribute` in a class library will prevent `ResourceManagerStringLocalizerFactory` from discovering the resources.</span></span>
- <span data-ttu-id="a389d-125">Nomenclatura de los archivos de recursos.</span><span class="sxs-lookup"><span data-stu-id="a389d-125">Resource file naming.</span></span> <span data-ttu-id="a389d-126">Para obtener más información, consulte la sección [Problemas con la nomenclatura de los archivos de recursos](#resource-file-naming-issues).</span><span class="sxs-lookup"><span data-stu-id="a389d-126">For more information, see [Resource file naming issues](#resource-file-naming-issues) section.</span></span>
- <span data-ttu-id="a389d-127">Cambio del espacio de nombres raíz de la biblioteca de clases.</span><span class="sxs-lookup"><span data-stu-id="a389d-127">Changing the root namespace of the class library.</span></span> <span data-ttu-id="a389d-128">Para obtener más información, vea la sección [Problemas con el espacio de nombres raíz](#root-namespace-issues).</span><span class="sxs-lookup"><span data-stu-id="a389d-128">For more information, see [Root Namespace issues](#root-namespace-issues) section.</span></span>

## <a name="customrequestcultureprovider-doesnt-work-as-expected"></a><span data-ttu-id="a389d-129">CustomRequestCultureProvider no funciona según lo previsto</span><span class="sxs-lookup"><span data-stu-id="a389d-129">CustomRequestCultureProvider doesn't work as expected</span></span>

<span data-ttu-id="a389d-130">La clase `RequestLocalizationOptions` tiene tres proveedores predeterminados:</span><span class="sxs-lookup"><span data-stu-id="a389d-130">The `RequestLocalizationOptions` class has three default providers:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="a389d-131">[CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) permite personalizar el modo en el que se proporciona la referencia cultural de localización en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a389d-131">The [CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) allows you to customize how the localization culture is provided in your app.</span></span> <span data-ttu-id="a389d-132">Si los proveedores predeterminados no cumplen con los requisitos, se usa `CustomRequestCultureProvider`.</span><span class="sxs-lookup"><span data-stu-id="a389d-132">The `CustomRequestCultureProvider` is used when the default providers don't meet your requirements.</span></span>

- <span data-ttu-id="a389d-133">Un motivo habitual por el que un proveedor personalizado no funciona adecuadamente es que no es el primero en la lista `RequestCultureProviders`.</span><span class="sxs-lookup"><span data-stu-id="a389d-133">A common reason custom provider don't work properly is that it isn't the first provider in the `RequestCultureProviders` list.</span></span> <span data-ttu-id="a389d-134">Para resolver este problema, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a389d-134">To resolve this issue:</span></span>

- <span data-ttu-id="a389d-135">Inserte el proveedor personalizado en la posición 0 de la lista `RequestCultureProviders`, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a389d-135">Insert the custom provider at the position 0 in the `RequestCultureProviders` list as the following:</span></span>

```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
```

- <span data-ttu-id="a389d-136">Use el método de extensión `AddInitialRequestCultureProvider` para establecer el proveedor personalizado como el inicial.</span><span class="sxs-lookup"><span data-stu-id="a389d-136">Use `AddInitialRequestCultureProvider` extension method to set the custom provider as initial provider.</span></span>

## <a name="root-namespace-issues"></a><span data-ttu-id="a389d-137">Problemas con el espacio de nombres raíz</span><span class="sxs-lookup"><span data-stu-id="a389d-137">Root Namespace issues</span></span>

<span data-ttu-id="a389d-138">Si el espacio de nombres raíz de un ensamblado es distinto al nombre del ensamblado, la localización no funcionará de manera predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a389d-138">When the root namespace of an assembly is different than the assembly name, localization doesn't work by default.</span></span> <span data-ttu-id="a389d-139">Para evitar este problema, use [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), procedimiento que se describe detalladamente [aquí](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span><span class="sxs-lookup"><span data-stu-id="a389d-139">To avoid this issue use [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), which is described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)</span></span>

## <a name="resources--build-action"></a><span data-ttu-id="a389d-140">Acción de compilación y recursos</span><span class="sxs-lookup"><span data-stu-id="a389d-140">Resources & Build Action</span></span>

<span data-ttu-id="a389d-141">Si usa archivos de recursos para la localización, es importante que estos tengan una acción de compilación adecuada.</span><span class="sxs-lookup"><span data-stu-id="a389d-141">If you use resource files for localization, it's important that they have an appropriate build action.</span></span> <span data-ttu-id="a389d-142">Deben ser **recursos insertados**, puesto que, de lo contrario, `ResourceStringLocalizer` no podrá encontrarlos.</span><span class="sxs-lookup"><span data-stu-id="a389d-142">They should be **Embedded Resource**, otherwise the `ResourceStringLocalizer` is not able to find these resources.</span></span>