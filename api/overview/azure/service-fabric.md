---
title: Bibliotecas de Azure Service Fabric para .NET
description: Referencia de las bibliotecas de Azure Service Fabric para .NET
ms.date: 10/19/2017
ms.topic: reference
ms.service: service-fabric
ms.openlocfilehash: 064f95a4eae3182c4ac5b31779a5d22b592a75b2
ms.sourcegitcommit: 5d9b713653b3d03e1d0a67f6e126ee399d1c2a60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47190768"
---
# <a name="azure-service-fabric-libraries-for-net"></a>Bibliotecas de Azure Service Fabric para .NET

## <a name="overview"></a>Información general

Azure Service Fabric es una plataforma de sistemas distribuidos que facilita el empaquetado, la implementación y la administración de microservicios y contenedores escalables y confiables.  Para más información, consulte la [documentación de Azure Service Fabric](/azure/service-fabric/).

## <a name="client-library"></a>Biblioteca de cliente

Utilice la biblioteca de cliente de Service Fabric para interactuar con un clúster de Service Fabric existente.  La biblioteca contiene tres categorías de API:

* **Cliente** Las API se utilizan para administrar, escalar y reciclar el clúster, así como para implementar paquetes de aplicaciones.
* **En tiempo de ejecución** Las API se utilizan para que la aplicación en ejecución pueda interactuar con el clúster que la aloja.
* **Común** Las API contienen tipos usados tanto en API de **cliente** como **en tiempo de ejecución**.

Instale el [paquete NuGet](https://www.nuget.org/packages/Microsoft.ServiceFabric) directamente desde la [Consola del Administración de paquetes][PackageManager] de Visual Studio o con la [CLI de .NET Core][DotNetCLI].

#### <a name="visual-studio-package-manager"></a>Administrador de paquetes de Visual Studio

```powershell
Install-Package Microsoft.ServiceFabric
```

```bash
dotnet add package Microsoft.ServiceFabric
```

### <a name="code-examples"></a>Ejemplos de código

En el ejemplo siguiente se utilizan las API de **cliente** de Service Fabric para copiar un paquete de aplicación en el almacén de imágenes, aprovisionar el tipo de aplicación y crear una instancia de la aplicación.

```csharp
/* Include these dependencies
using System.Fabric;
using System.Fabric.Description;
*/

// Connect to the cluster.
FabricClient fabricClient = new FabricClient(clusterConnection);

// Copy the application package to a location in the image store
fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);

// Provision the application.
fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

//  Create the application instance.
ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
```

> [!div class="nextstepaction"]
> [Explorar las API de cliente](/dotnet/api/overview/azure/servicefabric/client)

Este ejemplo utiliza las API **en tiempo de ejecución** y **común** de Service Fabric desde una aplicación hospedada para actualizar una [Colección confiable](/azure/service-fabric/service-fabric-reliable-services-reliable-collections) en tiempo de ejecución.

```csharp
using System.Fabric;
using Microsoft.ServiceFabric.Data.Collections;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

/// <summary>
/// This is the main entry point for your service replica.
/// This method executes when this replica of your service becomes primary and has write status.
/// </summary>
/// <param name="cancellationToken">Canceled when Service Fabric needs to shut down this service replica.</param>
protected override async Task RunAsync(CancellationToken cancellationToken)
{
    var myDictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<string, long>>("myDictionary");
    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();
        using (var tx = this.StateManager.CreateTransaction())
        {
            var result = await myDictionary.TryGetValueAsync(tx, "Counter");
            await myDictionary.AddOrUpdateAsync(tx, "Counter", 0, (key, value) => ++value);

            // If an exception is thrown before calling CommitAsync, the transaction aborts, all changes are
            // discarded, and nothing is saved to the secondary replicas.
            await tx.CommitAsync();
        }
        await Task.Delay(TimeSpan.FromSeconds(1), cancellationToken);
    }
}
```

> [!div class="nextstepaction"]
> [Explorar la API en tiempo de ejecución](/dotnet/api/overview/azure/servicefabric/runtime)

> [!div class="nextstepaction"]
> [Explorar la API común](/dotnet/api/overview/azure/servicefabric/common)

## <a name="management-library"></a>Biblioteca de administración

La biblioteca de administración se utiliza para crear, actualizar y eliminar clústeres de Service Fabric.

Instale el [paquete NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.ServiceFabric) directamente desde la [Consola del Administración de paquetes][PackageManager] de Visual Studio o con la [CLI de .NET Core][DotNetCLI].

#### <a name="visual-studio-package-manager"></a>Administrador de paquetes de Visual Studio

```powershell
Install-Package Microsoft.Azure.Management.ServiceFabric
```

```bash
dotnet add package Microsoft.Azure.Management.ServiceFabric
```

> [!div class="nextstepaction"]
> [Explorar las API de administración](/dotnet/api/overview/azure/servicefabric/management)

## <a name="samples"></a>Ejemplos

* [Implementación y eliminación de aplicaciones mediante FabricClient](/azure/service-fabric/service-fabric-deploy-remove-applications-fabricclient)

[PackageManager]: https://docs.microsoft.com/nuget/tools/package-manager-console
[DotNetCLI]: https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package
