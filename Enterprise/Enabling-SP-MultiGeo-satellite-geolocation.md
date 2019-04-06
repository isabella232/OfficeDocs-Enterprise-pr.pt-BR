---
title: Ativando as funcionalidades multigeográficas do SharePoint em sua localização geográfica por satélite
ms.author: mikeplum
author: MikePlumleyMSFT
manager: pamgreen
ms.audience: ITPro
ms.topic: article
ms.service: o365-solutions
ms.custom: ''
ms.collection: Strat_SP_gtc
localization_priority: Priority
description: Ativando as funcionalidades multigeográficas do SharePoint na sua localização geográfica por satélite.
ms.openlocfilehash: 98666f76a5b3ec055a6f26d30f502c3cc6b6d3bb
ms.sourcegitcommit: 0ddd9b0c9c23dc6479dce9f5701b69d533d76127
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/01/2019
ms.locfileid: "31025170"
---
# <a name="enabling-sharepoint-multi-geo-in-your-satellite-geo-location"></a><span data-ttu-id="67b64-103">Ativando as funcionalidades multigeográficas do SharePoint em sua localização geográfica por satélite</span><span class="sxs-lookup"><span data-stu-id="67b64-103">Enabling SharePoint Multi-Geo in your satellite geo location</span></span>

<span data-ttu-id="67b64-104">Este artigo destina-se a administradores Globais ou do SharePoint que tenham criado um local de satélite multigeográfico **antes** dos recursos multigeográficos do SharePoint terem se tornado disponíveis, em 27 de março de 2019, e que não tenham habilitado as funcionalidades multigeográficas do SharePoint em suas localizações geográficas por satélite.</span><span class="sxs-lookup"><span data-stu-id="67b64-104">This article is for Global or SharePoint administrators who have created a Multi-Geo satellite location **before** SharePoint Multi-Geo capabilities became generally available on March 27, 2019, and who have not enabled SharePoint Multi-Geo in their satellite geo location(s).</span></span> 

>[!Note]
><span data-ttu-id="67b64-105">Se você adicionou uma nova localização geográfica **depois de 27 de março**, não será necessário executar estas instruções, pois sua nova localização geográfica já estará habilitada para as funcionalidades geográficas do OneDrive e SharePoint.</span><span class="sxs-lookup"><span data-stu-id="67b64-105">If you have added a new geo location **after March 27th**, you do not need to perform these instructions, as your new geo location will already be enabled for OneDrive and SharePoint Multi-Geo.</span></span>

<span data-ttu-id="67b64-106">Estas instruções permitirão que você habilite o SharePoint em seu local de satélite, para que os seus usuários de satélite multigeográfico aproveitem as funcionalidades multigeográficas do OneDrive e SharePoint no O365.</span><span class="sxs-lookup"><span data-stu-id="67b64-106">These instructions will allow you to enable SharePoint in your satellite location, so your Multi-Geo satellite users can take advantage of both OneDrive and SharePoint Multi-Geo capabilities in O365.</span></span> 

>[!IMPORTANT]
><span data-ttu-id="67b64-107">Por favor, note que esta é uma habilitação unidirecional.</span><span class="sxs-lookup"><span data-stu-id="67b64-107">Please note that this is a one way enablement.</span></span> <span data-ttu-id="67b64-108">Depois de definir o modo SPO, você não poderá reverter seu locatário para somente o modo multigeográfico do OneDrive sem um escalonamento com suporte.</span><span class="sxs-lookup"><span data-stu-id="67b64-108">Once you set SPO mode, you will not be able to revert your tenant to OneDrive only Multi-Geo mode without an escalation with support.</span></span> 

## <a name="to-set-a-geo-location-into-spo-mode"></a><span data-ttu-id="67b64-109">Para definir uma localização geográfica no modo SPO</span><span class="sxs-lookup"><span data-stu-id="67b64-109">To set a geo location into SPO Mode</span></span>

