---
title: Implantar uma máquina virtual do Azure na linguagem Go
description: Implante uma máquina virtual usando o SDK do Azure para linguagem Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059128"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="9defc-103">Início rápido: implantar uma máquina virtual do Azure de um modelo com o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="9defc-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="9defc-104">Este início rápido mostra como implantar recursos de um modelo do Azure Resource Manager, usando o SDK do Azure para linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="9defc-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="9defc-105">Os modelos são instantâneos de todos os recursos em um [grupo de recursos do Azure](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="9defc-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="9defc-106">Ao longo do processo, você vai se familiarizar com a funcionalidade e as convenções do SDK.</span><span class="sxs-lookup"><span data-stu-id="9defc-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="9defc-107">No final deste início rápido, você terá uma VM em execução na qual pode entrar com um nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="9defc-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="9defc-108">Para ver a criação de uma VM na linguagem Go sem o uso de um modelo do Resource Manager, existe um [exemplo imperativo](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) que demonstra como criar e configurar todos os recursos VM com o SDK.</span><span class="sxs-lookup"><span data-stu-id="9defc-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="9defc-109">Usar um modelo neste exemplo permite focar-se nas convenções do SDK sem entrar em muitos detalhes sobre a arquitetura de serviço do Azure.</span><span class="sxs-lookup"><span data-stu-id="9defc-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="9defc-110">Caso use uma instalação local da CLI do Azure, este início rápido requer a CLI versão __2.0.28__ ou posterior.</span><span class="sxs-lookup"><span data-stu-id="9defc-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="9defc-111">Execute `az --version` para garantir que a instalação de sua CLI atende a esse requisito.</span><span class="sxs-lookup"><span data-stu-id="9defc-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="9defc-112">Se você precisar instalar ou atualizar, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9defc-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="9defc-113">Instale o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="9defc-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="9defc-114">Criar uma entidade de serviço</span><span class="sxs-lookup"><span data-stu-id="9defc-114">Create a service principal</span></span>

<span data-ttu-id="9defc-115">Para entrar em modo não interativo no Azure com um aplicativo, você precisa de uma entidade de serviço.</span><span class="sxs-lookup"><span data-stu-id="9defc-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="9defc-116">As entidades de serviço fazem parte do controle de acesso baseado em função (RBAC), que cria uma identidade de usuário exclusiva.</span><span class="sxs-lookup"><span data-stu-id="9defc-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="9defc-117">Para criar uma nova entidade de serviço com a CLI, execute o comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="9defc-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="9defc-118">Defina a variável de ambiente `AZURE_AUTH_LOCATION` para ser o caminho completo para esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="9defc-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="9defc-119">Em seguida, o SDK localiza e lê as credenciais diretamente desse arquivo, sem que você precise fazer quaisquer alterações ou registrar informações da entidade de serviço.</span><span class="sxs-lookup"><span data-stu-id="9defc-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="9defc-120">Obter o código</span><span class="sxs-lookup"><span data-stu-id="9defc-120">Get the code</span></span>

<span data-ttu-id="9defc-121">Obtenha o código de início rápido e todas as suas dependências com `go get`.</span><span class="sxs-lookup"><span data-stu-id="9defc-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="9defc-122">Você não precisa fazer nenhuma modificação no código-fonte se a variável `AZURE_AUTH_LOCATION` estiver definida corretamente.</span><span class="sxs-lookup"><span data-stu-id="9defc-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="9defc-123">Quando o programa for executado, ele carregará todas as informações de autenticação necessárias de lá.</span><span class="sxs-lookup"><span data-stu-id="9defc-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="9defc-124">Executando o código</span><span class="sxs-lookup"><span data-stu-id="9defc-124">Running the code</span></span>

<span data-ttu-id="9defc-125">Execute o início rápido com o comando `go run`.</span><span class="sxs-lookup"><span data-stu-id="9defc-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="9defc-126">Se a implantação for bem-sucedida, você receberá uma mensagem fornecendo o nome de usuário, o endereço IP e a senha para fazer logon na máquina virtual recém-criada.</span><span class="sxs-lookup"><span data-stu-id="9defc-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="9defc-127">Use SSH neste computador para ver se ele está funcionando.</span><span class="sxs-lookup"><span data-stu-id="9defc-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="9defc-128">Limpando</span><span class="sxs-lookup"><span data-stu-id="9defc-128">Cleaning up</span></span>

