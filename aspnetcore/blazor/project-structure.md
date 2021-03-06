---
title: Estructura del proyecto de Blazor de ASP.NET Core
author: guardrex
description: Conozca la estructura del proyecto de la aplicación de Blazor de ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2020
no-loc:
- appsettings.json
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
uid: blazor/project-structure
ms.openlocfilehash: 6df84366dc3fc99d31a8a0e614ca860a50df7f5c
ms.sourcegitcommit: 3593c4efa707edeaaceffbfa544f99f41fc62535
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2021
ms.locfileid: "97513538"
---
# <a name="aspnet-core-no-locblazor-project-structure"></a>Estructura del proyecto de Blazor de ASP.NET Core

Por [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)

Los siguientes archivos y carpetas componen una aplicación Blazor generada a partir de una plantilla de proyecto Blazor:

::: moniker range=">= aspnetcore-5.0"

* `Program.cs`: El punto de entrada de la aplicación que configura lo siguiente:

  * ASP.NET Core [hospedado](xref:fundamentals/host/generic-host) (Blazor Server)
  * Host de WebAssembly (Blazor WebAssembly): el código de este archivo es único en las aplicaciones creadas a partir de la plantilla de Blazor WebAssembly (`blazorwasm`).
    * El componente `App` es el componente raíz de la aplicación. El componente `App` se especifica como el elemento DOM `div` con un elemento `id` de `app` (`<div id="app">Loading...</div>` en `wwwroot/index.html`) en la colección de componentes raíz (`builder.RootComponents.Add<App>("#app")`).
    * Los [servicios](xref:blazor/fundamentals/dependency-injection) se agregan y configuran (por ejemplo, `builder.Services.AddSingleton<IMyDependency, MyDependency>()`).

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* `Program.cs`: El punto de entrada de la aplicación que configura lo siguiente:

  * ASP.NET Core [hospedado](xref:fundamentals/host/generic-host) (Blazor Server)
  * Host de WebAssembly (Blazor WebAssembly): el código de este archivo es único en las aplicaciones creadas a partir de la plantilla de Blazor WebAssembly (`blazorwasm`).
    * El componente `App` es el componente raíz de la aplicación. El componente `App` se especifica como el elemento DOM `app` (`<app>Loading...</app>` en `wwwroot/index.html`) de la colección de componentes raíz (`builder.RootComponents.Add<App>("app")`).
    * Los [servicios](xref:blazor/fundamentals/dependency-injection) se agregan y configuran (por ejemplo, `builder.Services.AddSingleton<IMyDependency, MyDependency>()`).

::: moniker-end

* `Startup.cs` (Blazor Server): contiene la lógica de inicio de la aplicación. La clase `Startup` define dos métodos:

  * `ConfigureServices`: configura los servicios de [inserción de dependencias](xref:fundamentals/dependency-injection) de la aplicación. En las aplicaciones Blazor Server, los servicios se agregan llamando a <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor%2A>, y el elemento `WeatherForecastService` se agrega al contenedor de servicios para que lo use el componente `FetchData` de ejemplo.
  * `Configure`: configura la canalización de administración de solicitudes de la aplicación:
    * Se llama a <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> para configurar un punto de conexión para la conexión en tiempo real con el explorador. La conexión se crea con [SignalR](xref:signalr/introduction), que es un marco para agregar funcionalidad web a las aplicaciones en tiempo real.
    * Se llama a [`MapFallbackToPage("/_Host")`](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) para configurar la página raíz de la aplicación (`Pages/_Host.cshtml`) y habilitar la navegación.

::: moniker range=">= aspnetcore-5.0"

* `wwwroot/index.html` (Blazor WebAssembly): página raíz de la aplicación implementada como página HTML:
  * Cuando se solicita inicialmente cualquier página de la aplicación, esta página se representa y se devuelve en la respuesta.
  * La página especifica dónde se va a representar el componente `App` raíz. El componente se representa en la ubicación del elemento DOM `div` con un elemento `id` de `app` (`<div id="app">Loading...</div>`).
  * Se carga el archivo JavaScript `_framework/blazor.webassembly.js`, que hace lo siguiente:
    * Descarga el runtime de .NET, la aplicación y las dependencias de la aplicación.
    * Inicializa el runtime para ejecutar la aplicación.

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* `wwwroot/index.html` (Blazor WebAssembly): página raíz de la aplicación implementada como página HTML:
  * Cuando se solicita inicialmente cualquier página de la aplicación, esta página se representa y se devuelve en la respuesta.
  * La página especifica dónde se va a representar el componente `App` raíz. El componente se representa en la ubicación del elemento DOM `app` (`<app>Loading...</app>`).
  * Se carga el archivo JavaScript `_framework/blazor.webassembly.js`, que hace lo siguiente:
    * Descarga el runtime de .NET, la aplicación y las dependencias de la aplicación.
    * Inicializa el runtime para ejecutar la aplicación.

