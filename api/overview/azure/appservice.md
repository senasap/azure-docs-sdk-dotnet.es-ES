---
title: Bibliotecas de Azure App Service para .NET
description: Referencia de las bibliotecas de Azure App Service para .NET
ms.date: 10/19/2017
ms.topic: reference
ms.service: app-service
ms.openlocfilehash: 82f8eccfafd2f7b1cf1df1ce0f40212509ccddd3
ms.sourcegitcommit: 5d9b713653b3d03e1d0a67f6e126ee399d1c2a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47189998"
---
# <a name="azure-app-service-libraries-for-net"></a>Bibliotecas de Azure App Service para .NET

## <a name="overview"></a>Información general

[Azure App Service](/azure/app-service/app-service-value-prop-what-is) le permite implementar y escalar sitios web, aplicaciones web, servicios y API de REST.

## <a name="management-api"></a>API de administración

Implemente, administre y escale elementos hospedados en Azure App Service con la API de administración.

Instale el [paquete NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.AppService.Fluent) directamente desde la [Consola del Administración de paquetes][PackageManager] de Visual Studio o con la [CLI de .NET Core][DotNetCLI].


#### <a name="visual-studio-package-manager"></a>Administrador de paquetes de Visual Studio

```powershell
Install-Package Microsoft.Azure.Management.AppService.Fluent
```

#### <a name="net-core-cli"></a>CLI de .NET Core

```bash
dotnet add package Microsoft.Azure.Management.AppService.Fluent
```

### <a name="code-example"></a>Ejemplo de código

Cree una aplicación web nueva.

```csharp
/* Include these "using" directives...
using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
using Microsoft.Azure.Management.AppService.Fluent;
*/

IWebApp app1 = azure.WebApps
    .Define("MyUniqueWebAddress")
    .WithRegion(Region.USWest)
    .WithNewResourceGroup("MyResourceGroup")
    .WithNewWindowsPlan(PricingTier.StandardS1)
    .Create();
```

> [!div class="nextstepaction"]
> [Explorar las API de administración](/dotnet/api/overview/azure/appservice/management)

### <a name="samples"></a>Ejemplos

* [Administración de aplicaciones web con el SDK de .NET de Azure](https://azure.microsoft.com/resources/samples/app-service-web-dotnet-manage/)
* [Ejemplo de ASP.NET para Azure App Service](https://azure.microsoft.com/resources/samples/app-service-web-dotnet-get-started/)

Consulte la [lista completa](https://azure.microsoft.com/resources/samples/?platform=dotnet&term=app%20service) de ejemplos de Azure App Service.

[PackageManager]: https://docs.microsoft.com/nuget/tools/package-manager-console
[DotNetCLI]: https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package