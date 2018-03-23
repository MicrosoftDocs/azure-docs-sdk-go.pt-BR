---
title: Implantar uma máquina virtual do Azure na linguagem Go
description: Implante uma máquina virtual usando o SDK do Azure para linguagem Go.
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 46a1243ff2ff6bfcf3831e2cea3137c1f6051c78
ms.sourcegitcommit: fcc1786d59d2e32c97a9a8e0748e06f564a961bd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="52b54-103">Início rápido: implantar uma máquina virtual do Azure de um modelo com o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="52b54-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="52b54-104">Este início rápido foca na implantação de recursos de um modelo com o SDK do Azure para linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="52b54-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="52b54-105">Os modelos são instantâneos de todos os recursos contidos em um [grupo de recursos do Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="52b54-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="52b54-106">Ao longo do processo, você vai se familiarizar com a funcionalidade e as convenções do SDK enquanto executa uma tarefa útil.</span><span class="sxs-lookup"><span data-stu-id="52b54-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="52b54-107">No final deste início rápido, você terá uma VM em execução na qual pode entrar com um nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="52b54-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="52b54-108">Se você usa uma instalação local da CLI do Azure, este início rápido requer a versão 2.0.24 da CLI ou posterior.</span><span class="sxs-lookup"><span data-stu-id="52b54-108">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="52b54-109">Execute `az --version` para garantir que a instalação de sua CLI atende a esse requisito.</span><span class="sxs-lookup"><span data-stu-id="52b54-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="52b54-110">Se você precisar instalar ou atualizar, confira [Instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="52b54-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="52b54-111">Instale o SDK do Azure para linguagem Go</span><span class="sxs-lookup"><span data-stu-id="52b54-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="52b54-112">Criar uma entidade de serviço</span><span class="sxs-lookup"><span data-stu-id="52b54-112">Create a service principal</span></span>

<span data-ttu-id="52b54-113">Para efetuar logon em modo não interativo com um aplicativo, você precisa de uma entidade de serviço.</span><span class="sxs-lookup"><span data-stu-id="52b54-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="52b54-114">As entidades de serviço fazem parte do controle de acesso baseado em função (RBAC), que cria uma identidade de usuário exclusiva.</span><span class="sxs-lookup"><span data-stu-id="52b54-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="52b54-115">Para criar uma nova entidade de serviço com a CLI, execute o comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="52b54-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="52b54-116">__Não se esqueça__ de registrar `appId`, `password` e os valores de `tenant` na saída.</span><span class="sxs-lookup"><span data-stu-id="52b54-116">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="52b54-117">Esses valores são usados pelo aplicativo a ser autenticado com o Azure.</span><span class="sxs-lookup"><span data-stu-id="52b54-117">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="52b54-118">Para obter mais informações sobre a criação e o gerenciamento de entidades de serviço com a CLI 2.0 do Azure, confira [Criar uma entidade de serviço com a CLI 2.0 do Azure](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="52b54-118">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="52b54-119">Obter o código</span><span class="sxs-lookup"><span data-stu-id="52b54-119">Get the code</span></span>

<span data-ttu-id="52b54-120">Obtenha o código de início rápido e todas as suas dependências com `go get`.</span><span class="sxs-lookup"><span data-stu-id="52b54-120">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="52b54-121">Esse código compila, mas não funciona corretamente até que você forneça informações sobre sua conta do Azure e a entidade de serviço criada.</span><span class="sxs-lookup"><span data-stu-id="52b54-121">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="52b54-122">Em `main.go` há uma variável, `config`, que contém um struct `authInfo`.</span><span class="sxs-lookup"><span data-stu-id="52b54-122">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="52b54-123">Esse struct precisa ter seus valores de campo substituídos para autenticar corretamente.</span><span class="sxs-lookup"><span data-stu-id="52b54-123">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="52b54-124">`SubscriptionID`: a ID da assinatura, que pode ser obtida no comando CLI</span><span class="sxs-lookup"><span data-stu-id="52b54-124">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="52b54-125">`TenantID`: A ID do locatário, o valor de `tenant` registrado ao criar a entidade de serviço</span><span class="sxs-lookup"><span data-stu-id="52b54-125">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="52b54-126">`ServicePrincipalID`: O valor de `appId` registrado ao criar a entidade de serviço</span><span class="sxs-lookup"><span data-stu-id="52b54-126">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="52b54-127">`ServicePrincipalSecret`: O valor de `password` registrado ao criar a entidade de serviço</span><span class="sxs-lookup"><span data-stu-id="52b54-127">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="52b54-128">Você também precisa editar um valor no arquivo `vm-quickstart-params.json`.</span><span class="sxs-lookup"><span data-stu-id="52b54-128">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="52b54-129">`vm_password`: A senha para a conta de usuário da VM.</span><span class="sxs-lookup"><span data-stu-id="52b54-129">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="52b54-130">Ela deve ter de 12 a 72 caracteres de comprimento e conter 3 dos seguintes caracteres:</span><span class="sxs-lookup"><span data-stu-id="52b54-130">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="52b54-131">Uma letra minúscula</span><span class="sxs-lookup"><span data-stu-id="52b54-131">A lowercase letter</span></span>
  * <span data-ttu-id="52b54-132">Uma letra maiúscula</span><span class="sxs-lookup"><span data-stu-id="52b54-132">An uppercase letter</span></span>
  * <span data-ttu-id="52b54-133">Um número</span><span class="sxs-lookup"><span data-stu-id="52b54-133">A number</span></span>
  * <span data-ttu-id="52b54-134">Um símbolo</span><span class="sxs-lookup"><span data-stu-id="52b54-134">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="52b54-135">Executando o código</span><span class="sxs-lookup"><span data-stu-id="52b54-135">Running the code</span></span>

<span data-ttu-id="52b54-136">Execute o início rápido com o comando `go run`.</span><span class="sxs-lookup"><span data-stu-id="52b54-136">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="52b54-137">Se houver uma falha na implantação, você recebe uma mensagem indicando que houve um problema, mas sem quaisquer detalhes específicos.</span><span class="sxs-lookup"><span data-stu-id="52b54-137">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="52b54-138">Usando a CLI do Azure, você obtém os detalhes da falha de implantação com o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="52b54-138">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="52b54-139">Se a implantação for bem-sucedida, você receberá uma mensagem fornecendo o nome de usuário, o endereço IP e a senha para fazer logon na máquina virtual recém-criada.</span><span class="sxs-lookup"><span data-stu-id="52b54-139">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="52b54-140">Use SSH neste computador para confirmar se ele está em execução.</span><span class="sxs-lookup"><span data-stu-id="52b54-140">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="52b54-141">Limpando</span><span class="sxs-lookup"><span data-stu-id="52b54-141">Cleaning up</span></span>

<span data-ttu-id="52b54-142">Limpe todos os recursos criados nesse início rápido excluindo o grupo de recursos com a CLI.</span><span class="sxs-lookup"><span data-stu-id="52b54-142">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="52b54-143">Código em detalhes</span><span class="sxs-lookup"><span data-stu-id="52b54-143">Code in depth</span></span>

<span data-ttu-id="52b54-144">O que o código de início rápido faz é dividido em um bloco de variáveis e várias funções pequenas, que serão uma a uma discutidas aqui.</span><span class="sxs-lookup"><span data-stu-id="52b54-144">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="52b54-145">Structs e atribuições de variável</span><span class="sxs-lookup"><span data-stu-id="52b54-145">Variable assignments and structs</span></span>

<span data-ttu-id="52b54-146">Como o início rápido é independente, ele usa variáveis globais, em vez de opções de linha de comando ou variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="52b54-146">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="52b54-147">O struct `authInfo` é declarado para encapsular todas as informações necessárias para autorização com os serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="52b54-147">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

<span data-ttu-id="52b54-148">Os valores são declarados, fornecendo os nomes dos recursos criados.</span><span class="sxs-lookup"><span data-stu-id="52b54-148">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="52b54-149">O local também é especificado aqui, que pode ser alterado para ver como as implantações se comportam em outros data centers.</span><span class="sxs-lookup"><span data-stu-id="52b54-149">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="52b54-150">Nem todo datacenter tem todos os recursos necessários disponíveis.</span><span class="sxs-lookup"><span data-stu-id="52b54-150">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="52b54-151">As constantes `templateFile` e `parametersFile` apontam para os arquivos necessários para a implantação.</span><span class="sxs-lookup"><span data-stu-id="52b54-151">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="52b54-152">O token da entidade de serviço é abordado posteriormente, e a variável `ctx` é um [contexto Go](https://blog.golang.org/context) para as operações de rede.</span><span class="sxs-lookup"><span data-stu-id="52b54-152">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="52b54-153">init() e autorização</span><span class="sxs-lookup"><span data-stu-id="52b54-153">init() and authorization</span></span>

<span data-ttu-id="52b54-154">O método `init()` para o código configura a autorização.</span><span class="sxs-lookup"><span data-stu-id="52b54-154">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="52b54-155">Como a autorização é uma pré-condição para tudo no início rápido, faz sentido tê-la como parte da inicialização.</span><span class="sxs-lookup"><span data-stu-id="52b54-155">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

<span data-ttu-id="52b54-156">Esse código conclui duas etapas da autorização:</span><span class="sxs-lookup"><span data-stu-id="52b54-156">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="52b54-157">As informações de configuração do OAuth para o `TenantID` são recuperadas por meio de uma interface com o Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="52b54-157">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="52b54-158">O objeto [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) contém os pontos de extremidade usados na configuração padrão do Azure.</span><span class="sxs-lookup"><span data-stu-id="52b54-158">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="52b54-159">A função [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) é chamada.</span><span class="sxs-lookup"><span data-stu-id="52b54-159">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="52b54-160">Essa função usa as informações do OAuth junto com o logon da entidade de serviço, bem como o estilo de gerenciamento do Azure que está sendo usado.</span><span class="sxs-lookup"><span data-stu-id="52b54-160">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="52b54-161">A menos que você tenha os requisitos específicos e saiba o que está fazendo, esse valor deve ser sempre `.ResourceManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="52b54-161">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="52b54-162">Fluxo de operações em main()</span><span class="sxs-lookup"><span data-stu-id="52b54-162">Flow of operations in main()</span></span>

<span data-ttu-id="52b54-163">A função `main()` é simples, apenas indica o fluxo das operações e executa a verificação de erros.</span><span class="sxs-lookup"><span data-stu-id="52b54-163">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

<span data-ttu-id="52b54-164">As etapas que o código executa são, em ordem:</span><span class="sxs-lookup"><span data-stu-id="52b54-164">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="52b54-165">Criar o grupo de recursos para implantar em (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="52b54-165">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="52b54-166">Criar a implantação dentro deste grupo (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="52b54-166">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="52b54-167">Obter e exibir as informações de logon da VM implantada (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="52b54-167">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="52b54-168">Criando o grupo de recursos</span><span class="sxs-lookup"><span data-stu-id="52b54-168">Creating the resource group</span></span>

<span data-ttu-id="52b54-169">A função `createGroup()` cria o grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="52b54-169">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="52b54-170">Ao observar o fluxo de chamadas e os argumentos vemos a maneira como as interações de serviço são estruturadas no SDK.</span><span class="sxs-lookup"><span data-stu-id="52b54-170">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="52b54-171">O fluxo geral de interação com um serviço do Azure é:</span><span class="sxs-lookup"><span data-stu-id="52b54-171">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="52b54-172">Criar o cliente usando o método `service.NewXClient()`, no qual `X` é o tipo de recurso do `service` com o qual você deseja interagir.</span><span class="sxs-lookup"><span data-stu-id="52b54-172">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="52b54-173">Essa função sempre usa uma ID de assinatura.</span><span class="sxs-lookup"><span data-stu-id="52b54-173">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="52b54-174">Defina o método de autorização para o cliente, permitindo que ele interaja com a API remota.</span><span class="sxs-lookup"><span data-stu-id="52b54-174">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="52b54-175">Realize o método de chamada no cliente correspondente à API remota.</span><span class="sxs-lookup"><span data-stu-id="52b54-175">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="52b54-176">Os métodos de serviço do cliente geralmente levam o nome do recurso e um objeto de metadados.</span><span class="sxs-lookup"><span data-stu-id="52b54-176">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="52b54-177">A função [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) é usada aqui para executar uma conversão de tipo.</span><span class="sxs-lookup"><span data-stu-id="52b54-177">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="52b54-178">Os structs de parâmetros para métodos do SDK usam ponteiros quase exclusivamente, portanto, esses métodos são fornecidos para facilitar as conversões de tipo.</span><span class="sxs-lookup"><span data-stu-id="52b54-178">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="52b54-179">Confira a documentação do módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obter a lista completa e o comportamento dos conversores de conveniência.</span><span class="sxs-lookup"><span data-stu-id="52b54-179">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="52b54-180">A operação `groupsClient.CreateOrUpdate()` retorna um ponteiro para um struct de dados que representa o grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="52b54-180">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="52b54-181">Um valor de retorno direto desse tipo indica uma operação de execução curta que deve ser síncrona.</span><span class="sxs-lookup"><span data-stu-id="52b54-181">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="52b54-182">Na próxima seção, você verá um exemplo de uma operação de longa execução e como interagir com ela.</span><span class="sxs-lookup"><span data-stu-id="52b54-182">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="52b54-183">Executando a implantação</span><span class="sxs-lookup"><span data-stu-id="52b54-183">Performing the deployment</span></span>

<span data-ttu-id="52b54-184">Depois de criar o grupo para conter seus recursos, é hora de executar a implantação.</span><span class="sxs-lookup"><span data-stu-id="52b54-184">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="52b54-185">Esse código é dividido em seções menores para enfatizar diferentes partes de sua lógica.</span><span class="sxs-lookup"><span data-stu-id="52b54-185">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

        // ...
```

<span data-ttu-id="52b54-186">Os arquivos de implantação são carregados por `readJSON`, cujos detalhes são ignorados aqui.</span><span class="sxs-lookup"><span data-stu-id="52b54-186">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="52b54-187">Essa função retorna um `*map[string]interface{}`, o tipo usado para construir os metadados para a chamada de implantação de recursos.</span><span class="sxs-lookup"><span data-stu-id="52b54-187">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

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
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

<span data-ttu-id="52b54-188">Esse código segue o mesmo padrão da criação do grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="52b54-188">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="52b54-189">Um novo cliente é criado, considerando a capacidade de autenticar com o Azure e, em seguida, um método é chamado.</span><span class="sxs-lookup"><span data-stu-id="52b54-189">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="52b54-190">O método tem até o mesmo nome (`CreateOrUpdate`) que o método correspondente dos grupos de recurso.</span><span class="sxs-lookup"><span data-stu-id="52b54-190">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="52b54-191">Esse padrão é visto repetidamente no SDK.</span><span class="sxs-lookup"><span data-stu-id="52b54-191">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="52b54-192">Os métodos que executam um trabalho semelhante, normalmente têm o mesmo nome.</span><span class="sxs-lookup"><span data-stu-id="52b54-192">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="52b54-193">A maior diferença é o valor de retorno do método `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="52b54-193">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="52b54-194">Esse valor é um objeto `Future`, que segue o [padrão de design do objeto Future](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="52b54-194">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="52b54-195">Os objetos Future representam uma operação de longa execução no Azure que talvez você queira sondar ocasionalmente durante a execução de outro trabalho.</span><span class="sxs-lookup"><span data-stu-id="52b54-195">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="52b54-196">Neste exemplo, a melhor coisa a fazer é aguardar a conclusão da operação.</span><span class="sxs-lookup"><span data-stu-id="52b54-196">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="52b54-197">Aguardar um objeto Future requer um [objeto de contexto](https://blog.golang.org/context) e o cliente que criou o objeto Future.</span><span class="sxs-lookup"><span data-stu-id="52b54-197">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="52b54-198">Há duas fontes de erro possíveis: um erro causado por parte do cliente durante a tentativa de invocar o método e uma resposta de erro do servidor.</span><span class="sxs-lookup"><span data-stu-id="52b54-198">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="52b54-199">O segundo é retornado como parte da chamada `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="52b54-199">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="52b54-200">Obtendo o endereço IP atribuído</span><span class="sxs-lookup"><span data-stu-id="52b54-200">Obtaining the assigned IP address</span></span>

<span data-ttu-id="52b54-201">Para fazer algo com a VM recém-criada, é necessário o endereço IP atribuído.</span><span class="sxs-lookup"><span data-stu-id="52b54-201">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="52b54-202">Os endereços IP são seus próprios recursos do Azure separados, associados aos recursos do Controlador de Interface de Rede (NIC).</span><span class="sxs-lookup"><span data-stu-id="52b54-202">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

<span data-ttu-id="52b54-203">Esse método se baseia em informações armazenadas no arquivo de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="52b54-203">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="52b54-204">O código pode consultar a VM diretamente para obter seu NIC, consultar o NIC para obter seu recurso de IP e, em seguida, consultar diretamente o recurso de IP.</span><span class="sxs-lookup"><span data-stu-id="52b54-204">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="52b54-205">Isso consiste em uma grande cadeia de dependências e operações para resolver, o que se torna caro.</span><span class="sxs-lookup"><span data-stu-id="52b54-205">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="52b54-206">Como as informações do JSON são locais, elas podem ser carregadas como alternativa.</span><span class="sxs-lookup"><span data-stu-id="52b54-206">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="52b54-207">Da mesma forma, os valores para o usuário da VM e a senha são carregados do JSON.</span><span class="sxs-lookup"><span data-stu-id="52b54-207">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52b54-208">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="52b54-208">Next steps</span></span>

<span data-ttu-id="52b54-209">Neste início rápido, você usou um modelo existente e implantou-o por meio da linguagem Go.</span><span class="sxs-lookup"><span data-stu-id="52b54-209">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="52b54-210">Em seguida, você se conectou à VM recém-criada via SSH para garantir sua execução.</span><span class="sxs-lookup"><span data-stu-id="52b54-210">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="52b54-211">Para continuar aprendendo sobre como trabalhar com máquinas virtuais no ambiente do Azure com a linguagem Go, confira os [exemplos de computação do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou os [exemplos de gerenciamento de recursos do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="52b54-211">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
