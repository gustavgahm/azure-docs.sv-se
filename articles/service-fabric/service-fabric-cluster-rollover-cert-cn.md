---
title: Förnya ett certifikat för Azure Service Fabric-kluster | Microsoft Docs
description: Lär dig hur att förnya ett certifikat för Service Fabric-kluster identifieras av certifikatets unika namn.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: aljo
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/24/2018
ms.author: ryanwi
ms.openlocfilehash: 72640a4d917ddb2485199f0df1fead8b0bdcd1c9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55192981"
---
# <a name="manually-roll-over-a-service-fabric-cluster-certificate"></a>Manuellt förnya ett certifikat för Service Fabric-kluster
När ett certifikat för Service Fabric-klustret är att löpa ut kan behöva du uppdatera certifikatet.  Förnya certifikatet är enkel om klustret var [ställts in använder certifikat baserat på nätverksnamn](service-fabric-cluster-change-cert-thumbprint-to-cn.md) (i stället för tumavtryck).  Få ett nytt certifikat från en certifikatutfärdare med ett nytt utgångsdatum.  Självsignerade certifikat är inte stöd för produktion Service Fabric-kluster för att inkludera certifikat som genereras under Azure portal kluster skapa arbetsflöde. Det nya certifikatet måste ha samma allmänna namn som det äldre certifikatet. 

Service Fabric-klustret ska använda deklarerade certifikatet automatiskt med ytterligare i framtida utgångsdatum; När fler än en verifiera installeras certifikatet på värden. Ett bra tips är att använda en Resource Manager-mall för att etablera Azure-resurser. För icke-produktionsmiljö skriptet nedan kan användas för att ladda upp ett nytt certifikat till key vault och installerar certifikatet på virtuella datorns skalningsuppsättning: 

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  <subscription ID>

# Sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "keyvaultgroup"
$VaultName = "cntestvault2"
$certFilename = "C:\users\sfuser\sftutorialcluster20180419110824.pfx"
$certname = "cntestcert"
$Password  = "!P@ssw0rd321"
$VmssResourceGroupName     = "sfclustertutorialgroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzureRmResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Get the key vault.  The key vault must be enabled for deployment.
$keyVault = Get-AzureRmKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName 
$resourceId = $keyVault.ResourceId  

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certName  -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzureRmVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzureRmVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzureRmVmss -ResourceGroupName $VmssResourceGroupName -Name $VmssName -VirtualMachineScaleSet $vmss  -Verbose
```

>[!NOTE]
> Beräknar VM Scale ange hemligheter inte stöder samma resurs-id för två separata hemligheter, eftersom varje hemlighet är en version unik resurs. 

Om du vill veta mer kan du läsa följande:
* Lär dig mer om [kluster security](service-fabric-cluster-security.md).
* [Uppdatera och hantera klustercertifikat](service-fabric-cluster-security-update-certs-azure.md)

