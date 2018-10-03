---
title: Autenticação com o SDK do Azure para Go
description: Saiba mais sobre os métodos de autenticação disponíveis no SDK do Azure para Go e como usá-los.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.component: authentication
ms.openlocfilehash: f5c2c56e43828f0bedad0b5781dc71991ce1fd3e
ms.sourcegitcommit: 172f81dd6e4c6a275dc8031815aa87cdb488cbf0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47231668"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Métodos de autenticação no SDK do Azure para Go

O SDK do Azure para linguagem Go oferece várias maneiras de se autenticar no Azure. Esses _tipos_ de autenticação são invocados por meio de diferentes _métodos_ de autenticação. Este artigo aborda os tipos e métodos disponíveis e como escolher quais são ideais para seu aplicativo.

## <a name="available-authentication-types-and-methods"></a>Métodos e tipos de autenticação disponíveis

O SDK do Azure para Go oferece vários tipos diferentes de autenticação, usando conjuntos de credenciais diferentes. Cada tipo de autenticação está disponível por métodos de autenticação diferentes, que são como o SDK usa essas credenciais como entrada. A tabela a seguir descreve os tipos disponíveis de autenticação e as situações em que eles são recomendados para uso pelo seu aplicativo.

| Tipo de autenticação | Recomendado quando... |
|---------------------|---------------------|
| Autenticação baseada em certificado | Você tem um certificado X509 que foi configurado para um usuário do Azure Active Directory (AAD) ou uma entidade de serviço. Para saber mais, consulte [Introdução à autenticação baseada em certificado no Azure Active Directory]. |
| Credenciais do cliente | Você tem uma entidade de serviço configurada que está definida para esse aplicativo ou para uma classe de aplicativos à qual ele pertence. Para saber mais, consulte [Criar uma entidade de serviço com a CLI do Azure]. |
| Identidades gerenciadas dos recursos do Azure | O aplicativo está em execução em um recurso do Azure que foi configurado com uma identidade gerenciada. Para saber mais, confira [Identidades gerenciadas para os recursos do Azure]. |
| Token de dispositivo | Seu aplicativo foi projetado para ser usado interativamente __apenas__. Os usuários podem ter a autenticação multifator habilitada. Os usuários têm acesso a um navegador da Web para fazer entrar. Para obter mais informações, consulte [Usar a autenticação de token do dispositivo](#use-device-token-authentication).|
| Nome de usuário/senha | Você tem um aplicativo interativo que não pode usar outros métodos de autenticação. Os usuários não têm a autenticação multifator habilitada para entrar no AAD. |

> [!IMPORTANT]
> Se você usar um tipo de autenticação que não sejam as credenciais do cliente, seu aplicativo deve estar registrado no Active Directory do Azure. Para saber mais, [Integrando aplicativos com o Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).
>
> [!NOTE]
> A menos que você tenha requisitos especiais, evite a autenticação de nome de usuário e senha. Em situações em que a entrada baseada em usuário for apropriada, pode ser usada a autenticação de token do dispositivo.

[Introdução à autenticação baseada em certificado no Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Criar uma entidade de serviço com a CLI do Azure]: /cli/azure/create-an-azure-service-principal-azure-cli
[Identidades gerenciadas para os recursos do Azure]: /azure/active-directory/managed-identities-azure-resources/overview

Esses tipos de autenticação estão disponíveis por meio de métodos diferentes.

* A [_Autenticação baseada em ambiente_](#use-environment-based-authentication) lê credenciais diretamente do ambiente do programa.
* A [_Autenticação baseada em arquivo_](#use-file-based-authentication) carrega um arquivo contendo as credenciais de entidade de serviço.
* A [_Autenticação baseada em cliente_](#use-an-authentication-client) usa um objeto em código e torna você responsável por fornecer as credenciais durante a execução do programa.
* A [_Autenticação de token do dispositivo_](#use-device-token-authentication) exige que os usuários façam logon interativamente por meio de um navegador da Web com um token.

Todos os tipos e funções de autenticação estão disponíveis no pacote `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> A menos que você tenha requisitos especiais, evite a autenticação baseada em cliente. Esse método de autenticação incentiva práticas incorretas. Em especial, o uso da autenticação baseada em cliente torna tentador o uso de credenciais embutidas em códigos. Escrever um código personalizado para a autenticação também pode gerar falha em versões futuras do SDK, caso os requisitos de autenticação sejam alterados.

## <a name="use-environment-based-authentication"></a>Usar a autenticação baseada em ambiente

Se você estiver executando seu aplicativo em um ambiente controlado, a autenticação baseada em ambiente é uma opção natural. Com esse método de autenticação, você pode configurar o ambiente do shell antes de executar seu aplicativo. No tempo de execução, o SDK da linguagem Go lê essas variáveis de ambiente para se autenticar no Azure.

A autenticação baseada em ambiente oferece suporte para todos os métodos de autenticação, exceto os tokens de dispositivo, avaliados na seguinte ordem:

* Credenciais do cliente
* Certificados X509
* Nome de usuário/senha
* Identidades gerenciadas dos recursos do Azure

Se um tipo de autenticação não tiver valores definidos ou for recusado, o SDK tenta automaticamente o próximo tipo de autenticação. O SDK retornará um erro quando não houver mais tipos disponíveis para realizar as tentativas.

A tabela a seguir detalha as variáveis de ambiente que precisam ser definidas para cada tipo de autenticação com suporte da autenticação baseada em ambiente.

| Tipo de autenticação | Variável de ambiente | DESCRIÇÃO |
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
| | `AZURE_USERNAME` | O nome de usuário com o qual entrar. |
| | `AZURE_PASSWORD` | A senha com a qual entrar. |
| __Identidade gerenciada__ | | Nenhuma credencial é necessária para a autenticação da identidade gerenciada. O aplicativo deve estar em execução em um recurso do Azure configurado para usar as identidades gerenciadas. Para obter mais detalhes, confira [Identidades gerenciadas para os recursos do Azure]. |

Para se conectar a um ponto de extremidade de gerenciamento ou de nuvem que não seja o padrão de nuvem pública do Azure, defina as seguintes variáveis de ambiente. Se você usa o Azure Stack, os motivos mais comuns são: uma nuvem em uma região geográfica diferente ou o modelo de implantação clássico.

| Variável de ambiente | DESCRIÇÃO  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | O nome do ambiente de nuvem ao qual se conectar. |
| `AZURE_AD_RESOURCE` | A ID de recurso do Active Directory para usar ao conectar-se, como um URI para seu ponto de extremidade de gerenciamento. |

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

Para ter mais informações sobre como usar o SDK do Azure para linguagem Go no Azure Stack, confira [Usar perfis de versão de API com a linguagem Go no Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go)

## <a name="use-file-based-authentication"></a>Usar a autenticação baseada em arquivo

A autenticação baseada em arquivo usa um formato de arquivo gerado pela [CLI do Azure](/cli/azure). É possível criar esse arquivo facilmente ao criar uma nova entidade de serviço com o parâmetro `--sdk-auth`. Caso planeje usar a autenticação baseada em arquivo, verifique se esse argumento é fornecido durante a criação de uma entidade de serviço. Uma vez que a CLI imprime a saída para `stdout`, redirecione a saída para um arquivo.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Defina a variável de ambiente `AZURE_AUTH_LOCATION` para o local em que se encontra o arquivo de autorização. Essa variável de ambiente é lida pelo aplicativo, e as credenciais dentro dela são analisadas. Caso precise selecionar o arquivo de autorização em tempo de execução, manipule o ambiente do programa usando a função [os.Setenv](https://golang.org/pkg/os/#Setenv).

Para carregar as informações de autenticação, chame a função [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). Ao contrário da autorização baseada em ambiente, a autorização baseada em arquivo exige um ponto de extremidade do recurso.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Para obter mais informações sobre como usar as entidades de serviço e como gerenciar suas permissões de acesso, confira [Criar uma entidade de serviço com a CLI do Azure].

## <a name="use-device-token-authentication"></a>Usar autenticação de token do dispositivo

Caso queira que os usuários entrem interativamente, a autenticação de token do dispositivo é a melhor forma. Esse fluxo de autenticação passa ao usuário um token para ser colado em um site de entrada da Microsoft, onde eles, então, autenticam com uma conta do Azure Active Directory (AAD). Esse método de autenticação oferece suporte a contas que têm a autenticação multifator habilitada, ao contrário de autenticação padrão de nome de usuário/senha.

Para usar uma autenticação de token do dispositivo, crie um autorizador [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) com a função [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig). Chame o [Autorizador](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) no objeto resultante para iniciar o processo de autenticação. A autenticação de fluxo do dispositivo bloqueia a execução do programa até que todo o fluxo de autenticação seja concluído.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Usar um cliente de autenticação

Caso precise de um tipo específico de autenticação e esteja disposto que seu programa faça o trabalho de carregar informações de autenticação do usuário, você pode usar qualquer cliente que esteja de acordo com a interface [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Use um tipo que implementará essa interface quando você:

* Escrever um programa interativo
* Usar arquivos de configuração especializados
* Tiver um requisito que impeça o uso de um método de autenticação interno

> [!WARNING]
> Nunca use credenciais do Azure embutidas em código em um aplicativo. Colocar segredos em um binário de aplicativo facilita sua extração por um invasor, quer o aplicativo esteja em execução ou não. Isso coloca em risco todos os recursos do Azure para os quais as credenciais são autorizadas!

A tabela a seguir lista os tipos no SDK que estão de acordo com a interface `AuthorizerConfig`.

| Tipo de autenticação | Tipo de autorizador |
|---------------------|-----------------------|
| Autenticação baseada em certificado | [ClientCertificateConfig] |
| Credenciais do cliente | [ClientCredentialsConfig] |
| Identidades gerenciadas dos recursos do Azure | [MSIConfig] |
| Nome de usuário/senha | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Crie um autenticador com sua função `New` associada e, em seguida, chame `Authorize` no objeto resultante para autenticar. Por exemplo, para usar a autenticação baseada em certificado:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
