---
title: Conectividade de rede para Office 365
ms.author: kvice
author: kelleyvice-msft
manager: laurawi
ms.date: 4/2/2018
ms.audience: ITPro
ms.topic: conceptual
ms.service: o365-administration
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: Adm_O365
search.appverid:
- MET150
- MOE150
- BCS160
ms.assetid: 64b420ef-0218-48f6-8a34-74bb27633b10
description: O Office 365 foi projetado para permitir que os clientes em todo o mundo para se conectar ao serviço usando uma conexão de internet. Como o serviço se desenvolve, a segurança, desempenho e confiabilidade do Office 365 foram aprimorados com base em clientes que usam a internet para estabelecer uma conexão ao serviço.
ms.openlocfilehash: b72b0a4584542e4c8673c7cf009c66aa97c6b81c
ms.sourcegitcommit: 69d60723e611f3c973a6d6779722aa9da77f647f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2018
ms.locfileid: "22539380"
---
# <a name="network-connectivity-to-office-365"></a>Conectividade de rede para Office 365

O Office 365 foi projetado para permitir que os clientes em todo o mundo para se conectar ao serviço usando uma conexão de internet. Como o serviço se desenvolve, a segurança, desempenho e confiabilidade do Office 365 foram aprimorados com base em clientes que usam a internet para estabelecer uma conexão ao serviço.
  
Clientes que planejam usar o Office 365 devem avaliar suas necessidades de conectividade de internet existente e previstas como parte do projeto de implantação. Para implantações de classe empresarial confiável e tamanho apropriado de conectividade de internet é uma parte importante de consumo de recursos do Office 365 e cenários.
  
Avaliações de rede podem ser realizadas por muitas pessoas diferentes e organizações, dependendo do tamanho e preferências. O escopo de rede da avaliação também pode variar dependendo de onde você está no seu processo de implantação. Para ajudá-lo a entender melhor o que é necessário para realizar uma avaliação de rede, podemos ter produzido um guia de avaliação de rede para ajudá-lo a compreender as opções disponíveis para você. Essa avaliação determinará o que precisam etapas e os recursos a ser adicionado ao projeto de implantação para habilitá-lo adotar com êxito o Office 365.
  
