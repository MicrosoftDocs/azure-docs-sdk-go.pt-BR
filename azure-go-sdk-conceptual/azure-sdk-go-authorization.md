---
title: Autenticação com o SDK do Azure para Go
description: Saiba mais sobre os métodos de autenticação disponíveis no SDK do Azure para Go e como usá-los.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.service: active-directory
ms.component: authentication
ms.openlocfilehash: 370f5607b89c0044022f7987d06c3a55c9d6f352
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319876"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Métodos de autenticação no SDK do Azure para Go

O SDK do Azure para Go oferece uma variedade de tipos de autenticação e métodos que podem ser usados pelo seu aplicativo. Os métodos de autenticação com suporte variam do pull de informações de variáveis de ambiente até a autenticação interativa baseada na Web. Este artigo apresenta os tipos de autenticação disponíveis no SDK, além dos métodos para usá-los. Você também aprenderá as melhores práticas para selecionar o tipo de autenticação adequado para seu aplicativo.

## <a name="available-authentication-types-and-methods"></a>Métodos e tipos de autenticação disponíveis

O SDK do Azure para Go oferece vários tipos diferentes de autenticação, usando conjuntos de credenciais diferentes. Cada um desses tipos de autenticação está disponível por métodos de autenticação diferentes, que são como o SDK usa essas credenciais como entrada. A tabela a seguir descreve os tipos disponíveis de autenticação e as situações em que eles são recomendados para uso pelo seu aplicativo.

