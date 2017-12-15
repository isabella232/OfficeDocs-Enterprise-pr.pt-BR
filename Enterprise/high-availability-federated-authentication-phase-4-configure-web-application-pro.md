---
title: "Alta disponibilidade federado proxies de aplicativo de web fase 4 configurar autenticação"
ms.author: josephd
author: JoeDavies-MSFT
manager: laurawi
ms.date: 12/15/2017
ms.audience: ITPro
ms.topic: article
ms.service: o365-solutions
localization_priority: Normal
ms.collection:
- Ent_O365
- Ent_O365_Hybrid
- Ent_O365_Hybrid_Top
ms.custom:
- DecEntMigration
- Ent_Solutions
ms.assetid: 1c903173-67cd-47da-86d9-d333972dda80
description: "Resumo: Configure os servidores de proxy de aplicativo web para sua autenticação federada de alta disponibilidade para o Office 365 in Microsoft Azure."
ms.openlocfilehash: 02aeac727815a82c15cd602094e945a14ed551af
ms.sourcegitcommit: d31cf57295e8f3d798ab971d405baf3bd3eb7a45
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/15/2017
---
# <a name="high-availability-federated-authentication-phase-4-configure-web-application-proxies"></a>Autenticação federada de alta disponibilidade Fase 4: Configurar proxies de aplicativos Web

 **Resumo:** Configure os servidores de proxy de aplicativo web para sua autenticação federada de alta disponibilidade para o Office 365 in Microsoft Azure.
  
Nessa fase de implantação da alta disponibilidade para a autenticação federada do Office 365 nos serviços de infraestrutura do Azure, você cria um balanceador de carga interno e dois servidores AD FS.
  
Você deve concluir esta fase antes de mover o logon em [alta disponibilidade federado autenticação fase 5: configurar a autenticação federada para o Office 365](high-availability-federated-authentication-phase-5-configure-federated-authentic.md). Consulte [autenticação federada de alta disponibilidade de implantação para o Office 365 no Windows Azure](deploy-high-availability-federated-authentication-for-office-365-in-azure.md) para todas as fases.
  
## <a name="create-the-internet-facing-load-balancer-in-azure"></a>Criar o balanceador de carga voltado para a Internet no Azure

Você deve criar um balanceador de carga voltado para a Internet para que o Azure distribua o tráfego de autenticação de cliente de entrada proveniente da Internet uniformemente entre os dois servidores proxy de aplicativos Web.
  
