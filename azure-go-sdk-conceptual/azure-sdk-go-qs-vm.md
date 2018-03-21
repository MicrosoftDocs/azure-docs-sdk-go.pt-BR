---
title: "Implantar uma máquina virtual do Azure na linguagem Go"
description: "Implante uma máquina virtual usando o SDK do Azure para linguagem Go."
keywords: "Azure, máquina virtual, vm, go, golang, sdk do azure"
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Início rápido: implantar uma máquina virtual do Azure de um modelo com o SDK do Azure para linguagem Go

Este início rápido foca na implantação de recursos de um modelo com o SDK do Azure para linguagem Go. Os modelos são instantâneos de todos os recursos contidos em um [grupo de recursos do Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). Ao longo do processo, você vai se familiarizar com a funcionalidade e as convenções do SDK enquanto executa uma tarefa útil.

No final deste início rápido, você terá uma VM em execução na qual pode entrar com um nome de usuário e senha.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Se você usa uma instalação local da CLI do Azure, este início rápido requer a versão 2.0.24 da CLI ou posterior. Execute `az --version` para garantir que a instalação de sua CLI atende a esse requisito. Se você precisar instalar ou atualizar, confira [Instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Instale o SDK do Azure para linguagem Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Criar uma entidade de serviço

Para efetuar logon em modo não interativo com um aplicativo, você precisa de uma entidade de serviço. As entidades de serviço fazem parte do controle de acesso baseado em função (RBAC), que cria uma identidade de usuário exclusiva. Para criar uma nova entidade de serviço com a CLI, execute o comando a seguir:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Não se esqueça__ de registrar `appId`, `password` e os valores de `tenant` na saída. Esses valores são usados pelo aplicativo a ser autenticado com o Azure.

Para obter mais informações sobre a criação e o gerenciamento de entidades de serviço com a CLI 2.0 do Azure, confira [Criar uma entidade de serviço com a CLI 2.0 do Azure](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>Obter o código

Obtenha o código de início rápido e todas as suas dependências com `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Esse código compila, mas não funciona corretamente até que você forneça informações sobre sua conta do Azure e a entidade de serviço criada. Em `main.go` há uma variável, `config`, que contém um struct `authInfo`. Esse struct precisa ter seus valores de campo substituídos para autenticar corretamente.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: a ID da assinatura, que pode ser obtida no comando CLI

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: A ID do locatário, o valor de `tenant` registrado ao criar a entidade de serviço
* `ServicePrincipalID`: O valor de `appId` registrado ao criar a entidade de serviço
* `ServicePrincipalSecret`: O valor de `password` registrado ao criar a entidade de serviço

Você também precisa editar um valor no arquivo `vm-quickstart-params.json`.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: A senha para a conta de usuário da VM. Ela deve ter de 12 a 72 caracteres de comprimento e conter 3 dos seguintes caracteres:
  * Uma letra minúscula
  * Uma letra maiúscula
  * Um número
  * Um símbolo

## <a name="running-the-code"></a>Executando o código

Execute o início rápido com o comando `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Se houver uma falha na implantação, você recebe uma mensagem indicando que houve um problema, mas sem quaisquer detalhes específicos. Usando a CLI do Azure, você obtém os detalhes da falha de implantação com o seguinte comando:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Se a implantação for bem-sucedida, você receberá uma mensagem fornecendo o nome de usuário, o endereço IP e a senha para fazer logon na máquina virtual recém-criada. Use SSH neste computador para confirmar se ele está em execução.

## <a name="cleaning-up"></a>Limpando

Limpe todos os recursos criados nesse início rápido excluindo o grupo de recursos com a CLI.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>Código em detalhes

O que o código de início rápido faz é dividido em um bloco de variáveis e várias funções pequenas, que serão uma a uma discutidas aqui.

### <a name="variable-assignments-and-structs"></a>Structs e atribuições de variável

Como o início rápido é independente, ele usa variáveis globais, em vez de opções de linha de comando ou variáveis de ambiente.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

O struct `authInfo` é declarado para encapsular todas as informações necessárias para autorização com os serviços do Azure.

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

Os valores são declarados, fornecendo os nomes dos recursos criados. O local também é especificado aqui, que pode ser alterado para ver como as implantações se comportam em outros data centers. Nem todo datacenter tem todos os recursos necessários disponíveis.

As constantes `templateFile` e `parametersFile` apontam para os arquivos necessários para a implantação. O token da entidade de serviço é abordado posteriormente, e a variável `ctx` é um [contexto Go](https://blog.golang.org/context) para as operações de rede.

### <a name="init-and-authorization"></a>init() e autorização

O método `init()` para o código configura a autorização. Como a autorização é uma pré-condição para tudo no início rápido, faz sentido tê-la como parte da inicialização. 

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

Esse código conclui duas etapas da autorização:

* As informações de configuração do OAuth para o `TenantID` são recuperadas por meio de uma interface com o Azure Active Directory. O objeto [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) contém os pontos de extremidade usados na configuração padrão do Azure.
* A função [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) é chamada. Essa função usa as informações do OAuth junto com o logon da entidade de serviço, bem como o estilo de gerenciamento do Azure que está sendo usado. A menos que você tenha os requisitos específicos e saiba o que está fazendo, esse valor deve ser sempre `.ResourceManagerEndpoint`.

### <a name="flow-of-operations-in-main"></a>Fluxo de operações em main()

A função `main()` é simples, apenas indica o fluxo das operações e executa a verificação de erros.

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

As etapas que o código executa são, em ordem:

* Criar o grupo de recursos para implantar em (`createGroup()`)
* Criar a implantação dentro deste grupo (`createDeployment()`)
* Obter e exibir as informações de logon da VM implantada (`getLogin()`)

### <a name="creating-the-resource-group"></a>Criando o grupo de recursos

A função `createGroup()` cria o grupo de recursos. Ao observar o fluxo de chamadas e os argumentos vemos a maneira como as interações de serviço são estruturadas no SDK.

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

O fluxo geral de interação com um serviço do Azure é:

* Criar o cliente usando o método `service.NewXClient()`, no qual `X` é o tipo de recurso do `service` com o qual você deseja interagir. Essa função sempre usa uma ID de assinatura.
* Defina o método de autorização para o cliente, permitindo que ele interaja com a API remota.
* Realize o método de chamada no cliente correspondente à API remota. Os métodos de serviço do cliente geralmente levam o nome do recurso e um objeto de metadados.

A função [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) é usada aqui para executar uma conversão de tipo. Os structs de parâmetros para métodos do SDK usam ponteiros quase exclusivamente, portanto, esses métodos são fornecidos para facilitar as conversões de tipo. Confira a documentação do módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obter a lista completa e o comportamento dos conversores de conveniência.

A operação `groupsClient.CreateOrUpdate()` retorna um ponteiro para um struct de dados que representa o grupo de recursos. Um valor de retorno direto desse tipo indica uma operação de execução curta que deve ser síncrona. Na próxima seção, você verá um exemplo de uma operação de longa execução e como interagir com ela.

### <a name="performing-the-deployment"></a>Executando a implantação

Depois de criar o grupo para conter seus recursos, é hora de executar a implantação. Esse código é dividido em seções menores para enfatizar diferentes partes de sua lógica.


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

Os arquivos de implantação são carregados por `readJSON`, cujos detalhes são ignorados aqui. Essa função retorna um `*map[string]interface{}`, o tipo usado para construir os metadados para a chamada de implantação de recursos.

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

Esse código segue o mesmo padrão da criação do grupo de recursos. Um novo cliente é criado, considerando a capacidade de autenticar com o Azure e, em seguida, um método é chamado. O método tem até o mesmo nome (`CreateOrUpdate`) que o método correspondente dos grupos de recurso. Esse padrão é visto repetidamente no SDK. Os métodos que executam um trabalho semelhante, normalmente têm o mesmo nome.

A maior diferença é o valor de retorno do método `deploymentsClient.CreateOrUpdate()`. Esse valor é um objeto `Future`, que segue o [padrão de design do objeto Future](https://en.wikipedia.org/wiki/Futures_and_promises). Os objetos Future representam uma operação de longa execução no Azure que talvez você queira sondar ocasionalmente durante a execução de outro trabalho.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

Neste exemplo, a melhor coisa a fazer é aguardar a conclusão da operação. Aguardar um objeto Future requer um [objeto de contexto](https://blog.golang.org/context) e o cliente que criou o objeto Future. Há duas fontes de erro possíveis: um erro causado por parte do cliente durante a tentativa de invocar o método e uma resposta de erro do servidor. O segundo é retornado como parte da chamada `deploymentFuture.Result()`.

### <a name="obtaining-the-assigned-ip-address"></a>Obtendo o endereço IP atribuído

Para fazer algo com a VM recém-criada, é necessário o endereço IP atribuído. Os endereços IP são seus próprios recursos do Azure separados, associados aos recursos do Controlador de Interface de Rede (NIC).

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

Esse método se baseia em informações armazenadas no arquivo de parâmetros. O código pode consultar a VM diretamente para obter seu NIC, consultar o NIC para obter seu recurso de IP e, em seguida, consultar diretamente o recurso de IP. Isso consiste em uma grande cadeia de dependências e operações para resolver, o que se torna caro. Como as informações do JSON são locais, elas podem ser carregadas como alternativa.

Da mesma forma, os valores para o usuário da VM e a senha são carregados do JSON.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você usou um modelo existente e implantou-o por meio da linguagem Go. Em seguida, você se conectou à VM recém-criada via SSH para garantir sua execução.

Para continuar aprendendo sobre como trabalhar com máquinas virtuais no ambiente do Azure com a linguagem Go, confira os [exemplos de computação do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou os [exemplos de gerenciamento de recursos do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).