::: moniker-end

* `App.razor`: componente raíz de la aplicación que configura el enrutamiento del lado cliente mediante el componente <xref:Microsoft.AspNetCore.Components.Routing.Router>. El componente <xref:Microsoft.AspNetCore.Components.Routing.Router> intercepta la navegación del explorador y representa la página que coincide con la dirección solicitada.

* Carpeta `Pages`: contiene las páginas o componentes enrutables (`.razor`) que conforman la aplicación Blazor y la página raíz de Razor de una aplicación Blazor Server. La ruta de cada página se especifica usando la directiva [`@page`](xref:mvc/views/razor#page). En la plantilla se incluye lo siguiente:
  * `_Host.cshtml` (Blazor Server): página raíz de la aplicación implementada como página Razor:
    * Cuando se solicita inicialmente cualquier página de la aplicación, esta página se representa y se devuelve en la respuesta.
    * Se carga el archivo JavaScript `_framework/blazor.server.js`, que configura la conexión de SignalR en tiempo real entre el explorador y el servidor.
    * La página Host especifica dónde se va a representar el componente `App` raíz (`App.razor`).
  * Componente `Counter` (`Pages/Counter.razor`): implementa la página Counter.
  * Componente `Error` (`Error.razor`, solo en la aplicación Blazor Server): se representa cuando se produce una excepción no controlada en la aplicación.
  * Componente `FetchData` (`Pages/FetchData.razor`): implementa la página Fetch data (Recuperar datos).
  * Componente `Index` (`Pages/Index.razor`): implementa la página principal.
  
* `Properties/launchSettings.json`: Contiene la [configuración del entorno de desarrollo](xref:fundamentals/environments#development-and-launchsettingsjson).

::: moniker range=">= aspnetcore-5.0"

* Carpeta `Shared`: contiene otros componentes de interfaz de usuario (`.razor`) que la aplicación usa:
  * Componente `MainLayout` (`MainLayout.razor`): El [componente de diseño](xref:blazor/layouts) de la aplicación.
  * `MainLayout.razor.css`: hoja de estilos del diseño principal de la aplicación.
  * Componente `NavMenu` (`NavMenu.razor`): implementa la navegación de barra lateral. Incluye el [componente `NavLink`](xref:blazor/fundamentals/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), que representa los vínculos de navegación a otros componentes Razor. El componente <xref:Microsoft.AspNetCore.Components.Routing.NavLink> indica automáticamente un estado seleccionado cuando su componente se carga, lo que ayuda al usuario a saber qué componente se está mostrando actualmente.
  * `NavMenu.razor.css`: hoja de estilos del menú de navegación de la aplicación.

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* Carpeta `Shared`: contiene otros componentes de interfaz de usuario (`.razor`) que la aplicación usa:
  * Componente `MainLayout` (`MainLayout.razor`): El [componente de diseño](xref:blazor/layouts) de la aplicación.
  * Componente `NavMenu` (`NavMenu.razor`): implementa la navegación de barra lateral. Incluye el [componente `NavLink`](xref:blazor/fundamentals/routing#navlink-component) (<xref:Microsoft.AspNetCore.Components.Routing.NavLink>), que representa los vínculos de navegación a otros componentes Razor. El componente <xref:Microsoft.AspNetCore.Components.Routing.NavLink> indica automáticamente un estado seleccionado cuando su componente se carga, lo que ayuda al usuario a saber qué componente se está mostrando actualmente.
  
::: moniker-end

* `_Imports.razor`: Engloba las directivas de Razor comunes que se van a incluir en los componentes de la aplicación (`.razor`), como las directivas [`@using`](xref:mvc/views/razor#using) de los espacios de nombres.

* Carpeta `Data` (Blazor Server): contiene la clase `WeatherForecast` y la implementación de `WeatherForecastService`, que proporcionan datos meteorológicos de ejemplo al componente `FetchData` de la aplicación.

* `wwwroot`: carpeta [Web Root](xref:fundamentals/index#web-root) de la aplicación que contiene los recursos estáticos públicos de la aplicación.

* `appsettings.json`: Contiene las [opciones de configuración](xref:blazor/fundamentals/configuration) de la aplicación. En una aplicación Blazor WebAssembly, el archivo de configuración de la aplicación se encuentra en la carpeta `wwwroot`. En una aplicación Blazor Server, el archivo de configuración de la aplicación se encuentra en la raíz del proyecto.