<span data-ttu-id="9defc-129">Limpe todos os recursos criados nesse início rápido excluindo o grupo de recursos com a CLI.</span><span class="sxs-lookup"><span data-stu-id="9defc-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="9defc-130">Além disso, exclua a entidade de serviço que foi criada.</span><span class="sxs-lookup"><span data-stu-id="9defc-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="9defc-131">No arquivo `quickstart.auth`, há uma chave JSON para `clientId`.</span><span class="sxs-lookup"><span data-stu-id="9defc-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="9defc-132">Copie este valor para a variável de ambiente `CLIENT_ID_VALUE` e execute o seguinte comando da CLI do Azure:</span><span class="sxs-lookup"><span data-stu-id="9defc-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="9defc-133">Em que você fornece o valor para `CLIENT_ID_VALUE` de `quickstart.auth`.</span><span class="sxs-lookup"><span data-stu-id="9defc-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="9defc-134">Não excluir a entidade de serviço desse aplicativo deixa-o ativo no locatário do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9defc-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="9defc-135">Embora o nome e a senha para a entidade de serviço sejam gerados como UUIDs, certifique-se de seguir as boas práticas de segurança, excluindo quaisquer entidades de serviço não utilizadas e aplicativos do Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9defc-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="9defc-136">Código em detalhes</span><span class="sxs-lookup"><span data-stu-id="9defc-136">Code in depth</span></span>

<span data-ttu-id="9defc-137">O que o código de início rápido faz é dividido em um bloco de variáveis e várias funções pequenas, que serão uma a uma discutidas aqui.</span><span class="sxs-lookup"><span data-stu-id="9defc-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="9defc-138">Variáveis, constantes e tipos</span><span class="sxs-lookup"><span data-stu-id="9defc-138">Variables, constants, and types</span></span>

<span data-ttu-id="9defc-139">Uma vez que o início rápido é autossuficiente, ele usa variáveis e constantes globais.</span><span class="sxs-lookup"><span data-stu-id="9defc-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

<span data-ttu-id="9defc-140">Os valores são declarados, fornecendo os nomes dos recursos criados.</span><span class="sxs-lookup"><span data-stu-id="9defc-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="9defc-141">O local também é especificado aqui, que pode ser alterado para ver como as implantações se comportam em outros data centers.</span><span class="sxs-lookup"><span data-stu-id="9defc-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="9defc-142">Nem todo datacenter tem todos os recursos necessários disponíveis.</span><span class="sxs-lookup"><span data-stu-id="9defc-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="9defc-143">O tipo `clientInfo` possui as informações carregadas do arquivo de autenticação para configurar clientes no SDK e definir a senha da VM.</span><span class="sxs-lookup"><span data-stu-id="9defc-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="9defc-144">As constantes `templateFile` e `parametersFile` apontam para os arquivos necessários para a implantação.</span><span class="sxs-lookup"><span data-stu-id="9defc-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="9defc-145">O `authorizer` será configurados pelo SDK do Go para autenticação, e a variável `ctx` é um [contexto do Go](https://blog.golang.org/context) para as operações de rede.</span><span class="sxs-lookup"><span data-stu-id="9defc-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="9defc-146">Autenticação e inicialização</span><span class="sxs-lookup"><span data-stu-id="9defc-146">Authentication and initialization</span></span>

<span data-ttu-id="9defc-147">A função `init` configura a autenticação.</span><span class="sxs-lookup"><span data-stu-id="9defc-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="9defc-148">Uma vez que a autorização é uma pré-condição para tudo no início rápido, faz sentido que ela seja parte da inicialização.</span><span class="sxs-lookup"><span data-stu-id="9defc-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="9defc-149">Ele também carrega algumas informações necessárias do arquivo de autenticação para configurar os clientes e a VM.</span><span class="sxs-lookup"><span data-stu-id="9defc-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

