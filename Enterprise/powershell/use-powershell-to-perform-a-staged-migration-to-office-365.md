---
title: "Usar o PowerShell para realizar uma migração em estágios para o Office 365"
ms.author: sirkkuw
author: sirkkuw
manager: scotv
ms.date: 12/15/2017
ms.audience: Admin
ms.topic: article
ms.service: o365-administration
localization_priority: Normal
ms.collection: Ent_O365
ms.custom: 
ms.assetid: a20f9dbd-6102-4ffa-b72c-ff813e700930
description: "Resumo: Saiba como usar o Windows PowerShell para executar uma migração em estágios para o Office 365."
ms.openlocfilehash: 5143b039937389d965386de0e09f4f59db071c86
ms.sourcegitcommit: 9f1fe023f7e2924477d6e9003fdc805e3cb6e2be
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="use-powershell-to-perform-a-staged-migration-to-office-365"></a><span data-ttu-id="ee651-103">Usar o PowerShell para realizar uma migração em estágios para o Office 365</span><span class="sxs-lookup"><span data-stu-id="ee651-103">Use PowerShell to perform a staged migration to Office 365</span></span>

 <span data-ttu-id="ee651-104">**Resumo:** saiba como usar o Windows PowerShell para executar uma migração em estágios para o Office 365.</span><span class="sxs-lookup"><span data-stu-id="ee651-104">**Summary:** Learn how to use Windows PowerShell to perform a staged migration to Office 365.</span></span>
  
<span data-ttu-id="ee651-105">Você pode migrar o conteúdo de caixas de correio de usuários de um sistema de email de origem para o Office 365 ao longo do tempo usando uma migração em estágios.</span><span class="sxs-lookup"><span data-stu-id="ee651-105">You can migrate the contents of user mailboxes from a source email system to Office 365 over time using a staged migration.</span></span>
  