> [!NOTE]
> O comando a seguir define usar a versão mais recente do Azure PowerShell. Consulte a [Introdução ao cmdlets do PowerShell do Windows Azure](https://docs.microsoft.com/en-us/powershell/azureps-cmdlets-docs/). 
  
Quando tiver fornecido os valores de localização e grupo de recursos, execute o bloco resultante no prompt de comando do Azure PowerShell ou no ISE do PowerShell.
  
> [!TIP]
> Para um arquivo de texto que contém todos os comandos do PowerShell este artigo e uma pasta de trabalho de configuração Microsoft Excel que gera blocos de comando do PowerShell pronto para executar com base em suas configurações personalizadas, consulte o [autenticação federada para o Office 365 Kit de implantação do Azure](https://gallery.technet.microsoft.com/Federated-Authentication-8a9f1664). 
  
```
# Set up key variables
$locName="<your Azure location>"
$rgName="<Table R - Item 4 - Resource group name column>"

$publicIP=New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Name "WebProxyPublicIP" -Location $LocName -AllocationMethod "Static"
$frontendIP=New-AzureRmLoadBalancerFrontendIpConfig -Name "WebAppProxyServers-LBFE" -PublicIpAddress $publicIP
$beAddressPool=New-AzureRMLoadBalancerBackendAddressPoolConfig -Name "WebAppProxyServers-LBBE"
$healthProbe=New-AzureRMLoadBalancerProbeConfig -Name "WebServersProbe" -Protocol "TCP" -Port 443 -IntervalInSeconds 15 -ProbeCount 2
$lbrule=New-AzureRMLoadBalancerRuleConfig -Name "WebTraffic" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "TCP" -FrontendPort 443 -BackendPort 443
New-AzureRMLoadBalancer -ResourceGroupName $rgName -Name "WebAppProxyServers" -Location $locName -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe -FrontendIpConfiguration $frontendIP
```

Para exibir o endereço IP público atribuído ao seu balanceador de carga voltado para a Internet, execute estes comandos no prompt de comando do Azure PowerShell no computador local:
  
```
Write-Host (Get-AzureRMPublicIpaddress -Name "WebProxyPublicIP" -ResourceGroup $rgName).IPAddress
```

## <a name="determine-your-federation-service-fqdn-and-create-dns-records"></a>Determinar o FQDN do seu serviço de federação e criar registros DNS

Você precisa determinar o nome DNS para identificar o nome do seu serviço de federação na Internet. O Azure AD Connect configurará o Office 365 com este nome na Fase 5, e ele se tornará parte da URL enviada pelo Office 365 aos clientes que se conectam para obter um token de segurança. Um exemplo é fs.contoso.com (fs representa serviço de federação).
  
Depois que você tiver o FQDN do serviço de federação, crie para ele um registro A de domínio DNS público que seja resolvido para o endereço IP público do balanceador de carga voltado para a Internet do Azure.
  
|**Nome**|**Tipo**|**TTL**|**Valor**|
|:-----|:-----|:-----|:-----|
|FQDN do serviço de federação  <br/> |A  <br/> |3600  <br/> |endereço IP público do balanceador de carga voltados para a Internet do Windows Azure (exibido pelo comando **Write-Host** na seção anterior) <br/> |
   
Este é um exemplo:
  
|**Nome**|**Tipo**|**TTL**|**Valor**|
|:-----|:-----|:-----|:-----|
|FS.contoso.com  <br/> |A  <br/> |3600  <br/> |131.107.249.117  <br/> |
   
Em seguida, adicione um registro de endereço DNS ao namespace DNS particular da sua organização que resolva o FQDN do serviço de federação para o endereço IP privado atribuído ao balanceador de carga interno dos servidores do AD FS (Tabela I, item 4, coluna Valor).
  
## <a name="create-the-web-application-proxy-server-virtual-machines-in-azure"></a>Criar máquinas virtuais dos servidores proxy de aplicativos Web no Azure

Use o seguinte bloco de comandos do Azure PowerShell para criar as máquinas virtuais para os dois servidores proxy de aplicativos Web.  
  
Observe que o seguinte comando do Azure PowerShell define valores de uso das tabelas a seguir:
  
- Tabela M, para suas máquinas virtuais
    
- Tabela R, para seus grupos de recursos
    
- Tabela V, para suas configurações de rede virtual
    
- Tabela S, para suas sub-redes
    
- Tabela I, para seu endereço IP estático
    
- Tabela A, para seus conjuntos de disponibilidade
    
Lembre-se de que você definiu milhões de tabela em [alta disponibilidade federado autenticação fase 2: Configure os controladores de domínio](high-availability-federated-authentication-phase-2-configure-domain-controllers.md) e tabelas R, V, S, I e A na [alta disponibilidade federado autenticação fase 1: configurar o Azure](high-availability-federated-authentication-phase-1-configure-azure.md).
  
Quando tiver fornecido todos os valores apropriados, execute o bloco resultante no prompt de comando do Azure PowerShell ou no ISE do PowerShell.
  
```
# Set up variables common to both virtual machines
$locName="<your Azure location>"
$vnetName="<Table V - Item 1 - Value column>"
$subnetName="<Table R - Item 3 - Subnet name column>"
$avName="<Table A - Item 3 - Availability set name column>"
$rgNameTier="<Table R - Item 3 - Resource group name column>"
$rgNameInfra="<Table R - Item 4 - Resource group name column>"

$rgName=$rgNameInfra
$vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
$backendSubnet=Get-AzureRMVirtualNetworkSubnetConfig -Name $subnetName -VirtualNetwork $vnet
$webLB=Get-AzureRMLoadBalancer -ResourceGroupName $rgName -Name "WebAppProxyServers"

$rgName=$rgNameTier
$avSet=Get-AzureRMAvailabilitySet -Name $avName -ResourceGroupName $rgName

# Create the first web application proxy server virtual machine
$vmName="<Table M - Item 6 - Virtual machine name column>"
$vmSize="<Table M - Item 6 - Minimum size column>"
$staticIP="<Table I - Item 7 - Value column>"
$diskStorageType="<Table M - Item 6 - Storage type column>"

$nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -Subnet $backendSubnet -LoadBalancerBackendAddressPool $webLB.BackendAddressPools[0] -PrivateIpAddress $staticIP
$vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id

$cred=Get-Credential -Message "Type the name and password of the local administrator account for the first web application proxy server." 
$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version "latest"
$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
$vm=Set-AzureRmVMOSDisk -VM $vm -Name ($vmName +"-OS") -DiskSizeInGB 128 -CreateOption FromImage -StorageAccountType $diskStorageType
New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

# Create the second web application proxy virtual machine
$vmName="<Table M - Item 7 - Virtual machine name column>"
$vmSize="<Table M - Item 7 - Minimum size column>"
$staticIP="<Table I - Item 8 - Value column>"
$diskStorageType="<Table M - Item 7 - Storage type column>"

$nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName  -Subnet $backendSubnet -LoadBalancerBackendAddressPool $webLB.BackendAddressPools[0] -PrivateIpAddress $staticIP
$vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id

$cred=Get-Credential -Message "Type the name and password of the local administrator account for the second web application proxy server." 
$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version "latest"
$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
$vm=Set-AzureRmVMOSDisk -VM $vm -Name ($vmName +"-OS") -DiskSizeInGB 128 -CreateOption FromImage -StorageAccountType $diskStorageType
New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm
```

> [!NOTE]
> Como essas máquinas virtuais são para um aplicativo de intranet, não estão atribuídos a um endereço IP público ou um rótulo de nome de domínio DNS e expostos à Internet. No entanto, isso também significa que você não pode se conectar a eles do portal do Azure. A opção **Conectar** está disponível quando você exibe as propriedades da máquina virtual. Use o acessório de Conexão de área de trabalho remota ou outra ferramenta de área de trabalho remota para se conectar à máquina virtual usando seu privada endereço IP ou intranet DNS nome e as credenciais da conta de administrador local.
  
Veja a seguir a configuração resultante da conclusão bem-sucedida dessa fase, com nomes de computador de espaço reservado.
  
**Fase 4: O voltado à Internet carregar balanceador e servidores de proxy de aplicativo da web para a sua infraestrutura de autenticação federada de alta disponibilidade no Windows Azure**

![Fase 4 da infraestrutura de autenticação federada de alta disponibilidade para o Office 365 no Azure com os servidores Proxy de aplicativo Web](images/7e03183f-3b3b-4cbe-9028-89cc3f195a63.png)
  
## <a name="next-step"></a>Próxima etapa

Uso [alta disponibilidade federado autenticação fase 5: configurar a autenticação federada para o Office 365](high-availability-federated-authentication-phase-5-configure-federated-authentic.md) para continuar a configurar essa carga de trabalho.
  
## <a name="see-also"></a>See Also

[Implantar a autenticação federada de alta disponibilidade para o Office 365 no Windows Azure](deploy-high-availability-federated-authentication-for-office-365-in-azure.md)
  
[Identidade federada para seu ambiente de desenvolvimento e teste do Office 365](federated-identity-for-your-office-365-dev-test-environment.md)
  
[Adoção da nuvem e soluções híbridas](cloud-adoption-and-hybrid-solutions.md)

[Identidade federada para o Office 365](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

