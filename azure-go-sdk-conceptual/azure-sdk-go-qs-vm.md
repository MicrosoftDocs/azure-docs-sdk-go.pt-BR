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
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Início rápido: implantar uma máquina virtual do Azure de um modelo com o SDK do Azure para linguagem Go

Este início rápido mostra como implantar recursos de um modelo do Azure Resource Manager, usando o SDK do Azure para linguagem Go. Os modelos são instantâneos de todos os recursos em um [grupo de recursos do Azure](/azure/azure-resource-manager/resource-group-overview). Ao longo do processo, você vai se familiarizar com a funcionalidade e as convenções do SDK.

No final deste início rápido, você terá uma VM em execução na qual pode entrar com um nome de usuário e senha.

> [!NOTE]
> Para ver a criação de uma VM na linguagem Go sem o uso de um modelo do Resource Manager, existe um [exemplo imperativo](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) que demonstra como criar e configurar todos os recursos VM com o SDK. Usar um modelo neste exemplo permite focar-se nas convenções do SDK sem entrar em muitos detalhes sobre a arquitetura de serviço do Azure.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Caso use uma instalação local da CLI do Azure, este início rápido requer a CLI versão __2.0.28__ ou posterior. Execute `az --version` para garantir que a instalação de sua CLI atende a esse requisito. Se você precisar instalar ou atualizar, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Instale o SDK do Azure para linguagem Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Criar uma entidade de serviço

Para entrar em modo não interativo no Azure com um aplicativo, você precisa de uma entidade de serviço. As entidades de serviço fazem parte do controle de acesso baseado em função (RBAC), que cria uma identidade de usuário exclusiva. Para criar uma nova entidade de serviço com a CLI, execute o comando a seguir:

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

Defina a variável de ambiente `AZURE_AUTH_LOCATION` para ser o caminho completo para esse arquivo. Em seguida, o SDK localiza e lê as credenciais diretamente desse arquivo, sem que você precise fazer quaisquer alterações ou registrar informações da entidade de serviço.

## <a name="get-the-code"></a>Obter o código

Obtenha o código de início rápido e todas as suas dependências com `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

Você não precisa fazer nenhuma modificação no código-fonte se a variável `AZURE_AUTH_LOCATION` estiver definida corretamente. Quando o programa for executado, ele carregará todas as informações de autenticação necessárias de lá.

## <a name="running-the-code"></a>Executando o código

Execute o início rápido com o comando `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Se a implantação for bem-sucedida, você receberá uma mensagem fornecendo o nome de usuário, o endereço IP e a senha para fazer logon na máquina virtual recém-criada. Use SSH neste computador para ver se ele está funcionando. 

## <a name="cleaning-up"></a>Limpando

Limpe todos os recursos criados nesse início rápido excluindo o grupo de recursos com a CLI.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

Além disso, exclua a entidade de serviço que foi criada. No arquivo `quickstart.auth`, há uma chave JSON para `clientId`. Copie este valor para a variável de ambiente `CLIENT_ID_VALUE` e execute o seguinte comando da CLI do Azure:

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

Em que você fornece o valor para `CLIENT_ID_VALUE` de `quickstart.auth`.

> [!WARNING]
> Não excluir a entidade de serviço desse aplicativo deixa-o ativo no locatário do Azure Active Directory.
> Embora o nome e a senha para a entidade de serviço sejam gerados como UUIDs, certifique-se de seguir as boas práticas de segurança, excluindo quaisquer entidades de serviço não utilizadas e aplicativos do Azure Active Directory.

## <a name="code-in-depth"></a>Código em detalhes

O que o código de início rápido faz é dividido em um bloco de variáveis e várias funções pequenas, que serão uma a uma discutidas aqui.

### <a name="variables-constants-and-types"></a>Variáveis, constantes e tipos

Uma vez que o início rápido é autossuficiente, ele usa variáveis e constantes globais.

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

Os valores são declarados, fornecendo os nomes dos recursos criados. O local também é especificado aqui, que pode ser alterado para ver como as implantações se comportam em outros data centers. Nem todo datacenter tem todos os recursos necessários disponíveis.

O tipo `clientInfo` possui as informações carregadas do arquivo de autenticação para configurar clientes no SDK e definir a senha da VM.

As constantes `templateFile` e `parametersFile` apontam para os arquivos necessários para a implantação. O `authorizer` será configurados pelo SDK do Go para autenticação, e a variável `ctx` é um [contexto do Go](https://blog.golang.org/context) para as operações de rede.

### <a name="authentication-and-initialization"></a>Autenticação e inicialização

A função `init` configura a autenticação. Uma vez que a autorização é uma pré-condição para tudo no início rápido, faz sentido que ela seja parte da inicialização. Ele também carrega algumas informações necessárias do arquivo de autenticação para configurar os clientes e a VM.

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

Primeiro, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) é chamado para carregar as informações de autenticação do arquivo localizado em `AZURE_AUTH_LOCATION`. Em seguida, esse arquivo é carregado manualmente pela função `readJSON` (omitida aqui) para extrair os dois valores necessários para executar o restante do programa: a ID de assinatura do cliente e o segredo da entidade de serviço, que também é usado para a senha da VM.