<span data-ttu-id="ee651-p101">Este artigo o orienta ao longo das tarefas envolvidas para uma migração em estágios de email usando o PowerShell do Exchange Online. O tópico [O que você precisa saber sobre uma migração de email em estágios para o Office 365](https://go.microsoft.com/fwlink/p/?LinkId=536487) fornece uma visão geral do processo de migração. Quando você estiver familiarizado com o conteúdo daquele artigo, use este para começar a migrar caixas de correio de um sistema de email para outro.</span><span class="sxs-lookup"><span data-stu-id="ee651-p101">This article walks you through the tasks involved with for a staged email migration using Exchange Online PowerShell. The topic, [What you need to know about a staged email migration to Office 365](https://go.microsoft.com/fwlink/p/?LinkId=536487), gives you an overview of the migration process. When you're comfortable with the contents of that article, use this one to begin migrating mailboxes from one email system to another.</span></span>
  
> [!NOTE]
> <span data-ttu-id="ee651-p102">Você também pode usar o Centro de administração do Exchange para executar a migração em estágios. Confira [Realizar uma migração em estágios de email para o Office 365](https://go.microsoft.com/fwlink/p/?LinkId=536687).</span><span class="sxs-lookup"><span data-stu-id="ee651-p102">You can also use the Exchange admin center to perform staged migration. See [Perform a staged migration of email to Office 365](https://go.microsoft.com/fwlink/p/?LinkId=536687).</span></span> 
  
## <a name="what-do-you-need-to-know-before-you-begin"></a><span data-ttu-id="ee651-111">O que você precisa saber antes de começar?</span><span class="sxs-lookup"><span data-stu-id="ee651-111">What do you need to know before you begin?</span></span>

<span data-ttu-id="ee651-p103">Tempo estimado para a conclusão da tarefa: 2 a 5 minutos para criar um lote de migração. Depois que o lote de migração é iniciado, a duração da migração irá variar com base no número de caixas de correio no lote, no tamanho de cada caixa de correio e na sua capacidade de rede disponível. Para saber mais sobre outros fatores que afetam o tempo de migração de caixas de correio para o Office 365, confira [Desempenho de migração](https://go.microsoft.com/fwlink/p/?LinkId=275079).</span><span class="sxs-lookup"><span data-stu-id="ee651-p103">Estimated time to complete this task: 2-5 minutes to create a migration batch. After the migration batch is started, the duration of the migration will vary based on the number of mailboxes in the batch, the size of each mailbox, and your available network capacity. For information about other factors that affect how long it takes to migrate mailboxes to Office 365, see [Migration Performance](https://go.microsoft.com/fwlink/p/?LinkId=275079).</span></span>
  
<span data-ttu-id="ee651-p104">Para executar esses procedimentos, você precisa receber permissões. Para ver quais são as permissões necessárias, confira a entrada "Migração" no tópico [Permissões de destinatários](https://go.microsoft.com/fwlink/p/?LinkId=534105).</span><span class="sxs-lookup"><span data-stu-id="ee651-p104">You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Migration" entry in the [Recipients Permissions](https://go.microsoft.com/fwlink/p/?LinkId=534105) topic.</span></span>
  
<span data-ttu-id="ee651-p105">Para usar os cmdlets do PowerShell do Exchange Online, você precisa entrar e importar os cmdlets para sua sessão local do Windows PowerShell. Confira [Conectar-se ao Exchange Online usando o PowerShell remoto](https://go.microsoft.com/fwlink/p/?LinkId=534121) para obter instruções.</span><span class="sxs-lookup"><span data-stu-id="ee651-p105">To use the Exchange Online PowerShell cmdlets, you need to sign in and import the cmdlets into your local Windows PowerShell session. See [Connect to Exchange Online using remote PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=534121) for instructions.</span></span>
  
<span data-ttu-id="ee651-119">Para obter uma lista completa dos comandos de migração, confira [Cmdlets de movimentação e migração](https://go.microsoft.com/fwlink/p/?LinkId=534750).</span><span class="sxs-lookup"><span data-stu-id="ee651-119">For a full list of migration commands, see [Move and migration cmdlets](https://go.microsoft.com/fwlink/p/?LinkId=534750).</span></span>
  
## <a name="migration-steps"></a><span data-ttu-id="ee651-120">Etapas da migração</span><span class="sxs-lookup"><span data-stu-id="ee651-120">Migration steps</span></span>

### <a name="step-1-prepare-for-a-staged-migration"></a><span data-ttu-id="ee651-121">Etapa 1: preparar para uma migração em estágios</span><span class="sxs-lookup"><span data-stu-id="ee651-121">Step 1: Prepare for a staged migration</span></span>

<span data-ttu-id="ee651-122">Antes de migrar as caixas de correio para o Office 365 usando uma migração em estágios, há algumas alterações que você deve fazer em seu ambiente do Exchange.</span><span class="sxs-lookup"><span data-stu-id="ee651-122">Before you migrate mailboxes to Office 365 by using a staged migration, there are a few changes you must make to your Exchange environment.</span></span>
  
 <span data-ttu-id="ee651-p106">**Configurar o Outlook em Qualquer Lugar em seu Exchange Server** local O serviço de migração de email usa o Outlook em Qualquer Lugar (também conhecido como RPC sobre HTTP) para se conectar a seu Exchange Server local. Para saber mais sobre como configurar o Outlook em Qualquer Lugar para o Exchange Server 2007 e para o Exchange 2003, confira o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ee651-p106">**Configure Outlook Anywhere on your on-premises Exchange Server** The email migration service uses Outlook Anywhere (also known as RPC over HTTP), to connect to your on-premises Exchange Server. For information about how to set up Outlook Anywhere for Exchange Server 2007, and Exchange 2003, see the following:</span></span>
  
- [<span data-ttu-id="ee651-125">Exchange 2007: Como Habilitar o Outlook em Qualquer Lugar</span><span class="sxs-lookup"><span data-stu-id="ee651-125">Exchange 2007: How to Enable Outlook Anywhere</span></span>](https://go.microsoft.com/fwlink/?LinkID=167210)
    
- [<span data-ttu-id="ee651-126">Como configurar o Outlook em Qualquer Lugar com o Exchange 2003</span><span class="sxs-lookup"><span data-stu-id="ee651-126">How to configure Outlook Anywhere with Exchange 2003</span></span>](https://go.microsoft.com/fwlink/?LinkID=167209)
    
> [!IMPORTANT]
> <span data-ttu-id="ee651-p107">Você deve usar um certificado emitido por uma AC (autoridade de certificação) confiável com sua configuração do Outlook em Qualquer Lugar. O Outlook em Qualquer Lugar não pode ser configurado com um certificado autoassinado. Para saber mais, confira [Como configurar o SSL para o Outlook em Qualquer Lugar](https://go.microsoft.com/fwlink/?LinkID=80875)</span><span class="sxs-lookup"><span data-stu-id="ee651-p107">You must use a certificate issued by a trusted certification authority (CA) with your Outlook Anywhere configuration. Outlook Anywhere can't be configured with a self-signed certificate. For more information, see [How to configure SSL for Outlook Anywhere](https://go.microsoft.com/fwlink/?LinkID=80875).</span></span> 
  
 <span data-ttu-id="ee651-130">**Opcional: Verifique se você pode se conectar à sua organização do Exchange usando o Outlook em Qualquer Lugar** Tente um dos métodos a seguir para testar as configurações de conexão.</span><span class="sxs-lookup"><span data-stu-id="ee651-130">**Optional: Verify that you can connect to your Exchange organization using Outlook Anywhere** Try one of the following methods to test your connection settings.</span></span>
  
- <span data-ttu-id="ee651-131">Use o Outlook fora de sua rede corporativa para se conectar a sua caixa de correio local do Exchange.</span><span class="sxs-lookup"><span data-stu-id="ee651-131">Use Outlook from outside your corporate network to connect to your on-premises Exchange mailbox.</span></span>
    
- <span data-ttu-id="ee651-p108">Use o [Analisador de Conectividade Remota do Microsoft Exchange](https://www.testexchangeconnectivity.com/) para testar as configurações de conexão. Use o Outlook em Qualquer Lugar (RPC sobre HTTP) ou os testes de descoberta automática do Outlook.</span><span class="sxs-lookup"><span data-stu-id="ee651-p108">Use the [Microsoft Exchange Remote Connectivity Analyzer](https://www.testexchangeconnectivity.com/) to test your connection settings. Use the Outlook Anywhere (RPC over HTTP) or Outlook Autodiscover tests.</span></span>
    
- <span data-ttu-id="ee651-134">Execute os seguintes comandos no PowerShell do Exchange Online:</span><span class="sxs-lookup"><span data-stu-id="ee651-134">Run the following commands in Exchange Online PowerShell:</span></span>
    
  ```
  $Credentials = Get-Credential
  ```

  ```
  Test-MigrationServerAvailability -ExchangeOutlookAnywhere -Autodiscover -EmailAddress <email address for on-premises administrator> -Credentials $credentials
  ```

 <span data-ttu-id="ee651-p109">**Definir permissões** A conta de usuário local que você usa para se conectar à sua organização local do Exchange (também chamada de administrador de migração) deve ter as permissões necessárias para acessar e modificar as caixas de correio locais que você deseja migrar para o Office 365. Essa conta de usuário é usada quando você se conecta a seu sistema de email criando um ponto de extremidade de migração, mais adiante neste procedimento ([Etapa 3: criar um ponto de extremidade de migração](use-powershell-to-perform-a-staged-migration-to-office-365.md#BK_Endpoint)).</span><span class="sxs-lookup"><span data-stu-id="ee651-p109">**Set permissions** The on-premises user account that you use to connect to your on-premises Exchange organization (also called the migration administrator) must have the necessary permissions to access the on-premises mailboxes that you want to migrate to Office 365. This user account is used when you connect to your email system by creating a migration endpoint later in this procedure ([Step 3: Create a migration endpoint](use-powershell-to-perform-a-staged-migration-to-office-365.md#BK_Endpoint) ).</span></span>
  
<span data-ttu-id="ee651-137">Para migrar as caixas de correio, o administrador deve ter um dos seguintes conjuntos de permissão:</span><span class="sxs-lookup"><span data-stu-id="ee651-137">To migrate the mailboxes, the admin must have one of the following permission sets:</span></span>
  
- <span data-ttu-id="ee651-138">Ser membro do grupo **Administradores de Domínio** no Active Directory na organização local.</span><span class="sxs-lookup"><span data-stu-id="ee651-138">Be a member of the **Domain Admins** group in Active Directory in the on-premises organization.</span></span>
    
    <span data-ttu-id="ee651-139">ou</span><span class="sxs-lookup"><span data-stu-id="ee651-139">or</span></span>
    
- <span data-ttu-id="ee651-140">Ter a permissão **FullAccess** para cada caixa de correio local e a permissão **WriteProperty** para modificar a propriedade **TargetAddress** nas contas de usuário locais.</span><span class="sxs-lookup"><span data-stu-id="ee651-140">Be assigned the **FullAccess** permission for each on-premises mailbox and the **WriteProperty** permission to modify the **TargetAddress** property on the on-premises user accounts.</span></span>
    
    <span data-ttu-id="ee651-141">ou</span><span class="sxs-lookup"><span data-stu-id="ee651-141">or</span></span>
    
- <span data-ttu-id="ee651-142">Ter a permissão **Receive As** no banco de dados de caixa de correio local que armazena as caixas de correio do usuário e a permissão **WriteProperty** para modificar a propriedade **TargetAddress** nas contas de usuário locais.</span><span class="sxs-lookup"><span data-stu-id="ee651-142">Be assigned the **Receive As** permission on the on-premises mailbox database that stores user mailboxes and the **WriteProperty** permission to modify the **TargetAddress** property on the on-premises user accounts.</span></span>
    
<span data-ttu-id="ee651-143">Para obter instruções sobre como definir essas permissões, confira [Atribuir permissões para migrar caixas de correio para o Office 365](https://go.microsoft.com/fwlink/?LinkId=521656).</span><span class="sxs-lookup"><span data-stu-id="ee651-143">For instructions about how to set these permissions, see [Assign permissions to migrate mailboxes to Office 365](https://go.microsoft.com/fwlink/?LinkId=521656).</span></span>
  
 <span data-ttu-id="ee651-p110">**Desabilitar a UM (Unificação de Mensagens)** Se a UM estiver habilitada para as caixas de correio locais que você está migrando, desabilite a UM antes da migração. Habilite a UM para as caixas de correio após a conclusão da migração. Para obter instruções, confira[desabilitar a unificação de mensagens](https://go.microsoft.com/fwlink/?LinkId=521891).</span><span class="sxs-lookup"><span data-stu-id="ee651-p110">**Disable Unified Messaging (UM)** If UM is turned on for the on-premises mailboxes you're migrating, turn off UM before migration. Turn on UM for the mailboxes after migration is complete. For how-to steps, see[disable unified messaging](https://go.microsoft.com/fwlink/?LinkId=521891).</span></span>
  
 <span data-ttu-id="ee651-p111">**Use a sincronização de diretório para criar novos usuários no Office 365.** Você usa a sincronização de diretório para criar todos os usuários locais em sua organização do Office 365.</span><span class="sxs-lookup"><span data-stu-id="ee651-p111">**Use directory synchronization to create new users in Office 365.** You use directory synchronization to create all the on-premises users in your Office 365 organization.</span></span>
  
<span data-ttu-id="ee651-p112">Você precisa licenciar os usuários depois que eles são criados. O prazo é de 30 dias para adicionar licenças depois que os usuários são criados. Para obter as etapas para adicionar licenças, confira [Etapa 8: concluir tarefas pós-migração](use-powershell-to-perform-a-staged-migration-to-office-365.md#BK_Postmigration).</span><span class="sxs-lookup"><span data-stu-id="ee651-p112">You need to license the users after they're created. You have 30 days to add licenses after the users are created. For steps to add licenses, see [Step 8: Complete post-migration tasks](use-powershell-to-perform-a-staged-migration-to-office-365.md#BK_Postmigration).</span></span>
  
 <span data-ttu-id="ee651-p113">Você pode usar a Ferramenta de Sincronização do Microsoft Azure Active Directory ou o AAD Sync (Serviços de Sincronização do Microsoft Azure Active Directory) para sincronizar e criar seus usuários locais no Office 365. Depois que as caixas de correio são migradas para o Office 365, você gerencia as contas de usuário em sua organização local e elas são sincronizadas com sua organização do Office 365. Para saber mais, confira[Integração de diretórios](https://go.microsoft.com/fwlink/?LinkId=521788) .</span><span class="sxs-lookup"><span data-stu-id="ee651-p113">You can use either the Microsoft Azure Active Directory Synchronization Tool or the Microsoft Azure Active Directory Sync Services (AAD Sync) to synchronize and create your on-premises users in Office 365. After mailboxes are migrated to Office 365, you manage user accounts in your on-premises organization, and they're synchronized with your Office 365 organization. For more information, see[Directory Integration](https://go.microsoft.com/fwlink/?LinkId=521788) .</span></span>
  
### <a name="step-2-create-a-csv-file-for-a-staged-migration-batch"></a><span data-ttu-id="ee651-155">Etapa 2: criar um arquivo CSV para um lote de migração em estágios</span><span class="sxs-lookup"><span data-stu-id="ee651-155">Step 2: Create a CSV file for a staged migration batch</span></span>

<span data-ttu-id="ee651-p114">Depois de identificar os usuários cujas caixas de correio locais você deseja migrar para o Office 365, use um arquivo CSV (de valores separados por vírgula) para criar um lote de migração. Cada linha no arquivo CSV, usado pelo Office 365 para executar a migração, contém informações sobre uma caixa de correio local.</span><span class="sxs-lookup"><span data-stu-id="ee651-p114">After you identify the users whose on-premises mailboxes you want to migrate to Office 365, you use a comma separated value (CSV ) file to create a migration batch. Each row in the CSV file—used by Office 365 to run the migration—contains information about an on-premises mailbox.</span></span> 
  
> [!NOTE]
> <span data-ttu-id="ee651-p115">Não há um limite para o número de caixas de correio que você pode migrar para o Office 365 usando uma migração em estágios. No entanto, o arquivo CSV de um lote de migração pode conter no máximo 2.000 linhas. Para migrar mais de 2.000 caixas de correio, crie arquivos CSV adicionais e use cada arquivo para criar um novo lote de migração.</span><span class="sxs-lookup"><span data-stu-id="ee651-p115">There isn't a limit for the number of mailboxes that you can migrate to Office 365 using a staged migration. The CSV file for a migration batch can contain a maximum of 2,000 rows. To migrate more than 2,000 mailboxes, create additional CSV files and use each file to create a new migration batch.</span></span> 
  
 <span data-ttu-id="ee651-161">**Atributos com suporte**</span><span class="sxs-lookup"><span data-stu-id="ee651-161">**Supported attributes**</span></span>
  
<span data-ttu-id="ee651-p116">O arquivo CSV para uma migração em estágios dá suporte aos três atributos a seguir. Cada linha no arquivo CSV corresponde a uma caixa de correio e deve conter um valor para cada um desses atributos.</span><span class="sxs-lookup"><span data-stu-id="ee651-p116">The CSV file for a staged migration supports the following three attributes. Each row in the CSV file corresponds to a mailbox and must contain a value for each of these attributes.</span></span>
  
|<span data-ttu-id="ee651-164">**Atributo**</span><span class="sxs-lookup"><span data-stu-id="ee651-164">**Attribute**</span></span>|<span data-ttu-id="ee651-165">**Descrição**</span><span class="sxs-lookup"><span data-stu-id="ee651-165">**Description**</span></span>|<span data-ttu-id="ee651-166">**Obrigatório?**</span><span class="sxs-lookup"><span data-stu-id="ee651-166">**Required?**</span></span>|
|:-----|:-----|:-----|
|<span data-ttu-id="ee651-167">EmailAddress</span><span class="sxs-lookup"><span data-stu-id="ee651-167">EmailAddress</span></span>  <br/> |<span data-ttu-id="ee651-168">Especifica o endereço de email SMTP principal, por exemplo, laurac@contoso.com, para caixas de correio locais.</span><span class="sxs-lookup"><span data-stu-id="ee651-168">Specifies the primary SMTP email address, for example, pilarp@contoso.com, for on-premises mailboxes.</span></span>  <br/> <span data-ttu-id="ee651-p117">Use o endereço SMTP principal para caixas de correio locais, e não IDs de usuário do Office 365. Por exemplo, se o domínio local se chamar contoso.com, mas o domínio de email do Office 365 se chamar service.contoso.com, você usará o nome de domínio contoso.com para endereços de email no arquivo CSV.</span><span class="sxs-lookup"><span data-stu-id="ee651-p117">Use the primary SMTP address for on-premises mailboxes and not user IDs from the Office 365. For example, if the on-premises domain is named contoso.com but the Office 365 email domain is named service.contoso.com, you would use the contoso.com domain name for email addresses in the CSV file.</span></span>  <br/> |<span data-ttu-id="ee651-171">Obrigatório</span><span class="sxs-lookup"><span data-stu-id="ee651-171">Required</span></span>  <br/> |
|<span data-ttu-id="ee651-172">Senha</span><span class="sxs-lookup"><span data-stu-id="ee651-172">Password</span></span>  <br/> |<span data-ttu-id="ee651-p118">A senha a ser definida para a nova caixa de correio do Office 365. As restrições de senha aplicadas à sua organização do Office 365 também são aplicáveis às senhas incluídas no arquivo CSV.</span><span class="sxs-lookup"><span data-stu-id="ee651-p118">The password to be set for the new Office 365 mailbox. Any password restrictions that are applied to your Office 365 organization also apply to the passwords included in the CSV file.</span></span>  <br/> |<span data-ttu-id="ee651-175">Opcional</span><span class="sxs-lookup"><span data-stu-id="ee651-175">Optional</span></span>  <br/> |
|<span data-ttu-id="ee651-176">ForceChangePassword</span><span class="sxs-lookup"><span data-stu-id="ee651-176">ForceChangePassword</span></span>  <br/> |<span data-ttu-id="ee651-p119">Especifica se um usuário deve alterar a senha na primeira vez que entrar em sua nova caixa de correio do Office 365. Use **True** ou **False** para o valor desse parâmetro. </span><span class="sxs-lookup"><span data-stu-id="ee651-p119">Specifies whether a user must change the password the first time they sign in to their new Office 365 mailbox. Use **True** or **False** for the value of this parameter. </span></span><br/> <span data-ttu-id="ee651-179">> [!NOTE]> Se você implementou uma solução de SSO (Logon Único) com a implantação do AD FS (Serviços de Federação do Active Directory) em sua organização local, deve usar **False** para o valor do atributo **ForceChangePassword**.</span><span class="sxs-lookup"><span data-stu-id="ee651-179">> [!NOTE]> If you've implemented a single sign-on (SSO) solution by deploying Active Directory Federation Services (AD FS) or greater in your on-premises organization, you must use **False** for the value of the **ForceChangePassword** attribute.</span></span>          |<span data-ttu-id="ee651-180">Opcional</span><span class="sxs-lookup"><span data-stu-id="ee651-180">Optional</span></span>  <br/> |
   
 <span data-ttu-id="ee651-181">**Formato de arquivo CSV**</span><span class="sxs-lookup"><span data-stu-id="ee651-181">**CSV file format**</span></span>
  
<span data-ttu-id="ee651-p120">Aqui está um exemplo do formato do arquivo CSV. Neste exemplo, três caixas de correio locais são migradas para o Office 365.</span><span class="sxs-lookup"><span data-stu-id="ee651-p120">Here's an example of the format for the CSV file. In this example, three on-premises mailboxes are migrated to Office 365.</span></span>
  
<span data-ttu-id="ee651-p121">A primeira linha, ou linha de cabeçalho, do arquivo CSV lista os nomes dos atributos ou campos especificados nas linhas a seguir. Cada nome de atributo é separado por uma vírgula.</span><span class="sxs-lookup"><span data-stu-id="ee651-p121">The first row, or header row, of the CSV file lists the names of the attributes, or fields, specified in the rows that follow. Each attribute name is separated by a comma.</span></span>
  
```
EmailAddress,Password,ForceChangePassword 
pilarp@contoso.com,Pa$$w0rd,False 
tobyn@contoso.com,Pa$$w0rd,False 
briant@contoso.com,Pa$$w0rd,False 
```

<span data-ttu-id="ee651-p122">Cada linha embaixo da linha de cabeçalho representa um usuário e fornece as informações que serão utilizadas para migrar a caixa de correio dele. Os valores de atributo em cada linha devem estar na mesma ordem que os nomes dos atributos na linha de cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="ee651-p122">Each row under the header row represents one user and supplies the information that will be used to migrate the user's mailbox. The attribute values in each row must be in the same order as the attribute names in the header row.</span></span> 
  
<span data-ttu-id="ee651-p123">Use qualquer editor de texto ou um aplicativo como o Excel para criar o arquivo CSV. Salve-o como um arquivo .csv ou .txt.</span><span class="sxs-lookup"><span data-stu-id="ee651-p123">Use any text editor, or an application like Excel , to create the CSV file. Save the file as a .csv or .txt file.</span></span>
  
> [!NOTE]
> <span data-ttu-id="ee651-p124">Se o arquivo CSV contiver caracteres não-ASCII ou especiais, salve-o com UTF-8 ou outra codificação Unicode. Dependendo do aplicativo, salvar o arquivo CSV com UTF-8 ou outra codificação Unicode pode ser mais fácil quando a localidade do sistema do computador corresponde ao idioma usado no arquivo CSV.</span><span class="sxs-lookup"><span data-stu-id="ee651-p124">If the CSV file contains non-ASCII or special characters, save the CSV file with UTF-8 or other Unicode encoding. Depending on the application, saving the CSV file with UTF-8 or other Unicode encoding can be easier when the system locale of the computer matches the language used in the CSV file.</span></span> 
  
### <a name="step-3-create-a-migration-endpoint"></a><span data-ttu-id="ee651-192">Etapa 3: criar um ponto de extremidade de migração</span><span class="sxs-lookup"><span data-stu-id="ee651-192">Step 3: Create a migration endpoint</span></span>
<span data-ttu-id="ee651-193"><a name="BK_Endpoint"> </a></span><span class="sxs-lookup"><span data-stu-id="ee651-193"><a name="BK_Endpoint"> </a></span></span>

<span data-ttu-id="ee651-p125">Para migrar o email com êxito, o Office 365 precisa se conectar e se comunicar com o sistema de email de origem. Para fazer isso, o Office 365 usa um ponto de extremidade de migração. A fim de criar um ponto de extremidade de migração do Outlook em Qualquer Lugar usando o PowerShell, para a migração em estágios, primeiro [conecte-se ao Exchange Online](https://go.microsoft.com/fwlink/p/?LinkId=534121).</span><span class="sxs-lookup"><span data-stu-id="ee651-p125">To migrate email successfully, Office 365 needs to connect and communicate with the source email system. To do this, Office 365 uses a migration endpoint. To create an Outlook Anywhere migration endpoint by using PowerShell, for staged migration, first [connect to Exchange Online](https://go.microsoft.com/fwlink/p/?LinkId=534121).</span></span> 
  
<span data-ttu-id="ee651-197">Para obter uma lista completa dos comandos de migração, confira [Cmdlets de movimentação e migração](https://go.microsoft.com/fwlink/p/?LinkId=534750).</span><span class="sxs-lookup"><span data-stu-id="ee651-197">For a full list of migration commands, see [Move and migration cmdlets](https://go.microsoft.com/fwlink/p/?LinkId=534750).</span></span>
  
<span data-ttu-id="ee651-198">Para criar um ponto de extremidade de migração do Outlook em Qualquer Lugar chamado "StagedEndpoint" no PowerShell do Exchange Online, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="ee651-198">To create an Outlook Anywhere migration endpoint called "StagedEndpoint" in Exchange Online PowerShell, run the following commands:</span></span>
  
```
$Credentials = Get-Credential
```

```
New-MigrationEndpoint -ExchangeOutlookAnywhere -Name StagedEndpoint -Autodiscover -EmailAddress administrator@contoso.com -Credentials $Credentials
```

<span data-ttu-id="ee651-199">Para saber mais sobre o cmdlet **New-MigrationEndpoint**, confira[New-MigrationEndpoint](https://go.microsoft.com/fwlink/p/?LinkId=536437).</span><span class="sxs-lookup"><span data-stu-id="ee651-199">For more information about the **New-MigrationEndpoint** cmdlet, see[New-MigrationEndpoint](https://go.microsoft.com/fwlink/p/?LinkId=536437).</span></span>
  
> [!NOTE]
> <span data-ttu-id="ee651-p126">O cmdlet **New-MigrationEndpoint** pode ser usado para especificar um banco de dados do serviço a usar, utilizando a opção **-TargetDatabase**. Caso contrário, o banco de dados é atribuído de forma aleatória do site do Serviços de Federação do Active Directory (AD FS) 2.0, no qual a caixa de correio de gerenciamento está localizada.</span><span class="sxs-lookup"><span data-stu-id="ee651-p126">The **New-MigrationEndpoint** cmdlet can be used to specify a database for the service to use by using the **-TargetDatabase** option. Otherwise a database is randomly assigned from the Active Directory Federation Services (AD FS) 2.0 site where the management mailbox is located.</span></span>
  
#### <a name="verify-it-worked"></a><span data-ttu-id="ee651-202">Verifique se funcionou</span><span class="sxs-lookup"><span data-stu-id="ee651-202">Verify it worked</span></span>

<span data-ttu-id="ee651-203">No PowerShell do Exchange Online, execute o seguinte comando para exibir informações sobre o ponto de extremidade de migração "StagedEndpoint":</span><span class="sxs-lookup"><span data-stu-id="ee651-203">In Exchange Online PowerShell, run the following command to display information about the "StagedEndpoint" migration endpoint:</span></span>
  
```
Get-MigrationEndpoint StagedEndpoint | Format-List EndpointType,ExchangeServer,UseAutoDiscover,Max*
```

### <a name="step-4-create-and-start-a-stage-migration-batch"></a><span data-ttu-id="ee651-204">Etapa 4: criar e iniciar um lote de migração em estágios</span><span class="sxs-lookup"><span data-stu-id="ee651-204">Step 4: Create and start a stage migration batch</span></span>
<span data-ttu-id="ee651-205"><a name="BK_Endpoint"> </a></span><span class="sxs-lookup"><span data-stu-id="ee651-205"><a name="BK_Endpoint"> </a></span></span>

<span data-ttu-id="ee651-p127">Você pode usar o cmdlet **New-MigrationBatch** no PowerShell do Exchange Online para criar um lote de migração de uso em uma migração de transferência. É possível criar um lote de migração e iniciá-lo automaticamente incluindo o parâmetro _AutoStart_. Como alternativa, você pode criar o lote de migração e iniciá-lo manualmente mais tarde usando o cmdlet **Start-MigrationBatch**. Este exemplo cria um lote de migração chamado "StagedBatch1" e utiliza o ponto de extremidade de migração criado na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="ee651-p127">You can use the **New-MigrationBatch** cmdlet in Exchange Online PowerShell to create a migration batch for a cutover migration. You can create a migration batch and start it automatically by including the _AutoStart_ parameter. Alternatively, you can create the migration batch and then manually start it afterwards by using the **Start-MigrationBatch** cmdlet. This example creates a migration batch called "StagedBatch1" and uses the migration endpoint that was created in the previous step.</span></span>
  
```
New-MigrationBatch -Name StagedBatch1 -SourceEndpoint StagedEndpoint -AutoStart
```

<span data-ttu-id="ee651-p128">Este exemplo também cria um lote de migração chamado "StagedBatch1" e utiliza o ponto de extremidade de migração criado na etapa anterior. Como o parâmetro  _AutoStart_ não está incluído, o lote de migração deve ser iniciado manualmente no painel de migração ou usando o cmdlet **Start-MigrationBatch**. Como dito anteriormente, apenas um lote de migração de transferência pode existir de cada vez.</span><span class="sxs-lookup"><span data-stu-id="ee651-p128">This example also creates a migration batch called "StagedBatch1" and uses the migration endpoint that was created in the previous step. Because the  _AutoStart_ parameter isn't included, the migration batch has to be manually started on the migration dashboard or by using **Start-MigrationBatch** cmdlet. As previously stated, only one cutover migration batch can exist at a time.</span></span>
  
```
New-MigrationBatch -Name StagedBatch1 -SourceEndpoint StagedEndpoint
```

#### <a name="verify-it-worked"></a><span data-ttu-id="ee651-213">Verifique se funcionou</span><span class="sxs-lookup"><span data-stu-id="ee651-213">Verify it worked</span></span>

<span data-ttu-id="ee651-214">Execute o seguinte comando no PowerShell do Exchange Online para exibir informações sobre o "StagedBatch1":</span><span class="sxs-lookup"><span data-stu-id="ee651-214">Run the following command in Exchange Online PowerShell to display information about the "StagedBatch1":</span></span>
  
```
Get-MigrationBatch -Identity StagedBatch1 | Format-List
```

<span data-ttu-id="ee651-215">Você também pode verificar se o lote foi iniciado executando o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="ee651-215">You can also verify that the batch has started by running the following command:</span></span>
  
```
Get-MigrationBatch -Identity StagedBatch1 | Format-List Status
```

<span data-ttu-id="ee651-216">Para saber mais sobre o cmdlet **Get-MigrationBatch**, confira[Get-MigrationBatch](https://go.microsoft.com/fwlink/p/?LinkId=536441).</span><span class="sxs-lookup"><span data-stu-id="ee651-216">For more information about the **Get-MigrationBatch** cmdlet, see[Get-MigrationBatch](https://go.microsoft.com/fwlink/p/?LinkId=536441).</span></span>
  
### <a name="step-5-convert-on-premises-mailboxes-to-mail-enabled-users"></a><span data-ttu-id="ee651-217">Etapa 5: converter caixas de correio locais em usuários habilitados para email</span><span class="sxs-lookup"><span data-stu-id="ee651-217">Step 5: Convert on-premises mailboxes to mail-enabled users</span></span>
<span data-ttu-id="ee651-218"><a name="BK_Endpoint"> </a></span><span class="sxs-lookup"><span data-stu-id="ee651-218"><a name="BK_Endpoint"> </a></span></span>

<span data-ttu-id="ee651-p129">Depois de migrar com êxito um lote de caixas de correio, é necessário permitir que os usuários acessem suas mensagens de alguma forma. Um usuário cuja caixa de correio foi migrada agora tem uma caixa de correio local e outra no Office 365. Os usuários que tiverem uma caixa de correio no Office 365 deixarão de receber novas mensagens na caixa de correio local.</span><span class="sxs-lookup"><span data-stu-id="ee651-p129">After you have successfully migrated a batch of mailboxes, you need some way to let users get to their mail. A user whose mailbox has been migrated now has both a mailbox on-premises and one in Office 365. Users who have a mailbox in Office 365 will stop receiving new mail in their on-premises mailbox.</span></span> 
  
<span data-ttu-id="ee651-p130">Como não concluiu suas migrações, você ainda não está pronto para direcionar todos os usuários para o Office 365 e seus emails. Então, o que fazer com as pessoas que têm ambas? Você pode alterar as caixas de correio locais que já migrou para usuários habilitados para email. Ao mudar de uma caixa de correio para um usuário habilitado para email, você pode direcionar o usuário para o Office 365 para acessar o email em vez de ir para a caixa de correio local.</span><span class="sxs-lookup"><span data-stu-id="ee651-p130">Because you are not done with your migrations, you are not yet ready to direct all users to Office 365 for their email. So what do you do for those people who have both? What you can do is change the on-premises mailboxes that you've already migrated to mail-enabled users. When you change from a mailbox to a mail-enabled user, you can direct the user to Office 365 for their email instead of going to their on-premises mailbox.</span></span> 
  
<span data-ttu-id="ee651-p131">Outro motivo importante para converter as caixas de correio locais em usuários habilitados para email é manter os endereços proxy das caixas de correio do Office 365 copiando endereços proxy para usuários habilitados para email. Isso permite que você gerencie os usuários baseados em nuvem de sua organização local utilizando o Active Directory. Além disso, se você decidir desprogramar sua organização do Exchange Server local após todas as caixas de correio serem migradas para o Office 365, os endereços de proxy que você copiou para os usuários habilitados para email permanecerão em seu Active Directory local.</span><span class="sxs-lookup"><span data-stu-id="ee651-p131">Another important reason to convert on-premises mailboxes to mail-enabled users is to retain proxy addresses from the Office 365 mailboxes by copying proxy addresses to the mail-enabled users. This lets you manage cloud-based users from your on-premises organization by using Active Directory. Also, if you decide to decommission your on-premises Exchange Server organization after all mailboxes are migrated to Office 365, the proxy addresses you've copied to the mail-enabled users will remain in your on-premises Active Directory.</span></span>
  
<span data-ttu-id="ee651-229">Para saber mais e para baixar os scripts que você pode executar para converter as caixas de correio em usuários habilitados para email, confira o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ee651-229">For more information, and to download scripts that you can run to convert mailboxes to mail-enabled users, see the following:</span></span>
  
- [<span data-ttu-id="ee651-230">Converter caixas de correio do Exchange 2007 em usuários habilitados para email</span><span class="sxs-lookup"><span data-stu-id="ee651-230">Convert Exchange 2007 mailboxes to mail-enabled users</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=233648)
    
- [<span data-ttu-id="ee651-231">Converter caixas de correio do Exchange 2003 em usuários habilitados para email</span><span class="sxs-lookup"><span data-stu-id="ee651-231">Convert Exchange 2003 mailboxes to mail-enabled users</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=233647)
    
### <a name="step-6-delete-a-staged-migration-batch"></a><span data-ttu-id="ee651-232">Etapa 6: excluir um lote de migração em estágios</span><span class="sxs-lookup"><span data-stu-id="ee651-232">Step 6: Delete a staged migration batch</span></span>
<span data-ttu-id="ee651-233"><a name="BK_Endpoint"> </a></span><span class="sxs-lookup"><span data-stu-id="ee651-233"><a name="BK_Endpoint"> </a></span></span>

 <span data-ttu-id="ee651-p132">Após todas as caixas de correio em um lote de migração terem migrado com êxito e você ter convertido as caixas de correio locais no lote em usuários habilitados para email, tudo está pronto para excluir um lote de migração em estágios. Verifique se o email está sendo encaminhado para as caixas de correio do Office 365 no lote de migração. Ao excluir um lote de migração em estágios, o serviço de migração limpa todos os registros relacionados ao lote e o exclui.</span><span class="sxs-lookup"><span data-stu-id="ee651-p132">After all mailboxes in a migration batch have been successfully migrated, and you've converted the on-premises mailboxes in the batch to mail-enabled users, you're ready to delete a staged migration batch. Be sure to verify that mail is being forwarded to the Office 365 mailboxes in the migration batch. When you delete a staged migration batch, the migration service cleans up any records related to the migration batch and deletes the migration batch.</span></span>
  
<span data-ttu-id="ee651-237">Para excluir o lote de migração "StagedBatch1" no PowerShell do Exchange Online, execute o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="ee651-237">To delete the "StagedBatch1" migration batch in Exchange Online PowerShell, run the following command.</span></span>
  
```
Remove-MigrationBatch -Identity StagedBatch1
```

<span data-ttu-id="ee651-238">Para saber mais sobre o cmdlet **Remove-MigrationBatch**, confira[Remove-MigrationBatch](https://go.microsoft.com/fwlink/p/?LinkId=536481).</span><span class="sxs-lookup"><span data-stu-id="ee651-238">For more information about the **Remove-MigrationBatch** cmdlet, see[Remove-MigrationBatch](https://go.microsoft.com/fwlink/p/?LinkId=536481).</span></span>
  
#### <a name="verify-it-worked"></a><span data-ttu-id="ee651-239">Verifique se funcionou</span><span class="sxs-lookup"><span data-stu-id="ee651-239">Verify it worked</span></span>

<span data-ttu-id="ee651-240">Execute o seguinte comando no PowerShell do Exchange Online para exibir informações sobre o "IMAPBatch1":</span><span class="sxs-lookup"><span data-stu-id="ee651-240">Run the following command in Exchange Online PowerShell to display information about the "IMAPBatch1":</span></span>
  
```
Get-MigrationBatch StagedBatch1
```

<span data-ttu-id="ee651-241">O comando retornará o lote de migração com um status de **Removing** ou retornará um erro afirmando que o lote de migração não foi encontrado, confirmando que o lote foi excluído.</span><span class="sxs-lookup"><span data-stu-id="ee651-241">The command will return either the migration batch with a status of **Removing**, or it will return an error stating that migration batch couldn't be found, verifying that the batch was deleted.</span></span>
  
<span data-ttu-id="ee651-242">Para saber mais sobre o cmdlet **Get-MigrationBatch**, confira[Get-MigrationBatch](https://go.microsoft.com/fwlink/p/?LinkId=536441).</span><span class="sxs-lookup"><span data-stu-id="ee651-242">For more information about the **Get-MigrationBatch** cmdlet, see[Get-MigrationBatch](https://go.microsoft.com/fwlink/p/?LinkId=536441).</span></span>
  
### <a name="step7-assign-licenses-to-office-365-users"></a><span data-ttu-id="ee651-243">Etapa7: atribuir licenças aos usuários do Office 365</span><span class="sxs-lookup"><span data-stu-id="ee651-243">Step7: Assign licenses to Office 365 users</span></span>
<span data-ttu-id="ee651-244"><a name="BK_Endpoint"> </a></span><span class="sxs-lookup"><span data-stu-id="ee651-244"><a name="BK_Endpoint"> </a></span></span>

<span data-ttu-id="ee651-p133">Ative as contas de usuário do Office 365 para as contas migradas atribuindo licenças. Se você não atribuir uma licença, a caixa de correio será desabilitada quando terminar o período de carência (30 dias). Para atribuir uma licença no Centro de administração do Office 365, confira [Atribuir ou cancelar a atribuição de licenças para o Office 365 para empresas](https://go.microsoft.com/fwlink/?LinkId=536681).</span><span class="sxs-lookup"><span data-stu-id="ee651-p133">Activate Office 365 user accounts for the migrated accounts by assigning licenses. If you don't assign a license, the mailbox is disabled when the grace period (30 days) ends. To assign a license in the Office 365 admin center, see [Assign or unassign licenses for Office 365 for business](https://go.microsoft.com/fwlink/?LinkId=536681).</span></span>
  
### <a name="step-8-complete-post-migration-tasks"></a><span data-ttu-id="ee651-248">Etapa 8: concluir tarefas pós-migração</span><span class="sxs-lookup"><span data-stu-id="ee651-248">Step 8: Complete post-migration tasks</span></span>
<span data-ttu-id="ee651-249"><a name="BK_Postmigration"> </a></span><span class="sxs-lookup"><span data-stu-id="ee651-249"><a name="BK_Postmigration"> </a></span></span>

- <span data-ttu-id="ee651-p134">**Crie um registro DNS de Descoberta Automática para que os usuários possam facilmente acessar suas caixas de correio.**Após todas as caixas de correio locais serem migradas para o Office 365, você pode configurar um registro DNS de Descoberta Automática para sua organização do Office 365 e habilitar os usuários a se conectarem facilmente a suas novas caixas de correio do Office 365 com o Outlook e clientes móveis. Esse novo registro DNS de descoberta automática tem que usar o mesmo namespace que você usar para a organização do Office 365. Por exemplo, se seu namespace baseado em nuvem for cloud.contoso.com, o registro DNS de Descoberta Automática a ser criado será autodiscover.cloud.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="ee651-p134">**Create an Autodiscover DNS record so users can easily get to their mailboxes.** After all on-premises mailboxes are migrated to Office 365, you can configure an Autodiscover DNS record for your Office 365 organization to enable users to easily connect to their new Office 365 mailboxes with Outlook and mobile clients. This new Autodiscover DNS record has to use the same namespace that you're using for your Office 365 organization. For example, if your cloud-based namespace is cloud.contoso.com, the Autodiscover DNS record you need to create is autodiscover.cloud.contoso.com.</span></span>
    
    <span data-ttu-id="ee651-p135">O Office 365 usa um registro CNAME para implementar o serviço Descoberta Automática para o Outlook e para clientes móveis. O registro CNAME de Descoberta Automática deve conter as seguintes informações:</span><span class="sxs-lookup"><span data-stu-id="ee651-p135">Office 365 uses a CNAME record to implement the Autodiscover service for Outlook and mobile clients. The Autodiscover CNAME record must contain the following information:</span></span>
    
  - <span data-ttu-id="ee651-256">**Alias:** descoberta automática</span><span class="sxs-lookup"><span data-stu-id="ee651-256">**Alias:** autodiscover</span></span>
    
  - <span data-ttu-id="ee651-257">**Destino:** autodiscover.outlook.com</span><span class="sxs-lookup"><span data-stu-id="ee651-257">**Target:** autodiscover.outlook.com</span></span>
    
    <span data-ttu-id="ee651-258">Para saber mais, confira [Criar registros DNS para o Office 365 ao gerenciar seus registros DNS](https://go.microsoft.com/fwlink/p/?LinkId=535028).</span><span class="sxs-lookup"><span data-stu-id="ee651-258">For more information, see [Create DNS records for Office 365 when you manage your DNS records](https://go.microsoft.com/fwlink/p/?LinkId=535028).</span></span>
    
- <span data-ttu-id="ee651-p136">**Desativar servidores locais do Exchange.** Depois de verificar se todos os emails estão sendo roteados diretamente para as caixas de correio do Office 365 e quando você não precisar mais manter sua organização de email local ou não planejar implementar uma solução SSO, você poderá desinstalar o Exchange de seus servidores e remover sua organização local do Exchange.</span><span class="sxs-lookup"><span data-stu-id="ee651-p136">**Decommission on-premises Exchange servers.** After you've verified that all email is being routed directly to the Office 365 mailboxes, and you no longer need to maintain your on-premises email organization or don't plan on implementing an SSO solution, you can uninstall Exchange from your servers and remove your on-premises Exchange organization.</span></span>
    
    <span data-ttu-id="ee651-261">Para obter mais informações, confira o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ee651-261">For more information, see the following:</span></span>
    
  - [<span data-ttu-id="ee651-262">Modificar ou remover o Exchange 2010</span><span class="sxs-lookup"><span data-stu-id="ee651-262">Modify or Remove Exchange 2010</span></span>](https://go.microsoft.com/fwlink/?LinkId=217936)
    
  - [<span data-ttu-id="ee651-263">Como remover uma Organização do Exchange 2007</span><span class="sxs-lookup"><span data-stu-id="ee651-263">How to Remove an Exchange 2007 Organization</span></span>](https://go.microsoft.com/fwlink/?LinkID=100485)
    
  - [<span data-ttu-id="ee651-264">Como desinstalar o Exchange Server 2003</span><span class="sxs-lookup"><span data-stu-id="ee651-264">How to Uninstall Exchange Server 2003</span></span>](https://go.microsoft.com/fwlink/?LinkID=56561)
    