<span data-ttu-id="67b64-110">Para definir uma localização geográfica no modo SPO, conecte-se à localização geográfica que você deseja definir no modo SPO:</span><span class="sxs-lookup"><span data-stu-id="67b64-110">To set a geo location into SPO mode, connect to the geo location you want to set in SPO Mode:</span></span>

1.  <span data-ttu-id="67b64-111">Abra seu Shell de Gerenciamento do SharePoint Online</span><span class="sxs-lookup"><span data-stu-id="67b64-111">Open your SharePoint Online Management Shell</span></span> 
2.  <span data-ttu-id="67b64-112">Connect-SPOService -URL "https://$tenantGeo-admin.sharepoint.com" -Credential $credential</span><span class="sxs-lookup"><span data-stu-id="67b64-112">Connect-SPOService -URL "https://$tenantGeo-admin.sharepoint.com" -Credential $credential</span></span>
3.  <span data-ttu-id="67b64-113">Set-SPOMultiGeoExperience</span><span class="sxs-lookup"><span data-stu-id="67b64-113">Set-SPOMultiGeoExperience</span></span></br></br>
<span data-ttu-id="67b64-114">![Set-SPOMultiGeoExperience](media/Set-SPO-MultiGeo.jpg)</span><span class="sxs-lookup"><span data-stu-id="67b64-114">![Set-SPOMultiGeoExperience](media/Set-SPO-MultiGeo.jpg)</span></span>
4.  <span data-ttu-id="67b64-115">Essa operação normalmente leva cerca de uma hora enquanto executamos vários retornos de publicação no serviço e recarimbamos seu locatário.</span><span class="sxs-lookup"><span data-stu-id="67b64-115">This operation usually takes about an hour while we perform various publish backs in the service and re-stamp your tenant.</span></span> <span data-ttu-id="67b64-116">Após pelo menos 1 hora, execute um Get-SPOMultiGeoExperience.</span><span class="sxs-lookup"><span data-stu-id="67b64-116">After at least 1 hour, please perform a Get-SPOMultiGeoExperience.</span></span>  <span data-ttu-id="67b64-117">Isso mostrará se esta localização geográfica está no modo SPO.</span><span class="sxs-lookup"><span data-stu-id="67b64-117">This will show you whether this geo location is in SPO mode.</span></span></br></br>
<span data-ttu-id="67b64-118">![Set-SPOMultiGeoExperience](media/Get-SPO-MultiGeo.jpg)</span><span class="sxs-lookup"><span data-stu-id="67b64-118">![Set-SPOMultiGeoExperience](media/Get-SPO-MultiGeo.jpg)</span></span>

 
 
 
>[!Note]
><span data-ttu-id="67b64-119">Certos caches no serviço são atualizados a cada 24 horas, portanto, é possível que, por um período de até 24 horas, o seu satélite geográfico possa se comportar intermitentemente como se ainda estivesse no modo ODB.</span><span class="sxs-lookup"><span data-stu-id="67b64-119">Certain caches in the service update every 24 hours, so it is possible that for a period of up to 24 hours, your satellite geo may intermittently behave as if it was still in ODB mode.</span></span> <span data-ttu-id="67b64-120">Isso não gera problemas técnicos.</span><span class="sxs-lookup"><span data-stu-id="67b64-120">This does not cause any technical issues.</span></span> 
 
<span data-ttu-id="67b64-121">Para obter informações adicionais sobre as funcionalidades multigeográficas do SharePoint, consulte [aka.ms/sharepointmultigeo](https://docs.microsoft.com/pt-BR/office365/enterprise/multi-geo-capabilities-in-onedrive-and-sharepoint-online-in-office-365)</span><span class="sxs-lookup"><span data-stu-id="67b64-121">For additional information regarding SharePoint Multi-Geo, please refer to [aka.ms/sharepointmultigeo](https://docs.microsoft.com/pt-BR/office365/enterprise/multi-geo-capabilities-in-onedrive-and-sharepoint-online-in-office-365)</span></span>