Uma avaliação abrangente de rede, como esses scritpt indicado do [Skype Operations Framework](https://www.skypeoperationsframework.com/) fornecerá soluções possíveis para a rede desafios de design junto com os detalhes da implementação. A maioria das avaliações de rede indicará a conectividade de rede para o Office 365 pode ser acomodada com [configuração secundária ou alterações de design](https://aka.ms/manageo365endpoints) para a infra-estrutura de saída de internet e de rede existente.

Algumas avaliações indicará a conectividade de rede para o Office 365 exigirá investimentos adicionais em componentes de rede. Por exemplo, os investimentos em infraestrutura de roteamento otimizada para dar suporte a conectividade com a internet para o Office 365 ou largura de banda WAN. Ocasionalmente, uma avaliação indicará a conectividade de rede para o Office 365 é influenciada por requisitos de desempenho ou de regulamentação para cenários como [Skype para qualidade de mídia Business Online](https://support.office.com/article/Media-Quality-and-Network-Connectivity-Performance-in-Skype-for-Business-Online-5fe3e01b-34cf-44e0-b897-b0b2a83f0917). Esses requisitos adicionais podem levar à investimentos em infraestrutura de conectividade de internet, otimização de roteamento e especializada conectividade direta.
  
> [!NOTE]
> Microsoft alterado como o domínio de roteamento do Microsoft Peering seja analisado para ExpressRoute do Windows Azure. Iniciando 31 de julho de 2017, todos os clientes do Windows Azure ExpressRoute podem habilitar Microsoft Peering diretamente no console administrativo do Windows Azure ou por meio do PowerShell. Após a habilitação do Microsoft Peering, qualquer cliente pode criar filtros de roteamento para receber os anúncios de rota BGP para aplicativos do contrato de cliente do Dynamics 365 (anteriormente conhecidos como CRM Online). Clientes que precisam de ExpressRoute do Windows Azure para o Office 365 devem obter a revisão da Microsoft antes que possam criar filtros de roteamento para o Office 365. Entre em contato com sua equipe de Account da Microsoft para saber mais sobre como solicitar uma revisão para habilitar ExpressRoute do Office 365. As assinaturas não autorizadas tentar criar filtros de roteamento para o Office 365 receberá uma [mensagem de erro](https://support.microsoft.com/kb/3181709).
  
Principais pontos a serem considerados ao planejar sua avaliação de rede para o Office 365:
  
- O Office 365 é um serviço de segura, confiável e de alto desempenho que executa na Internet pública. Podemos continuar investir para aprimorar esses aspectos do serviço. Todos os serviços do Office 365 estão disponíveis via conectividade à internet.

- Podemos continuamente estiver otimizando os principais aspectos do Office 365, como disponibilidade, global chegar e desempenho para internet baseado conectividade. Por exemplo, muitos serviços do Office 365 aproveitam um conjunto crescente de internet voltada para nós da borda. Esta rede borda oferece o melhor desempenho conexões chegando pela internet e a proximidade.

- Ao considerar usando o Office 365 para qualquer um dos serviços incluídos como Skype para Business Online recursos em voz, vídeos ou reunião, os clientes devem concluir uma avaliação de rede de ponta a ponta e atender aos requisitos de nossos Skype Operations Framework. Esses clientes terá várias opções para atender a requisitos específicos para conectividade de nuvem, incluindo ExpressRoute.

Dê uma olhada no estudo de caso da Microsoft [Otimizando o desempenho de rede para o Microsoft Office 365](https://msdn.microsoft.com/en-us/library/mt450488.aspx). Se você está avaliando o Office 365 e não verifique se onde começam com a avaliação da rede ou determinamos o design da rede desafios que precisar de ajuda para superar, por favor, trabalho com a sua equipe de conta da Microsoft.
  
Além disso, leia os [Princípios de conectividade de rede do Office 365](https://aka.ms/o365networkingprinciples) para entender os princípios de conectividade para gerenciar o tráfego de Office 365 de forma segura e obter o melhor desempenho possível. Este artigo ajudará você a entender as orientações mais recentes para otimizar com segurança de conectividade de rede do Office 365.
  
Aqui está um link curto que você pode usar para voltar: [ https://aka.ms/o365networkconnectivity.](https://aka.ms/o365networkconnectivity)
  
## <a name="see-also"></a>Confira também

[Gerenciando pontos de extremidade do Office 365](https://support.office.com/article/99cab9d4-ef59-4207-9f2b-3728eb46bf9a)
  
[Pontos de extremidade perguntas Frequentes do Office 365](https://support.office.com/article/d4088321-1c89-4b96-9c99-54c75cae2e6d)
  
[Ajuste de desempenho e de rede do Office 365](network-planning-and-performance.md)
  
[Microsoft Azure ExpressRoute para Office 365](azure-expressroute.md)
  
[Como rotear com o ExpressRoute para Office 365](routing-with-expressroute.md)
  
[Planejamento de rede com o ExpressRoute para Office 365](network-planning-with-expressroute.md)
  
[Usando o comunidades BGP em ExpressRoute para cenários do Office 365 (preview)](bgp-communities-in-expressroute.md)
  
[Qualidade de mídia e o desempenho de conectividade de rede no Skype para negócios on-line](https://support.office.com/article/5fe3e01b-34cf-44e0-b897-b0b2a83f0917)
  
[URLs e intervalos de endereços IP do Office 365](https://support.office.com/article/8548a211-3fe7-47cb-abb1-355ea5aa88a2)
  
[Princípios de conectividade de rede do Office 365](https://aka.ms/o365networkingprinciples)