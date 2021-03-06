---
title: "Azure Service Fabric-behållaren säkerhet | Microsoft Docs"
description: "Lär dig att skydda behållartjänster."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 3e41e293cc5340c0e32cf2cc6ef7ab7534330884
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="container-security"></a>Behållaren säkerhet

Service Fabric är en mekanism för tjänster i en behållare för att komma åt ett certifikat som är installerad på noder i ett Windows- eller Linux-kluster (version 5.7 eller högre). Service Fabric stöder dessutom också gMSA (grupphanterade tjänstkonton) för Windows-behållare. 

## <a name="certificate-management-for-containers"></a>Certifikathantering för behållare

Du kan skydda dina behållartjänster genom att ange ett certifikat. Certifikatet måste installeras i LocalMachine på alla noder i klustret. Certifikatinformationen som har angetts i applikationsmanifestet under den `ContainerHostPolicies` tagg som i följande fragment visas:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

För windows-kluster, när programmet startas, körningsmiljön läser certifikat och genererar en PFX-filen och lösenordet för varje certifikat. Det här PFX-filen och lösenordet är tillgängliga i behållaren med hjälp av följande miljövariabler: 

* **Certificate_ServicePackageName_CodePackageName_CertName_PFX**
* **Certificate_ServicePackageName_CodePackageName_CertName_Password**

För Linux-kluster kopieras bara certificates(PEM) över från arkivet som anges av X509StoreName till behållaren. Motsvarande miljövariabler i linux är:

* **Certificate_ServicePackageName_CodePackageName_CertName_PEM**
* **Certificate_ServicePackageName_CodePackageName_CertName_PrivateKey**

Om du redan har certifikat i formuläret och vill bara åtkomst till den i behållaren du också skapa ett datapaket i app-paket och ange följande i programmanifestet:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
   <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Behållartjänsten eller process ansvarar för importerar certifikatfilerna i behållaren. Du kan använda för att importera certifikatet `setupentrypoint.sh` skript eller köra anpassad kod i behållaren-processen. Följande exempelkod i C# för att importera PFX-filen:

```c#
    string certificateFilePath = Environment.GetEnvironmentVariable("Certificate_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
    string passwordFilePath = Environment.GetEnvironmentVariable("Certificate_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
    X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
    string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
    password = password.Replace("\0", string.Empty);
    X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
    store.Open(OpenFlags.ReadWrite);
    store.Add(cert);
    store.Close();
```
Den här PFX-certifikat kan användas för att autentisera program eller tjänst eller säker kommunikation med andra tjänster. Som standard är filerna ACLed endast till systemet. Du kan ACL det till andra konton som krävs av tjänsten.


## <a name="set-up-gmsa-for-windows-containers"></a>Ställ in gMSA för Windows-behållare

Att ställa in gMSA (grupp hanterade tjänstkonton), en fil för specifikation av autentiseringsuppgifter (`credspec`) är placerad på alla noder i klustret. Filen kan kopieras på alla noder som använder ett tillägg för virtuell dator.  Den `credspec` filen måste innehålla gMSA-kontoinformation. Mer information om den `credspec` fil, se [tjänstkonton](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). Specifikationen autentiseringsuppgifter och `Hostname` taggen har angetts i applikationsmanifestet. Den `Hostname` taggen måste matcha namnet på gMSA-kontot som behållaren körs under.  Den `Hostname` tillåter behållare för att autentisera sig till andra tjänster i domänen via Kerberos-autentisering.  Ett exempel för att ange den `Hostname` och `credspec` manifestet i programmet visas i följande utdrag:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Nästa steg

* [Distribuera en Windows-behållare till Service Fabric på Windows Server 2016](service-fabric-get-started-containers.md)
* [Distribuera en dockerbehållare till Service Fabric på Linux](service-fabric-get-started-containers-linux.md)