<span data-ttu-id="9defc-150">Primeiro, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) é chamado para carregar as informações de autenticação do arquivo localizado em `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="9defc-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="9defc-151">Em seguida, esse arquivo é carregado manualmente pela função `readJSON` (omitida aqui) para extrair os dois valores necessários para executar o restante do programa: a ID de assinatura do cliente e o segredo da entidade de serviço, que também é usado para a senha da VM.</span><span class="sxs-lookup"><span data-stu-id="9defc-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="9defc-152">Para simplificar o início rápido, a senha da entidade de serviço é reutilizada.</span><span class="sxs-lookup"><span data-stu-id="9defc-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="9defc-153">Na produção, tome cuidado para __nunca__ reutilizar uma senha que forneça acesso aos recursos do Azure.</span><span class="sxs-lookup"><span data-stu-id="9defc-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="9defc-154">Fluxo de operações em main()</span><span class="sxs-lookup"><span data-stu-id="9defc-154">Flow of operations in main()</span></span>

<span data-ttu-id="9defc-155">A função `main` é simples, apenas indica o fluxo das operações e executa a verificação de erros.</span><span class="sxs-lookup"><span data-stu-id="9defc-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

<span data-ttu-id="9defc-156">As etapas que o código executa são, em ordem:</span><span class="sxs-lookup"><span data-stu-id="9defc-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="9defc-157">Criar o grupo de recursos para implantar em (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="9defc-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="9defc-158">Criar a implantação dentro deste grupo (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="9defc-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="9defc-159">Obter e exibir as informações de logon da VM implantada (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="9defc-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="9defc-160">Criar o grupo de recursos</span><span class="sxs-lookup"><span data-stu-id="9defc-160">Create the resource group</span></span>

<span data-ttu-id="9defc-161">A função `createGroup` cria o grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="9defc-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="9defc-162">Ao observar o fluxo de chamadas e os argumentos vemos a maneira como as interações de serviço são estruturadas no SDK.</span><span class="sxs-lookup"><span data-stu-id="9defc-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="9defc-163">O fluxo geral de interação com um serviço do Azure é:</span><span class="sxs-lookup"><span data-stu-id="9defc-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="9defc-164">Criar o cliente usando o método `service.New*Client()`, no qual `*` é o tipo de recurso do `service` com o qual você deseja interagir.</span><span class="sxs-lookup"><span data-stu-id="9defc-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="9defc-165">Essa função sempre usa uma ID de assinatura.</span><span class="sxs-lookup"><span data-stu-id="9defc-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="9defc-166">Defina o método de autorização para o cliente, permitindo que ele interaja com a API remota.</span><span class="sxs-lookup"><span data-stu-id="9defc-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="9defc-167">Realize o método de chamada no cliente correspondente à API remota.</span><span class="sxs-lookup"><span data-stu-id="9defc-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="9defc-168">Os métodos de serviço do cliente geralmente levam o nome do recurso e um objeto de metadados.</span><span class="sxs-lookup"><span data-stu-id="9defc-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="9defc-169">A função [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) é usada aqui para executar uma conversão de tipo.</span><span class="sxs-lookup"><span data-stu-id="9defc-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="9defc-170">Os parâmetros para métodos do SDK usam ponteiros quase exclusivamente, portanto, métodos de conveniência são fornecidos para facilitar as conversões de tipo.</span><span class="sxs-lookup"><span data-stu-id="9defc-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="9defc-171">Confira a documentação do módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obter a lista completa de conversores de conveniência e seu comportamento.</span><span class="sxs-lookup"><span data-stu-id="9defc-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="9defc-172">O método `groupsClient.CreateOrUpdate` retorna um ponteiro para um tipo de dados que representa o grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="9defc-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="9defc-173">Um valor de retorno direto desse tipo indica uma operação de execução curta que deve ser síncrona.</span><span class="sxs-lookup"><span data-stu-id="9defc-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="9defc-174">Na próxima seção, você verá um exemplo de uma operação de longa execução e como interagir com ela.</span><span class="sxs-lookup"><span data-stu-id="9defc-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="9defc-175">Executar a implantação</span><span class="sxs-lookup"><span data-stu-id="9defc-175">Perform the deployment</span></span>

<span data-ttu-id="9defc-176">Depois de criar o grupo de recursos, deve-se executar a implantação.</span><span class="sxs-lookup"><span data-stu-id="9defc-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="9defc-177">Esse código é dividido em seções menores para enfatizar diferentes partes de sua lógica.</span><span class="sxs-lookup"><span data-stu-id="9defc-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

<span data-ttu-id="9defc-178">Os arquivos de implantação são carregados por `readJSON`, cujos detalhes são ignorados aqui.</span><span class="sxs-lookup"><span data-stu-id="9defc-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="9defc-179">Essa função retorna um `*map[string]interface{}`, o tipo usado para construir os metadados para a chamada de implantação de recursos.</span><span class="sxs-lookup"><span data-stu-id="9defc-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="9defc-180">A senha da VM também é definida manualmente nos parâmetros de implantação.</span><span class="sxs-lookup"><span data-stu-id="9defc-180">The VM's password is also set manually on the deployment parameters.</span></span>

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

    deploymentFuture, err := deploymentsClient.CreateOrUpdate(
        ctx,
        resourceGroupName,
        deploymentName,
        resources.Deployment{
            Properties: &resources.DeploymentProperties{
                Template:   template,
                Parameters: params,
                Mode:       resources.Incremental,
            },
        },
    )
    if err != nil {
        return
    }
```