> [!WARNING]
> Para simplificar o início rápido, a senha da entidade de serviço é reutilizada. Na produção, tome cuidado para __nunca__ reutilizar uma senha que forneça acesso aos recursos do Azure.

### <a name="flow-of-operations-in-main"></a>Fluxo de operações em main()

A função `main` é simples, apenas indica o fluxo das operações e executa a verificação de erros.

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

As etapas que o código executa são, em ordem:

* Criar o grupo de recursos para implantar em (`createGroup`)
* Criar a implantação dentro deste grupo (`createDeployment`)
* Obter e exibir as informações de logon da VM implantada (`getLogin`)

### <a name="create-the-resource-group"></a>Criar o grupo de recursos

A função `createGroup` cria o grupo de recursos. Ao observar o fluxo de chamadas e os argumentos vemos a maneira como as interações de serviço são estruturadas no SDK.

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

O fluxo geral de interação com um serviço do Azure é:

* Criar o cliente usando o método `service.New*Client()`, no qual `*` é o tipo de recurso do `service` com o qual você deseja interagir. Essa função sempre usa uma ID de assinatura.
* Defina o método de autorização para o cliente, permitindo que ele interaja com a API remota.
* Realize o método de chamada no cliente correspondente à API remota. Os métodos de serviço do cliente geralmente levam o nome do recurso e um objeto de metadados.

A função [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) é usada aqui para executar uma conversão de tipo. Os parâmetros para métodos do SDK usam ponteiros quase exclusivamente, portanto, métodos de conveniência são fornecidos para facilitar as conversões de tipo. Confira a documentação do módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obter a lista completa de conversores de conveniência e seu comportamento.

O método `groupsClient.CreateOrUpdate` retorna um ponteiro para um tipo de dados que representa o grupo de recursos. Um valor de retorno direto desse tipo indica uma operação de execução curta que deve ser síncrona. Na próxima seção, você verá um exemplo de uma operação de longa execução e como interagir com ela.

### <a name="perform-the-deployment"></a>Executar a implantação

Depois de criar o grupo de recursos, deve-se executar a implantação. Esse código é dividido em seções menores para enfatizar diferentes partes de sua lógica.

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

Os arquivos de implantação são carregados por `readJSON`, cujos detalhes são ignorados aqui. Essa função retorna um `*map[string]interface{}`, o tipo usado para construir os metadados para a chamada de implantação de recursos. A senha da VM também é definida manualmente nos parâmetros de implantação.

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

Esse código segue o mesmo padrão da criação do grupo de recursos. Um novo cliente é criado, considerando a capacidade de autenticar com o Azure e, em seguida, um método é chamado.
O método tem até o mesmo nome (`CreateOrUpdate`) que o método correspondente dos grupos de recurso. Esse padrão é visto em todo o SDK.
Os métodos que executam um trabalho semelhante, normalmente têm o mesmo nome.

A maior diferença é o valor de retorno do método `deploymentsClient.CreateOrUpdate`. Esse valor é do tipo [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), que segue o [padrão de design do Future](https://en.wikipedia.org/wiki/Futures_and_promises). Valores Futures representam uma operação de longa execução no Azure que você pode pesquisar, cancelar ou bloquear ao ser concluída.

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

Neste exemplo, a melhor coisa a fazer é aguardar a conclusão da operação. Aguardar um valor Future requer um [objeto de contexto](https://blog.golang.org/context) e o cliente que criou o `Future`. Há duas fontes de erro possíveis: um erro causado por parte do cliente durante a tentativa de invocar o método e uma resposta de erro do servidor. O segundo é retornado como parte da chamada `deploymentFuture.Result`.

### <a name="get-the-assigned-ip-address"></a>Obter o endereço IP atribuído

Para fazer algo com a VM recém-criada, é necessário o endereço IP atribuído. Os endereços IP são seus próprios recursos do Azure separados, associados aos recursos do Controlador de Interface de Rede (NIC).

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

Esse método se baseia em informações armazenadas no arquivo de parâmetros. O código pode consultar a VM diretamente para obter seu NIC, consultar o NIC para obter seu recurso de IP e, em seguida, consultar diretamente o recurso de IP. Isso consiste em uma grande cadeia de dependências e operações para resolver, o que se torna caro. Como as informações do JSON são locais, elas podem ser carregadas como alternativa.

O valor para o usuário da VM também é carregado do JSON. A senha da VM foi carregada anteriormente do arquivo de autenticação.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você usou um modelo existente e implantou-o por meio da linguagem Go. Em seguida, você se conectou à VM recém-criada via SSH.

Para continuar aprendendo sobre como trabalhar com máquinas virtuais no ambiente do Azure com a linguagem Go, confira os [exemplos de computação do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) ou os [exemplos de gerenciamento de recursos do Azure para linguagem Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

Para saber mais sobre os métodos de autenticação disponíveis no SDK e para quais tipos de autenticação eles oferecem suporte, consulte [Autenticação com o SDK do Azure para Go](azure-sdk-go-authorization.md).
