---
title: Implantar uma máquina virtual do Azure na linguagem Go
description: Implante uma máquina virtual usando o SDK do Azure para linguagem Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="b8b84-103">Início rápido: implantar uma máquina virtual do Azure de um modelo com o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="b8b84-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="b8b84-104">Este início rápido foca na implantação de recursos de um modelo com o SDK do Azure para linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="b8b84-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="b8b84-105">Os modelos são instantâneos de todos os recursos contidos em um [grupo de recursos do Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="b8b84-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="b8b84-106">Ao longo do processo, você vai se familiarizar com a funcionalidade e as convenções do SDK enquanto executa uma tarefa útil.</span><span class="sxs-lookup"><span data-stu-id="b8b84-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="b8b84-107">No final deste início rápido, você terá uma VM em execução na qual pode entrar com um nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="b8b84-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="b8b84-108">Caso use uma instalação local da CLI do Azure, este início rápido requer a CLI versão __2.0.28__ ou posterior.</span><span class="sxs-lookup"><span data-stu-id="b8b84-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="b8b84-109">Execute `az --version` para garantir que a instalação de sua CLI atende a esse requisito.</span><span class="sxs-lookup"><span data-stu-id="b8b84-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="b8b84-110">Se você precisar instalar ou atualizar, confira [Instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b8b84-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="b8b84-111">Instale o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="b8b84-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="b8b84-112">Criar uma entidade de serviço</span><span class="sxs-lookup"><span data-stu-id="b8b84-112">Create a service principal</span></span>


<span data-ttu-id="b8b84-113">Para efetuar logon em modo não interativo com um aplicativo, você precisa de uma entidade de serviço.</span><span class="sxs-lookup"><span data-stu-id="b8b84-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="b8b84-114">As entidades de serviço fazem parte do controle de acesso baseado em função (RBAC), que cria uma identidade de usuário exclusiva.</span><span class="sxs-lookup"><span data-stu-id="b8b84-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="b8b84-115">Para criar uma nova entidade de serviço com a CLI, execute o comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="b8b84-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="b8b84-116">Defina a variável de ambiente `AZURE_AUTH_LOCATION` para ser o caminho completo para esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="b8b84-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="b8b84-117">Em seguida, o SDK localiza e lê as credenciais diretamente desse arquivo, sem que você precise fazer quaisquer alterações ou registrar informações da entidade de serviço.</span><span class="sxs-lookup"><span data-stu-id="b8b84-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="b8b84-118">Obter o código</span><span class="sxs-lookup"><span data-stu-id="b8b84-118">Get the code</span></span>

<span data-ttu-id="b8b84-119">Obtenha o código de início rápido e todas as suas dependências com `go get`.</span><span class="sxs-lookup"><span data-stu-id="b8b84-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="b8b84-120">Você não precisa fazer nenhuma modificação no código-fonte se a variável `AZURE_AUTH_LOCATION` estiver definida corretamente.</span><span class="sxs-lookup"><span data-stu-id="b8b84-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="b8b84-121">Quando o programa for executado, ele carregará todas as informações de autenticação necessárias de lá.</span><span class="sxs-lookup"><span data-stu-id="b8b84-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="b8b84-122">Executando o código</span><span class="sxs-lookup"><span data-stu-id="b8b84-122">Running the code</span></span>

<span data-ttu-id="b8b84-123">Execute o início rápido com o comando `go run`.</span><span class="sxs-lookup"><span data-stu-id="b8b84-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="b8b84-124">Se houver uma falha na implantação, você receberá uma mensagem indicando que houve um problema, mas ela pode não apresentar detalhes suficientes.</span><span class="sxs-lookup"><span data-stu-id="b8b84-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="b8b84-125">Usando a CLI do Azure, você obtém todos os detalhes da falha de implantação com o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="b8b84-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="b8b84-126">Se a implantação for bem-sucedida, você receberá uma mensagem fornecendo o nome de usuário, o endereço IP e a senha para fazer logon na máquina virtual recém-criada.</span><span class="sxs-lookup"><span data-stu-id="b8b84-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="b8b84-127">Use SSH neste computador para confirmar se ele está em execução.</span><span class="sxs-lookup"><span data-stu-id="b8b84-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="b8b84-128">Limpando</span><span class="sxs-lookup"><span data-stu-id="b8b84-128">Cleaning up</span></span>

<span data-ttu-id="b8b84-129">Limpe todos os recursos criados nesse início rápido excluindo o grupo de recursos com a CLI.</span><span class="sxs-lookup"><span data-stu-id="b8b84-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="b8b84-130">Código em detalhes</span><span class="sxs-lookup"><span data-stu-id="b8b84-130">Code in depth</span></span>

<span data-ttu-id="b8b84-131">O que o código de início rápido faz é dividido em um bloco de variáveis e várias funções pequenas, que serão uma a uma discutidas aqui.</span><span class="sxs-lookup"><span data-stu-id="b8b84-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="b8b84-132">Variáveis, constantes e tipos</span><span class="sxs-lookup"><span data-stu-id="b8b84-132">Variables, constants, and types</span></span>

<span data-ttu-id="b8b84-133">Uma vez que o início rápido é autossuficiente, ele usa variáveis e constantes globais.</span><span class="sxs-lookup"><span data-stu-id="b8b84-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="b8b84-134">Os valores são declarados, fornecendo os nomes dos recursos criados.</span><span class="sxs-lookup"><span data-stu-id="b8b84-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="b8b84-135">O local também é especificado aqui, que pode ser alterado para ver como as implantações se comportam em outros data centers.</span><span class="sxs-lookup"><span data-stu-id="b8b84-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="b8b84-136">Nem todo datacenter tem todos os recursos necessários disponíveis.</span><span class="sxs-lookup"><span data-stu-id="b8b84-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="b8b84-137">O tipo `clientInfo` é projetado para encapsular todas as informações que devem ser carregadas de forma independente do arquivo de autenticação para configurar clientes no SDK e definir a senha da VM.</span><span class="sxs-lookup"><span data-stu-id="b8b84-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="b8b84-138">As constantes `templateFile` e `parametersFile` apontam para os arquivos necessários para a implantação.</span><span class="sxs-lookup"><span data-stu-id="b8b84-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="b8b84-139">O `authorizer` será configurados pelo SDK do Go para autenticação, e a variável `ctx` é um [contexto do Go](https://blog.golang.org/context) para as operações de rede.</span><span class="sxs-lookup"><span data-stu-id="b8b84-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="b8b84-140">Autenticação e inicialização</span><span class="sxs-lookup"><span data-stu-id="b8b84-140">Authentication and initialization</span></span>

<span data-ttu-id="b8b84-141">A função `init` configura a autenticação.</span><span class="sxs-lookup"><span data-stu-id="b8b84-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="b8b84-142">Uma vez que a autorização é uma pré-condição para tudo no início rápido, faz sentido que ela seja parte da inicialização.</span><span class="sxs-lookup"><span data-stu-id="b8b84-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="b8b84-143">Ele também carrega algumas informações necessárias do arquivo de autenticação para configurar os clientes e a VM.</span><span class="sxs-lookup"><span data-stu-id="b8b84-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="b8b84-144">Primeiro, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) é chamado para carregar as informações de autenticação do arquivo localizado em `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="b8b84-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="b8b84-145">Em seguida, esse arquivo é carregado manualmente pela função `readJSON` (omitida aqui) para extrair os dois valores necessários para executar o restante do programa: a ID de assinatura do cliente e o segredo da entidade de serviço, que também é usado para a senha da VM.</span><span class="sxs-lookup"><span data-stu-id="b8b84-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="b8b84-146">Para simplificar o início rápido, a senha da entidade de serviço é reutilizada.</span><span class="sxs-lookup"><span data-stu-id="b8b84-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="b8b84-147">Na produção, tome cuidado para __nunca__ reutilizar uma senha que forneça acesso aos recursos do Azure.</span><span class="sxs-lookup"><span data-stu-id="b8b84-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="b8b84-148">Fluxo de operações em main()</span><span class="sxs-lookup"><span data-stu-id="b8b84-148">Flow of operations in main()</span></span>

<span data-ttu-id="b8b84-149">A função `main` é simples, apenas indica o fluxo das operações e executa a verificação de erros.</span><span class="sxs-lookup"><span data-stu-id="b8b84-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="b8b84-150">As etapas que o código executa são, em ordem:</span><span class="sxs-lookup"><span data-stu-id="b8b84-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="b8b84-151">Criar o grupo de recursos para implantar em (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="b8b84-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="b8b84-152">Criar a implantação dentro deste grupo (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="b8b84-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="b8b84-153">Obter e exibir as informações de logon da VM implantada (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="b8b84-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="b8b84-154">Criando o grupo de recursos</span><span class="sxs-lookup"><span data-stu-id="b8b84-154">Creating the resource group</span></span>

<span data-ttu-id="b8b84-155">A função `createGroup` cria o grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="b8b84-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="b8b84-156">Ao observar o fluxo de chamadas e os argumentos vemos a maneira como as interações de serviço são estruturadas no SDK.</span><span class="sxs-lookup"><span data-stu-id="b8b84-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="b8b84-157">O fluxo geral de interação com um serviço do Azure é:</span><span class="sxs-lookup"><span data-stu-id="b8b84-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="b8b84-158">Criar o cliente usando o método `service.New*Client()`, no qual `*` é o tipo de recurso do `service` com o qual você deseja interagir.</span><span class="sxs-lookup"><span data-stu-id="b8b84-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="b8b84-159">Essa função sempre usa uma ID de assinatura.</span><span class="sxs-lookup"><span data-stu-id="b8b84-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="b8b84-160">Defina o método de autorização para o cliente, permitindo que ele interaja com a API remota.</span><span class="sxs-lookup"><span data-stu-id="b8b84-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="b8b84-161">Realize o método de chamada no cliente correspondente à API remota.</span><span class="sxs-lookup"><span data-stu-id="b8b84-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="b8b84-162">Os métodos de serviço do cliente geralmente levam o nome do recurso e um objeto de metadados.</span><span class="sxs-lookup"><span data-stu-id="b8b84-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="b8b84-163">A função [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) é usada aqui para executar uma conversão de tipo.</span><span class="sxs-lookup"><span data-stu-id="b8b84-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="b8b84-164">Os parâmetros para métodos do SDK usam ponteiros quase exclusivamente, portanto, métodos de conveniência são fornecidos para facilitar as conversões de tipo.</span><span class="sxs-lookup"><span data-stu-id="b8b84-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="b8b84-165">Confira a documentação do módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obter a lista completa de conversores de conveniência e seu comportamento.</span><span class="sxs-lookup"><span data-stu-id="b8b84-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="b8b84-166">O método `groupsClient.CreateOrUpdate` retorna um ponteiro para um tipo de dados que representa o grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="b8b84-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="b8b84-167">Um valor de retorno direto desse tipo indica uma operação de execução curta que deve ser síncrona.</span><span class="sxs-lookup"><span data-stu-id="b8b84-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="b8b84-168">Na próxima seção, você verá um exemplo de uma operação de longa execução e como interagir com ela.</span><span class="sxs-lookup"><span data-stu-id="b8b84-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="b8b84-169">Executando a implantação</span><span class="sxs-lookup"><span data-stu-id="b8b84-169">Performing the deployment</span></span>

<span data-ttu-id="b8b84-170">Depois de criar o grupo de recursos, deve-se executar a implantação.</span><span class="sxs-lookup"><span data-stu-id="b8b84-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="b8b84-171">Esse código é dividido em seções menores para enfatizar diferentes partes de sua lógica.</span><span class="sxs-lookup"><span data-stu-id="b8b84-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="b8b84-172">Os arquivos de implantação são carregados por `readJSON`, cujos detalhes são ignorados aqui.</span><span class="sxs-lookup"><span data-stu-id="b8b84-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="b8b84-173">Essa função retorna um `*map[string]interface{}`, o tipo usado para construir os metadados para a chamada de implantação de recursos.</span><span class="sxs-lookup"><span data-stu-id="b8b84-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="b8b84-174">A senha da VM também é definida manualmente nos parâmetros de implantação.</span><span class="sxs-lookup"><span data-stu-id="b8b84-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="b8b84-175">Esse código segue o mesmo padrão da criação do grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="b8b84-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="b8b84-176">Um novo cliente é criado, considerando a capacidade de autenticar com o Azure e, em seguida, um método é chamado.</span><span class="sxs-lookup"><span data-stu-id="b8b84-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="b8b84-177">O método tem até o mesmo nome (`CreateOrUpdate`) que o método correspondente dos grupos de recurso.</span><span class="sxs-lookup"><span data-stu-id="b8b84-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="b8b84-178">Esse padrão é visto em todo o SDK.</span><span class="sxs-lookup"><span data-stu-id="b8b84-178">This pattern is seen throughout the SDK.</span></span> <span data-ttu-id="b8b84-179">Os métodos que executam um trabalho semelhante, normalmente têm o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="b8b84-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="b8b84-180">A maior diferença é o valor de retorno do método `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="b8b84-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="b8b84-181">Esse valor é do tipo [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), que segue o [padrão de design do Future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="b8b84-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="b8b84-182">Valores Futures representam uma operação de longa execução no Azure que você pode pesquisar, cancelar ou bloquear ao ser concluída.</span><span class="sxs-lookup"><span data-stu-id="b8b84-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

<span data-ttu-id="b8b84-183">Neste exemplo, a melhor coisa a fazer é aguardar a conclusão da operação.</span><span class="sxs-lookup"><span data-stu-id="b8b84-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="b8b84-184">Aguardar um valor Future requer um [objeto de contexto](https://blog.golang.org/context) e o cliente que criou o `Future`.</span><span class="sxs-lookup"><span data-stu-id="b8b84-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="b8b84-185">Há duas fontes de erro possíveis: um erro causado por parte do cliente durante a tentativa de invocar o método e uma resposta de erro do servidor.</span><span class="sxs-lookup"><span data-stu-id="b8b84-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="b8b84-186">O segundo é retornado como parte da chamada `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="b8b84-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="b8b84-187">Assim que as informações de implantação forem recuperadas, há uma solução alternativa para possíveis bugs em que as informações de implantação podem estar vazias: fazer uma chamada manual para `deploymentsClient.Get` para garantir que os dados sejam preenchidos.</span><span class="sxs-lookup"><span data-stu-id="b8b84-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="b8b84-188">Obtendo o endereço IP atribuído</span><span class="sxs-lookup"><span data-stu-id="b8b84-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="b8b84-189">Para fazer algo com a VM recém-criada, é necessário o endereço IP atribuído.</span><span class="sxs-lookup"><span data-stu-id="b8b84-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="b8b84-190">Os endereços IP são seus próprios recursos do Azure separados, associados aos recursos do Controlador de Interface de Rede (NIC).</span><span class="sxs-lookup"><span data-stu-id="b8b84-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="b8b84-191">Esse método se baseia em informações armazenadas no arquivo de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="b8b84-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="b8b84-192">O código pode consultar a VM diretamente para obter seu NIC, consultar o NIC para obter seu recurso de IP e, em seguida, consultar diretamente o recurso de IP.</span><span class="sxs-lookup"><span data-stu-id="b8b84-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="b8b84-193">Isso consiste em uma grande cadeia de dependências e operações para resolver, o que se torna caro.</span><span class="sxs-lookup"><span data-stu-id="b8b84-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="b8b84-194">Como as informações do JSON são locais, elas podem ser carregadas como alternativa.</span><span class="sxs-lookup"><span data-stu-id="b8b84-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="b8b84-195">O valor para o usuário da VM também é carregado do JSON.</span><span class="sxs-lookup"><span data-stu-id="b8b84-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="b8b84-196">A senha da VM foi carregada anteriormente do arquivo de autenticação.</span><span class="sxs-lookup"><span data-stu-id="b8b84-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8b84-197">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="b8b84-197">Next steps</span></span>

<span data-ttu-id="b8b84-198">Neste início rápido, você usou um modelo existente e implantou-o por meio da linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="b8b84-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="b8b84-199">Em seguida, você se conectou à VM recém-criada via SSH para garantir sua execução.</span><span class="sxs-lookup"><span data-stu-id="b8b84-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="b8b84-200">Para continuar aprendendo sobre como trabalhar com máquinas virtuais no ambiente do Azure com a linguagem Go, confira os [exemplos de computação do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou os [exemplos de gerenciamento de recursos do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="b8b84-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="b8b84-201">Para saber mais sobre os métodos de autenticação disponíveis no SDK e para quais tipos de autenticação eles oferecem suporte, consulte [Autenticação com o SDK do Azure para Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="b8b84-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