<span data-ttu-id="9defc-181">Esse código segue o mesmo padrão da criação do grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="9defc-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="9defc-182">Um novo cliente é criado, considerando a capacidade de autenticar com o Azure e, em seguida, um método é chamado.</span><span class="sxs-lookup"><span data-stu-id="9defc-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="9defc-183">O método tem até o mesmo nome (`CreateOrUpdate`) que o método correspondente dos grupos de recurso.</span><span class="sxs-lookup"><span data-stu-id="9defc-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="9defc-184">Esse padrão é visto em todo o SDK.</span><span class="sxs-lookup"><span data-stu-id="9defc-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="9defc-185">Os métodos que executam um trabalho semelhante, normalmente têm o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="9defc-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="9defc-186">A maior diferença é o valor de retorno do método `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="9defc-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="9defc-187">Esse valor é do tipo [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), que segue o [padrão de design do Future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="9defc-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="9defc-188">Valores Futures representam uma operação de longa execução no Azure que você pode pesquisar, cancelar ou bloquear ao ser concluída.</span><span class="sxs-lookup"><span data-stu-id="9defc-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="9defc-189">Neste exemplo, a melhor coisa a fazer é aguardar a conclusão da operação.</span><span class="sxs-lookup"><span data-stu-id="9defc-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="9defc-190">Aguardar um valor Future requer um [objeto de contexto](https://blog.golang.org/context) e o cliente que criou o `Future`.</span><span class="sxs-lookup"><span data-stu-id="9defc-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="9defc-191">Há duas fontes de erro possíveis: um erro causado por parte do cliente durante a tentativa de invocar o método e uma resposta de erro do servidor.</span><span class="sxs-lookup"><span data-stu-id="9defc-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="9defc-192">O segundo é retornado como parte da chamada `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="9defc-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="9defc-193">Obter o endereço IP atribuído</span><span class="sxs-lookup"><span data-stu-id="9defc-193">Get the assigned IP address</span></span>

<span data-ttu-id="9defc-194">Para fazer algo com a VM recém-criada, é necessário o endereço IP atribuído.</span><span class="sxs-lookup"><span data-stu-id="9defc-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="9defc-195">Os endereços IP são seus próprios recursos do Azure separados, associados aos recursos do Controlador de Interface de Rede (NIC).</span><span class="sxs-lookup"><span data-stu-id="9defc-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

<span data-ttu-id="9defc-196">Esse método se baseia em informações armazenadas no arquivo de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="9defc-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="9defc-197">O código pode consultar a VM diretamente para obter seu NIC, consultar o NIC para obter seu recurso de IP e, em seguida, consultar diretamente o recurso de IP.</span><span class="sxs-lookup"><span data-stu-id="9defc-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="9defc-198">Isso consiste em uma grande cadeia de dependências e operações para resolver, o que se torna caro.</span><span class="sxs-lookup"><span data-stu-id="9defc-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="9defc-199">Como as informações do JSON são locais, elas podem ser carregadas como alternativa.</span><span class="sxs-lookup"><span data-stu-id="9defc-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="9defc-200">O valor para o usuário da VM também é carregado do JSON.</span><span class="sxs-lookup"><span data-stu-id="9defc-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="9defc-201">A senha da VM foi carregada anteriormente do arquivo de autenticação.</span><span class="sxs-lookup"><span data-stu-id="9defc-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9defc-202">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="9defc-202">Next steps</span></span>

<span data-ttu-id="9defc-203">Neste início rápido, você usou um modelo existente e implantou-o por meio da linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="9defc-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="9defc-204">Em seguida, você se conectou à VM recém-criada via SSH.</span><span class="sxs-lookup"><span data-stu-id="9defc-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="9defc-205">Para continuar aprendendo sobre como trabalhar com máquinas virtuais no ambiente do Azure com a linguagem Go, confira os [exemplos de computação do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou os [exemplos de gerenciamento de recursos do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="9defc-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="9defc-206">Para saber mais sobre os métodos de autenticação disponíveis no SDK e para quais tipos de autenticação eles oferecem suporte, consulte [Autenticação com o SDK do Azure para Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="9defc-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