| Tipo de autenticação. | Recomendado quando... |
|---------------------|---------------------|
| Autenticação baseada em certificado | Você tem um certificado X509 que foi configurado para um usuário do Azure Active Directory (AAD) ou uma entidade de serviço. Para saber mais, consulte [Introdução à autenticação baseada em certificado no Azure Active Directory]. |
| Credenciais do cliente | Você tem uma entidade de serviço configurada que está definida para esse aplicativo ou para uma classe de aplicativos à qual ele pertence. Para saber mais, consulte [Criar uma entidade de serviço com a CLI do Azure 2.0]. |
| MSI (Identidade do serviço gerenciado) | O aplicativo está em execução em um recurso do Azure que foi configurado com a MSI. Para saber mais, consulte [MSI (Identidade do serviço gerenciado) para recursos do Azure]. |
| Token de dispositivo | Seu aplicativo foi projetado para ser usado __apenas__ interativamente, além disso, ele terá uma variedade de usuários, potencialmente de vários locatários do AAD. Os usuários têm acesso a um navegador da Web para fazer logon. Para obter mais informações, consulte [Usar a autenticação de token do dispositivo](#use-device-token-authentication).|
| Nome de usuário/senha | Você tem um aplicativo interativo que não pode usar nenhum outro método de autenticação. Os usuários não têm a autenticação multifator habilitada para seu logon no AAD. |

> [!IMPORTANT]
> Se você usar um tipo de autenticação que não sejam as credenciais do cliente, seu aplicativo deve estar registrado no Active Directory do Azure. Para saber mais, [Integrando aplicativos com o Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

> [!NOTE]
> A menos que você tenha requisitos especiais, evite a autenticação de nome de usuário e senha. Em situações em que o logon baseado em usuário for apropriado, pode ser usada a autenticação de token do dispositivo.

[Introdução à autenticação baseada em certificado no Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Criar uma entidade de serviço com a CLI do Azure 2.0]: /cli/azure/create-an-azure-service-principal-azure-cli
[MSI (Identidade do serviço gerenciado) para recursos do Azure]: /azure/active-directory/managed-service-identity/overview

Esses tipos de autenticação estão disponíveis por meio de métodos diferentes. A [_Autenticação baseada em ambiente_](#use-environment-based-authentication) lê credenciais diretamente do ambiente do programa. A [_Autenticação baseada em arquivo_](#use-file-based-authentication) carrega um arquivo contendo as credenciais de entidade de serviço. A [_Autenticação baseada em cliente_](#use-an-authentication-client) usa um objeto em código Go e torna você responsável por fornecer as credenciais durante a execução do programa. Por fim, a [_autenticação de token do dispositivo_](#use-device-token-authentication) requer que os usuários façam logon interativamente por meio de um navegador da Web com um token, não podendo ser usada com autenticações baseadas em ambiente ou arquivo.

Todos os tipos e funções de autenticação estão disponíveis no pacote `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> A menos que você tenha requisitos especiais, evite a autenticação baseada em cliente. Esse método de autenticação incentiva práticas incorretas. Em especial, o uso da autenticação baseada em cliente torna tentador o uso de credenciais embutidas em códigos. Escrever um código personalizado para a autenticação também pode gerar falha em versões futuras do SDK, caso os requisitos de autenticação sejam alterados.

## <a name="use-environment-based-authentication"></a>Usar a autenticação baseada em ambiente

Se você estiver executando seu aplicativo em um ambiente rigidamente controlado, como um contêiner, a autenticação baseada em ambiente é uma opção natural. Você configura o ambiente de shell antes de executar seu aplicativo, e o SDK do Go lê essas variáveis de ambiente em tempo de execução para autenticar com o Azure. 

A autenticação baseada em ambiente oferece suporte para todos os métodos de autenticação, exceto os tokens de dispositivo, avaliados na seguinte ordem: credenciais de cliente, certificados, nome de usuário/senha e identidade de serviço gerenciado (MSI). Se uma variável de ambiente necessária for removida ou se o SDK obtiver uma recusa do serviço de autenticação, tenta-se usar o próximo tipo de autenticação. Se o SDK não puder ser autenticado a partir do ambiente, ele retornará um erro.

A tabela a seguir detalha as variáveis de ambiente que precisam ser definidas para cada tipo de autenticação com suporte da autenticação baseada em ambiente.

| Tipo de autenticação. | Variável de ambiente | DESCRIÇÃO |
| ------------------- | -------------------- | ----------- |
| __Credenciais do cliente__ | `AZURE_TENANT_ID` | A ID do locatário do Active Directory à qual pertence a entidade de serviço. |
| | `AZURE_CLIENT_ID` | O nome ou a ID da entidade de serviço. |
| | `AZURE_CLIENT_SECRET` | O segredo associado à entidade de serviço. |
| __Certificate__ | `AZURE_TENANT_ID` | A ID do locatário do Active Directory com qual o certificado está registrado. |
| | `AZURE_CLIENT_ID` | A ID de cliente do aplicativo associada ao certificado. |
| | `AZURE_CERTIFICATE_PATH` | O caminho para o arquivo do certificado do cliente. |
| | `AZURE_CERTIFICATE_PASSWORD` | A senha para o certificado do cliente. |
| __Nome de usuário/senha__ | `AZURE_TENANT_ID` | A ID do locatário do Active Directory à qual o usuário pertence. |
| | `AZURE_CLIENT_ID` | A ID de cliente do aplicativo. |
| | `AZURE_USERNAME` | O nome de usuário para fazer logon. |
| | `AZURE_PASSWORD` | A senha para fazer logon. |
| __MSI__ | | A MSI não requer que nenhuma credencial seja definida. O aplicativo deve estar em execução em um recurso do Azure configurado para usar a MSI. Para obter detalhes, consulte [MSI (Identidade do serviço gerenciado) para recursos do Azure]. |

Caso precise se conectar a um ponto de extremidade de nuvem ou de gerenciamento que não seja o padrão de nuvem pública do Azure, também é possível definir as seguintes variáveis de ambiente. Os motivos mais comuns para defini-las são: se você usa o Azure Stack, uma nuvem em uma região geográfica diferente ou o modelo de implantação clássico do Azure.

| Variável de ambiente | DESCRIÇÃO  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | O nome do ambiente de nuvem ao qual se conectar. |
| `AZURE_AD_RESOURCE` | A ID de recurso do Active Directory para usar ao se conectar. Deve ser um URI apontando para seu ponto de extremidade de gerenciamento. |

Ao usar a autenticação baseada em ambiente, chame a função [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) para obter o objeto autorizador. Esse objeto é então ativado na propriedade `Authorizer` de clientes para permitir o acesso ao Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Autenticação no Azure Stack

Para autenticar no Azure Stack, você precisa definir as seguintes variáveis:

| Variável de ambiente | DESCRIÇÃO  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | O ponto de extremidade do Active Directory. |
| `AZURE_AD_RESOURCE` | A ID de recurso do Active Directory. |

Essas variáveis podem ser recuperadas de informações de metadados do Azure Stack. Para recuperar os metadados, abra um navegador da Web em seu ambiente do Azure Stack e use a URL: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`

O `ResourceManagerURL` varia de acordo com o nome da região, nome do computador e nome externo de domínio totalmente qualificado (FQDN) da sua implantação do Azure Stack:

| Ambiente | ResourceManagerURL |
|----------------------|--------------|
| Kit de desenvolvimento | `https://management.local.azurestack.external/` |
| Sistemas Integrados | `https://management.(region).ext-(machine-name).(FQDN)` |

Para obter mais detalhes sobre como usar o SDK do Azure para Go no Azure Stack, confira [Usar perfis de versão de API com o Go no Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-version-profiles-go)


## <a name="use-file-based-authentication"></a>Usar a autenticação baseada em arquivo

A autenticação baseada em arquivo só funciona com as credenciais do cliente quando elas estão armazenadas em um formato de arquivo local gerado pela [CLI do Azure 2.0](/cli/azure). É possível criar esse arquivo facilmente ao criar uma nova entidade de serviço com o parâmetro `--sdk-auth`. Caso planeje usar a autenticação baseada em arquivo, verifique se esse argumento é fornecido durante a criação de uma entidade de serviço. Uma vez que a CLI imprime a saída para `stdout`, redirecione a saída para um arquivo.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Defina a variável de ambiente `AZURE_AUTH_LOCATION` para o local em que se encontra o arquivo de autorização. Essa variável de ambiente é lida pelo aplicativo, e as credenciais dentro dela são analisadas. Caso precise selecionar o arquivo de autorização em tempo de execução, manipule o ambiente do programa usando a função [os.Setenv](https://golang.org/pkg/os/#Setenv).

Para carregar as informações de autenticação, chame a função [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). Ao contrário da autorização baseada em ambiente, a autorização baseada em arquivo exige um ponto de extremidade do recurso.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Para obter mais informações sobre como usar as entidades de serviço e como gerenciar suas permissões de acesso, consulte [Criar uma entidade de serviço com a CLI do Azure 2.0].

## <a name="use-device-token-authentication"></a>Usar autenticação de token do dispositivo

Caso queira que os usuários façam logon interativamente, a melhor maneira de oferecer essa capacidade é por meio da autenticação de token do dispositivo. Esse fluxo de autenticação passa ao usuário um token para ser colado em um site de logon da Microsoft, onde eles, então, fazem logon com uma conta do Azure Active Directory (AAD). Esse método de autenticação oferece suporte a contas que têm a autenticação multifator habilitada, ao contrário de autenticação padrão de nome de usuário/senha.

Para usar uma autenticação de token do dispositivo, crie um autorizador [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) com a função [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig). Chame o [Autorizador](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) no objeto resultante para iniciar o processo de autenticação. A autenticação de fluxo do dispositivo bloqueia a execução do programa até que todo o fluxo de autenticação seja concluído.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Usar um cliente de autenticação

Caso precise de um tipo específico de autenticação e esteja disposto que seu programa faça o trabalho de carregar informações de autenticação do usuário, você pode usar qualquer cliente que esteja de acordo com a interface [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Use um tipo que implemente essa interface quando você desejar um programa interativo, use arquivos de configuração especializados ou tenha um requisito que impeça o uso de outro método de autenticação.

> [!WARNING]
> Nunca use credenciais do Azure embutidas em código em um aplicativo. Colocar segredos em um binário de aplicativo facilita sua extração por um invasor, quer o aplicativo esteja em execução ou não. Isso coloca em risco todos os recursos do Azure para os quais as credenciais são autorizadas!

A tabela a seguir lista os tipos no SDK que estão de acordo com a interface `AuthorizerConfig`.

| Tipo de autenticação. | Tipo de autorizador |
|---------------------|-----------------------|
| Autenticação baseada em certificado | [ClientCertificateConfig] |
| Credenciais do cliente | [ClientCredentialsConfig] |
| MSI (Identidade do serviço gerenciado) | [MSIConfig] |
| Nome de usuário/senha | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Crie um autenticador com sua função `New` associada, depois chame `Authorize` no objeto resultante para realizar a autenticação. Por exemplo, para usar a autenticação baseada em certificado:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
