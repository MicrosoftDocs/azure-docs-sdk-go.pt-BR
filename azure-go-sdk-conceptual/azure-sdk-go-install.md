---
title: Instalando o SDK do Azure para linguagem Go
description: Como instalar, fornecer e configurar o SDK do Azure para linguagem Go.
keywords: azure, sdk, go, golang, azure sdk
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 7fc0a3ff71b0b06f616ae43cff311352fe873345
ms.sourcegitcommit: 890f5f01a70e7e376e6bb98a2030afbfc016f538
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2018
---
# <a name="installing-the-azure-sdk-for-go"></a>Instalando o SDK do Azure para linguagem Go

Bem-vindo ao SDK do Azure para linguagem Go! Esse SDK permite gerenciar e interagir com os serviços do Azure em seus aplicativos Go.

## <a name="get-the-azure-sdk-for-go"></a>Obtenha o SDK do Azure para linguagem Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Trabalhar com os Azure Storage Blobs requer um SDK separado.

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a>Vendoring no SDK do Azure para linguagem Go

O SDK do Azure para linguagem Go pode usar a funcionalidade de vendoring pelo [dep](https://github.com/golang/dep). Por motivos de estabilidade, é recomendado usar vendoring. Para usar o suporte `dep`, adicione o `github.com/Azure/azure-sdk-for-go` a uma seção de `[[constraint]]` do seu `Gopkg.toml`. Por exemplo, para o vendor na versão `14.0.0`, adicione a seguinte entrada:

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a>Incluindo o SDK do Azure para linguagem Go em seu projeto

Para usar os serviços do Azure a partir de seu código Go, importe todos os serviços com os quais você interage e os módulos `autorest` necessários.
Você obterá uma lista completa dos módulos disponíveis do GoDoc para os [serviços disponíveis](https://godoc.org/github.com/Azure/azure-sdk-for-go) e [pacotes AutoRest](https://godoc.org/github.com/Azure/go-autorest). Os pacotes mais comuns do `go-autorest` de que você precisa são:

| Pacote | DESCRIÇÃO |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objetos para lidar com a autenticação de cliente de serviço |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Constantes para interações com os serviços do Azure |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Mecanismos de autenticação para acessar os serviços do Azure |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Digite os auxiliares de asserção para trabalhar com as estruturas de dados do SDK do Azure |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Os módulos para os serviços do Azure têm controle de versão independentemente das APIs do SDK para eles. Essas versões são parte do caminho de importação do módulo e são de uma _versão do serviço_ ou um _perfil_. No momento, é recomendável que você use uma versão de serviço específica para desenvolvimento e versão. Os serviços são localizados no módulo `services`. O caminho completo para a importação é o nome do serviço, seguido da versão no formato `YYYY-MM-DD`, seguido do nome do serviço novamente. Por exemplo, para incluir a versão `2017-03-30` do serviço de computação:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

No momento, é recomendável que você use a versão mais recente de um serviço, a menos que você tenha um motivo para fazer o contrário.

Se você precisar de um instantâneo coletivo dos serviços, você também pode selecionar uma versão de perfil. Agora, o único perfil bloqueado é a versão `2017-03-30`, que pode não ter os recursos mais recentes dos serviços. Os perfis estão localizados no módulo `profiles`, com a versão no formato `YYYY-MM-DD`. Os serviços são agrupados em sua versão de perfil. Por exemplo, para importar o módulo de gerenciamento de recursos do Azure a partir do perfil `2017-03-09`:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> Os perfis `preview` e `latest` também estão disponíveis. Porém, não é recomendado usá-los. Esses perfis são versões progressivas, e o comportamento do serviço poderá mudar a qualquer momento.

## <a name="next-steps"></a>Próximas etapas

Para começar a usar o SDK do Azure para linguagem Go, experimente um início rápido.

* [Implantar uma máquina virtual no Azure a partir de um modelo](azure-sdk-go-qs-vm.md)
* [Transferir objetos para o Armazenamento de Blobs do Azure usando o SDK de Blobs do Azure para linguagem Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Conectar-se ao Banco de Dados do Azure para PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Se você quiser começar com outros serviços no SDK Go imediatamente, dê uma olhada em alguns códigos de exemplo disponíveis.

* [Autenticar com os serviços do Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [Implantar novas máquinas virtuais com autenticação SSH](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Implantar uma imagem de contêiner nas Instâncias de Contêiner do Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Criar um cluster no Serviço Kubernetes do Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Trabalhar com serviços de armazenamento do Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Todos os exemplos do SDK do Azure para linguagem Go](https://github.com/azure-samples/azure-sdk-for-go-samples)