---
title: Protección de aplicaciones de ASP.NET Core Blazor Server
author: guardrex
description: Aprenda a proteger las aplicaciones de Blazor Server igual que las aplicaciones de ASP.NET Core.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/security/server/index
ms.openlocfilehash: 69a24fc955a0f2fb524ec817eb50372052f538a1
ms.sourcegitcommit: 66fca14611eba141d455fe0bd2c37803062e439c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2020
ms.locfileid: "85944261"
---
# <a name="secure-aspnet-core-blazor-server-apps"></a>Protección de aplicaciones de ASP.NET Core Blazor Server

Por [Luke Latham](https://github.com/guardrex)

La seguridad de las aplicaciones de Blazor Server se configura de la misma forma que la de las aplicaciones de ASP.NET Core. Para obtener más información, vea los artículos en <xref:security/index>. Los temas de esta información general se aplican específicamente a Blazor Server. 

## <a name="blazor-server-project-template"></a>Plantilla de proyecto de Blazor Server

Se puede configurar la autenticación de la plantilla de proyecto de Blazor Server cuando se crea el proyecto.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Siga las instrucciones de Visual Studio en <xref:blazor/tooling> para crear un proyecto de Blazor Server con un mecanismo de autenticación.

Después de elegir la plantilla de aplicación de **Blazor Server** en el cuadro de diálogo **Crear una aplicación web ASP.NET Core**, seleccione **Cambiar** en **Autenticación**.

Se abre un cuadro de diálogo para ofrecer el mismo conjunto de mecanismos de autenticación disponibles para otros proyectos ASP.NET Core:

* **Sin autenticación**
* **Cuentas de usuario individuales**: las cuentas de usuario se pueden almacenar:
  * Dentro de la aplicación mediante el sistema de [Identity](xref:security/authentication/identity) de ASP.NET Core.
  * Con [Azure AD B2C](xref:security/authentication/azure-ad-b2c)
* **Cuentas profesionales o educativas**
* **Autenticación de Windows**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Siga las instrucciones de Visual Studio Code en <xref:blazor/tooling> para crear un proyecto de Blazor Server con un mecanismo de autenticación:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).

| Mecanismo de autenticación | Descripción |
| ------------------------ | ----------- |
| `None` (valor predeterminado)         | Sin autenticación |
| `Individual`             | Usuarios almacenados en la aplicación con ASP.NET Core Identity |
| `IndividualB2C`          | Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c) |
| `SingleOrg`              | Autenticación organizativa para un solo inquilino |
| `MultiOrg`               | Autenticación organizativa para varios inquilinos |
| `Windows`                | Autenticación de Windows |

Con la opción `-o|--output`, el comando usa el valor proporcionado para el marcador de posición `{APP NAME}` para:

* Cree una carpeta para el proyecto.
* Asigne el nombre al proyecto.

Para más información, consulte el comando [`dotnet new`](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.

# <a name="visual-studio-for-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

1. Siga las instrucciones de Visual Studio para Mac en <xref:blazor/tooling>.

1. En el paso **Configure your new Blazor Server App** (Configurar la nueva aplicación de Blazor Server), seleccione **Individual Authentication (in-app)** (Autenticación individual [en aplicación]) en la lista desplegable **Authentication** (Autenticación).

1. La aplicación se crea para usuarios individuales almacenados en la aplicación con ASP.NET Core Identity.

# <a name="net-core-cli"></a>[CLI de .NET Core](#tab/netcore-cli/)

Cree un nuevo proyecto de Blazor Server con un mecanismo de autenticación que use el siguiente comando en un shell de comandos:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

En la tabla siguiente se muestran los valores de autenticación permitidos (`{AUTHENTICATION}`).

| Mecanismo de autenticación | Descripción |
| ------------------------ | ----------- |
| `None` (valor predeterminado)         | Sin autenticación |
| `Individual`             | Usuarios almacenados en la aplicación con ASP.NET Core Identity |
| `IndividualB2C`          | Usuarios almacenados en [Azure AD B2C](xref:security/authentication/azure-ad-b2c) |
| `SingleOrg`              | Autenticación organizativa para un solo inquilino |
| `MultiOrg`               | Autenticación organizativa para varios inquilinos |
| `Windows`                | Autenticación de Windows |

Con la opción `-o|--output`, el comando usa el valor proporcionado para el marcador de posición `{APP NAME}` para:

* Cree una carpeta para el proyecto.
* Asigne el nombre al proyecto.

Para más información, consulte el comando [`dotnet new`](/dotnet/core/tools/dotnet-new) de la guía de .NET Core.

---

## <a name="scaffold-identity"></a>Scaffolding para Identity

Scaffolding para Identity en un proyecto de Blazor Server:

* [Sin autorización existente](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization)
* [Con autorización](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization)